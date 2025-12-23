---
inclusion: manual
---

# Color Schemes

## Overview

Vuetify 3 theme configuration with custom color palettes.

## Vuetify Theme Setup

```typescript
// src/plugins/vuetify.ts
import { createVuetify, type ThemeDefinition } from 'vuetify'

const lightTheme: ThemeDefinition = {
  dark: false,
  colors: {
    background: '#FFFFFF',
    surface: '#FFFFFF',
    'surface-bright': '#FFFFFF',
    'surface-light': '#EEEEEE',
    'surface-variant': '#424242',
    'on-surface-variant': '#EEEEEE',
    primary: '#1867C0',
    'primary-darken-1': '#1F5592',
    secondary: '#48A9A6',
    'secondary-darken-1': '#018786',
    error: '#B00020',
    info: '#2196F3',
    success: '#4CAF50',
    warning: '#FB8C00',
  },
  variables: {
    'border-color': '#000000',
    'border-opacity': 0.12,
    'high-emphasis-opacity': 0.87,
    'medium-emphasis-opacity': 0.60,
    'disabled-opacity': 0.38,
    'idle-opacity': 0.04,
    'hover-opacity': 0.04,
    'focus-opacity': 0.12,
    'selected-opacity': 0.08,
    'activated-opacity': 0.12,
    'pressed-opacity': 0.12,
    'dragged-opacity': 0.08,
    'theme-kbd': '#212529',
    'theme-on-kbd': '#FFFFFF',
    'theme-code': '#F5F5F5',
    'theme-on-code': '#000000',
  },
}

const darkTheme: ThemeDefinition = {
  dark: true,
  colors: {
    background: '#121212',
    surface: '#212121',
    'surface-bright': '#ccbfd6',
    'surface-light': '#424242',
    'surface-variant': '#a3a3a3',
    'on-surface-variant': '#424242',
    primary: '#2196F3',
    'primary-darken-1': '#1976D2',
    secondary: '#54B6B2',
    'secondary-darken-1': '#48A9A6',
    error: '#CF6679',
    info: '#2196F3',
    success: '#4CAF50',
    warning: '#FB8C00',
  },
}

export default createVuetify({
  theme: {
    defaultTheme: 'light',
    themes: {
      light: lightTheme,
      dark: darkTheme,
    },
  },
})
```

## Color Usage in Components

```vue
<template>
  <!-- Using theme colors -->
  <v-btn color="primary">Primary Button</v-btn>
  <v-btn color="secondary">Secondary Button</v-btn>
  
  <!-- Background colors -->
  <v-card color="surface">Card with surface color</v-card>
  
  <!-- Text colors -->
  <span class="text-primary">Primary text</span>
  <span class="text-error">Error text</span>
  
  <!-- Custom opacity -->
  <v-sheet color="primary" class="bg-opacity-50">
    50% opacity primary
  </v-sheet>
</template>
```

## Color Palette Generator

Use these tools to generate harmonious color palettes:
- [Vuetify Theme Generator](https://vuetifyjs.com/en/features/theme/)
- [Coolors](https://coolors.co/)
- [Adobe Color](https://color.adobe.com/)

## Accessibility Guidelines

- Ensure contrast ratio of at least 4.5:1 for normal text
- Use 3:1 minimum for large text and UI components
- Test with color blindness simulators

## Project-Specific Colors

<!-- AGENT: Replace this section with actual color scheme -->

### Brand Colors
- Primary: `#______`
- Secondary: `#______`
- Accent: `#______`

### Semantic Colors
- Success: `#4CAF50`
- Warning: `#FB8C00`
- Error: `#B00020`
- Info: `#2196F3`

### Theme Configuration
- [ ] Define light theme colors
- [ ] Define dark theme colors
- [ ] Set default theme
