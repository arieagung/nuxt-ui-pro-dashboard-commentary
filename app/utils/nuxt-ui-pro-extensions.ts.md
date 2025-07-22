# Nuxt UI Pro Extensions and Custom Utilities

This file documents custom utilities and extensions specifically designed to enhance Nuxt UI Pro components and functionality within the dashboard application.

## Overview

The Nuxt UI Pro extensions provide specialized utilities that:

- Extend native Nuxt UI Pro component functionality
- Provide consistent theming and styling patterns
- Enhance component configuration with reusable presets
- Create custom composite components using Nuxt UI Pro primitives
- Standardize data presentation patterns

## Component Extensions

### 1. UTable Enhancements

#### Table Column Factory Functions

```typescript
/**
 * Creates a standardized ID column configuration
 * @param options - Configuration options
 * @returns UTable column definition
 */
export function createIdColumn(options: {
  accessorKey?: string
  prefix?: string
  header?: string
} = {}): TableColumn<any> {
  const { accessorKey = 'id', prefix = '#', header = 'ID' } = options
  
  return {
    accessorKey,
    header,
    cell: ({ row }) => formatId(row.getValue(accessorKey), prefix)
  }
}

/**
 * Creates a standardized date column configuration
 * @param options - Configuration options
 * @returns UTable column definition
 */
export function createDateColumn(options: {
  accessorKey?: string
  header?: string
  formatter?: (date: Date | string) => string
} = {}): TableColumn<any> {
  const { accessorKey = 'date', header = 'Date', formatter = formatTableDate } = options
  
  return {
    accessorKey,
    header,
    cell: ({ row }) => formatter(row.getValue(accessorKey))
  }
}

/**
 * Creates a standardized status badge column
 * @param options - Configuration options
 * @returns UTable column definition with UBadge integration
 */
export function createStatusColumn(options: {
  accessorKey?: string
  header?: string
  colorMap?: Record<string, BadgeColor>
  variant?: BadgeVariant
} = {}): TableColumn<any> {
  const { 
    accessorKey = 'status', 
    header = 'Status',
    colorMap,
    variant = 'subtle'
  } = options
  
  return {
    accessorKey,
    header,
    cell: ({ row }) => {
      const status = row.getValue(accessorKey)
      const color = colorMap ? colorMap[status] : getStatusColor(status)
      
      return h(UBadge, {
        color,
        variant,
        class: 'capitalize'
      }, () => capitalize(status))
    }
  }
}

/**
 * Creates a standardized currency column
 * @param options - Configuration options
 * @returns UTable column definition with currency formatting
 */
export function createCurrencyColumn(options: {
  accessorKey?: string
  header?: string
  formatter?: (value: number) => string
  align?: 'left' | 'right' | 'center'
} = {}): TableColumn<any> {
  const { 
    accessorKey = 'amount', 
    header = 'Amount',
    formatter = formatCurrency,
    align = 'right'
  } = options
  
  return {
    accessorKey,
    header: align === 'right' 
      ? () => h('div', { class: 'text-right' }, header)
      : header,
    cell: ({ row }) => {
      const value = safeParseFloat(row.getValue(accessorKey))
      const formatted = formatter(value)
      
      return h('div', { 
        class: `font-medium ${align === 'right' ? 'text-right' : ''}`
      }, formatted)
    }
  }
}
```

#### Usage Examples

```vue
<script setup>
// Create standardized table columns
const columns = [
  createIdColumn({ prefix: 'ORD-' }),
  createDateColumn({ header: 'Created' }),
  createStatusColumn({ 
    colorMap: { 
      shipped: 'primary',
      delivered: 'success',
      cancelled: 'error' 
    }
  }),
  createCurrencyColumn({ header: 'Total' })
]
</script>

<template>
  <UTable :data="orders" :columns="columns" />
</template>
```

### 2. UCard Presets

#### Dashboard Card Configurations

