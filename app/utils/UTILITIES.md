# Dashboard Utility Functions Documentation

This document provides comprehensive documentation for all utility functions, helper methods, data transformation utilities, and custom extensions to Nuxt UI Pro functionality in the dashboard application.

## Overview

The utilities module provides a complete set of type-safe helper functions organized into logical categories:

- **Random Utilities**: For generating random data and mock content
- **Formatting Utilities**: For consistent number, currency, and date formatting
- **Data Transformation Utilities**: For transforming data between formats
- **Chart & Visualization Utilities**: Specialized functions for chart components
- **Date Range Utilities**: For date range calculations and formatting
- **Validation Utilities**: Type-safe validation functions
- **Array Utilities**: Advanced array manipulation functions

## Nuxt UI Pro Integration

All utilities are designed to work seamlessly with Nuxt UI Pro components, particularly:

- **UBadge**: Status color mapping functions
- **UTable**: Date and currency formatting for table cells
- **UCalendar**: Date range selection utilities
- **Chart Components**: Formatting functions for visualizations

## Function Categories

### 1. Random Utilities

#### `randomInt(min: number, max: number): number`

Generates random integers for mock data generation.

**Usage in Dashboard:**
```typescript
// Generate random user count for stats
const userCount = randomInt(400, 1000)

// Generate random revenue
const revenue = randomInt(200000, 500000)

// Generate random variation percentage
const variation = randomInt(-20, 30)
```

**Component Integration:**
```vue
<script setup>
// In HomeStats.vue
const stats = baseStats.map((stat) => ({
  ...stat,
  value: randomInt(stat.minValue, stat.maxValue),
  variation: randomInt(stat.minVariation, stat.maxVariation)
}))
</script>
```

#### `randomFrom<T>(array: T[]): T`

Type-safe random array element selection with full type preservation.

**Usage Examples:**
```typescript
// Random status selection with type safety
const statuses: UserStatus[] = ['subscribed', 'unsubscribed', 'bounced']
const userStatus = randomFrom(statuses) // Type: UserStatus

// Random email selection
const sampleEmails = ['user1@example.com', 'user2@example.com']
const randomEmail = randomFrom(sampleEmails) // Type: string

// Random sale status for mock data
const saleStatuses: SaleStatus[] = ['paid', 'failed', 'refunded']
const saleStatus = randomFrom(saleStatuses) // Type: SaleStatus
```

### 2. Formatting Utilities

#### `formatCurrency(value: number): string`

Primary currency formatter for USD with no decimals (extracted from HomeStats.vue).

**Usage:**
```typescript
// Format revenue display
const revenue = 245000
const formatted = formatCurrency(revenue) // "$245,000"

// In component template
<span>{{ formatCurrency(stat.value) }}</span>
```

**Component Integration:**
```vue
<template>
  <UPageCard>
    <span class="text-2xl font-semibold">
      {{ formatCurrency(revenue) }}
    </span>
  </UPageCard>
</template>
```

#### `formatCurrencyEUR(value: number, currency?: string, locale?: string): string`

Flexible currency formatter (extracted from HomeSales.vue) with configurable options.

**Usage:**
```typescript
// Format EUR amounts in tables
const amount = 1500
const formatted = formatCurrencyEUR(amount) // "€1,500.00"

// Custom currency and locale
const usdAmount = formatCurrencyEUR(1500, 'USD', 'en-US') // "$1,500.00"
```

**Table Integration:**
```vue
<script setup>
// Table column definition
const columns = [
  {
    accessorKey: 'amount',
    cell: ({ row }) => {
      const amount = Number.parseFloat(row.getValue('amount'))
      return formatCurrencyEUR(amount)
    }
  }
]
</script>
```

#### `createNumberFormatter(options?: Intl.NumberFormatOptions)`

Factory function for creating reusable formatters (inspired by HomeChart.client.vue).

**Usage:**
```typescript
// Create specialized formatters
const currencyFormatter = createNumberFormatter({
  style: 'currency',
  currency: 'USD',
  maximumFractionDigits: 0
})

const percentFormatter = createNumberFormatter({
  style: 'percent',
  minimumFractionDigits: 1
})

// Usage in components
const formattedRevenue = currencyFormatter(125000) // "$125,000"
const formattedGrowth = percentFormatter(0.15) // "15.0%"
```

#### `formatTableDate(date: Date | string): string`

Date formatter for table display (extracted from HomeSales.vue).

