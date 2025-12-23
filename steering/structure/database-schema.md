---
inclusion: manual
---

# Database Schema

## Overview

SQLite database schema for the Laravel API backend.

## Migration Template

```php
// database/migrations/YYYY_MM_DD_HHMMSS_create_table_name_table.php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::create('table_name', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->text('description')->nullable();
            $table->foreignId('user_id')->constrained()->cascadeOnDelete();
            $table->timestamps();
            $table->softDeletes(); // Optional
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('table_name');
    }
};
```

## Common Column Types

| Type | SQLite | Usage |
|------|--------|-------|
| `id()` | INTEGER PRIMARY KEY | Auto-increment ID |
| `string('name')` | TEXT | Short text (255 chars) |
| `text('content')` | TEXT | Long text |
| `integer('count')` | INTEGER | Whole numbers |
| `boolean('active')` | INTEGER (0/1) | True/false |
| `decimal('price', 8, 2)` | REAL | Money/decimals |
| `date('due_date')` | TEXT | Date only |
| `datetime('scheduled_at')` | TEXT | Date and time |
| `timestamps()` | TEXT | created_at, updated_at |
| `softDeletes()` | TEXT | deleted_at |
| `json('metadata')` | TEXT | JSON data |

## Relationship Columns

```php
// One-to-Many (belongs to)
$table->foreignId('user_id')->constrained()->cascadeOnDelete();

// Many-to-Many (pivot table)
Schema::create('role_user', function (Blueprint $table) {
    $table->foreignId('role_id')->constrained()->cascadeOnDelete();
    $table->foreignId('user_id')->constrained()->cascadeOnDelete();
    $table->primary(['role_id', 'user_id']);
});

// Polymorphic
$table->morphs('taggable'); // Creates taggable_id and taggable_type
```

## Index Best Practices

```php
// Single column index
$table->index('email');

// Composite index
$table->index(['user_id', 'created_at']);

// Unique constraint
$table->unique('email');
$table->unique(['user_id', 'slug']); // Composite unique
```

## Project-Specific Schema

<!-- AGENT: Replace this section with actual database schema -->

### Tables

```sql
-- Example: Users table (Laravel default)
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    email TEXT NOT NULL UNIQUE,
    email_verified_at TEXT,
    password TEXT NOT NULL,
    remember_token TEXT,
    created_at TEXT,
    updated_at TEXT
);

-- Add project-specific tables below
```

### Migrations List
- [ ] List all migrations to create
