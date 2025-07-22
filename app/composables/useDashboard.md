# useDashboard Composable

## Purpose
The `useDashboard` composable provides centralized dashboard state management and keyboard shortcuts. It manages global UI state like slideouts and implements application-wide navigation shortcuts for improved user productivity.

## Reusability
This composable demonstrates excellent patterns for global state management in Nuxt applications. The shared composable pattern and keyboard shortcuts system can be adapted for any dashboard or admin interface requiring global state and navigation.

## API

### Returns
```typescript
{
  isNotificationsSlideoverOpen: Ref<boolean>
}
```

## Nuxt UI Pro Integration
This composable integrates with:
- `useRoute()` and `useRouter()` for navigation
- `defineShortcuts()` for keyboard navigation
- Works with `NotificationsSlideover` component
- Follows Nuxt UI Pro state management patterns

## Usage Examples
```vue
<!-- In NotificationsSlideover component -->
<script setup>
const { isNotificationsSlideoverOpen } = useDashboard()
</script>

<template>
  <USlideover v-model:open="isNotificationsSlideoverOpen" title="Notifications">
    <!-- Slideover content -->
  </USlideover>
</template>
```

```vue
<!-- In layout or navigation component -->
<script setup>
const { isNotificationsSlideoverOpen } = useDashboard()

// Open notifications programmatically
function openNotifications() {
  isNotificationsSlideoverOpen.value = true
}
</script>
```

## State Management Features

### Shared Composable Pattern
```typescript
export const useDashboard = createSharedComposable(_useDashboard)
```

This ensures that the same state instance is shared across all components that use the composable, providing true global state management.

### Reactive State
- `isNotificationsSlideoverOpen` - Controls the notifications slideover visibility
- Automatically synced across all components using the composable
- Reactive updates trigger UI changes immediately

## Keyboard Shortcuts System

### Navigation Shortcuts
```typescript
defineShortcuts({
  'g-h': () => router.push('/'),           // Go to Home
  'g-i': () => router.push('/inbox'),      // Go to Inbox  
  'g-c': () => router.push('/customers'),  // Go to Customers
  'g-s': () => router.push('/settings'),   // Go to Settings
  'n': () => isNotificationsSlideoverOpen.value = !isNotificationsSlideoverOpen.value // Toggle Notifications
})
```

### Shortcut Patterns
- **'g-' prefix**: Gmail-style navigation shortcuts
- **Single keys**: Quick actions (like 'n' for notifications)
- **Intuitive mapping**: First letter of destination (h=home, i=inbox, etc.)

## Advanced Features

### Route-based State Management
```typescript
watch(() => route.fullPath, () => {
  isNotificationsSlideoverOpen.value = false
})
```

Automatically closes slideouts when navigating to different pages, preventing UI state conflicts and improving user experience.

### Global State Synchronization
The shared composable ensures that:
- State changes propagate to all components immediately
- No prop drilling required for global UI state
- Consistent state management across the entire application

## Extension Points

### Adding New Global State
```typescript
const _useDashboard = () => {
  const route = useRoute()
  const router = useRouter()
  const isNotificationsSlideoverOpen = ref(false)
  const isSidebarCollapsed = ref(false) // New state
  const isSearchModalOpen = ref(false)  // New state
  
  // Add new shortcuts
  defineShortcuts({
    // Existing shortcuts...
    'cmd+k': () => isSearchModalOpen.value = true,
    'cmd+b': () => isSidebarCollapsed.value = !isSidebarCollapsed.value
  })
  
  return {
    isNotificationsSlideoverOpen,
    isSidebarCollapsed,      // Export new state
    isSearchModalOpen        // Export new state
  }
}
```

### Custom Shortcut Handlers
```typescript
// Add more complex navigation logic
'g-d': () => {
  if (route.path.startsWith('/dashboard')) {
    router.push('/dashboard/analytics')
  } else {
    router.push('/dashboard')
  }
}
```

## Best Practices Demonstrated

### State Management
- **Shared State**: Uses `createSharedComposable` for true global state
- **Reactive Design**: Leverages Vue's reactivity system
- **Clean API**: Simple, focused interface

### User Experience  
- **Keyboard Navigation**: Power-user friendly shortcuts
- **Automatic Cleanup**: Routes changes close modals/slideouts
- **Consistent Behavior**: Predictable state management

### Performance
- **Minimal Overhead**: Lightweight state management
- **Efficient Updates**: Only necessary reactive updates
- **Memory Management**: Automatic cleanup on unmount

## Integration with Nuxt UI Pro

### Component Integration
```vue
<!-- Any component can access global dashboard state -->
<template>
  <UButton 
    @click="isNotificationsSlideoverOpen = true"
    icon="i-lucide-bell"
    :badge="unreadCount > 0 ? unreadCount : null"
  />
</template>

<script setup>
const { isNotificationsSlideoverOpen } = useDashboard()
const { data: unreadCount } = useFetch('/api/notifications/unread-count')
</script>
```

### Layout Integration
```vue
<!-- In default.vue layout -->
<template>
  <UDashboardLayout>
    <!-- Layout content -->
    <NotificationsSlideover />
  </UDashboardLayout>
</template>

<script setup>
// Shortcuts are automatically available app-wide
const { isNotificationsSlideoverOpen } = useDashboard()
</script>
```

## Accessibility Considerations
- **Keyboard Navigation**: Provides keyboard alternatives to mouse navigation
- **Predictable Shortcuts**: Uses common shortcut patterns (Gmail-style)
- **Screen Reader**: State changes can be announced to screen readers
- **Focus Management**: Route changes appropriately manage focus

This composable exemplifies modern Vue 3 + Nuxt state management patterns and provides an excellent foundation for dashboard applications requiring global state and keyboard navigation.
