---
inclusion: manual
---

# Local Development Setup

## Overview

Instructions for setting up and running the Vuetify + Laravel SPA locally.

## Prerequisites

- **Node.js** 18+ (for frontend)
- **PHP** 8.1+ (for backend)
- **Composer** (PHP package manager)
- **SQLite** (database)

## Quick Start

### 1. Clone Repository

```bash
git clone <repository-url>
cd <project-name>
```

### 2. Backend Setup (Laravel)

```bash
cd backend

# Install PHP dependencies
composer install

# Copy environment file
cp .env.example .env

# Generate application key
php artisan key:generate

# Create SQLite database
touch database/database.sqlite

# Run migrations
php artisan migrate

# Seed database (optional)
php artisan db:seed

# Start Laravel development server
php artisan serve
```

Backend will be available at: `http://localhost:8000`

### 3. Frontend Setup (Vue/Vuetify)

```bash
cd frontend

# Install Node dependencies
npm install

# Copy environment file
cp .env.example .env.local

# Update API URL in .env.local
# VITE_API_URL=http://localhost:8000/api

# Start Vite development server
npm run dev
```

Frontend will be available at: `http://localhost:5173`

## Environment Configuration

### Backend (.env)

```env
APP_NAME="App Name"
APP_ENV=local
APP_KEY=base64:generated-key
APP_DEBUG=true
APP_URL=http://localhost:8000

DB_CONNECTION=sqlite
DB_DATABASE=/absolute/path/to/database/database.sqlite

SANCTUM_STATEFUL_DOMAINS=localhost:5173
SESSION_DOMAIN=localhost
```

### Frontend (.env.local)

```env
VITE_API_URL=http://localhost:8000/api
VITE_APP_NAME="App Name"
```

## Common Commands

### Backend

```bash
# Run migrations
php artisan migrate

# Rollback migrations
php artisan migrate:rollback

# Fresh migration with seeding
php artisan migrate:fresh --seed

# Create new migration
php artisan make:migration create_table_name_table

# Create model with migration, factory, seeder
php artisan make:model ModelName -mfs

# Create controller
php artisan make:controller Api/ControllerName --api

# Clear caches
php artisan cache:clear
php artisan config:clear
php artisan route:clear

# Run tests
php artisan test
```

### Frontend

```bash
# Development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview

# Run tests
npm run test

# Lint code
npm run lint

# Type check
npm run type-check
```

## Database Management

### SQLite Browser

Use [DB Browser for SQLite](https://sqlitebrowser.org/) to view/edit the database directly.

### Tinker (Laravel REPL)

```bash
php artisan tinker

# Example queries
>>> User::all()
>>> User::factory()->create()
>>> Task::where('user_id', 1)->get()
```

## Troubleshooting

### CORS Issues

Ensure `config/cors.php` allows your frontend origin:

```php
'allowed_origins' => ['http://localhost:5173'],
```

### SQLite Permission Errors

```bash
chmod 664 database/database.sqlite
chmod 775 database/
```

### Vite HMR Not Working

Check `vite.config.ts` server configuration:

```typescript
server: {
  host: true,
  port: 5173,
}
```

### API 401 Unauthorized

1. Check Sanctum configuration
2. Verify CSRF token is being sent
3. Check `SANCTUM_STATEFUL_DOMAINS` in `.env`

## Project-Specific Setup

<!-- AGENT: Replace this section with project-specific instructions -->

### Additional Setup Steps
- [ ] List any project-specific setup

### Environment Variables
- [ ] List required environment variables

### Sample Data
- [ ] Describe seeder data available