**Usage:**
```typescript
// Format dates in tables
const saleDate = new Date()
const formatted = formatTableDate(saleDate) // "Dec 15, 14:30"

// Handle string dates
const stringDate = "2024-01-15T10:30:00Z"
const formatted = formatTableDate(stringDate) // "Jan 15, 10:30"
```

**Table Integration:**
```vue
<script setup>
const columns = [
  {
    accessorKey: 'date',
    header: 'Date',
    cell: ({ row }) => formatTableDate(row.getValue('date'))
  }
]
</script>
```

#### `createDateFormatter(locale?: string)`

Factory for consistent date formatting across the application.

**Usage:**
```typescript
// Create formatter for date range picker
const df = createDateFormatter('en-US')
const formattedDate = df.format(new Date()) // "Jan 15, 2024"

// Different locales
const germanFormatter = createDateFormatter('de-DE')
const germanDate = germanFormatter.format(new Date()) // "15. Jan. 2024"
```

### 3. Data Transformation Utilities

#### `getStatusColor(status: string): BadgeColor`

Maps status strings to Nuxt UI Pro badge colors (extracted from HomeSales.vue status mapping).

**Usage:**
```typescript
// Transform status to badge color
const paymentStatus = 'paid'
const badgeColor = getStatusColor(paymentStatus) // 'success'

// Handle various status types
const userStatus = 'unsubscribed'
const color = getStatusColor(userStatus) // 'error'
```

**UBadge Integration:**
```vue
<template>
  <UBadge 
    :color="getStatusColor(sale.status)"
    variant="subtle"
    class="capitalize"
  >
    {{ sale.status }}
  </UBadge>
</template>

<script setup>
import { getStatusColor } from '~/utils'
</script>
```

**Status Mappings:**
- **Success**: `paid`, `active`, `subscribed`, `completed`
- **Error**: `failed`, `cancelled`, `unsubscribed`, `bounced`
- **Warning**: `pending`, `processing`
- **Neutral**: `refunded`, `draft`
- **Primary**: `published`

#### `capitalize(str: string): string`

String capitalization utility for consistent text formatting.

**Usage:**
```typescript
// Capitalize status text
const status = 'pending'
const displayStatus = capitalize(status) // 'Pending'

// Use in templates
<UBadge class="capitalize">{{ capitalize(status) }}</UBadge>
```

#### `formatId(id: string | number, prefix?: string): string`

ID formatting with prefixes (extracted from HomeSales.vue ID column).

**Usage:**
```typescript
// Format sale IDs
const saleId = 4601
const formatted = formatId(saleId) // "#4601"

// Custom prefix
const userId = formatId(123, 'USER-') // "USER-123"
```

**Table Integration:**
```vue
<script setup>
const columns = [
  {
    accessorKey: 'id',
    header: 'ID',
    cell: ({ row }) => formatId(row.getValue('id'))
  }
]
</script>
```

#### `safeParseFloat(value: any, fallback?: number): number`

Safe number parsing with fallback values.

**Usage:**
```typescript
// Parse potentially invalid numbers
const amount = safeParseFloat("123.45") // 123.45
const invalid = safeParseFloat("invalid", 0) // 0

// Use in data processing
const processedAmount = safeParseFloat(row.getValue('amount'), 0)
```

### 4. Chart & Visualization Utilities

#### `formatChartDate(date: Date, period: Period): string`

Period-aware date formatting for charts (extracted from HomeChart.client.vue).

**Usage:**
```typescript
// Format dates for different chart periods
const date = new Date()

const dailyFormat = formatChartDate(date, 'daily') // "15 Dec"
const weeklyFormat = formatChartDate(date, 'weekly') // "15 Dec"
const monthlyFormat = formatChartDate(date, 'monthly') // "Dec 2024"
```

**Chart Integration:**
```vue
<script setup>
// X-axis tick formatter
const xTicks = (i: number) => {
  if (i === 0 || i === data.value.length - 1) return ''
  return formatChartDate(data.value[i].date, period)
}
</script>
```

#### `createChartTooltip(date: Date, value: number, formatter?: Function): string`

Chart tooltip template generator.

**Usage:**
```typescript
// Create tooltip with currency formatting
const tooltipText = createChartTooltip(
  new Date(),
  125000,
  formatCurrency
) // "15 Dec: $125,000"

// Simple tooltip without formatter
const simpleTooltip = createChartTooltip(
  new Date(),
  42
) // "15 Dec: 42"
```