```typescript
/**
 * Standard dashboard stat card configuration
 */
export const dashboardStatCardConfig = {
  ui: {
    container: 'gap-y-1.5',
    wrapper: 'items-start',
    leading: 'p-2.5 rounded-full bg-primary/10 ring ring-inset ring-primary/25 flex-col',
    title: 'font-normal text-muted text-xs uppercase'
  }
}

/**
 * Metric card with variation indicator
 */
export const metricCardConfig = {
  ui: {
    body: 'flex items-center justify-between',
    header: 'pb-2'
  }
}

/**
 * Chart container card configuration
 */
export const chartCardConfig = {
  ui: {
    root: 'overflow-visible',
    body: '!px-0 !pt-0 !pb-3'
  }
}
```

#### Usage with UPageCard

```vue
<template>
  <!-- Stat Card -->
  <UPageCard
    :icon="stat.icon"
    :title="stat.title"
    :ui="dashboardStatCardConfig.ui"
    variant="subtle"
  >
    <div class="flex items-center gap-2">
      <span class="text-2xl font-semibold text-highlighted">
        {{ formatCurrency(stat.value) }}
      </span>
      <UBadge
        :color="stat.variation > 0 ? 'success' : 'error'"
        variant="subtle"
      >
        {{ stat.variation > 0 ? '+' : '' }}{{ stat.variation }}%
      </UBadge>
    </div>
  </UPageCard>

  <!-- Chart Card -->
  <UCard :ui="chartCardConfig.ui">
    <template #header>
      <div>
        <p class="text-xs text-muted uppercase mb-1.5">Revenue</p>
        <p class="text-3xl text-highlighted font-semibold">
          {{ formatCurrency(totalRevenue) }}
        </p>
      </div>
    </template>
    
    <!-- Chart content -->
    <VisXYContainer>
      <!-- Chart components -->
    </VisXYContainer>
  </UCard>
</template>
```

### 3. UBadge Extensions

#### Status Badge Factory

```typescript
/**
 * Creates a status badge with consistent styling
 * @param status - Status string
 * @param options - Badge configuration options
 * @returns VNode for UBadge component
 */
export function createStatusBadge(
  status: string,
  options: {
    variant?: BadgeVariant
    size?: BadgeSize
    colorMap?: Record<string, BadgeColor>
    showIcon?: boolean
  } = {}
) {
  const {
    variant = 'subtle',
    size = 'sm',
    colorMap,
    showIcon = false
  } = options

  const color = colorMap ? colorMap[status.toLowerCase()] : getStatusColor(status)
  const displayText = capitalize(status)

  const props: any = {
    color,
    variant,
    size,
    class: 'capitalize'
  }

  if (showIcon) {
    const iconMap: Record<BadgeColor, string> = {
      success: 'i-lucide-check-circle',
      error: 'i-lucide-x-circle',
      warning: 'i-lucide-alert-circle',
      neutral: 'i-lucide-minus-circle',
      primary: 'i-lucide-circle'
    }
    props.icon = iconMap[color]
  }

  return h(UBadge, props, () => displayText)
}

/**
 * Predefined badge configurations for common use cases
 */
export const badgePresets = {
  orderStatus: {
    colorMap: {
      pending: 'warning' as const,
      processing: 'primary' as const,
      shipped: 'primary' as const,
      delivered: 'success' as const,
      cancelled: 'error' as const,
      refunded: 'neutral' as const
    }
  },
  userStatus: {
    colorMap: {
      active: 'success' as const,
      inactive: 'neutral' as const,
      suspended: 'error' as const,
      pending: 'warning' as const
    }
  },
  paymentStatus: {
    colorMap: {
      paid: 'success' as const,
      pending: 'warning' as const,
      failed: 'error' as const,
      refunded: 'neutral' as const
    }
  }
}
```

#### Usage Examples

```vue
<template>
  <!-- Using status badge factory -->
  <component 
    :is="createStatusBadge(order.status, { 
      ...badgePresets.orderStatus,
      showIcon: true 
    })" 
  />

  <!-- In table cell -->
  <UTable :columns="columnsWithBadges" :data="data" />
</template>

<script setup>
const columnsWithBadges = [
  {
    accessorKey: 'status',
    header: 'Status',
    cell: ({ row }) => createStatusBadge(
      row.getValue('status'),
      badgePresets.orderStatus
    )
  }
]
</script>
```

### 4. Form Extensions

#### Form Field Factory Functions

