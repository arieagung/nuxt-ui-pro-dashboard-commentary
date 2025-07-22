# App.vue - Root Application Component

This is the main entry point for the Nuxt application, serving as the root component that wraps all other components and pages. It handles global configurations, SEO metadata, color mode management, and provides the foundational structure for the entire application.

## Key Responsibilities

1. **Color Mode Management**: Implements dynamic theme switching between light and dark modes
2. **SEO Configuration**: Sets up comprehensive meta tags, Open Graph, and Twitter card metadata
3. **Global Head Configuration**: Manages viewport, charset, favicon, and theme color
4. **Application Structure**: Provides the foundational layout structure using Nuxt UI components

## Features

### Dynamic Color Mode
- Automatically detects and responds to color mode changes (light/dark)
- Updates theme-color meta tag dynamically based on current mode
- Dark mode: `#1b1718` (deep dark gray)
- Light mode: `white`

### SEO Optimization
- **Title**: "Nuxt Dashboard Template"
- **Description**: Professional dashboard template description
- **Open Graph**: Complete OG tags for social media sharing
- **Twitter Cards**: Summary large image card configuration
- **Dynamic Images**: Uses Nuxt Hub assets for social media previews

### Global Configuration
- **Language**: Set to English (`lang="en"`)
- **Viewport**: Responsive viewport configuration
- **Favicon**: Standard favicon.ico integration
- **Charset**: UTF-8 encoding

## Component Structure

The template uses Nuxt UI Pro components:
- `<UApp>`: Root application wrapper
- `<NuxtLoadingIndicator>`: Loading progress indicator
- `<NuxtLayout>`: Layout system integration
- `<NuxtPage>`: Page routing component

## Usage

This component is automatically used by Nuxt as the root component. It:
1. Wraps all pages and layouts
2. Provides global styling and theme management
3. Handles SEO meta configuration
4. Integrates with Nuxt UI Pro's theming system

## Dependencies

- `@nuxt/ui`: For UApp component and color mode utilities
- `nuxt/seo`: For useSeoMeta functionality
- `nuxt`: For useHead, NuxtLayout, NuxtPage components

---

```vue
<script setup lang="ts">
// Color mode management - provides reactive theme switching
const colorMode = useColorMode()

// Dynamic theme color based on current color mode
const color = computed(() => colorMode.value === 'dark' ? '#1b1718' : 'white')

// Global HTML head configuration
useHead({
  meta: [
    { charset: 'utf-8' },
    { name: 'viewport', content: 'width=device-width, initial-scale=1' },
    { key: 'theme-color', name: 'theme-color', content: color }
  ],
  link: [
    { rel: 'icon', href: '/favicon.ico' }
  ],
  htmlAttrs: {
    lang: 'en'
  }
})

// SEO metadata configuration
const title = 'Nuxt Dashboard Template'
const description = 'A professional dashboard template built with Nuxt UI Pro, featuring multiple pages, data visualization, and comprehensive management capabilities for creating powerful admin interfaces.'

useSeoMeta({
  title,
  description,
  ogTitle: title,
  ogDescription: description,
  ogImage: 'https://assets.hub.nuxt.com/eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1cmwiOiJodHRwczovL2Rhc2hib2FyZC10ZW1wbGF0ZS5udXh0LmRldiIsImlhdCI6MTczOTQ2MzU2N30._VElt4uvLjvAMdnTLytCInOajMElzWDKbmvOaMZhZUI.jpg?theme=light',
  twitterImage: 'https://assets.hub.nuxt.com/eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1cmwiOiJodHRwczovL2Rhc2hib2FyZC10ZW1wbGF0ZS5udXh0LmRldiIsImlhdCI6MTczOTQ2MzU2N30._VElt4uvLjvAMdnTLytCInOajMElzWDKbmvOaMZhZUI.jpg?theme=light',
  twitterCard: 'summary_large_image'
})
</script>

<template>
  <!-- Root application wrapper with Nuxt UI Pro theming -->
  <UApp>
    <!-- Global loading indicator for page transitions -->
    <NuxtLoadingIndicator />

    <!-- Layout system integration -->
    <NuxtLayout>
      <!-- Dynamic page component rendering -->
      <NuxtPage />
    </NuxtLayout>
  </UApp>
</template>
```