**Chart Component Usage:**
```vue
<script setup>
const template = (d: DataRecord) => 
  createChartTooltip(d.date, d.amount, formatCurrency)
</script>

<template>
  <VisCrosshair :template="template" />
</template>
```

### 5. Date Range Utilities

#### `dateToCalendarDate(date: Date)`

Converts Date to calendar component format (extracted from HomeDateRangePicker.vue).

**Usage:**
```typescript
// Convert for calendar components
const date = new Date()
const calendarDate = dateToCalendarDate(date)
// { year: 2024, month: 1, day: 15 }
```

**Calendar Integration:**
```vue
<script setup>
// Convert between Date and CalendarDate
const calendarRange = computed({
  get: () => ({
    start: selected.value.start ? dateToCalendarDate(selected.value.start) : undefined,
    end: selected.value.end ? dateToCalendarDate(selected.value.end) : undefined
  }),
  set: (newValue) => {
    // Handle calendar date changes
  }
})
</script>
```

#### `createDateRanges()`

Predefined date range options for filters (extracted from HomeDateRangePicker.vue).

**Usage:**
```typescript
// Get standard date range options
const ranges = createDateRanges()
/*
[
  { label: 'Last 7 days', days: 7 },
  { label: 'Last 14 days', days: 14 },
  { label: 'Last 30 days', days: 30 },
  { label: 'Last 3 months', months: 3 },
  { label: 'Last 6 months', months: 6 },
  { label: 'Last year', years: 1 }
]
*/
```

**Component Integration:**
```vue
<template>
  <UButton
    v-for="range in dateRanges"
    :key="range.label"
    :label="range.label"
    @click="selectRange(range)"
  />
</template>

<script setup>
const dateRanges = createDateRanges()
</script>
```

#### `calculateDateRange(offset, fromDate?): { start: Date, end: Date }`

Calculates date ranges from time offsets.

**Usage:**
```typescript
// Calculate last 30 days
const last30Days = calculateDateRange({ days: 30 })
// { start: Date (30 days ago), end: Date (today) }

// Calculate last 3 months from specific date
const customRange = calculateDateRange(
  { months: 3 },
  new Date('2024-01-15')
)
```

### 6. Validation Utilities

#### `isInRange(value: number, min: number, max: number): boolean`

Validates numeric ranges with NaN handling.

**Usage:**
```typescript
// Validate user input
const isValidAge = isInRange(25, 0, 120) // true
const isValidPrice = isInRange(NaN, 0, 1000) // false

// Form validation
const validateAmount = (amount: number) => {
  return isInRange(amount, 1, 10000)
}
```

#### `isValidDate(date: any): date is Date`

Type guard for date validation.

**Usage:**
```typescript
// Type-safe date checking
const userInput = "2024-01-15"
const dateObj = new Date(userInput)

if (isValidDate(dateObj)) {
  // TypeScript knows dateObj is a valid Date
  console.log(dateObj.getFullYear()) // Safe to use Date methods
}
```

#### `isValidEmail(email: string): boolean`

Email format validation.

**Usage:**
```typescript
// Form validation
const validateEmail = (email: string) => {
  return isValidEmail(email) // true for valid@email.com
}

// Component validation
const isEmailValid = computed(() => 
  isValidEmail(form.email)
)
```

### 7. Array Utilities

#### `groupBy<T, K>(array: T[], keyFn: (item: T) => K): Record<K, T[]>`

Groups array elements by key function.

**Usage:**
```typescript
// Group sales by status
const sales: Sale[] = [...]
const groupedSales = groupBy(sales, sale => sale.status)
/*
{
  paid: [Sale, Sale, ...],
  failed: [Sale, ...],
  refunded: [Sale, ...]
}
*/

// Group users by location
const users: User[] = [...]
const usersByLocation = groupBy(users, user => user.location)
```

**Component Usage:**
```vue
<script setup>
// Group notifications by type
const notifications = ref<Notification[]>([...])

const groupedNotifications = computed(() => 
  groupBy(notifications.value, n => n.type)
)
</script>

<template>
  <div v-for="(group, type) in groupedNotifications" :key="type">
    <h3>{{ type }}</h3>
    <div v-for="notification in group" :key="notification.id">
      {{ notification.message }}
    </div>
  </div>
</template>
```

#### `sortBy<T>(array: T[], keyFn: (item: T) => any, direction?: 'asc' | 'desc'): T[]`

