# Dashboard Layout Architecture Documentation
**File: `layouts/default.vue`**

## Overview
The `layouts/default.vue` file serves as the foundational layout structure for the entire dashboard application. It implements a sophisticated sidebar-based dashboard layout using Nuxt UI Pro components, providing a consistent UI shell that wraps all dashboard pages with navigation, search, and notification capabilities.

## Architecture Components

### Core Layout Structure
The layout is built around **Nuxt UI Pro Dashboard components** that provide enterprise-grade dashboard functionality:

```vue
<UDashboardGroup unit="rem">
  <UDashboardSidebar>
    <!-- Navigation and menu content -->
  </UDashboardSidebar>
  
  <UDashboardSearch />
  <slot /> <!-- Page content -->
  <NotificationsSlideover />
</UDashboardGroup>
```

## Key Components Analysis

### 1. UDashboardGroup
**Purpose**: Root container that manages the overall layout state and coordination between sidebar and content areas.

**Configuration**:
- `unit="rem"`: Uses rem units for responsive sizing calculations
- Provides centralized state management for sidebar open/collapsed states
- Handles responsive behavior across different screen sizes

### 2. UDashboardSidebar
**Primary Features**:
- **Collapsible**: `collapsible` prop enables sidebar collapse/expand functionality
- **Resizable**: `resizable` prop allows users to drag and resize sidebar width
- **Responsive**: Automatically converts to drawer/modal on mobile devices
- **Persistent State**: Maintains user's sidebar size preferences

**Key Props**:
```vue
<UDashboardSidebar
  id="default"
  v-model:open="open"
  collapsible
  resizable
  class="bg-elevated/25"
  :ui="{ footer: 'lg:border-t lg:border-default' }"
>
```

### 3. Sidebar Slot Structure

#### Header Slot (`#header`)
```vue
<template #header="{ collapsed }">
  <TeamsMenu :collapsed="collapsed" />
</template>
```
- **TeamsMenu Component**: Dropdown selector for switching between different teams/organizations
- **Adaptive Design**: Adjusts display based on collapsed state
- **Teams Management**: Allows users to select from available teams and manage team settings

#### Default Slot (`#default`) - Navigation Area
```vue
<template #default="{ collapsed }">
  <UDashboardSearchButton :collapsed="collapsed" />
  <UNavigationMenu :collapsed="collapsed" :items="links[0]" />
  <UNavigationMenu :collapsed="collapsed" :items="links[1]" class="mt-auto" />
</template>
```

**Components**:
1. **UDashboardSearchButton**: Triggers the command palette/search functionality
2. **Primary Navigation Menu**: Main application routes (Home, Inbox, Customers, Settings)
3. **Secondary Navigation Menu**: External links and support options (positioned at bottom with `mt-auto`)

#### Footer Slot (`#footer`)
```vue
<template #footer="{ collapsed }">
  <UserMenu :collapsed="collapsed" />
</template>
```
- **UserMenu Component**: User profile dropdown with settings, theme controls, and logout

## Navigation Structure

### Primary Navigation Links
Configured with comprehensive navigation items including:

```typescript
const links = [[{
  label: 'Home',
  icon: 'i-lucide-house',
  to: '/',
  onSelect: () => { open.value = false }
}, {
  label: 'Inbox',
  icon: 'i-lucide-inbox',
  to: '/inbox',
  badge: '4',  // Dynamic notification badge
  onSelect: () => { open.value = false }
}, {
  label: 'Customers',
  icon: 'i-lucide-users',
  to: '/customers',
  onSelect: () => { open.value = false }
}, {
  label: 'Settings',
  to: '/settings',
  icon: 'i-lucide-settings',
  defaultOpen: true,
  type: 'trigger',
  children: [/* Nested settings pages */]
}]]
```

**Key Features**:
- **Lucide Icons**: Consistent icon system using Lucide icon library
- **Badge Support**: Dynamic badges for notifications (e.g., Inbox shows "4")
- **Nested Navigation**: Settings section includes sub-routes (General, Members, Notifications, Security)
- **Mobile Optimization**: `onSelect` handlers close sidebar on mobile navigation

