# useDashboard Composable Documentation

## Overview

The `useDashboard` composable is a centralized state management solution for dashboard-wide functionality in the Nuxt application. It provides shared reactive state and keyboard navigation shortcuts that persist across the entire dashboard interface.

## Architecture

The composable is implemented as a **shared composable** using VueUse's `createSharedComposable`, ensuring that the same instance and state are shared across all components that consume it.

## Core Features

### 1. Reactive State Management
- **Notifications Slideover**: Manages the open/closed state of the notifications panel
- **Route-aware State**: Automatically closes slideovers when navigating between pages

### 2. Keyboard Navigation
- **Global Shortcuts**: Provides quick navigation shortcuts throughout the dashboard
- **Accessibility**: Enhances user experience with keyboard-first navigation

### 3. Shared Instance
- **Singleton Pattern**: Uses `createSharedComposable` to ensure state consistency
- **Global Access**: Available across all dashboard components without prop drilling

## Implementation Details

### Dependencies

```typescript
import { createSharedComposable } from '@vueuse/core'
```

- **@vueuse/core**: For creating shared composable instances
- **Nuxt Router**: For navigation functionality
- **Nuxt Route**: For route watching

### State Properties

| Property | Type | Description |
|----------|------|-------------|
| `isNotificationsSlideoverOpen` | `Ref<boolean>` | Controls the visibility state of the notifications slideover panel |

### Keyboard Shortcuts

| Shortcut | Action | Description |
|----------|--------|-------------|
| `g-h` | Navigate Home | Navigates to the dashboard home page (/) |
| `g-i` | Navigate Inbox | Navigates to the inbox page (/inbox) |
| `g-c` | Navigate Customers | Navigates to the customers page (/customers) |
| `g-s` | Navigate Settings | Navigates to the settings page (/settings) |
| `n` | Toggle Notifications | Opens/closes the notifications slideover |

### Route Watching

The composable automatically watches for route changes and:
- Closes the notifications slideover when navigating to a different page
- Ensures clean UI state transitions between pages

## Usage Patterns

### Basic Usage

```typescript
// In any component
const { isNotificationsSlideoverOpen } = useDashboard()

// Toggle notifications programmatically
isNotificationsSlideoverOpen.value = true
```

### Template Integration

```vue
<template>
  <!-- Notifications slideover controlled by shared state -->
  <USlideover v-model="isNotificationsSlideoverOpen">
    <NotificationsPanel />
  </USlideover>
</template>

<script setup>
const { isNotificationsSlideoverOpen } = useDashboard()
</script>
```

### Layout Integration

```vue
<!-- layouts/dashboard.vue -->
<template>
  <div class="dashboard-layout">
    <DashboardSidebar />
    <main>
      <slot />
    </main>
    
    <!-- Shared notifications panel -->
    <USlideover v-model="isNotificationsSlideoverOpen">
      <NotificationsPanel />
    </USlideover>
  </div>
</template>

<script setup>
const { isNotificationsSlideoverOpen } = useDashboard()
</script>
```

## Return Values

### Reactive State

```typescript
interface UseDashboardReturn {
  isNotificationsSlideoverOpen: Ref<boolean>
}
```

| Property | Type | Reactive | Description |
|----------|------|----------|-------------|
| `isNotificationsSlideoverOpen` | `Ref<boolean>` | ✅ | Reactive reference to notifications slideover state |

## Component Integration Examples

### Navigation Component

```vue
<!-- components/DashboardNavigation.vue -->
<template>
  <nav>
    <UButton 
      @click="isNotificationsSlideoverOpen = true"
      icon="i-heroicons-bell"
    >
      Notifications
    </UButton>
  </nav>
</template>

<script setup>
const { isNotificationsSlideoverOpen } = useDashboard()
</script>
```

### Header Component

```vue
<!-- components/DashboardHeader.vue -->
<template>
  <header>
    <div class="header-actions">
      <UButton 
        variant="ghost" 
        @click="toggleNotifications"
        :class="{ 'bg-primary-50': isNotificationsSlideoverOpen }"
      >
        <UIcon name="i-heroicons-bell" />
        <UBadge v-if="unreadCount" size="xs" color="red">
          {{ unreadCount }}
        </UBadge>
      </UButton>
    </div>
  </header>
</template>

<script setup>
const { isNotificationsSlideoverOpen } = useDashboard()
const unreadCount = ref(3) // From notifications API

const toggleNotifications = () => {
  isNotificationsSlideoverOpen.value = !isNotificationsSlideoverOpen.value
}
</script>
```

## Best Practices

### 1. State Management
- Use the shared composable for dashboard-wide state only
- Keep component-specific state in individual components
- Leverage reactive updates for UI synchronization

### 2. Keyboard Shortcuts
- Follow consistent patterns for shortcut keys
- Use 'g-' prefix for navigation shortcuts
- Keep shortcuts memorable and intuitive

### 3. Route Integration
- Always watch for route changes in shared state
- Clean up UI state during navigation
- Prevent state leakage between pages

### 4. Performance
- The shared composable pattern prevents unnecessary re-instantiation
- State is preserved across route changes
- Minimal overhead due to VueUse optimizations

## Advanced Usage

### Custom Notifications Handler

```typescript
// composables/useNotifications.ts
export const useNotifications = () => {
  const { isNotificationsSlideoverOpen } = useDashboard()
  const notifications = ref([])
  
  const openNotifications = () => {
    isNotificationsSlideoverOpen.value = true
  }
  
  const markAsRead = (id: string) => {
    // Handle marking notification as read
  }
  
  return {
    notifications,
    openNotifications,
    markAsRead
  }
}
```

### Integration with External State

```typescript
// stores/dashboard.ts
export const useDashboardStore = defineStore('dashboard', () => {
  const { isNotificationsSlideoverOpen } = useDashboard()
  
  // Sync with external state management
  watch(isNotificationsSlideoverOpen, (isOpen) => {
    if (isOpen) {
      // Track analytics, fetch notifications, etc.
    }
  })
  
  return {
    // Store specific state and actions
  }
})
```

## Migration Guide

### From Component State
```typescript
// Before: Component-level state
const isNotificationsOpen = ref(false)

// After: Shared composable
const { isNotificationsSlideoverOpen } = useDashboard()
```

### From Props/Emit Pattern
```vue
<!-- Before: Props drilling -->
<template>
  <NotificationButton @toggle="$emit('toggle-notifications')" />
</template>

<!-- After: Shared state -->
<template>
  <NotificationButton />
</template>

<script setup>
// No props or emits needed - uses shared state directly
</script>
```

## Troubleshooting

### Common Issues

1. **State Not Syncing**: Ensure you're using the same `useDashboard()` import
2. **Shortcuts Not Working**: Verify `defineShortcuts` is properly imported from Nuxt
3. **Route Changes Not Detected**: Check that `useRoute()` is accessible in component context

### Debug Helpers

```typescript
// Enable debugging in development
if (process.dev) {
  const { isNotificationsSlideoverOpen } = useDashboard()
  
  watch(isNotificationsSlideoverOpen, (newVal) => {
    console.log('Notifications slideover:', newVal ? 'opened' : 'closed')
  })
}
```

## Related Documentation

- [Nuxt Composables](https://nuxt.com/docs/guide/directory-structure/composables)
- [VueUse Shared Composables](https://vueuse.org/shared/createSharedComposable/)
- [Nuxt UI Components](https://ui.nuxt.com/)
- [Keyboard Shortcuts with defineShortcuts](https://nuxt.com/docs/api/utils/define-shortcuts)
