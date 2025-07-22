# Inbox Message Center - `pages/inbox.vue`

## Overview

The inbox page provides a comprehensive message and notification center implementation with a modern, responsive interface. It features a split-panel layout for efficient message browsing and reading, with advanced filtering capabilities and mobile-optimized interactions.

## Route Information

- **Path**: `/inbox`
- **File**: `pages/inbox.vue`
- **Layout**: Default (no specific layout defined)
- **Page Name**: `inbox`

## Key Features

### 1. Split-Panel Interface
- **Resizable Panels**: Left panel for message list (25% default, 20-30% range)
- **Responsive Design**: Automatic mobile adaptations with slideover
- **Dynamic Sizing**: User-configurable panel widths

### 2. Message Filtering
- **Tab-based Filtering**: "All" and "Unread" message views
- **Real-time Updates**: Live filtered message counts
- **State Persistence**: Filter state maintained during navigation

### 3. Mobile Optimization
- **Responsive Breakpoints**: Automatic mobile/desktop detection
- **Slideover Interface**: Full-screen message view on mobile
- **Touch-friendly**: Optimized for mobile interactions

## Component Structure

```vue
<script setup lang="ts">
import { computed, ref, watch } from 'vue'
import { breakpointsTailwind } from '@vueuse/core'
import type { Mail } from '~/types'

// Tab configuration
const tabItems = [{
  label: 'All',
  value: 'all'
}, {
  label: 'Unread',
  value: 'unread'
}]
const selectedTab = ref('all')

// Data fetching
const { data: mails } = await useFetch<Mail[]>('/api/mails', { 
  default: () => [] 
})

// Computed filtered mails
const filteredMails = computed(() => {
  if (selectedTab.value === 'unread') {
    return mails.value.filter(mail => !!mail.unread)
  }
  return mails.value
})

// Selected mail state
const selectedMail = ref<Mail | null>()

// Mobile responsiveness
const breakpoints = useBreakpoints(breakpointsTailwind)
const isMobile = breakpoints.smaller('lg')
</script>
```

## Data Fetching Patterns

### API Integration
```typescript
const { data: mails } = await useFetch<Mail[]>('/api/mails', { 
  default: () => [] 
})
```

**Benefits:**
- **Type Safety**: Full TypeScript support with `Mail[]` typing
- **Default Values**: Empty array fallback prevents undefined errors
- **Reactive Data**: Automatic UI updates when mail data changes
- **SSR Support**: Server-side rendering compatibility

### Computed Filtering
```typescript
const filteredMails = computed(() => {
  if (selectedTab.value === 'unread') {
    return mails.value.filter(mail => !!mail.unread)
  }
  return mails.value
})
```

**Features:**
- **Reactive Filtering**: Automatic re-computation when tab changes
- **Performance Optimized**: Computed caching prevents unnecessary recalculation
- **Type Safety**: Maintains Mail[] type throughout filtering

## Responsive Layout Implementation

### Breakpoint Management
```typescript
const breakpoints = useBreakpoints(breakpointsTailwind)
const isMobile = breakpoints.smaller('lg')
```

### Panel Configuration
```vue
<UDashboardPanel
  id="inbox-1"
  :default-size="25"
  :min-size="20"
  :max-size="30"
  resizable
>
```

**Responsive Features:**
- **Desktop**: Resizable split-panel layout
- **Mobile**: Full-width list with slideover detail view
- **Tablet**: Adaptive behavior based on screen size

### Mobile Slideover
```vue
<ClientOnly>
  <USlideover v-if="isMobile" v-model:open="isMailPanelOpen">
    <template #content>
      <InboxMail v-if="selectedMail" :mail="selectedMail" @close="selectedMail = null" />
    </template>
  </USlideover>
</ClientOnly>
```

## State Management

### Mail Selection
```typescript
const selectedMail = ref<Mail | null>()

const isMailPanelOpen = computed({
  get() {
    return !!selectedMail.value
  },
  set(value: boolean) {
    if (!value) {
      selectedMail.value = null
    }
  }
})
```

### Filter Synchronization
```typescript
// Reset selected mail if it's not in the filtered mails
watch(filteredMails, () => {
  if (!filteredMails.value.find(mail => mail.id === selectedMail.value?.id)) {
    selectedMail.value = null
  }
})
```

