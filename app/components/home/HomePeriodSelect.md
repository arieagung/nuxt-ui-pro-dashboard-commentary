# HomePeriodSelect Component

## Purpose
The `HomePeriodSelect` component provides intelligent period granularity selection (daily, weekly, monthly) based on the selected date range. It automatically adapts available options based on the time span to ensure meaningful data visualization.

## Reusability
This component is highly reusable for any time-based data visualization where users need to adjust granularity. The smart period filtering logic makes it perfect for analytics dashboards, reporting tools, and data visualization interfaces.

## Props
- `range: Range` - The selected date range that determines available periods

## Emits
- None (uses `defineModel` for reactive two-way binding)

## Model
- `v-model: Period` - The selected time period ('daily', 'weekly', 'monthly')

## Slots
- None (uses USelect default slots)

## Nuxt UI Pro Integration
This component utilizes:
- `USelect` - Main dropdown selection component
- Integrates with Nuxt UI Pro styling system and design tokens
- Uses ghost variant for seamless dashboard integration

## Usage Examples
```vue
<!-- Basic usage with reactive date range -->
<HomePeriodSelect v-model="period" :range="dateRange" />

<!-- In complete dashboard controls -->
<template>
  <div class="flex items-center gap-4">
    <HomeDateRangePicker v-model="dateRange" />
    <HomePeriodSelect v-model="period" :range="dateRange" />
  </div>
  
  <HomeChart :period="period" :range="dateRange" />
  <HomeStats :period="period" :range="dateRange" />
</template>

<script setup>
const dateRange = ref({
  start: subDays(new Date(), 30),
  end: new Date()
})
const period = ref('daily')
</script>
```

## Component Relationships
- Integrates with `date-fns` for date calculations
- Works closely with `HomeDateRangePicker` for complete filtering
- Feeds period information to chart and analytics components
- Follows dashboard state management patterns

## State Management
- Uses `defineModel` for reactive two-way data binding
- Automatically validates and corrects invalid period selections
- Manages available periods based on date range calculations
- Ensures UI consistency when range changes

## Key Features

### Intelligent Period Filtering
```typescript
const periods = computed<Period[]>(() => {
  if (days.value.length <= 8) {
    return ['daily']
  }
  
  if (days.value.length <= 31) {
    return ['daily', 'weekly']
  }
  
  return ['weekly', 'monthly']
})
```

### Smart Period Validation
- **Auto-correction**: Automatically selects valid period when range changes
- **Logical Constraints**: Prevents meaningless period selections
- **Smooth Transitions**: Graceful handling of period changes

### Date Range Analysis
```typescript
const days = computed(() => eachDayOfInterval(props.range))
```

## Logic Rules

### Period Selection Rules
- **≤ 8 days**: Only 'daily' available
- **≤ 31 days**: 'daily' and 'weekly' available  
- **> 31 days**: 'weekly' and 'monthly' available

### User Experience Benefits
- **Prevents Clutter**: Hides irrelevant period options
- **Meaningful Data**: Ensures charts display useful granularity
- **Smart Defaults**: Automatically selects appropriate periods

## Advanced Features

### Reactive Period Updates
```typescript
watch(periods, () => {
  if (!periods.value.includes(model.value)) {
    model.value = periods.value[0]!
  }
})
```

### Styling Integration
- **Ghost Variant**: Seamless integration with dashboard design
- **Rotation Animation**: Smooth dropdown icon transitions
- **State Styling**: Visual feedback for open/closed states
- **Capitalized Text**: Proper text formatting for periods

## Accessibility Features
- **Keyboard Navigation**: Full keyboard support
- **Screen Reader**: Proper labeling and announcements
- **Focus Management**: Logical focus handling
- **ARIA Attributes**: Complete accessibility markup

## Extension Points
This component can be enhanced with:
- Custom period definitions (quarterly, yearly)
- Minimum/maximum period restrictions
- Period-specific configurations
- Custom period labels and descriptions
- Business calendar integration
- Multi-language period names

## Performance Considerations
- **Computed Properties**: Efficient period calculation
- **Minimal Recalculation**: Only updates when range changes
- **Lightweight Logic**: Fast period filtering algorithms
- **Memory Efficient**: No unnecessary data storage

## Integration Patterns

### Dashboard Integration
```vue
<template>
  <div class="dashboard-controls">
    <HomeDateRangePicker v-model="filters.range" />
    <HomePeriodSelect v-model="filters.period" :range="filters.range" />
  </div>
</template>

<script setup>
const filters = reactive({
  range: { start: new Date(), end: new Date() },
  period: 'daily'
})

// All components automatically update when filters change
</script>
```

## Best Practices Demonstrated
- **Smart Defaults**: Intelligent period selection based on data range
- **User Experience**: Prevents confusing or meaningless options
- **Reactive Design**: Seamless integration with other dashboard components
- **Type Safety**: Full TypeScript integration with proper types
- **Performance**: Efficient computed property usage for dynamic filtering