```typescript
/**
 * Creates standardized form field configuration
 * @param options - Field configuration
 * @returns Form field object for UForm
 */
export function createFormField(options: {
  name: string
  label: string
  type?: 'text' | 'email' | 'password' | 'number' | 'select' | 'textarea'
  placeholder?: string
  required?: boolean
  validation?: any[]
  options?: Array<{ label: string; value: any }>
}) {
  const {
    name,
    label,
    type = 'text',
    placeholder,
    required = false,
    validation = [],
    options = []
  } = options

  const field: any = {
    name,
    label,
    type,
    placeholder: placeholder || `Enter ${label.toLowerCase()}...`
  }

  if (required) {
    field.required = true
    validation.push('required')
  }

  if (type === 'email') {
    validation.push('email')
  }

  if (type === 'select' && options.length > 0) {
    field.options = options
  }

  if (validation.length > 0) {
    field.validation = validation
  }

  return field
}

/**
 * Common form configurations
 */
export const formPresets = {
  userForm: [
    createFormField({ 
      name: 'name', 
      label: 'Full Name', 
      required: true 
    }),
    createFormField({ 
      name: 'email', 
      label: 'Email', 
      type: 'email', 
      required: true 
    }),
    createFormField({ 
      name: 'role', 
      label: 'Role', 
      type: 'select',
      required: true,
      options: [
        { label: 'Admin', value: 'admin' },
        { label: 'User', value: 'user' },
        { label: 'Manager', value: 'manager' }
      ]
    })
  ],
  
  orderForm: [
    createFormField({ 
      name: 'customerId', 
      label: 'Customer', 
      type: 'select',
      required: true 
    }),
    createFormField({ 
      name: 'amount', 
      label: 'Amount', 
      type: 'number', 
      required: true 
    }),
    createFormField({ 
      name: 'notes', 
      label: 'Notes', 
      type: 'textarea' 
    })
  ]
}
```

### 5. Navigation Extensions

#### Sidebar Navigation Builder

```typescript
/**
 * Creates navigation items with proper structure
 * @param items - Navigation configuration
 * @returns Processed navigation items
 */
export function createNavigation(items: Array<{
  label: string
  to?: string
  icon?: string
  badge?: string | number
  children?: Array<{
    label: string
    to: string
    badge?: string | number
  }>
}>): NavigationItem[] {
  return items.map(item => {
    const navItem: NavigationItem = {
      label: item.label,
      icon: item.icon,
      to: item.to
    }

    if (item.badge) {
      navItem.badge = String(item.badge)
    }

    if (item.children) {
      navItem.children = item.children.map(child => ({
        label: child.label,
        to: child.to,
        badge: child.badge ? String(child.badge) : undefined
      }))
    }

    return navItem
  })
}

/**
 * Dashboard navigation preset
 */
export const dashboardNavigation = createNavigation([
  {
    label: 'Dashboard',
    to: '/',
    icon: 'i-lucide-layout-dashboard'
  },
  {
    label: 'Inbox',
    to: '/inbox',
    icon: 'i-lucide-inbox',
    badge: 3
  },
  {
    label: 'Customers',
    to: '/customers',
    icon: 'i-lucide-users'
  },
  {
    label: 'Settings',
    to: '/settings',
    icon: 'i-lucide-settings',
    children: [
      { label: 'General', to: '/settings' },
      { label: 'Members', to: '/settings/members' },
      { label: 'Notifications', to: '/settings/notifications' },
      { label: 'Security', to: '/settings/security' }
    ]
  }
])
```

### 6. Data Display Extensions

#### Stat Grid Builder

```typescript
/**
 * Creates stat grid configuration
 * @param stats - Stat definitions
 * @param options - Grid options
 * @returns Processed stat items
 */
export function createStatGrid(
  stats: Array<{
    title: string
    icon: string
    getValue: () => number | string
    getVariation?: () => number
    formatter?: (value: number) => string
  }>,
  options: {
    refreshInterval?: number
    animated?: boolean
  } = {}
) {
  const { refreshInterval, animated = true } = options

  const processedStats = computed(() => 
    stats.map(stat => ({
      title: stat.title,
      icon: stat.icon,
      value: typeof stat.getValue() === 'number' && stat.formatter 
        ? stat.formatter(stat.getValue() as number)
        : stat.getValue(),
      variation: stat.getVariation ? stat.getVariation() : null,
      animated
    }))
  )

  if (refreshInterval) {
    const { pause, resume } = useIntervalFn(() => {
      // Trigger reactivity update
      stats.forEach(stat => stat.getValue())
    }, refreshInterval)

    onUnmounted(() => pause())
  }

  return {
    stats: processedStats,
    refresh: () => {
      // Force refresh all stats
      stats.forEach(stat => stat.getValue())
    }
  }
}
```

