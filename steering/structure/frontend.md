---
inclusion: manual
---

# Frontend Structure

## Overview

Vue 3 + Vuetify 3 Single-Page Application with Pinia state management.

## Directory Structure

```
frontend/
├── src/
│   ├── assets/              # Static assets (images, fonts)
│   ├── components/          # Reusable Vue components
│   │   ├── common/          # Shared components (buttons, inputs)
│   │   ├── layout/          # Layout components (header, footer, sidebar)
│   │   └── features/        # Feature-specific components
│   ├── composables/         # Vue composables (reusable logic)
│   ├── layouts/             # Page layouts
│   ├── pages/               # Route pages (views)
│   ├── plugins/             # Vuetify, router, pinia setup
│   │   ├── vuetify.ts       # Vuetify configuration
│   │   ├── router.ts        # Vue Router setup
│   │   └── pinia.ts         # Pinia store setup
│   ├── stores/              # Pinia stores
│   │   ├── auth.ts          # Authentication state
│   │   ├── app.ts           # App-wide state
│   │   └── [feature].ts     # Feature-specific stores
│   ├── services/            # API service layer
│   │   ├── api.ts           # Axios instance configuration
│   │   └── [resource].ts    # Resource-specific API calls
│   ├── types/               # TypeScript interfaces
│   ├── utils/               # Utility functions
│   ├── App.vue              # Root component
│   └── main.ts              # Application entry point
├── public/                  # Public static files
├── index.html               # HTML entry point
├── vite.config.ts           # Vite configuration
├── tsconfig.json            # TypeScript configuration
└── package.json             # Dependencies
```

## Component Naming Conventions

- **PascalCase** for component files: `UserProfile.vue`
- **Prefix** feature components: `TaskCard.vue`, `TaskList.vue`
- **Base** prefix for base components: `BaseButton.vue`, `BaseInput.vue`

## Pinia Store Pattern

```typescript
// stores/[feature].ts
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'

export const useFeatureStore = defineStore('feature', () => {
  // State
  const items = ref<Item[]>([])
  const loading = ref(false)
  const error = ref<string | null>(null)

  // Getters
  const itemCount = computed(() => items.value.length)

  // Actions
  async function fetchItems() {
    loading.value = true
    try {
      items.value = await api.getItems()
    } catch (e) {
      error.value = e.message
    } finally {
      loading.value = false
    }
  }

  return { items, loading, error, itemCount, fetchItems }
})
```

## API Service Pattern

```typescript
// services/api.ts
import axios from 'axios'

const api = axios.create({
  baseURL: import.meta.env.VITE_API_URL,
  headers: {
    'Content-Type': 'application/json',
  },
})

// Request interceptor for auth token
api.interceptors.request.use((config) => {
  const token = localStorage.getItem('token')
  if (token) {
    config.headers.Authorization = `Bearer ${token}`
  }
  return config
})

export default api
```

## Project-Specific Structure

<!-- AGENT: Replace this section with actual project structure -->

### Pages
- [ ] List all pages/routes

### Components
- [ ] List feature components

### Stores
- [ ] List Pinia stores needed

### Services
- [ ] List API services needed
