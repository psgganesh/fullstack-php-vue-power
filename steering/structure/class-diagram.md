---
inclusion: manual
---

# Class Diagram

## Overview

Mermaid class diagrams for Laravel models and their relationships.

## Diagram Syntax Reference

```mermaid
classDiagram
    class ClassName {
        +int id
        +string name
        +DateTime created_at
        +methodName() ReturnType
    }
    
    %% Relationships
    ClassA "1" --> "*" ClassB : has many
    ClassA "1" --> "1" ClassB : has one
    ClassA "*" --> "*" ClassB : many to many
    ClassA --> ClassB : belongs to
```

## Relationship Notation

| Notation | Meaning |
|----------|---------|
| `"1" --> "*"` | One-to-Many |
| `"1" --> "1"` | One-to-One |
| `"*" --> "*"` | Many-to-Many |
| `-->` | Association/Belongs To |
| `<\|--` | Inheritance |

## Base User Model

```mermaid
classDiagram
    class User {
        +int id
        +string name
        +string email
        +DateTime email_verified_at
        +string password
        +string remember_token
        +DateTime created_at
        +DateTime updated_at
    }
```

## Project-Specific Class Diagram

<!-- AGENT: Replace this section with actual class diagram -->

```mermaid
classDiagram
    class User {
        +int id
        +string name
        +string email
        +DateTime created_at
    }
    
    %% Add project-specific models and relationships below
    
    %% Example:
    %% class Task {
    %%     +int id
    %%     +string title
    %%     +text description
    %%     +boolean completed
    %%     +int user_id
    %% }
    %% User "1" --> "*" Task : has many
```

## Model List
- [ ] List all models with their attributes
- [ ] Define relationships between models