#### Usage Example

```vue
<template>
  <UPageGrid class="lg:grid-cols-4 gap-4">
    <UPageCard
      v-for="stat in statGrid.stats.value"
      :key="stat.title"
      :icon="stat.icon"
      :title="stat.title"
      :ui="dashboardStatCardConfig.ui"
      variant="subtle"
    >
      <div class="flex items-center gap-2">
        <span 
          class="text-2xl font-semibold text-highlighted"
          :class="{ 'animate-pulse': stat.animated }"
        >
          {{ stat.value }}
        </span>
        <UBadge
          v-if="stat.variation !== null"
          :color="stat.variation > 0 ? 'success' : 'error'"
          variant="subtle"
        >
          {{ stat.variation > 0 ? '+' : '' }}{{ stat.variation }}%
        </UBadge>
      </div>
    </UPageCard>
  </UPageGrid>
</template>

<script setup>
const statGrid = createStatGrid([
  {
    title: 'Total Revenue',
    icon: 'i-lucide-circle-dollar-sign',
    getValue: () => randomInt(200000, 500000),
    getVariation: () => randomInt(-20, 30),
    formatter: formatCurrency
  },
  {
    title: 'Active Users',
    icon: 'i-lucide-users',
    getValue: () => randomInt(1000, 5000),
    getVariation: () => randomInt(-10, 15)
  }
], {
  refreshInterval: 30000, // Refresh every 30 seconds
  animated: true
})
</script>
```

### 7. Chart Integration Helpers

#### Chart Configuration Factory

```typescript
/**
 * Creates standardized chart configurations
 */
export function createChartConfig(type: 'line' | 'area' | 'bar', options: {
  colors?: {
    primary?: string
    secondary?: string
    background?: string
  }
  responsive?: boolean
  tooltip?: boolean
  crosshair?: boolean
} = {}) {
  const {
    colors = {},
    responsive = true,
    tooltip = true,
    crosshair = true
  } = options

  const baseConfig = {
    colors: {
      primary: colors.primary || 'var(--ui-primary)',
      secondary: colors.secondary || 'var(--ui-primary-400)',
      background: colors.background || 'var(--ui-bg)',
      ...colors
    },
    responsive,
    tooltip,
    crosshair
  }

  const typeConfigs = {
    line: {
      ...baseConfig,
      area: false,
      strokeWidth: 2
    },
    area: {
      ...baseConfig,
      area: true,
      opacity: 0.1
    },
    bar: {
      ...baseConfig,
      barWidth: 0.8
    }
  }

  return typeConfigs[type]
}

/**
 * Chart data processor for dashboard metrics
 */
export function processChartData<T>(
  rawData: T[],
  options: {
    xAccessor: (item: T) => any
    yAccessor: (item: T) => number
    dateFormat?: (date: Date) => string
    valueFormat?: (value: number) => string
    period?: 'daily' | 'weekly' | 'monthly'
  }
) {
  const {
    xAccessor,
    yAccessor,
    dateFormat,
    valueFormat = (v) => v.toString(),
    period = 'daily'
  } = options

  const processedData = rawData.map((item, index) => ({
    x: index,
    y: yAccessor(item),
    originalValue: item,
    formattedX: dateFormat && xAccessor(item) instanceof Date
      ? dateFormat(xAccessor(item))
      : formatChartDate(xAccessor(item), period),
    formattedY: valueFormat(yAccessor(item))
  }))

  return {
    data: processedData,
    total: processedData.reduce((sum, item) => sum + item.y, 0),
    average: processedData.reduce((sum, item) => sum + item.y, 0) / processedData.length,
    max: Math.max(...processedData.map(item => item.y)),
    min: Math.min(...processedData.map(item => item.y))
  }
}
```

