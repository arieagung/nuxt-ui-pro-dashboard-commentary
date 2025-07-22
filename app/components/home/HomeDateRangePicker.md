# HomeDateRangePicker Component

## Purpose
The `HomeDateRangePicker` component provides a comprehensive date range selection interface with predefined range options and calendar-based selection. It's designed to filter dashboard data by date periods and supports both quick selections and custom date ranges.

## Reusability
This component is highly reusable for any dashboard or analytics interface requiring date range filtering. The predefined ranges and calendar integration make it suitable for various time-based data filtering scenarios.

## Props
- None (uses `defineModel` for v-model integration)

## Emits
- None (uses `defineModel` for reactive two-way binding)

## Model
- `v-model: Range` - The selected date range with start and end dates

## Slots
- `content` - Popover content containing range options and calendar
- `trailing` - Chevron icon that rotates on open/close

## Nuxt UI Pro Integration
This component utilizes several Nuxt UI Pro components:
- `UPopover` - Main popover container for date picker interface
- `UButton` - Date range display trigger button
- `UCalendar` - Interactive calendar for custom date selection
- Integrates with Nuxt UI Pro design tokens and styling system

## Usage Examples
```vue
<!-- Basic v-model usage -->
<HomeDateRangePicker v-model="selectedRange" />

<!-- In a dashboard with reactive data -->
<template>
  <div class="flex items-center gap-4">
    <HomeDateRangePicker v-model="dateRange" />
    <HomePeriodSelect v-model="period" :range="dateRange" />
  </div>
  
  <HomeChart :period="period" :range="dateRange" />
</template>

<script setup>
const dateRange = ref({ 
  start: new Date(), 
  end: new Date() 
})
const period = ref('daily')
</script>
```

## Component Relationships
- Integrates with `@internationalized/date` for robust date handling
- Works alongside `HomePeriodSelect` for complete time filtering
- Feeds date ranges to chart and stats components
- Uses international date formatting standards

## State Management
- Uses `defineModel` for reactive two-way data binding
- Manages calendar range state with computed properties
- Handles predefined range selection logic
- Synchronizes between button display and calendar selection

## Key Features

### Predefined Range Options
```typescript
const ranges = [
  { label: 'Last 7 days', days: 7 },
  { label: 'Last 14 days', days: 14 },
  { label: 'Last 30 days', days: 30 },
  { label: 'Last 3 months', months: 3 },
  { label: 'Last 6 months', months: 6 },
  { label: 'Last year', years: 1 }
]
```

### Custom Calendar Selection
- **Two-month View**: Side-by-side calendar months for easy range selection
- **Range Selection**: Visual range selection with start/end highlighting
- **International Standards**: Uses `@internationalized/date` for robust date handling

### Smart Date Formatting
- **Responsive Display**: Shows different date formats based on available space
- **Locale-aware**: Uses `DateFormatter` for consistent formatting
- **User-friendly**: Clear display of selected ranges

### Date Conversion Logic
```typescript
const toCalendarDate = (date: Date) => {
  return new CalendarDate(
    date.getFullYear(),
    date.getMonth() + 1,
    date.getDate()
  )
}
```

## Advanced Features

### Range Selection Logic
- **Smart Defaults**: Automatically selects reasonable date ranges
- **Validation**: Ensures valid start/end date combinations
- **Quick Selection**: One-click predefined range selection
- **Visual Feedback**: Active state for currently selected ranges

### Responsive Design
- **Mobile-friendly**: Adapts layout for mobile screens
- **Touch Support**: Full touch interaction support
- **Collision Detection**: Smart popover positioning

### Internationalization
- **Date Standards**: Uses international date handling libraries
- **Locale Support**: Adapts to user's locale settings
- **Timezone Handling**: Proper timezone conversion and display

## Accessibility Features
- **Keyboard Navigation**: Full keyboard accessibility
- **ARIA Labels**: Proper labeling for screen readers
- **Focus Management**: Logical tab order and focus handling
- **Date Announcements**: Screen reader announcements for date changes

## Extension Points
This component can be enhanced with:
- Custom predefined ranges
- Maximum/minimum date restrictions
- Holiday highlighting
- Timezone selection
- Custom date formats
- Business day filtering
- Recurring date patterns

## Performance Considerations
- **Computed Properties**: Efficient reactive calculations
- **Date Object Optimization**: Minimal date object creation
- **Calendar Rendering**: Efficient calendar month rendering
- **Memory Management**: Proper cleanup of date objects

## Best Practices Demonstrated
- **International Standards**: Proper use of international date libraries
- **Reactive Design**: Efficient two-way data binding
- **User Experience**: Intuitive date selection patterns
- **Type Safety**: Full TypeScript integration for date handling
- **Accessibility**: Complete accessibility implementation
