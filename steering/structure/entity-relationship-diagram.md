---
inclusion: manual
---

# Entity Relationship Diagram

## Overview

Mermaid ERD showing database table relationships.

## ERD Syntax Reference

```mermaid
erDiagram
    TABLE_A ||--o{ TABLE_B : "relationship"
    TABLE_A {
        int id PK
        string name
        int foreign_id FK
    }
```

## Relationship Notation

| Symbol | Meaning |
|--------|---------|
| `\|\|` | Exactly one |
| `o\|` | Zero or one |
| `}o` | Zero or many |
| `}\|` | One or many |
| `--` | Identifying relationship |
| `..` | Non-identifying relationship |

## Common Patterns

```mermaid
erDiagram
    %% One-to-Many
    USERS ||--o{ POSTS : "writes"
    
    %% Many-to-Many (with pivot)
    USERS ||--o{ USER_ROLES : "has"
    ROLES ||--o{ USER_ROLES : "assigned to"
    
    %% One-to-One
    USERS ||--|| PROFILES : "has"
    
    %% Self-referencing
    CATEGORIES ||--o{ CATEGORIES : "parent of"
```

## Base Schema

```mermaid
erDiagram
    USERS {
        int id PK
        string name
        string email UK
        string password
        datetime created_at
        datetime updated_at
    }
```

## Project-Specific ERD

<!-- AGENT: Replace this section with actual ERD -->

```mermaid
erDiagram
    USERS {
        int id PK
        string name
        string email UK
        string password
        datetime created_at
    }
    
    %% Add project-specific tables and relationships below
    
    %% Example:
    %% USERS ||--o{ TASKS : "owns"
    %% TASKS {
    %%     int id PK
    %%     string title
    %%     text description
    %%     boolean completed
    %%     int user_id FK
    %%     datetime created_at
    %% }
```

## Tables List
- [ ] List all tables with columns
- [ ] Define all relationships
