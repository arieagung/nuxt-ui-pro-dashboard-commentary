# HomeChart Client Component

## Purpose
The `HomeChart.client.vue` component provides an interactive revenue chart with line and area visualizations. It displays financial data over different time periods with responsive design and real-time updates based on selected date ranges and periods.

## Reusability
This component is highly reusable for any dashboard that needs time-series data visualization. The period/range-based data generation and chart configuration make it adaptable for various metrics beyond revenue.

## Props
- `period: Period` - The time period granularity ('daily', 'weekly', 'monthly')
- `range: Range` - The date range object with start and end dates

## Emits
- None

## Slots
- `header` - Contains the revenue title and total display

## Nuxt UI Pro Integration
This component utilizes:
- `UCard` - Main container with custom styling overrides
- Integrates with Nuxt UI Pro design tokens and CSS variables
- Uses UI Pro color system for chart theming

## Usage Examples
```vue
<!-- Basic usage -->
<HomeChartClient :period="'daily'" :range="{ start: startDate, end: endDate }" />

<!-- In a dashboard layout -->
<div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
  <HomeChartClient :period="selectedPeriod" :range="selectedRange" />
  <OtherChartComponent />
</div>

<!-- With reactive period/range -->
<HomeChartClient 
  :period="dashboardStore.period" 
  :range="dashboardStore.dateRange" 
/>
```

## Component Relationships
- Uses `@unovis/vue` for chart visualization components
- Integrates with `date-fns` for date manipulation
- Uses `@vueuse/core` for element size tracking
- Follows the client/server component pattern with corresponding server component

## State Management
- Reactive data generation based on props changes
- Element size tracking for responsive chart sizing
- Local state management for chart data and calculations

## Key Features

### Data Visualization
- **Line Chart**: Primary trend visualization with smooth curves
- **Area Chart**: Secondary fill visualization with opacity
- **Crosshair**: Interactive hover details
- **Tooltip**: Contextual data display on hover

### Responsive Design
- **Element Size Tracking**: Charts adapt to container size changes
- **Responsive Layout**: Card styling adapts to screen size
- **Dynamic Width**: Chart width automatically adjusts

### Date/Period Handling
```typescript
const dates = ({
  daily: eachDayOfInterval,
  weekly: eachWeekOfInterval,
  monthly: eachMonthOfInterval
})[props.period](props.range)
```

### Data Generation
- **Random Data**: Generates realistic revenue data for demonstration
- **Date-based**: Each data point has corresponding date
- **Configurable Range**: Min/max values easily adjustable

### Chart Styling
```vue
<style scoped>
.unovis-xy-container {
  --vis-crosshair-line-stroke-color: var(--ui-primary);
  --vis-crosshair-circle-stroke-color: var(--ui-bg);
  --vis-axis-grid-color: var(--ui-border);
  --vis-axis-tick-color: var(--ui-border);
  --vis-axis-tick-label-color: var(--ui-text-dimmed);
  --vis-tooltip-background-color: var(--ui-bg);
  --vis-tooltip-border-color: var(--ui-border);
  --vis-tooltip-text-color: var(--ui-text-highlighted);
}
</style>
```

## Advanced Features

### Currency Formatting
- **Intl.NumberFormat**: Proper currency display with USD formatting
- **No Decimal Places**: Clean integer display for large amounts

### Date Formatting
- **Dynamic Format**: Changes based on period (daily vs monthly)
- **Locale-aware**: Uses consistent date formatting throughout

### Accessibility
- **ARIA Labels**: Proper chart accessibility
- **Keyboard Navigation**: Chart interaction via keyboard
- **Screen Reader**: Meaningful data descriptions

## Extension Points
This component can be enhanced with:
- Real API data integration
- Multiple data series support
- Export functionality (PDF, PNG, CSV)
- Zoom and pan interactions
- Custom color schemes
- Animation transitions
- Data comparison features

## Performance Considerations
- **Client-side Rendering**: Uses `.client.vue` suffix for client-only rendering
- **Computed Properties**: Efficient reactive calculations
- **Element Size Observer**: Optimized resize handling
- **Memoized Calculations**: Prevents unnecessary recalculations

## Best Practices Demonstrated
- **Separation of Concerns**: Chart logic separate from data fetching
- **Reactive Programming**: Proper Vue 3 Composition API usage
- **Type Safety**: Full TypeScript integration
- **Responsive Design**: Container-query-like behavior
- **Performance**: Client-side rendering for heavy visualizations
