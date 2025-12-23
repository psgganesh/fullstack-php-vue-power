---
inclusion: manual
---

# State Flow

## Overview

Mermaid state diagrams for Pinia store state management.

## State Diagram Syntax Reference

```mermaid
stateDiagram-v2
    [*] --> Initial
    Initial --> Loading : fetch()
    Loading --> Success : data received
    Loading --> Error : request failed
    Success --> Loading : refresh()
    Error --> Loading : retry()
```

## Common State Patterns

### Authentication State

```mermaid
stateDiagram-v2
    [*] --> Unauthenticated
    
    Unauthenticated --> Authenticating : login()
    Authenticating --> Authenticated : success
    Authenticating --> Unauthenticated : failure
    
    Authenticated --> Unauthenticated : logout()
    Authenticated --> Refreshing : tokenExpiring
    Refreshing --> Authenticated : refreshSuccess
    Refreshing --> Unauthenticated : refreshFailed
```

### Data Fetching State

```mermaid
stateDiagram-v2
    [*] --> Idle
    
    Idle --> Loading : fetch()
    Loading --> Success : dataReceived
    Loading --> Error : requestFailed
    
    Success --> Loading : refresh()
    Success --> Idle : clear()
    
    Error --> Loading : retry()
    Error --> Idle : dismiss()
```

### Form State

```mermaid
stateDiagram-v2
    [*] --> Pristine
    
    Pristine --> Dirty : userInput
    Dirty --> Validating : blur/submit
    Validating --> Valid : passValidation
    Validating --> Invalid : failValidation
    
    Valid --> Submitting : submit()
    Invalid --> Dirty : userInput
    
    Submitting --> Success : submitSuccess
    Submitting --> Invalid : submitError
    
    Success --> Pristine : reset()
```

## Pinia Store Implementation

```typescript
// stores/auth.ts
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'

type AuthState = 'unauthenticated' | 'authenticating' | 'authenticated' | 'refreshing'

export const useAuthStore = defineStore('auth', () => {
  const state = ref<AuthState>('unauthenticated')
  const user = ref<User | null>(null)
  const error = ref<string | null>(null)

  const isAuthenticated = computed(() => state.value === 'authenticated')
  const isLoading = computed(() => 
    state.value === 'authenticating' || state.value === 'refreshing'
  )

  async function login(credentials: Credentials) {
    state.value = 'authenticating'
    error.value = null
    try {
      user.value = await api.login(credentials)
      state.value = 'authenticated'
    } catch (e) {
      error.value = e.message
      state.value = 'unauthenticated'
    }
  }

  function logout() {
    user.value = null
    state.value = 'unauthenticated'
  }

  return { state, user, error, isAuthenticated, isLoading, login, logout }
})
```

## Project-Specific State Flows

<!-- AGENT: Replace this section with actual state flows -->

### Authentication Store

```mermaid
stateDiagram-v2
    [*] --> Unauthenticated
    %% Add project-specific auth states
```

### Feature Store(s)

```mermaid
stateDiagram-v2
    [*] --> Idle
    %% Add project-specific feature states
```

## Stores List
- [ ] List all Pinia stores
- [ ] Define state transitions for each
- [ ] Identify shared state between stores