### Secondary Navigation Links
```typescript
[{
  label: 'Feedback',
  icon: 'i-lucide-message-circle',
  to: 'https://github.com/nuxt-ui-pro/dashboard',
  target: '_blank'
}, {
  label: 'Help & Support',
  icon: 'i-lucide-info',
  to: 'https://github.com/nuxt/ui-pro',
  target: '_blank'
}]
```

## Advanced Features

### 1. Command Palette Integration
```vue
<UDashboardSearch :groups="groups" />
```

**Search Groups Configuration**:
- **Links Group**: Quick navigation to all available routes
- **Code Group**: Developer tools (view page source on GitHub)
- **Dynamic Context**: Updates based on current route context

### 2. Cookie Consent Management
```typescript
onMounted(async () => {
  const cookie = useCookie('cookie-consent')
  if (cookie.value === 'accepted') {
    return
  }
  
  toast.add({
    title: 'We use first-party cookies to enhance your experience...',
    duration: 0,
    close: false,
    actions: [/* Accept/Opt out buttons */]
  })
})
```

**Implementation**:
- **Persistent Storage**: Uses Nuxt's `useCookie` for state persistence
- **Non-intrusive**: Only shows toast if consent hasn't been given
- **User Choice**: Provides accept/opt-out options

### 3. Notifications System
```vue
<NotificationsSlideover />
```

**Features**:
- **Slideover Panel**: Dedicated notification panel accessible from anywhere
- **Real-time Updates**: Fetches notifications from `/api/notifications`
- **Interactive**: Notifications link to relevant content (e.g., inbox items)
- **Status Indicators**: Shows unread status with visual indicators

## Component Dependencies

### Custom Components
1. **TeamsMenu.vue**: Team selection and management
2. **UserMenu.vue**: User profile, settings, and theme controls
3. **NotificationsSlideover.vue**: Notification panel with real-time updates

### Nuxt UI Pro Components
- `UDashboardGroup`: Layout container with state management
- `UDashboardSidebar`: Advanced sidebar with collapse/resize capabilities
- `UDashboardSearchButton`: Search trigger for command palette
- `UDashboardSearch`: Full-featured command palette
- `UNavigationMenu`: Intelligent navigation with nested support

## Responsive Behavior

### Desktop (lg+)
- Sidebar visible by default with resizable width
- Collapse button available for space optimization
- Full navigation menu with labels and icons
- Footer shows user details

### Mobile/Tablet
- Sidebar converts to drawer/modal overlay
- Toggle button appears in header areas
- Condensed navigation with icons only when collapsed
- Touch-optimized interactions

## State Management

### Sidebar State
```typescript
const open = ref(false)  // Controls mobile sidebar visibility
```

### Search Integration
```typescript
const groups = computed(() => [
  {
    id: 'links',
    label: 'Go to',
    items: links.flat()
  },
  {
    id: 'code',
    label: 'Code',
    items: [/* Developer tools */]
  }
])
```

## Styling and Theming

### Custom Styling
```vue
class="bg-elevated/25"  // Semi-transparent elevated background
:ui="{ footer: 'lg:border-t lg:border-default' }"  // Custom footer border
```

### Design System Integration
- Uses Nuxt UI Pro design tokens
- Consistent spacing and typography
- Dark/light mode support through UserMenu theme controls
- Accessible color contrasts and focus states

## Performance Considerations

1. **Lazy Loading**: Navigation items loaded on demand
2. **State Persistence**: Sidebar preferences saved to cookies
3. **Efficient Re-renders**: Computed properties for dynamic content
4. **Mobile Optimization**: Touch-friendly interactions and gestures

## Integration with Application

### Layout Usage
Pages automatically inherit this layout structure:
```vue
<!-- pages/index.vue -->
<template>
  <!-- Page content automatically wrapped in layout -->
  <div>Dashboard content</div>
</template>
```

### Global Navigation State
The layout provides consistent navigation across all dashboard pages while maintaining individual page contexts and functionality.

This architecture establishes a robust foundation for the entire dashboard application, providing users with an intuitive and feature-rich interface that scales from mobile to desktop environments.
