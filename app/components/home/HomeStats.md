# HomeStats Component

## Purpose
The `HomeStats` component displays key performance indicators (KPIs) in a responsive card grid layout. It shows important metrics like customers, conversions, revenue, and orders with trend indicators and interactive elements.

## Reusability
This component provides an excellent template for dashboard KPI displays. The stats configuration pattern, trend indicators, and responsive grid layout can be adapted for various dashboard metrics and business intelligence displays.

## Props
- `period: Period` - Time period for data filtering
- `range: Range` - Date range for data filtering

## Emits
- None

## Slots
- None (uses UPageCard structure)

## Nuxt UI Pro Integration
This component utilizes:
- `UPageGrid` - Responsive grid layout system
- `UPageCard` - Individual stat card components
- `UBadge` - Trend indicators with color coding
- Integrates with Nuxt UI Pro design tokens and styling

## Usage Examples
```vue
<!-- Basic usage -->
<HomeStats :period="'daily'" :range="{ start: startDate, end: endDate }" />

<!-- In dashboard layout -->
<div class="space-y-6">
  <HomeStats :period="period" :range="range" />
  <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
    <HomeChart :period="period" :range="range" />
    <HomeSales :period="period" :range="range" />
  </div>
</div>
```

## Component Relationships
- Uses `useAsyncData` for reactive data fetching
- Integrates with dashboard filtering system
- Links to related pages (like /customers)
- Follows dashboard navigation patterns

## State Management
- Reactive data generation based on props changes
- Automatic recalculation when period/range changes
- Simulated variation calculations for trend indicators

## Key Features

### Dynamic Statistics Configuration
```typescript
const baseStats = [{
  title: 'Customers',
  icon: 'i-lucide-users',
  minValue: 400,
  maxValue: 1000,
  minVariation: -15,
  maxVariation: 25
}, {
  title: 'Revenue',
  icon: 'i-lucide-circle-dollar-sign',
  minValue: 200000,
  maxValue: 500000,
  minVariation: -20,
  maxVariation: 30,
  formatter: formatCurrency
}]
```

### Currency Formatting
```typescript
function formatCurrency(value: number): string {
  return value.toLocaleString('en-US', {
    style: 'currency',
    currency: 'USD',
    maximumFractionDigits: 0
  })
}
```

### Trend Indicators
- **Color Coding**: Success/error colors for positive/negative trends
- **Percentage Display**: Clear percentage change indicators
- **Visual Hierarchy**: Prominent display of key metrics

### Responsive Grid Layout
```vue
<UPageGrid class="lg:grid-cols-4 gap-4 sm:gap-6 lg:gap-px">
  <UPageCard
    v-for="(stat, index) in stats"
    :key="index"
    class="lg:rounded-none first:rounded-l-lg last:rounded-r-lg hover:z-1"
  />
</UPageGrid>
```

## Advanced Features

### Smart Data Generation
- **Realistic Ranges**: Configurable min/max values for each metric
- **Trend Simulation**: Random but realistic percentage changes
- **Format Flexibility**: Support for different data formatters

### Interactive Elements
- **Navigation Links**: Cards link to relevant detail pages
- **Hover Effects**: Visual feedback with z-index elevation
- **Click Targets**: Full card clickable areas

### Professional Styling
- **Rounded Corners**: Seamless connected card appearance
- **Icon Integration**: Meaningful icons for each metric
- **Typography Hierarchy**: Clear title/value/trend organization

## Styling Features

### Custom Card Styling
```typescript
:ui="{
  container: 'gap-y-1.5',
  wrapper: 'items-start',
  leading: 'p-2.5 rounded-full bg-primary/10 ring ring-inset ring-primary/25 flex-col',
  title: 'font-normal text-muted text-xs uppercase'
}"
```

### Grid Customization
- **Responsive Breakpoints**: Adapts from 1 to 4 columns
- **Gap Management**: Consistent spacing across screen sizes
- **Border Handling**: Seamless card connections on desktop

## Accessibility Features
- **Semantic HTML**: Proper heading and content structure
- **Color Accessibility**: High contrast trend indicators
- **Screen Reader**: Meaningful metric descriptions
- **Keyboard Navigation**: Full keyboard support for links

## Extension Points
This component can be enhanced with:
- Real-time data integration
- Custom metric definitions
- Historical trend charts
- Comparison periods
- Drill-down functionality
- Export capabilities
- Alert thresholds

## Performance Considerations
- **Async Data**: Efficient data fetching with caching
- **Computed Values**: Optimized reactive calculations
- **Minimal Re-renders**: Updates only when necessary
- **Default Fallbacks**: Graceful loading states

## Best Practices Demonstrated
- **Configuration-driven**: Easy to modify stats without code changes
- **Type Safety**: Full TypeScript integration
- **Responsive Design**: Mobile-first approach with desktop enhancements
- **User Experience**: Clear visual hierarchy and meaningful interactions
- **Maintainability**: Clean separation of data and presentation logic

This component exemplifies modern dashboard design patterns and provides a solid foundation for KPI displays in data-driven applications.
