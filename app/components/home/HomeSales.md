# HomeSales Component

## Purpose
The `HomeSales` component displays recent sales transactions in a structured table format with status indicators, formatting, and responsive design. It provides real-time sales data visualization for dashboard monitoring.

## Reusability
This component demonstrates excellent table design patterns that can be reused for any tabular data display. The column configuration, data formatting, and status styling patterns are highly adaptable for different data types.

## Props
- `period: Period` - Time period for data filtering
- `range: Range` - Date range for data filtering

## Emits
- None

## Slots
- None (uses table cell renderers)

## Nuxt UI Pro Integration
This component utilizes:
- `UTable` - Main table component with advanced styling
- `UBadge` - Status indicators with color variants
- Integrates with Nuxt UI Pro design system and table styling

## Usage Examples
```vue
<!-- Basic usage with filtering -->
<HomeSales :period="'daily'" :range="{ start: startDate, end: endDate }" />

<!-- In dashboard layout -->
<div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
  <HomeChart :period="period" :range="range" />
  <HomeSales :period="period" :range="range" />
</div>
```

## Component Relationships
- Uses `useAsyncData` for reactive data fetching
- Integrates with dashboard filtering system
- Demonstrates advanced table column configuration
- Uses Vue's render function (`h`) for dynamic components

## State Management
- Reactive data fetching based on props changes
- Automatic data refresh when period/range changes
- Simulated data generation for demonstration purposes

## Key Features

### Dynamic Data Generation
```typescript
const sales: Sale[] = []
for (let i = 0; i < 5; i++) {
  const hoursAgo = randomInt(0, 48)
  const date = new Date(currentDate.getTime() - hoursAgo * 3600000)
  
  sales.push({
    id: (4600 - i).toString(),
    date: date.toISOString(),
    status: randomFrom(['paid', 'failed', 'refunded']),
    email: randomFrom(sampleEmails),
    amount: randomInt(100, 1000)
  })
}
```

### Advanced Column Configuration
```typescript
const columns: TableColumn<Sale>[] = [
  {
    accessorKey: 'id',
    header: 'ID',
    cell: ({ row }) => `#${row.getValue('id')}`
  },
  {
    accessorKey: 'status',
    header: 'Status',
    cell: ({ row }) => {
      const color = {
        paid: 'success' as const,
        failed: 'error' as const,
        refunded: 'neutral' as const
      }[row.getValue('status') as string]
      
      return h(UBadge, { class: 'capitalize', variant: 'subtle', color }, () =>
        row.getValue('status')
      )
    }
  }
]
```

### Status-based Styling
- **Visual Hierarchy**: Color-coded status badges
- **Semantic Colors**: Success/error/neutral color mapping
- **Consistent Design**: Follows Nuxt UI Pro badge patterns

### Currency Formatting
```typescript
const formatted = new Intl.NumberFormat('en-US', {
  style: 'currency',
  currency: 'EUR'
}).format(amount)
```

## Advanced Features

### Responsive Table Design
- **Custom Styling**: Border-separate table layout
- **Header Styling**: Elevated background for headers
- **Cell Borders**: Consistent border treatment
- **Rounded Corners**: Professional appearance

### Date/Time Formatting
- **Locale-aware**: Uses `toLocaleString` for proper formatting
- **Compact Display**: Day, month, hour, minute in compact format
- **24-hour Format**: Consistent time display

### Performance Optimization
- **Async Data**: Efficient data fetching with caching
- **Watch Integration**: Updates only when necessary
- **Default Values**: Prevents loading states

## Accessibility Features
- **Semantic HTML**: Proper table structure
- **Screen Reader**: Meaningful column headers
- **Keyboard Navigation**: Full keyboard support
- **ARIA Labels**: Complete accessibility markup

## Extension Points
This component can be enhanced with:
- Real API integration
- Pagination support
- Sorting and filtering
- Export functionality
- Row selection
- Action buttons
- Detailed views

## Best Practices Demonstrated
- **Type Safety**: Full TypeScript integration with proper types
- **Performance**: Efficient async data handling
- **Design System**: Consistent with Nuxt UI Pro patterns
- **User Experience**: Clear data presentation and status indicators
- **Maintainability**: Clean column configuration patterns
