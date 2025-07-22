# Vue Component TypeScript Patterns & Nuxt UI Pro Integration

This document outlines the TypeScript patterns and type safety implementations used in Vue components throughout the dashboard application, with special focus on Nuxt UI Pro integration.

## Overview

The dashboard application demonstrates advanced TypeScript patterns in Vue 3 components, including proper integration with Nuxt UI Pro components, form validation, table configurations, and reactive state management.

## Component Type Pattern Categories

### 1. Form Components with Validation

#### Example: `AddModal.vue`

```typescript
<script setup lang="ts">
import * as z from 'zod'
import type { FormSubmitEvent } from '@nuxt/ui'

// Runtime schema validation
const schema = z.object({
  name: z.string().min(2, 'Too short'),
  email: z.string().email('Invalid email')
})

// TypeScript type from Zod schema
type Schema = z.output<typeof schema>

// Reactive state with proper typing
const state = reactive<Partial<Schema>>({
  name: undefined,
  email: undefined
})

// Type-safe form submission handler
async function onSubmit(event: FormSubmitEvent<Schema>) {
  // event.data is fully typed as Schema
  toast.add({ 
    title: 'Success', 
    description: `New customer ${event.data.name} added`,
    color: 'success' 
  })
}
</script>
```

**Type Safety Features**:
- **Schema-First Approach**: Zod schema provides both runtime validation and TypeScript types
- **Type Inference**: `z.output<typeof schema>` automatically generates TypeScript type
- **Event Typing**: `FormSubmitEvent<Schema>` ensures event data is properly typed
- **Partial State**: `Partial<Schema>` allows optional initialization

### 2. Data Table Components

#### Example: `customers.vue`

```typescript
<script setup lang="ts">
import type { TableColumn } from '@nuxt/ui'
import type { Row } from '@tanstack/table-core'
import type { User } from '~/types'

// Type-safe data fetching
const { data } = await useFetch<User[]>('/api/customers')

// Strongly typed table columns
const columns: TableColumn<User>[] = [
  {
    accessorKey: 'name',
    header: 'Name',
    cell: ({ row }) => {
      // row.original is properly typed as User
      return h('div', { class: 'flex items-center gap-3' }, [
        h(UAvatar, {
          ...row.original.avatar,  // Type-safe avatar props
          size: 'lg'
        }),
        h('div', undefined, [
          h('p', { class: 'font-medium' }, row.original.name),
          h('p', { class: '' }, `@${row.original.name}`)
        ])
      ])
    }
  }
]

// Type-safe row action handlers
function getRowItems(row: Row<User>) {
  return [
    {
      label: 'Copy customer ID',
      icon: 'i-lucide-copy',
      onSelect() {
        // row.original.id is type-checked
        navigator.clipboard.writeText(row.original.id.toString())
      }
    }
  ]
}
</script>
```

**Type Safety Features**:
- **Generic Table Columns**: `TableColumn<User>` ensures column configuration matches User interface
- **Row Type Safety**: `Row<User>` provides typed access to row data
- **Cell Renderer Typing**: Cell functions receive properly typed row parameters
- **API Response Typing**: `useFetch<User[]>` ensures response data structure

### 3. Chart Components

#### Example: `HomeChart.client.vue`

```typescript
<script setup lang="ts">
import type { Period, Range } from '~/types'

// Component props with proper typing
const props = defineProps<{
  period: Period
  range: Range
}>()

// Local data type definition
type DataRecord = {
  date: Date
  amount: number
}

// Reactive data with generic type
const data = ref<DataRecord[]>([])

// Type-safe watchers
watch([() => props.period, () => props.range], () => {
  // Type mapping for period-specific functions
  const dates = ({
    daily: eachDayOfInterval,
    weekly: eachWeekOfInterval,
    monthly: eachMonthOfInterval
  } as Record<Period, typeof eachDayOfInterval>)[props.period](props.range)
  
  // Type-safe data generation
  data.value = dates.map(date => ({ 
    date, 
    amount: Math.floor(Math.random() * 9001) + 1000 
  }))
}, { immediate: true })

// Typed accessor functions
const x = (_: DataRecord, i: number) => i
const y = (d: DataRecord) => d.amount
</script>
```

**Type Safety Features**:
- **Props Typing**: `defineProps<{ period: Period; range: Range }>()` ensures correct prop types
- **Local Types**: Component-specific types like `DataRecord` for internal data structures
- **Generic Refs**: `ref<DataRecord[]>` provides typed reactive data
- **Record Type Mapping**: Type-safe function mapping based on Period enum

### 4. Component Composition Patterns

#### Example: `MembersList.vue`

```typescript
<script setup lang="ts">
import type { DropdownMenuItem } from '@nuxt/ui'
import type { Member } from '~/types'

// Typed props with interface
defineProps<{
  members: Member[]
}>()

// Type-safe menu items with const assertion
const items = [{
  label: 'Edit member',
  onSelect: () => console.log('Edit member')
}, {
  label: 'Remove member',
  color: 'error' as const,  // Const assertion for literal type
  onSelect: () => console.log('Remove member')
}] satisfies DropdownMenuItem[]  // Type validation
</script>
```

**Type Safety Features**:
- **Interface Props**: Direct use of application interfaces in props
- **Satisfies Operator**: Type checking without widening types
- **Const Assertions**: Preserving literal types for color values

## Nuxt UI Pro Integration Patterns

### 1. Component Resolution

