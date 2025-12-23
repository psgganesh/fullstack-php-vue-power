---
inclusion: manual
---

# AWS Deployment Guide

## Overview

AWS resources and deployment strategy for the Vuetify + Laravel SPA.

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                         CloudFront                               │
│                    (CDN + HTTPS + Caching)                       │
└─────────────────────────┬───────────────────────────────────────┘
                          │
          ┌───────────────┴───────────────┐
          │                               │
          ▼                               ▼
┌─────────────────┐             ┌─────────────────┐
│   S3 Bucket     │             │  API Gateway    │
│   (Frontend)    │             │  (REST API)     │
│   Vue/Vuetify   │             └────────┬────────┘
└─────────────────┘                      │
                                         ▼
                              ┌─────────────────┐
                              │     Lambda      │
                              │   (Laravel)     │
                              │   via Bref      │
                              └────────┬────────┘
                                       │
                              ┌────────┴────────┐
                              │                 │
                              ▼                 ▼
                    ┌─────────────┐   ┌─────────────┐
                    │    RDS      │   │     S3      │
                    │ (PostgreSQL)│   │  (Storage)  │
                    └─────────────┘   └─────────────┘
```

## AWS Resources Required

### Frontend Hosting

| Resource | Service | Purpose |
|----------|---------|---------|
| Static Hosting | S3 | Host Vue/Vuetify build files |
| CDN | CloudFront | Global distribution, HTTPS, caching |
| DNS | Route 53 | Custom domain management |
| SSL | ACM | Free SSL certificates |

### Backend API

| Resource | Service | Purpose |
|----------|---------|---------|
| Compute | Lambda | Run Laravel via Bref |
| API | API Gateway | REST API endpoints |
| Database | RDS PostgreSQL | Production database |
| Cache | ElastiCache Redis | Session/cache storage |
| Storage | S3 | File uploads |
| Secrets | Secrets Manager | Environment variables |

### Supporting Services

| Resource | Service | Purpose |
|----------|---------|---------|
| Monitoring | CloudWatch | Logs and metrics |
| CI/CD | CodePipeline | Automated deployments |
| Queue | SQS | Background job processing |

## Infrastructure as Code (Terraform)

### Directory Structure

```
infrastructure/
├── environments/
│   ├── dev/
│   │   └── main.tf
│   ├── staging/
│   │   └── main.tf
│   └── prod/
│       └── main.tf
├── modules/
│   ├── frontend/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   ├── backend/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   └── database/
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
└── shared/
    └── backend.tf
```

### Frontend Module (S3 + CloudFront)

```hcl
# modules/frontend/main.tf
resource "aws_s3_bucket" "frontend" {
  bucket = "${var.project_name}-frontend-${var.environment}"
}

resource "aws_s3_bucket_website_configuration" "frontend" {
  bucket = aws_s3_bucket.frontend.id
  
  index_document {
    suffix = "index.html"
  }
  
  error_document {
    key = "index.html"  # SPA fallback
  }
}

resource "aws_cloudfront_distribution" "frontend" {
  enabled             = true
  default_root_object = "index.html"
  
  origin {
    domain_name = aws_s3_bucket.frontend.bucket_regional_domain_name
    origin_id   = "S3-${aws_s3_bucket.frontend.id}"
    
    s3_origin_config {
      origin_access_identity = aws_cloudfront_origin_access_identity.frontend.cloudfront_access_identity_path
    }
  }
  
  default_cache_behavior {
    allowed_methods        = ["GET", "HEAD"]
    cached_methods         = ["GET", "HEAD"]
    target_origin_id       = "S3-${aws_s3_bucket.frontend.id}"
    viewer_protocol_policy = "redirect-to-https"
    
    forwarded_values {
      query_string = false
      cookies {
        forward = "none"
      }
    }
  }
  
  # SPA routing - return index.html for 404s
  custom_error_response {
    error_code         = 404
    response_code      = 200
    response_page_path = "/index.html"
  }
  
  restrictions {
    geo_restriction {
      restriction_type = "none"
    }
  }
  
  viewer_certificate {
    acm_certificate_arn = var.certificate_arn
    ssl_support_method  = "sni-only"
  }
}
```

### Backend Module (Lambda + API Gateway)

```hcl
# modules/backend/main.tf
resource "aws_lambda_function" "api" {
  function_name = "${var.project_name}-api-${var.environment}"
  role          = aws_iam_role.lambda.arn
  handler       = "public/index.php"
  runtime       = "provided.al2"
  timeout       = 30
  memory_size   = 1024
  
  layers = [
    "arn:aws:lambda:${var.region}:534081306603:layer:php-82-fpm:latest"
  ]
  
  environment {
    variables = {
      APP_ENV        = var.environment
      DB_CONNECTION  = "pgsql"
      DB_HOST        = var.db_host
      DB_DATABASE    = var.db_name
      CACHE_DRIVER   = "redis"
      SESSION_DRIVER = "redis"
      REDIS_HOST     = var.redis_host
    }
  }
  
  vpc_config {
    subnet_ids         = var.private_subnet_ids
    security_group_ids = [aws_security_group.lambda.id]
  }
}

resource "aws_apigatewayv2_api" "api" {
  name          = "${var.project_name}-api-${var.environment}"
  protocol_type = "HTTP"
  
  cors_configuration {
    allow_origins = var.allowed_origins
    allow_methods = ["GET", "POST", "PUT", "DELETE", "OPTIONS"]
    allow_headers = ["*"]
  }
}
```

### Database Module (RDS)

```hcl
# modules/database/main.tf
resource "aws_db_instance" "main" {
  identifier     = "${var.project_name}-db-${var.environment}"
  engine         = "postgres"
  engine_version = "15"
  instance_class = var.instance_class
  
  allocated_storage     = 20
  max_allocated_storage = 100
  storage_encrypted     = true
  
  db_name  = var.db_name
  username = var.db_username
  password = var.db_password
  
  vpc_security_group_ids = [aws_security_group.db.id]
  db_subnet_group_name   = aws_db_subnet_group.main.name
  
  backup_retention_period = 7
  skip_final_snapshot     = var.environment != "prod"
  
  tags = {
    Environment = var.environment
  }
}
```

## Deployment Commands

### Frontend Deployment

```bash
# Build frontend
cd frontend
npm run build

# Sync to S3
aws s3 sync dist/ s3://bucket-name --delete

# Invalidate CloudFront cache
aws cloudfront create-invalidation \
  --distribution-id DISTRIBUTION_ID \
  --paths "/*"
```

### Backend Deployment (Bref)

```bash
cd backend

# Install Bref
composer require bref/bref bref/laravel-bridge

# Deploy with Serverless
serverless deploy --stage prod
```

## Cost Estimation (Monthly)

| Service | Dev | Staging | Prod |
|---------|-----|---------|------|
| S3 | $1 | $1 | $5 |
| CloudFront | $1 | $5 | $50+ |
| Lambda | $0 (free tier) | $5 | $20+ |
| API Gateway | $0 (free tier) | $5 | $20+ |
| RDS (t3.micro) | $15 | $15 | $50+ |
| ElastiCache | $0 | $15 | $30+ |
| **Total** | ~$17 | ~$46 | ~$175+ |

## Project-Specific Resources

<!-- AGENT: Replace this section with project-specific AWS resources -->

### Required Resources
- [ ] List specific AWS resources needed

### Environment Variables
- [ ] List secrets to store in Secrets Manager

### Scaling Considerations
- [ ] Define auto-scaling rules
- [ ] Set CloudWatch alarms
