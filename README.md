# Fullstack PHP Vue Power

A Kiro Power for building Single-Page Applications with **Vuetify 3** frontend and **Laravel API** backend.

## What It Does

When you describe an app idea, this power generates comprehensive architecture documentation:

- **Database schema** with migrations
- **ERD & class diagrams** (Mermaid)
- **Frontend structure** (Vue 3 + Vuetify + Pinia)
- **Backend structure** (Laravel API)
- **User flows** & state diagrams
- **Testing plans** (BDD + Unit)
- **Deployment guides** (Local + AWS)

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | Vue 3, Vuetify 3, Pinia, Vue Router, Vite |
| Backend | Laravel 10+, SQLite/PostgreSQL, Sanctum |
| Testing | Vitest, CucumberJS, PHPUnit |

## MCP Servers Included

- `laravel-docs` - Laravel framework documentation
- `vuetify-docs` - Vuetify component documentation  
- `vuetify-mcp` - Vuetify API helpers

## Installation

1. Open Kiro Powers panel
2. Click "Add Custom Power"
3. Select "Local Directory"
4. Enter path: `/Users/gnshnk/Documents/Play/spa-site-power/powers/fullstack-php-vue-power`
5. Click "Add"

## Usage

Simply describe your app:

```
Build a timesheet management app with user authentication,
project tracking, and manager approvals
```

The power will generate all steering documentation in `.kiro/steering/`.

## Generated Steering Structure

```
.kiro/steering/
├── deployment/
│   ├── aws.md
│   └── local.md
├── design/
│   ├── color-schemes.md
│   └── fonts.md
├── structure/
│   ├── backend.md
│   ├── class-diagram.md
│   ├── database-schema.md
│   ├── entity-relationship-diagram.md
│   └── frontend.md
├── testing/
│   ├── bdd-testing-overview.md
│   └── unit-testing-overview.md
└── userflow/
    ├── pages-flow.md
    └── state-flow.md
```

## License

MIT