```typescript
// Explicit component resolution for better type safety
const UAvatar = resolveComponent('UAvatar')
const UButton = resolveComponent('UButton')
const UBadge = resolveComponent('UBadge')
const UDropdownMenu = resolveComponent('UDropdownMenu')
const UCheckbox = resolveComponent('UCheckbox')
```

**Benefits**:
- Explicit component imports for better tree-shaking
- Type checking for component availability
- Clear dependency declaration

### 2. Render Function Typing

```typescript
// Type-safe render function with component typing
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
```

### 3. Template Ref Typing

```typescript
// Generic template refs with proper typing
const cardRef = useTemplateRef<HTMLElement | null>('cardRef')
const table = useTemplateRef('table')

// Usage with type safety
const { width } = useElementSize(cardRef)  // Type-safe element size hook
```

### 4. UI Configuration Typing

```typescript
// Type-safe UI configuration objects
:ui="{
  root: 'overflow-visible',
  body: '!px-0 !pt-0 !pb-3'
}"

// Table UI configuration
:ui="{
  base: 'table-fixed border-separate border-spacing-0',
  thead: '[&>tr]:bg-elevated/50 [&>tr]:after:content-none',
  tbody: '[&>tr]:last:[&>td]:border-b-0',
  th: 'py-2 first:rounded-l-lg last:rounded-r-lg border-y border-default first:border-l last:border-r',
  td: 'border-b border-default'
}"
```

## Advanced Type Patterns

### 1. Discriminated Unions in Templates

```typescript
// Status-based styling with type safety
<template>
  <UBadge 
    :color="{
      subscribed: 'success',
      unsubscribed: 'error', 
      bounced: 'warning'
    }[user.status]"
    class="capitalize"
  >
    {{ user.status }}
  </UBadge>
</template>
```

### 2. Generic Component Props

```typescript
// Generic composable usage
const { width } = useElementSize(cardRef)
const { data, status } = await useFetch<User[]>('/api/customers', {
  lazy: true
})
```

### 3. Event Handler Type Safety

```typescript
// Type-safe event handlers with proper parameter typing
@update:page="(p: number) => table?.tableApi?.setPageIndex(p - 1)"
@update:model-value="table?.tableApi?.getColumn('email')?.setFilterValue($event)"
```

### 4. Conditional Rendering with Types

```typescript
// Type-safe conditional logic
<UButton
  v-if="table?.tableApi?.getFilteredSelectedRowModel().rows.length"
  :label="`Delete (${table?.tableApi?.getFilteredSelectedRowModel().rows.length})`"
  color="error"
  variant="subtle"
/>
```

## Form Validation Integration

### 1. Zod Schema Integration

```typescript
// Schema definition with TypeScript integration
const schema = z.object({
  name: z.string().min(2, 'Too short'),
  email: z.string().email('Invalid email')
})

// TypeScript type extraction
type Schema = z.output<typeof schema>

// Reactive state with partial typing
const state = reactive<Partial<Schema>>({
  name: undefined,
  email: undefined
})
```

### 2. Form Event Typing

```typescript
// Type-safe form submission
async function onSubmit(event: FormSubmitEvent<Schema>) {
  // event.data is fully typed
  const { name, email } = event.data
  // Both name and email are properly typed strings
}
```

## State Management Patterns

### 1. Reactive State with Interfaces

```typescript
// Typed reactive references
const columnFilters = ref([{
  id: 'email',
  value: ''
}])

const pagination = ref({
  pageIndex: 0,
  pageSize: 10
})
```

### 2. Computed Properties with Types

```typescript
// Type-safe computed properties
const total = computed(() => 
  data.value.reduce((acc: number, { amount }) => acc + amount, 0)
)
```

### 3. Watcher Type Safety

```typescript
// Type-safe watchers with proper dependency tracking
watch(() => statusFilter.value, (newVal) => {
  if (!table?.value?.tableApi) return
  
  const statusColumn = table.value.tableApi.getColumn('status')
  if (!statusColumn) return
  
  if (newVal === 'all') {
    statusColumn.setFilterValue(undefined)
  } else {
    statusColumn.setFilterValue(newVal)
  }
})
```

## Best Practices Demonstrated

### 1. Type-First Development

- Define interfaces before implementation
- Use schema-first approach for forms
- Leverage TypeScript's inference capabilities

### 2. Generic Type Usage

- Use generics for reusable components
- Preserve type information across boundaries
- Leverage library-provided generic types

### 3. Runtime Safety

- Combine TypeScript with runtime validation
- Use type guards for dynamic content
- Implement proper error handling

### 4. Component Integration

- Explicit component resolution
- Type-safe prop passing
- Proper event handler typing

### 5. Performance Considerations

- Use type-only imports where possible
- Leverage const assertions for literals
- Minimize runtime type checking

## Future Enhancements

### 1. Enhanced Type Safety

Consider implementing:
- Branded types for IDs and special values
- Result types for error handling
- More specific event handler types

### 2. Component Library Integration

Potential improvements:
- Custom type definitions for component props
- Enhanced generic component patterns
- Better integration with Nuxt UI Pro types

### 3. Development Experience

Future considerations:
- Auto-generated types from API schemas
- Enhanced IntelliSense support
- Better debugging with TypeScript

This comprehensive type system demonstrates excellent integration between Vue 3, TypeScript, and Nuxt UI Pro, providing a robust foundation for scalable dashboard applications.
