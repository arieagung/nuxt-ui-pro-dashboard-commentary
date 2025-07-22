# Settings Layout - `pages/settings.vue`

## Overview

The settings layout page provides a structured navigation framework for all settings-related pages in the application. It implements a hierarchical navigation system with horizontal menu integration and serves as the parent layout for nested settings routes.

## Route Information

- **Path**: `/settings`
- **File**: `pages/settings.vue`
- **Layout**: Default (no specific layout defined)
- **Page Name**: `settings`
- **Type**: Layout component with nested routing

## Key Features

### 1. Navigation Menu Structure
- **Hierarchical Organization**: Primary settings and external links sections
- **Active State Management**: Automatic highlighting of current page
- **Icon Integration**: Lucide icons for visual navigation cues
- **External Link Support**: Direct links to documentation and purchase

### 2. Nested Routing Container
- **Nuxt Page Slot**: Renders child settings pages
- **Centered Layout**: Optimal content width with responsive design
- **Consistent Spacing**: Uniform gap management between sections

### 3. Responsive Design
- **Mobile-First Approach**: Adaptive layout for all screen sizes
- **Flexible Navigation**: Menu adapts to different viewport widths
- **Optimized Spacing**: Progressive enhancement for larger screens

## Component Structure

```vue
<script setup lang="ts">
import type { NavigationMenuItem } from '@nuxt/ui'

// Navigation menu configuration
const links = [[{
  label: 'General',
  icon: 'i-lucide-user',
  to: '/settings',
  exact: true  // Exact match for root settings page
}, {
  label: 'Members',
  icon: 'i-lucide-users',
  to: '/settings/members'
}, {
  label: 'Notifications',
  icon: 'i-lucide-bell',
  to: '/settings/notifications'
}, {
  label: 'Security',
  icon: 'i-lucide-shield',
  to: '/settings/security'
}], [{
  label: 'Documentation',
  icon: 'i-lucide-book-open',
  to: 'https://ui.nuxt.com/getting-started/installation/pro/nuxt',
  target: '_blank'
}, {
  label: 'Buy now',
  icon: 'i-lucide-shopping-cart',
  to: 'https://ui.nuxt.com/pro/purchase',
  target: '_blank'
}]] satisfies NavigationMenuItem[][]
</script>
```

## Navigation Menu Configuration

### Primary Settings Section
```typescript
[{
  label: 'General',
  icon: 'i-lucide-user',
  to: '/settings',
  exact: true  // Important for root route matching
}, {
  label: 'Members',
  icon: 'i-lucide-users',
  to: '/settings/members'
}, {
  label: 'Notifications',
  icon: 'i-lucide-bell',
  to: '/settings/notifications'
}, {
  label: 'Security',
  icon: 'i-lucide-shield',
  to: '/settings/security'
}]
```

**Features:**
- **Exact Matching**: Root settings route uses `exact: true` for proper highlighting
- **Consistent Iconography**: Lucide icons for visual consistency
- **Hierarchical Routes**: Nested path structure for organization
- **Type Safety**: Full TypeScript support with NavigationMenuItem type

### External Links Section
```typescript
[{
  label: 'Documentation',
  icon: 'i-lucide-book-open',
  to: 'https://ui.nuxt.com/getting-started/installation/pro/nuxt',
  target: '_blank'
}, {
  label: 'Buy now',
  icon: 'i-lucide-shopping-cart',
  to: 'https://ui.nuxt.com/pro/purchase',
  target: '_blank'
}]
```

**Features:**
- **External Navigation**: Direct links to external resources
- **Security**: `target="_blank"` for external links
- **Resource Access**: Quick links to documentation and purchase

## Layout Structure

### Dashboard Panel Configuration
```vue
<UDashboardPanel id="settings" :ui="{ body: 'lg:py-12' }">
```

**Customizations:**
- **Unique ID**: "settings" for proper state management
- **Custom Body Styling**: Increased padding on large screens (`lg:py-12`)
- **Responsive Spacing**: Adaptive padding based on screen size

### Navigation Integration
```vue
<UDashboardToolbar>
  <UNavigationMenu :items="links" highlight class="-mx-1 flex-1" />
</UDashboardToolbar>
```

**Features:**
- **Highlight Active**: Automatic active state highlighting
- **Full Width**: `flex-1` makes menu span full toolbar width
- **Alignment**: `-mx-1` compensates for sidebar collapse button alignment
- **Visual Feedback**: Clear indication of current page