### 8. Modal and Dialog Extensions

#### Confirmation Dialog Builder

```typescript
/**
 * Creates standardized confirmation dialogs
 */
export function useConfirmationDialog() {
  const isOpen = ref(false)
  const config = ref<{
    title: string
    message: string
    type: 'danger' | 'warning' | 'info'
    confirmLabel?: string
    cancelLabel?: string
    onConfirm: () => void | Promise<void>
  }>()

  const open = (dialogConfig: typeof config.value) => {
    config.value = {
      confirmLabel: 'Confirm',
      cancelLabel: 'Cancel',
      ...dialogConfig
    }
    isOpen.value = true
  }

  const close = () => {
    isOpen.value = false
    config.value = undefined
  }

  const confirm = async () => {
    if (config.value?.onConfirm) {
      await config.value.onConfirm()
    }
    close()
  }

  return {
    isOpen: readonly(isOpen),
    config: readonly(config),
    open,
    close,
    confirm
  }
}
```

#### Usage

```vue
<template>
  <div>
    <UButton @click="deleteItem" color="red">
      Delete Item
    </UButton>

    <!-- Confirmation Dialog -->
    <UModal v-model="confirmDialog.isOpen.value">
      <UCard>
        <template #header>
          <h3>{{ confirmDialog.config.value?.title }}</h3>
        </template>

        <p>{{ confirmDialog.config.value?.message }}</p>

        <template #footer>
          <div class="flex justify-end gap-2">
            <UButton 
              variant="ghost" 
              @click="confirmDialog.close"
            >
              {{ confirmDialog.config.value?.cancelLabel }}
            </UButton>
            <UButton 
              :color="confirmDialog.config.value?.type === 'danger' ? 'red' : 'primary'"
              @click="confirmDialog.confirm"
            >
              {{ confirmDialog.config.value?.confirmLabel }}
            </UButton>
          </div>
        </template>
      </UCard>
    </UModal>
  </div>
</template>

<script setup>
const confirmDialog = useConfirmationDialog()

const deleteItem = () => {
  confirmDialog.open({
    title: 'Delete Item',
    message: 'Are you sure you want to delete this item? This action cannot be undone.',
    type: 'danger',
    confirmLabel: 'Delete',
    onConfirm: async () => {
      // Perform delete operation
      await deleteItemAPI()
      // Show success notification
    }
  })
}
</script>
```

### 9. Notification System Extensions

#### Toast Notification Builder

```typescript
/**
 * Enhanced toast notifications with consistent styling
 */
export function useEnhancedToast() {
  const toast = useToast()

  const showSuccess = (message: string, options: {
    title?: string
    timeout?: number
    actions?: Array<{ label: string; click: () => void }>
  } = {}) => {
    toast.add({
      title: options.title || 'Success',
      description: message,
      color: 'green',
      icon: 'i-lucide-check-circle',
      timeout: options.timeout || 3000,
      actions: options.actions
    })
  }

  const showError = (message: string, options: {
    title?: string
    timeout?: number
    actions?: Array<{ label: string; click: () => void }>
  } = {}) => {
    toast.add({
      title: options.title || 'Error',
      description: message,
      color: 'red',
      icon: 'i-lucide-x-circle',
      timeout: options.timeout || 5000,
      actions: options.actions
    })
  }

  const showWarning = (message: string, options: {
    title?: string
    timeout?: number
    actions?: Array<{ label: string; click: () => void }>
  } = {}) => {
    toast.add({
      title: options.title || 'Warning',
      description: message,
      color: 'yellow',
      icon: 'i-lucide-alert-triangle',
      timeout: options.timeout || 4000,
      actions: options.actions
    })
  }

  const showInfo = (message: string, options: {
    title?: string
    timeout?: number
    actions?: Array<{ label: string; click: () => void }>
  } = {}) => {
    toast.add({
      title: options.title || 'Information',
      description: message,
      color: 'blue',
      icon: 'i-lucide-info',
      timeout: options.timeout || 3000,
      actions: options.actions
    })
  }

  return {
    success: showSuccess,
    error: showError,
    warning: showWarning,
    info: showInfo
  }
}
```