Sorts arrays by key function with direction control.

**Usage:**
```typescript
// Sort sales by date (newest first)
const sortedSales = sortBy(sales, sale => new Date(sale.date), 'desc')

// Sort users by name
const sortedUsers = sortBy(users, user => user.name, 'asc')

// Sort stats by value
const sortedStats = sortBy(stats, stat => stat.value, 'desc')
```

**Table Sorting:**
```vue
<script setup>
const sortedData = computed(() => {
  if (!sortField.value) return data.value
  
  return sortBy(
    data.value,
    item => item[sortField.value],
    sortDirection.value
  )
})
</script>
```

#### `debounce<T>(fn: T, delay: number): Function`

Creates debounced function for performance optimization.

**Usage:**
```typescript
// Debounced search
const search = ref('')
const debouncedSearch = debounce((query: string) => {
  // Perform search API call
  console.log('Searching for:', query)
}, 300)

watch(search, debouncedSearch)
```

**Component Integration:**
```vue
<script setup>
// Debounced filter function
const filterTerm = ref('')
const debouncedFilter = debounce((term: string) => {
  // Filter large dataset
  filteredData.value = data.value.filter(item => 
    item.name.toLowerCase().includes(term.toLowerCase())
  )
}, 250)

watch(filterTerm, debouncedFilter)
</script>

<template>
  <UInput 
    v-model="filterTerm" 
    placeholder="Search..." 
  />
</template>
```

## Integration Examples

### Complete Component Integration

Here's how to integrate multiple utilities in a typical dashboard component:

```vue
<template>
  <UCard>
    <template #header>
      <div class="flex justify-between items-center">
        <h2>Sales Overview</h2>
        <UButton @click="refreshData">
          Refresh
        </UButton>
      </div>
    </template>

    <!-- Stats Grid -->
    <UPageGrid class="mb-6">
      <UPageCard 
        v-for="stat in stats" 
        :key="stat.title"
        :title="stat.title"
        :icon="stat.icon"
      >
        <div class="flex items-center justify-between">
          <span class="text-2xl font-semibold">
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
    </UPageGrid>

    <!-- Sales Table -->
    <UTable 
      :data="sortedSales" 
      :columns="columns"
      :loading="loading"
    />
  </UCard>
</template>

<script setup>
import {
  randomInt,
  randomFrom,
  formatCurrency,
  formatTableDate,
  formatId,
  getStatusColor,
  sortBy,
  debounce
} from '~/utils'

// Generate mock stats
const stats = computed(() => [
  {
    title: 'Revenue',
    icon: 'i-lucide-circle-dollar-sign',
    value: randomInt(200000, 500000),
    variation: randomInt(-20, 30)
  },
  {
    title: 'Orders',
    icon: 'i-lucide-shopping-cart',
    value: randomInt(100, 300),
    variation: randomInt(-5, 15)
  }
])

// Generate mock sales data
const sales = ref(
  Array.from({ length: 10 }, (_, i) => ({
    id: (4600 - i).toString(),
    date: new Date(Date.now() - randomInt(0, 48) * 3600000).toISOString(),
    status: randomFrom(['paid', 'failed', 'refunded']),
    amount: randomInt(100, 1000)
  }))
)

// Sort sales by date (newest first)
const sortedSales = computed(() =>
  sortBy(sales.value, sale => new Date(sale.date), 'desc')
)

// Table columns with formatting
const columns = [
  {
    accessorKey: 'id',
    header: 'ID',
    cell: ({ row }) => formatId(row.getValue('id'))
  },
  {
    accessorKey: 'date',
    header: 'Date',
    cell: ({ row }) => formatTableDate(row.getValue('date'))
  },
  {
    accessorKey: 'status',
    header: 'Status',
    cell: ({ row }) => h(UBadge, {
      color: getStatusColor(row.getValue('status')),
      variant: 'subtle',
      class: 'capitalize'
    }, () => row.getValue('status'))
  },
  {
    accessorKey: 'amount',
    header: 'Amount',
    cell: ({ row }) => formatCurrency(row.getValue('amount'))
  }
]

// Debounced refresh function
const loading = ref(false)
const refreshData = debounce(async () => {
  loading.value = true
  // Simulate API call
  await new Promise(resolve => setTimeout(resolve, 1000))
  // Regenerate data...
  loading.value = false
}, 500)
</script>
```

### Chart Component Integration