**Benefits:**
- **Data Consistency**: Ensures selected mail is always available in current filter
- **User Experience**: Prevents broken states when switching filters
- **Reactive Updates**: Automatic cleanup of invalid selections

## Nuxt UI Pro Components Used

### Layout Components
- **`UDashboardPanel`**: Resizable panel container with size constraints
- **`UDashboardNavbar`**: Navigation header with title and controls
- **`UDashboardSidebarCollapse`**: Mobile navigation toggle

### Navigation Components
- **`UTabs`**: Filter tab interface (All/Unread)
- **`UBadge`**: Message count display with subtle variant
- **`UIcon`**: Empty state icon for no selection

### Mobile Components
- **`USlideover`**: Full-screen mobile message interface
- **`ClientOnly`**: Prevents SSR hydration issues

### Custom Components
- **`InboxList`**: Custom message list component
- **`InboxMail`**: Custom message detail component

## User Interface States

### Empty State
```vue
<div v-else class="hidden lg:flex flex-1 items-center justify-center">
  <UIcon name="i-lucide-inbox" class="size-32 text-dimmed" />
</div>
```

### Loading State
- Handled by `useFetch` built-in loading states
- Graceful degradation during data fetching

### Error Handling
- Default empty array prevents runtime errors
- Graceful fallbacks for missing data

## Accessibility Features

### Keyboard Navigation
- Tab navigation through message list
- Arrow key message selection (via InboxList component)
- Escape key to close message detail

### Screen Reader Support
- Proper ARIA labels on interactive elements
- Semantic HTML structure
- Badge count announcements

### Focus Management
- Logical focus order between panels
- Focus restoration after modal close
- Clear focus indicators

## Performance Optimizations

### Lazy Loading
```vue
<ClientOnly>
  <USlideover v-if="isMobile" v-model:open="isMailPanelOpen">
```

**Benefits:**
- **SSR Optimization**: Mobile components only load client-side
- **Bundle Splitting**: Reduced initial page load
- **Conditional Loading**: Components load only when needed

### Computed Caching
- Filtered mails computation cached until dependencies change
- Reactive updates minimize unnecessary DOM updates

### Memory Management
- Proper cleanup of selected state
- Event listener cleanup in breakpoint management

## Integration Points

### Component Communication
```vue
<InboxList v-model="selectedMail" :mails="filteredMails" />
<InboxMail v-if="selectedMail" :mail="selectedMail" @close="selectedMail = null" />
```

### API Endpoints
- **`/api/mails`**: Mail data retrieval
- **Future endpoints**: Mark as read/unread, delete operations

### Navigation Integration
- **Dashboard Link**: "New mail" quick action
- **Global Navigation**: Accessible via sidebar

## Mobile-First Design

### Breakpoint Strategy
```typescript
const breakpoints = useBreakpoints(breakpointsTailwind)
// Uses standard Tailwind breakpoints:
// sm: 640px, md: 768px, lg: 1024px, xl: 1280px
```

### Responsive Patterns
- **Mobile**: Stack layout with slideover details
- **Tablet**: Adaptive panel sizing
- **Desktop**: Full split-panel experience

### Touch Interactions
- **Swipe Gestures**: Enabled through slideover component
- **Touch Targets**: Appropriate sizing for mobile interaction
- **Scroll Behavior**: Optimized for touch devices

## Styling & Theming

### Panel Styling
```vue
:default-size="25"
:min-size="20" 
:max-size="30"
```

### Icon System
```vue
<UIcon name="i-lucide-inbox" class="size-32 text-dimmed" />
```

### Responsive Classes
```vue
class="hidden lg:flex flex-1 items-center justify-center"
```

## Future Enhancement Opportunities

### Advanced Features
- **Search Functionality**: Full-text search through messages
- **Multiple Selection**: Bulk operations on messages
- **Categories/Labels**: Message organization system
- **Real-time Updates**: WebSocket integration for live updates

### Performance Improvements
- **Virtual Scrolling**: For large message lists
- **Infinite Loading**: Paginated message loading
- **Caching Strategy**: Improved data persistence

### User Experience
- **Keyboard Shortcuts**: Power user functionality
- **Drag & Drop**: Message organization
- **Offline Support**: PWA capabilities

This inbox implementation provides a solid foundation for a modern messaging interface with excellent mobile support and room for future enhancements.