## Performance Optimizations

### 1. Component Caching

```typescript
/**
 * Cached component factories to avoid recreation
 */
const componentCache = new Map()

export function getCachedComponent(key: string, factory: () => any) {
  if (!componentCache.has(key)) {
    componentCache.set(key, factory())
  }
  return componentCache.get(key)
}

// Usage
const statusBadge = getCachedComponent(
  `status-${status}-${variant}`,
  () => createStatusBadge(status, { variant })
)
```

### 2. Reactive Configuration

```typescript
/**
 * Reactive UI configurations that respond to theme changes
 */
export function useReactiveUIConfig() {
  const colorMode = useColorMode()
  
  const cardConfig = computed(() => ({
    ui: {
      background: colorMode.value === 'dark' 
        ? 'bg-gray-900' 
        : 'bg-white',
      ring: colorMode.value === 'dark'
        ? 'ring-gray-800'
        : 'ring-gray-200'
    }
  }))

  return {
    cardConfig
  }
}
```

### 3. Bundle Size Optimization

```typescript
// Lazy-loaded component factories
export const lazyComponents = {
  statusBadge: () => import('./status-badge-factory'),
  dataTable: () => import('./data-table-factory'),
  chartConfig: () => import('./chart-config-factory')
}

// Dynamic imports for large utilities
export async function getAdvancedChartUtils() {
  const { createAdvancedChart } = await import('./advanced-chart-utils')
  return createAdvancedChart
}
```

## Integration Guidelines

### 1. Component Composition

```vue
<template>
  <div class="dashboard-layout">
    <!-- Navigation with extensions -->
    <NavigationSidebar :items="dashboardNavigation" />
    
    <!-- Main content with enhanced components -->
    <main class="p-6">
      <!-- Stats grid with factory -->
      <component :is="StatsGrid" :stats="statsConfig" />
      
      <!-- Enhanced data table -->
      <UTable 
        :columns="enhancedColumns" 
        :data="processedData"
        :loading="loading"
      />
    </main>

    <!-- Confirmation dialogs -->
    <ConfirmationDialog v-bind="confirmDialog" />
    
    <!-- Enhanced notifications -->
    <UNotifications />
  </div>
</template>

<script setup>
import {
  dashboardNavigation,
  createIdColumn,
  createStatusColumn,
  createCurrencyColumn,
  useConfirmationDialog,
  useEnhancedToast
} from '~/utils/nuxt-ui-pro-extensions'

// Enhanced table configuration
const enhancedColumns = [
  createIdColumn(),
  createDateColumn({ header: 'Created Date' }),
  createStatusColumn(badgePresets.orderStatus),
  createCurrencyColumn({ header: 'Total Amount' })
]

// Enhanced notifications
const toast = useEnhancedToast()

// Confirmation dialogs
const confirmDialog = useConfirmationDialog()
</script>
```

### 2. Theme Integration

```typescript
// Theme-aware extensions
export function useThemeAwareExtensions() {
  const { $colorMode } = useNuxtApp()
  
  const getThemeConfig = (baseConfig: any) => {
    return computed(() => ({
      ...baseConfig,
      ui: {
        ...baseConfig.ui,
        background: $colorMode.value === 'dark' 
          ? 'bg-gray-900/50' 
          : 'bg-white/50'
      }
    }))
  }

  return {
    getThemeConfig
  }
}
```

### 3. Type Safety

```typescript
// Strong typing for all extensions
export interface EnhancedTableColumn<T> extends TableColumn<T> {
  formatter?: (value: any) => string
  badge?: {
    colorMap?: Record<string, BadgeColor>
    variant?: BadgeVariant
  }
}

export interface DashboardConfig {
  navigation: NavigationItem[]
  stats: StatConfig[]
  tables: {
    [key: string]: {
      columns: EnhancedTableColumn<any>[]
      data: any[]
    }
  }
  theme: {
    primary: string
    mode: 'light' | 'dark' | 'auto'
  }
}
```

This comprehensive set of Nuxt UI Pro extensions provides a robust foundation for building consistent, performant, and maintainable dashboard components. All extensions are designed to work seamlessly with existing Nuxt UI Pro components while adding enhanced functionality and developer experience improvements.