```vue
<template>
  <UCard>
    <template #header>
      <div>
        <p class="text-xs text-muted uppercase mb-1.5">Revenue</p>
        <p class="text-3xl text-highlighted font-semibold">
          {{ formatCurrency(totalRevenue) }}
        </p>
      </div>
    </template>

    <VisXYContainer
      :data="chartData"
      :width="width"
      class="h-96"
    >
      <VisLine :x="x" :y="y" />
      <VisAxis
        type="x"
        :x="x"
        :tick-format="formatChartTicks"
      />
      <VisCrosshair
        :template="createTooltipTemplate"
      />
    </VisXYContainer>
  </UCard>
</template>

<script setup>
import {
  randomInt,
  formatCurrency,
  formatChartDate,
  createChartTooltip,
  createNumberFormatter
} from '~/utils'

const props = defineProps<{
  period: 'daily' | 'weekly' | 'monthly'
  range: { start: Date, end: Date }
}>()

// Generate chart data
const chartData = ref([])
const totalRevenue = computed(() =>
  chartData.value.reduce((acc, d) => acc + d.amount, 0)
)

// Chart accessors
const x = (d, i) => i
const y = d => d.amount

// Format chart ticks
const formatChartTicks = (i: number) => {
  if (i === 0 || i === chartData.value.length - 1) return ''
  const dataPoint = chartData.value[i]
  return dataPoint ? formatChartDate(dataPoint.date, props.period) : ''
}

// Create tooltip template
const createTooltipTemplate = (d) =>
  createChartTooltip(d.date, d.amount, formatCurrency)

// Watch for period/range changes
watch([() => props.period, () => props.range], () => {
  // Generate new data based on period and range
  chartData.value = generateChartData()
}, { immediate: true })

function generateChartData() {
  // Generate mock data using utilities
  return Array.from({ length: 30 }, (_, i) => ({
    date: new Date(Date.now() - i * 24 * 60 * 60 * 1000),
    amount: randomInt(1000, 10000)
  })).reverse()
}
</script>
```

## Performance Considerations

### Function Purity
All utilities are pure functions with no side effects, making them:
- Safe for concurrent usage
- Easily testable
- Cacheable by bundlers
- Optimizable by JavaScript engines

### Type Safety Benefits
- Compile-time error detection
- Better IDE IntelliSense
- Self-documenting code through types
- Reduced runtime type checking

### Memory Efficiency
- No unnecessary object creation in hot paths
- Efficient array operations using native methods
- Minimal closure allocations
- Proper garbage collection friendly patterns

### Bundle Optimization
- Tree-shakeable exports
- Small function footprints
- No external dependencies for core utilities
- Optimal TypeScript compilation output

## Testing Strategies

### Unit Testing Example
```typescript
import { describe, it, expect } from 'vitest'
import {
  randomInt,
  formatCurrency,
  getStatusColor,
  groupBy,
  isValidDate
} from '~/utils'

describe('Utility Functions', () => {
  describe('randomInt', () => {
    it('generates numbers within range', () => {
      const result = randomInt(5, 5)
      expect(result).toBe(5)
    })

    it('handles negative ranges', () => {
      const result = randomInt(-10, -5)
      expect(result).toBeGreaterThanOrEqual(-10)
      expect(result).toBeLessThanOrEqual(-5)
    })
  })

  describe('formatCurrency', () => {
    it('formats USD currency correctly', () => {
      expect(formatCurrency(1234)).toBe('$1,234')
      expect(formatCurrency(0)).toBe('$0')
    })
  })

  describe('getStatusColor', () => {
    it('maps status to correct colors', () => {
      expect(getStatusColor('paid')).toBe('success')
      expect(getStatusColor('failed')).toBe('error')
      expect(getStatusColor('unknown')).toBe('neutral')
    })

    it('handles case insensitive input', () => {
      expect(getStatusColor('PAID')).toBe('success')
      expect(getStatusColor('Failed')).toBe('error')
    })
  })

  describe('groupBy', () => {
    it('groups array elements correctly', () => {
      const data = [
        { name: 'John', status: 'active' },
        { name: 'Jane', status: 'inactive' },
        { name: 'Bob', status: 'active' }
      ]

      const grouped = groupBy(data, item => item.status)
      
      expect(grouped.active).toHaveLength(2)
      expect(grouped.inactive).toHaveLength(1)
    })
  })

  describe('isValidDate', () => {
    it('validates dates correctly', () => {
      expect(isValidDate(new Date())).toBe(true)
      expect(isValidDate(new Date('invalid'))).toBe(false)
      expect(isValidDate('2024-01-01')).toBe(false)
      expect(isValidDate(null)).toBe(false)
    })
  })
})
```

