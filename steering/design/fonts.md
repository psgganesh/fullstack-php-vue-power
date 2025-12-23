---
inclusion: manual
---

# Typography & Fonts

## Overview

Font configuration for Vuetify 3 applications.

## Google Fonts Setup

```html
<!-- index.html -->
<head>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">
</head>
```

## Vuetify Typography Configuration

```typescript
// src/plugins/vuetify.ts
import { createVuetify } from 'vuetify'

export default createVuetify({
  defaults: {
    global: {
      // Default font settings
    },
  },
  theme: {
    themes: {
      light: {
        variables: {
          // Typography variables
          'theme-font-family': '"Roboto", sans-serif',
        },
      },
    },
  },
})
```

## Custom Font Setup

```scss
// src/styles/fonts.scss
@font-face {
  font-family: 'CustomFont';
  src: url('@/assets/fonts/CustomFont-Regular.woff2') format('woff2'),
       url('@/assets/fonts/CustomFont-Regular.woff') format('woff');
  font-weight: 400;
  font-style: normal;
  font-display: swap;
}

@font-face {
  font-family: 'CustomFont';
  src: url('@/assets/fonts/CustomFont-Bold.woff2') format('woff2'),
       url('@/assets/fonts/CustomFont-Bold.woff') format('woff');
  font-weight: 700;
  font-style: normal;
  font-display: swap;
}
```

## Vuetify Typography Classes

| Class | Size | Weight | Usage |
|-------|------|--------|-------|
| `.text-h1` | 96px | Light | Hero headlines |
| `.text-h2` | 60px | Light | Page titles |
| `.text-h3` | 48px | Regular | Section headers |
| `.text-h4` | 34px | Regular | Card titles |
| `.text-h5` | 24px | Regular | Subheadings |
| `.text-h6` | 20px | Medium | Small headers |
| `.text-subtitle-1` | 16px | Regular | Subtitles |
| `.text-subtitle-2` | 14px | Medium | Small subtitles |
| `.text-body-1` | 16px | Regular | Body text |
| `.text-body-2` | 14px | Regular | Secondary text |
| `.text-button` | 14px | Medium | Buttons |
| `.text-caption` | 12px | Regular | Captions |
| `.text-overline` | 10px | Regular | Overlines |

## Usage in Components

```vue
<template>
  <div>
    <h1 class="text-h2 font-weight-bold mb-4">Page Title</h1>
    <p class="text-body-1 text-medium-emphasis">
      Body text with medium emphasis
    </p>
    <span class="text-caption text-disabled">
      Caption text
    </span>
  </div>
</template>
```

## Font Weight Classes

- `.font-weight-thin` (100)
- `.font-weight-light` (300)
- `.font-weight-regular` (400)
- `.font-weight-medium` (500)
- `.font-weight-bold` (700)
- `.font-weight-black` (900)

## Project-Specific Typography

<!-- AGENT: Replace this section with actual font configuration -->

### Primary Font
- Font Family: `______`
- Weights: 400, 500, 700
- Source: Google Fonts / Custom

### Secondary Font (Optional)
- Font Family: `______`
- Usage: Headings / Code / Accent

### Typography Scale
- [ ] Define heading styles
- [ ] Define body text styles
- [ ] Configure font loading
