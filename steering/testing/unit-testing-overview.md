---
inclusion: manual
---

# Unit Testing Overview

## Overview

Unit testing strategy for Vue/Vuetify frontend (Vitest) and Laravel backend (PHPUnit).

## Frontend Testing (Vitest)

### Directory Structure

```
frontend/
├── src/
│   ├── components/
│   │   └── __tests__/
│   │       └── ComponentName.spec.ts
│   ├── stores/
│   │   └── __tests__/
│   │       └── storeName.spec.ts
│   ├── composables/
│   │   └── __tests__/
│   │       └── useComposable.spec.ts
│   └── utils/
│       └── __tests__/
│           └── utilName.spec.ts
├── vitest.config.ts
└── package.json
```

### Vitest Configuration

```typescript
// vitest.config.ts
import { defineConfig } from 'vitest/config'
import vue from '@vitejs/plugin-vue'
import vuetify from 'vite-plugin-vuetify'

export default defineConfig({
  plugins: [vue(), vuetify()],
  test: {
    globals: true,
    environment: 'jsdom',
    setupFiles: ['./tests/setup.ts'],
    coverage: {
      provider: 'v8',
      reporter: ['text', 'html'],
    },
  },
})
```

### Component Testing

```typescript
// components/__tests__/UserCard.spec.ts
import { describe, it, expect } from 'vitest'
import { mount } from '@vue/test-utils'
import { createVuetify } from 'vuetify'
import UserCard from '../UserCard.vue'

const vuetify = createVuetify()

describe('UserCard', () => {
  const mountComponent = (props = {}) => {
    return mount(UserCard, {
      props: {
        user: { id: 1, name: 'John Doe', email: 'john@example.com' },
        ...props,
      },
      global: {
        plugins: [vuetify],
      },
    })
  }

  it('renders user name', () => {
    const wrapper = mountComponent()
    expect(wrapper.text()).toContain('John Doe')
  })

  it('emits edit event when edit button clicked', async () => {
    const wrapper = mountComponent()
    await wrapper.find('[data-testid="edit-btn"]').trigger('click')
    expect(wrapper.emitted('edit')).toBeTruthy()
  })
})
```

### Pinia Store Testing

```typescript
// stores/__tests__/auth.spec.ts
import { describe, it, expect, beforeEach, vi } from 'vitest'
import { setActivePinia, createPinia } from 'pinia'
import { useAuthStore } from '../auth'

describe('Auth Store', () => {
  beforeEach(() => {
    setActivePinia(createPinia())
  })

  it('starts with unauthenticated state', () => {
    const store = useAuthStore()
    expect(store.isAuthenticated).toBe(false)
  })

  it('sets user on successful login', async () => {
    const store = useAuthStore()
    vi.spyOn(api, 'login').mockResolvedValue({ id: 1, name: 'John' })
    
    await store.login({ email: 'john@test.com', password: 'pass' })
    
    expect(store.isAuthenticated).toBe(true)
    expect(store.user?.name).toBe('John')
  })
})
```

## Backend Testing (PHPUnit)

### Directory Structure

```
backend/
├── tests/
│   ├── Feature/
│   │   ├── Auth/
│   │   │   ├── LoginTest.php
│   │   │   └── RegisterTest.php
│   │   └── Api/
│   │       └── ResourceTest.php
│   ├── Unit/
│   │   ├── Models/
│   │   │   └── UserTest.php
│   │   └── Services/
│   │       └── ServiceTest.php
│   └── TestCase.php
└── phpunit.xml
```

### Feature Test Example

```php
// tests/Feature/Api/TaskTest.php
namespace Tests\Feature\Api;

use App\Models\Task;
use App\Models\User;
use Illuminate\Foundation\Testing\RefreshDatabase;
use Tests\TestCase;

class TaskTest extends TestCase
{
    use RefreshDatabase;

    public function test_user_can_list_their_tasks(): void
    {
        $user = User::factory()->create();
        Task::factory()->count(3)->for($user)->create();

        $response = $this->actingAs($user)
            ->getJson('/api/v1/tasks');

        $response->assertOk()
            ->assertJsonCount(3, 'data');
    }

    public function test_user_can_create_task(): void
    {
        $user = User::factory()->create();

        $response = $this->actingAs($user)
            ->postJson('/api/v1/tasks', [
                'title' => 'New Task',
                'description' => 'Task description',
            ]);

        $response->assertCreated()
            ->assertJsonPath('data.title', 'New Task');
        
        $this->assertDatabaseHas('tasks', ['title' => 'New Task']);
    }

    public function test_user_cannot_access_others_tasks(): void
    {
        $user = User::factory()->create();
        $otherUser = User::factory()->create();
        $task = Task::factory()->for($otherUser)->create();

        $response = $this->actingAs($user)
            ->getJson("/api/v1/tasks/{$task->id}");

        $response->assertForbidden();
    }
}
```

### Unit Test Example

```php
// tests/Unit/Models/TaskTest.php
namespace Tests\Unit\Models;

use App\Models\Task;
use App\Models\User;
use Illuminate\Foundation\Testing\RefreshDatabase;
use Tests\TestCase;

class TaskTest extends TestCase
{
    use RefreshDatabase;

    public function test_task_belongs_to_user(): void
    {
        $user = User::factory()->create();
        $task = Task::factory()->for($user)->create();

        $this->assertInstanceOf(User::class, $task->user);
        $this->assertEquals($user->id, $task->user->id);
    }

    public function test_task_can_be_marked_complete(): void
    {
        $task = Task::factory()->create(['completed' => false]);
        
        $task->markComplete();
        
        $this->assertTrue($task->completed);
    }
}
```

### Running Tests

```bash
# Frontend (Vitest)
npm run test           # Run once
npm run test:watch     # Watch mode
npm run test:coverage  # With coverage

# Backend (PHPUnit)
php artisan test                    # Run all tests
php artisan test --filter=TaskTest  # Run specific test
php artisan test --coverage         # With coverage
```

## Project-Specific Unit Tests

<!-- AGENT: Replace this section with actual test plan -->

### Frontend Tests
- [ ] Component tests to write
- [ ] Store tests to write
- [ ] Composable tests to write

### Backend Tests
- [ ] Feature tests to write
- [ ] Unit tests to write
- [ ] Model tests to write

## Coverage Goals
- [ ] Minimum 80% coverage for critical paths
- [ ] 100% coverage for business logic
- [ ] All API endpoints tested
