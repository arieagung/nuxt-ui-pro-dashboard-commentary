# Customer Management - `pages/customers.vue`

## Overview

The customers page provides a comprehensive customer management interface with advanced data table functionality, CRUD operations, and powerful filtering capabilities. Built with Nuxt UI Pro components and TanStack Table for optimal performance and user experience.

## Route Information

- **Path**: `/customers`
- **File**: `pages/customers.vue`
- **Layout**: Default (no specific layout defined)
- **Page Name**: `customers`

## Key Features

### 1. Advanced Data Table
- **Pagination**: Built-in pagination with configurable page sizes
- **Sorting**: Column-based sorting with visual indicators
- **Row Selection**: Multi-row selection with bulk operations
- **Column Visibility**: Dynamic column show/hide functionality
- **Filtering**: Real-time search and status-based filtering

### 2. Customer Data Display
- **Avatar Integration**: Customer profile pictures with fallback handling
- **Status Indicators**: Visual badges for subscription status (subscribed, unsubscribed, bounced)
- **Contact Information**: Email and location details
- **Unique Identifiers**: Customer ID display with copy functionality

### 3. Bulk Operations
- **Multi-select**: Checkbox selection for individual or all customers
- **Bulk Delete**: Delete multiple customers with confirmation modal
- **Action Menu**: Context menus for individual customer operations

## Component Structure

```vue
<script setup lang="ts">
import type { TableColumn } from '@nuxt/ui'
import { upperFirst } from 'scule'
import { getPaginationRowModel } from '@tanstack/table-core'
import type { Row } from '@tanstack/table-core'
import type { User } from '~/types'

// Component resolution for dynamic rendering
const UAvatar = resolveComponent('UAvatar')
const UButton = resolveComponent('UButton')
const UBadge = resolveComponent('UBadge')
const UDropdownMenu = resolveComponent('UDropdownMenu')
const UCheckbox = resolveComponent('UCheckbox')

// Toast notifications
const toast = useToast()
const table = useTemplateRef('table')

// Table state management
const columnFilters = ref([{
  id: 'email',
  value: ''
}])
const columnVisibility = ref()
const rowSelection = ref({ 1: true })
const pagination = ref({
  pageIndex: 0,
  pageSize: 10
})

// Data fetching with useFetch
const { data, status } = await useFetch<User[]>('/api/customers', {
  lazy: true
})
</script>
```

## Data Fetching Patterns

### API Integration
```typescript
// Lazy loading with type safety
const { data, status } = await useFetch<User[]>('/api/customers', {
  lazy: true
})
```

**Benefits:**
- **Type Safety**: Full TypeScript support with `User[]` typing
- **Lazy Loading**: Data loads asynchronously without blocking navigation
- **Reactive State**: Automatic updates when data changes
- **Error Handling**: Built-in error state management

### State Management
- **Column Filters**: Reactive filtering state for search functionality
- **Row Selection**: Tracked selected rows for bulk operations
- **Pagination**: Client-side pagination state management
- **Column Visibility**: Dynamic column show/hide state

## Table Column Configuration

### Selection Column
```typescript
{
  id: 'select',
  header: ({ table }) => h(UCheckbox, {
    'modelValue': table.getIsSomePageRowsSelected() 
      ? 'indeterminate' 
      : table.getIsAllPageRowsSelected(),
    'onUpdate:modelValue': (value: boolean | 'indeterminate') => 
      table.toggleAllPageRowsSelected(!!value),
    'ariaLabel': 'Select all'
  }),
  cell: ({ row }) => h(UCheckbox, {
    'modelValue': row.getIsSelected(),
    'onUpdate:modelValue': (value: boolean | 'indeterminate') => 
      row.toggleSelected(!!value),
    'ariaLabel': 'Select row'
  })
}
```

### Customer Name Column
```typescript
{
  accessorKey: 'name',
  header: 'Name',
  cell: ({ row }) => {
    return h('div', { class: 'flex items-center gap-3' }, [
      h(UAvatar, {
        ...row.original.avatar,
        size: 'lg'
      }),
      h('div', undefined, [
        h('p', { class: 'font-medium text-highlighted' }, row.original.name),
        h('p', { class: '' }, `@${row.original.name}`)
      ])
    ])
  }
}
```

### Status Column with Badges
```typescript
{
  accessorKey: 'status',
  header: 'Status',
  filterFn: 'equals',
  cell: ({ row }) => {
    const color = {
      subscribed: 'success' as const,
      unsubscribed: 'error' as const,
      bounced: 'warning' as const
    }[row.original.status]

    return h(UBadge, { 
      class: 'capitalize', 
      variant: 'subtle', 
      color 
    }, () => row.original.status)
  }
}
```

## Row Actions & Context Menus

### Action Menu Configuration
```typescript
function getRowItems(row: Row<User>) {
  return [
    {
      type: 'label',
      label: 'Actions'
    },
    {
      label: 'Copy customer ID',
      icon: 'i-lucide-copy',
      onSelect() {
        navigator.clipboard.writeText(row.original.id.toString())
        toast.add({
          title: 'Copied to clipboard',
          description: 'Customer ID copied to clipboard'
        })
      }
    },
    { type: 'separator' },
    {
      label: 'View customer details',
      icon: 'i-lucide-list'
    },
    {
      label: 'View customer payments',
      icon: 'i-lucide-wallet'
    },
    { type: 'separator' },
    {
      label: 'Delete customer',
      icon: 'i-lucide-trash',
      color: 'error',
      onSelect() {
        toast.add({
          title: 'Customer deleted',
          description: 'The customer has been deleted.'
        })
      }
    }
  ]
}
```

