---
inclusion: manual
---

# Backend Structure

## Overview

Laravel API backend with SQLite database for the SPA.

## Directory Structure

```
backend/
├── app/
│   ├── Http/
│   │   ├── Controllers/
│   │   │   └── Api/           # API controllers
│   │   ├── Middleware/        # Custom middleware
│   │   ├── Requests/          # Form request validation
│   │   └── Resources/         # API resources (transformers)
│   ├── Models/                # Eloquent models
│   ├── Services/              # Business logic services
│   ├── Repositories/          # Data access layer (optional)
│   └── Policies/              # Authorization policies
├── config/                    # Configuration files
├── database/
│   ├── migrations/            # Database migrations
│   ├── seeders/               # Database seeders
│   └── factories/             # Model factories
├── routes/
│   └── api.php                # API routes
├── storage/
│   └── database.sqlite        # SQLite database file
├── tests/
│   ├── Feature/               # Feature tests
│   └── Unit/                  # Unit tests
├── .env                       # Environment configuration
├── composer.json              # PHP dependencies
└── artisan                    # Laravel CLI
```

## API Route Conventions

```php
// routes/api.php
use App\Http\Controllers\Api\{ResourceController};

Route::prefix('v1')->group(function () {
    // Public routes
    Route::post('/login', [AuthController::class, 'login']);
    Route::post('/register', [AuthController::class, 'register']);

    // Protected routes
    Route::middleware('auth:sanctum')->group(function () {
        Route::get('/user', [AuthController::class, 'user']);
        Route::post('/logout', [AuthController::class, 'logout']);
        
        // Resource routes
        Route::apiResource('resources', ResourceController::class);
    });
});
```

## Controller Pattern

```php
// app/Http/Controllers/Api/ResourceController.php
namespace App\Http\Controllers\Api;

use App\Http\Controllers\Controller;
use App\Http\Requests\StoreResourceRequest;
use App\Http\Resources\ResourceResource;
use App\Models\Resource;

class ResourceController extends Controller
{
    public function index()
    {
        return ResourceResource::collection(
            Resource::paginate(15)
        );
    }

    public function store(StoreResourceRequest $request)
    {
        $resource = Resource::create($request->validated());
        return new ResourceResource($resource);
    }

    public function show(Resource $resource)
    {
        return new ResourceResource($resource);
    }

    public function update(StoreResourceRequest $request, Resource $resource)
    {
        $resource->update($request->validated());
        return new ResourceResource($resource);
    }

    public function destroy(Resource $resource)
    {
        $resource->delete();
        return response()->noContent();
    }
}
```

## Model Pattern

```php
// app/Models/Resource.php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\BelongsTo;

class Resource extends Model
{
    use HasFactory;

    protected $fillable = [
        'name',
        'description',
        'user_id',
    ];

    protected $casts = [
        'created_at' => 'datetime',
        'updated_at' => 'datetime',
    ];

    public function user(): BelongsTo
    {
        return $this->belongsTo(User::class);
    }
}
```

## API Resource Pattern

```php
// app/Http/Resources/ResourceResource.php
namespace App\Http\Resources;

use Illuminate\Http\Resources\Json\JsonResource;

class ResourceResource extends JsonResource
{
    public function toArray($request): array
    {
        return [
            'id' => $this->id,
            'name' => $this->name,
            'description' => $this->description,
            'user' => new UserResource($this->whenLoaded('user')),
            'created_at' => $this->created_at->toISOString(),
            'updated_at' => $this->updated_at->toISOString(),
        ];
    }
}
```

## Project-Specific Structure

<!-- AGENT: Replace this section with actual project structure -->

### Models
- [ ] List all models

### Controllers
- [ ] List API controllers

### Routes
- [ ] List API endpoints

### Middleware
- [ ] List custom middleware
