---
name: "fullstack-php-vue-power"
displayName: "Fullstack PHP Vue Power"
description: "Build Single-Page Applications with Vuetify 3 frontend and Laravel API backend. Generates project structure, design docs, database schemas, and deployment guides."
keywords: ["vuetify", "laravel", "spa", "vue", "pinia", "sqlite", "api", "php", "fullstack"]
author: "Shankar <psgganesh@gmail.com>"
---

# Fullstack PHP Vue Power

## Overview

This power helps you build complete Single-Page Applications using:
- **Frontend**: Vue 3 + Vuetify 3 + Pinia (state management)
- **Backend**: Laravel API with SQLite database
- **Documentation**: Auto-generated steering docs for structure, design, testing, and deployment

When you start a new project, this power creates comprehensive documentation in `.kiro/steering/` covering your entire application architecture.

## Available Steering Files

This power generates the following steering documentation structure:

### Structure
- **backend.md** - Laravel API structure, routes, controllers, models
- **frontend.md** - Vue/Vuetify component hierarchy and organization
- **database-schema.md** - SQLite schema with migrations
- **class-diagram.md** - Mermaid class diagrams for models
- **entity-relationship-diagram.md** - ERD for database relationships

### Design
- **color-schemes.md** - Vuetify theme configuration and color palette
- **fonts.md** - Typography setup and font configuration

### User Flow
- **pages-flow.md** - Sitemap-style mermaid diagram of all pages
- **state-flow.md** - Pinia state management flow diagrams

### Testing
- **bdd-testing-overview.md** - CucumberJS test scenarios
- **unit-testing-overview.md** - JUnit/Vitest test coverage plan

### Deployment
- **local.md** - Local development setup instructions
- **aws.md** - AWS resource provisioning guide

## MCP Servers

This power includes three MCP servers:

| Server | Purpose |
|--------|---------|
| `laravel-docs` | Laravel framework documentation |
| `vuetify-docs` | Vuetify component documentation |
| `vuetify-mcp` | Vuetify API and component helpers |

## Onboarding

### Prerequisites
- Node.js 18+
- PHP 8.1+ with Composer
- SQLite

### Quick Start

1. **Create a new project** - Tell me what app you want to build
2. **I'll generate steering docs** - Complete architecture documentation
3. **Build incrementally** - Follow the generated structure

## Common Workflows

### Workflow 1: Start New Project

When you describe your app idea, I will:

1. Create the steering directory structure in `.kiro/steering/`
2. Generate all documentation files based on your requirements
3. Scaffold the initial project structure

**Example prompt:**
```
Build a task management app with user authentication, 
project boards, and task assignments
```

### Workflow 2: Add New Feature

For adding features to an existing project:

1. Update relevant steering docs (structure, database schema)
2. Generate Vue components with Vuetify
3. Create Laravel API endpoints
4. Update Pinia stores

### Workflow 3: Design System Setup

Configure your app's visual identity:

1. Define color scheme in `color-schemes.md`
2. Set up typography in `fonts.md`
3. Apply Vuetify theme configuration

## Best Practices

- Use Pinia for all state management (not Vuex)
- Follow Laravel API resource conventions
- Use Vuetify's built-in components before custom ones
- Keep SQLite for development, consider PostgreSQL for production
- Generate ERD and class diagrams before coding
- Write BDD scenarios before implementation

## Tech Stack Reference

### Frontend
```
Vue 3 (Composition API)
Vuetify 3
Pinia
Vue Router
Vite
```

### Backend
```
Laravel 10+
SQLite (dev) / PostgreSQL (prod)
Laravel Sanctum (auth)
```

### Testing
```
Vitest (frontend unit tests)
CucumberJS (BDD)
PHPUnit (backend)
```

---

**MCP Servers:** laravel-docs, vuetify-docs, vuetify-mcp