### Content Container
```vue
<template #body>
  <div class="flex flex-col gap-4 sm:gap-6 lg:gap-12 w-full lg:max-w-2xl mx-auto">
    <NuxtPage />
  </div>
</template>
```

**Layout Features:**
- **Progressive Enhancement**: Increasing gaps on larger screens
  - Mobile: `gap-4` (1rem)
  - Small: `gap-6` (1.5rem) 
  - Large: `gap-12` (3rem)
- **Centered Content**: Maximum width with auto margins
- **Responsive Width**: Full width on mobile, constrained on desktop
- **Nuxt Page Slot**: Renders child route components

## Nested Route Structure

### File Organization
```
pages/
├── settings.vue          # This layout file
└── settings/
    ├── index.vue         # /settings (General)
    ├── members.vue       # /settings/members
    ├── notifications.vue # /settings/notifications
    └── security.vue      # /settings/security
```

### Route Matching
```typescript
// Route patterns:
'/settings'               -> settings/index.vue
'/settings/members'       -> settings/members.vue  
'/settings/notifications' -> settings/notifications.vue
'/settings/security'      -> settings/security.vue
```

## Nuxt UI Pro Components Used

### Layout Components
- **`UDashboardPanel`**: Main container with custom body styling
- **`UDashboardNavbar`**: Header navigation with title
- **`UDashboardSidebarCollapse`**: Mobile navigation toggle
- **`UDashboardToolbar`**: Navigation menu container

### Navigation Components
- **`UNavigationMenu`**: Horizontal navigation with highlighting
- **NavigationMenuItem**: TypeScript interface for menu items

### Content Components
- **`NuxtPage`**: Renders child route components

## Responsive Design Patterns

### Mobile-First Approach
```css
class="flex flex-col gap-4 sm:gap-6 lg:gap-12 w-full lg:max-w-2xl mx-auto"
```

### Breakpoint Strategy
- **Mobile (default)**: Basic spacing and full width
- **Small (sm:640px)**: Increased spacing
- **Large (lg:1024px)**: Maximum spacing and width constraints

### Navigation Adaptation
- **Mobile**: Collapsible sidebar navigation
- **Desktop**: Persistent horizontal navigation menu
- **Responsive Icons**: Consistent across all screen sizes

## User Experience Features

### Navigation Feedback
- **Active States**: Visual indication of current page
- **Hover Effects**: Interactive feedback on menu items
- **Focus Management**: Proper keyboard navigation support

### Content Organization
- **Logical Grouping**: Related settings pages grouped together
- **Clear Hierarchy**: Visual distinction between internal and external links
- **Consistent Spacing**: Harmonious vertical rhythm

### Accessibility
- **Semantic HTML**: Proper heading and navigation structure
- **ARIA Labels**: Screen reader friendly navigation
- **Keyboard Support**: Full keyboard accessibility
- **Focus Indicators**: Clear focus states

## Performance Considerations

### Navigation Optimization
```typescript
satisfies NavigationMenuItem[][]
```

**Benefits:**
- **Type Safety**: Compile-time validation of navigation structure
- **Code Splitting**: Child routes load independently
- **Static Analysis**: Better tree-shaking and optimization

### Layout Efficiency
- **Single Layout**: Shared layout reduces code duplication
- **Consistent Structure**: Unified design patterns
- **Optimized Rendering**: Minimal re-renders on navigation

## Integration Points

### Global Navigation
- **Sidebar Integration**: Works with main application sidebar
- **Breadcrumb Potential**: Foundation for breadcrumb navigation
- **State Management**: Maintains navigation state across routes

### Child Page Integration
```vue
<NuxtPage />
```

**Communication:**
- **Props Passing**: Can pass props to child pages if needed
- **Event Handling**: Child pages can emit events to parent
- **Shared State**: Access to parent component state

### External Systems
- **Documentation Links**: Direct integration with Nuxt UI documentation
- **Commerce Integration**: Purchase links for Pro features

## Customization Options

### Menu Structure
- **Adding Items**: Easy to extend navigation menu
- **Icon Changes**: Customizable Lucide icons
- **Route Modifications**: Flexible route structure

### Layout Modifications
- **Spacing Adjustments**: Customizable gap sizes
- **Width Constraints**: Adjustable max-width values
- **Panel Configuration**: Modifiable dashboard panel settings

### Styling Overrides
- **UI Props**: Custom styling through `ui` prop
- **CSS Classes**: Additional styling via class attributes
- **Theme Integration**: Compatible with Nuxt UI theming system

This settings layout provides a solid foundation for a comprehensive settings system with excellent navigation, responsive design, and extensibility.