### Type Testing
```typescript
// Type-only tests to ensure correct TypeScript behavior
import type { Equal, Expect } from '@type-challenges/utils'
import { randomFrom, getStatusColor, groupBy } from '~/utils'

// Test type preservation in randomFrom
const stringArray = ['a', 'b', 'c']
const randomString = randomFrom(stringArray)
type TestRandomString = Expect<Equal<typeof randomString, string>>

const statusArray = ['paid', 'failed'] as const
const randomStatus = randomFrom(statusArray)
type TestRandomStatus = Expect<Equal<typeof randomStatus, 'paid' | 'failed'>>

// Test status color return type
const statusColor = getStatusColor('paid')
type TestStatusColor = Expect<Equal<typeof statusColor, 'success' | 'error' | 'neutral' | 'warning' | 'primary'>>

// Test groupBy type inference
const grouped = groupBy([{ id: 1, type: 'a' }], item => item.type)
type TestGroupBy = Expect<Equal<typeof grouped, Record<string, { id: number, type: string }[]>>>
```

## Best Practices

### 1. Import Strategy
```typescript
// Preferred: Named imports for tree-shaking
import { formatCurrency, getStatusColor } from '~/utils'

// Avoid: Default imports that prevent tree-shaking
import utils from '~/utils'
```

### 2. Function Composition
```typescript
// Compose utilities for complex operations
const processTableData = (rawData: RawSale[]) => {
  return sortBy(rawData, sale => new Date(sale.date), 'desc')
    .map(sale => ({
      ...sale,
      formattedDate: formatTableDate(sale.date),
      formattedAmount: formatCurrency(sale.amount),
      statusColor: getStatusColor(sale.status),
      displayId: formatId(sale.id)
    }))
}
```

### 3. Type Safety
```typescript
// Always use TypeScript's strict mode
// Prefer type guards over type assertions
if (isValidDate(inputDate)) {
  // Safe to use Date methods
  console.log(inputDate.getFullYear())
}

// Use generic constraints effectively
function processItems<T extends { id: number }>(items: T[]): T[] {
  return sortBy(items, item => item.id)
}
```

### 4. Performance Optimization
```typescript
// Create formatters once, reuse multiple times
const currencyFormatter = createNumberFormatter()

// Use in loops
const formattedPrices = prices.map(currencyFormatter)

// Debounce expensive operations
const expensiveOperation = debounce(heavyComputation, 300)
```

### 5. Error Handling
```typescript
// Use safe parsing for external data
const amount = safeParseFloat(externalData.amount, 0)

// Validate before processing
if (isValidEmail(userInput.email)) {
  await sendEmail(userInput.email)
}
```

## Future Enhancements

### 1. Internationalization Support
```typescript
// Planned: i18n-aware formatters
export function formatCurrencyI18n(
  value: number, 
  locale: string, 
  currency: string
): string {
  return new Intl.NumberFormat(locale, {
    style: 'currency',
    currency
  }).format(value)
}
```

### 2. Theme-Aware Color Utilities
```typescript
// Planned: Dynamic theme-based status colors
export function getStatusColorThemed(
  status: string, 
  theme: 'light' | 'dark'
): BadgeColor {
  // Implementation with theme awareness
}
```

### 3. Advanced Chart Utilities
```typescript
// Planned: Chart data transformation pipelines
export function createChartPipeline<T>() {
  return {
    data(source: T[]) { /* ... */ },
    filter(predicate: (item: T) => boolean) { /* ... */ },
    group(keyFn: (item: T) => string) { /* ... */ },
    format(formatters: FormatConfig) { /* ... */ },
    build(): ChartData { /* ... */ }
  }
}
```

### 4. Async Utilities
```typescript
// Planned: Promise-based utilities
export async function randomDelay(min: number, max: number): Promise<void> {
  const delay = randomInt(min, max)
  return new Promise(resolve => setTimeout(resolve, delay))
}
```

This comprehensive utility suite provides a robust foundation for the dashboard application, with excellent TypeScript support, Nuxt UI Pro integration, and performance optimizations. All functions are designed to be composable, testable, and maintainable while following modern JavaScript and TypeScript best practices.