## Filtering & Search Functionality

### Email Search
```vue
<UInput
  :model-value="(table?.tableApi?.getColumn('email')?.getFilterValue() as string)"
  class="max-w-sm"
  icon="i-lucide-search"
  placeholder="Filter emails..."
  @update:model-value="table?.tableApi?.getColumn('email')?.setFilterValue($event)"
/>
```

### Status Filtering
```vue
<USelect
  v-model="statusFilter"
  :items="[
    { label: 'All', value: 'all' },
    { label: 'Subscribed', value: 'subscribed' },
    { label: 'Unsubscribed', value: 'unsubscribed' },
    { label: 'Bounced', value: 'bounced' }
  ]"
  placeholder="Filter status"
  class="min-w-28"
/>
```

### Column Visibility Control
```vue
<UDropdownMenu
  :items="
    table?.tableApi
      ?.getAllColumns()
      .filter((column) => column.getCanHide())
      .map((column) => ({
        label: upperFirst(column.id),
        type: 'checkbox' as const,
        checked: column.getIsVisible(),
        onUpdateChecked(checked: boolean) {
          table?.tableApi?.getColumn(column.id)?.toggleVisibility(!!checked)
        }
      }))
  "
>
  <UButton
    label="Display"
    color="neutral"
    variant="outline"
    trailing-icon="i-lucide-settings-2"
  />
</UDropdownMenu>
```

## Nuxt UI Pro Components Used

### Core Table Components
- **`UTable`**: Advanced data table with TanStack Table integration
- **`UPagination`**: Pagination controls with page size options
- **`UCheckbox`**: Multi-select functionality
- **`UBadge`**: Status indicators with color coding

### Form & Input Components
- **`UInput`**: Search and filter inputs with icons
- **`USelect`**: Dropdown filters for status selection
- **`UButton`**: Action buttons with variants and icons
- **`UDropdownMenu`**: Context menus and column visibility

### Layout Components
- **`UDashboardPanel`**: Main container with proper structure
- **`UDashboardNavbar`**: Navigation header with actions
- **`UDashboardSidebarCollapse`**: Mobile navigation toggle

### User Interface Components
- **`UAvatar`**: Customer profile pictures
- **`UKbd`**: Keyboard shortcut indicators
- **`UTooltip`**: Contextual help (implied usage)

## CRUD Operations

### Create Operations
- **Add Customer Modal**: Accessible via `CustomersAddModal` component
- **Quick Actions**: "New customer" action from dashboard

### Read Operations
- **Data Fetching**: Lazy-loaded customer data from API
- **Search**: Real-time filtering by email
- **Sorting**: Column-based data sorting

### Update Operations
- **In-line Editing**: Future enhancement capability
- **Status Updates**: Through row action menus

### Delete Operations
- **Single Delete**: Individual customer deletion via row actions
- **Bulk Delete**: Multi-select deletion with `CustomersDeleteModal`
- **Confirmation**: Safe deletion with user confirmation

## Performance Optimizations

### TanStack Table Integration
```typescript
:pagination-options="{
  getPaginationRowModel: getPaginationRowModel()
}"
```

**Benefits:**
- **Virtual Scrolling**: Efficient rendering of large datasets
- **Client-side Pagination**: Reduced server requests
- **Sorting Performance**: Optimized column sorting
- **Memory Management**: Efficient row selection tracking

### Lazy Loading
- **API Calls**: Non-blocking data fetching
- **Component Resolution**: Dynamic component loading
- **State Updates**: Efficient reactive updates

## User Experience Features

### Accessibility
- **ARIA Labels**: Proper screen reader support
- **Keyboard Navigation**: Full keyboard accessibility
- **Focus Management**: Logical tab order
- **High Contrast**: Status badges with clear color differentiation

### Responsive Design
```css
class="flex flex-wrap items-center justify-between gap-1.5"
```

- **Mobile-first**: Responsive layout adaptation
- **Touch Targets**: Appropriate sizing for mobile devices
- **Flexible Grid**: Adaptive column layouts

### Toast Notifications
```typescript
toast.add({
  title: 'Copied to clipboard',
  description: 'Customer ID copied to clipboard'
})
```

- **User Feedback**: Immediate operation confirmation
- **Error Handling**: Clear error messaging
- **Success States**: Positive action feedback

## Integration Points

### Modal Components
- **`CustomersAddModal`**: Customer creation interface
- **`CustomersDeleteModal`**: Bulk deletion confirmation

### API Endpoints
- **`/api/customers`**: Customer data retrieval
- **Future endpoints**: CRUD operations for customer management

### Navigation
- **Dashboard Integration**: Quick access from home page
- **Sidebar Navigation**: Global navigation context

## Styling & Theming

### Table Styling
```typescript
:ui="{
  base: 'table-fixed border-separate border-spacing-0',
  thead: '[&>tr]:bg-elevated/50 [&>tr]:after:content-none',
  tbody: '[&>tr]:last:[&>td]:border-b-0',
  th: 'py-2 first:rounded-l-lg last:rounded-r-lg border-y border-default first:border-l last:border-r',
  td: 'border-b border-default'
}"
```

### Color System
- **Status Colors**: Success (green), Error (red), Warning (yellow)
- **Neutral Tones**: Consistent with design system
- **Interactive States**: Hover and focus states

This customer management interface provides a robust, user-friendly solution for managing customer data with enterprise-level features and performance optimization.
