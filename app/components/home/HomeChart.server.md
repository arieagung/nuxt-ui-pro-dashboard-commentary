# HomeChart Server Component

## Purpose
The `HomeChart.server.vue` component serves as a server-side placeholder for the chart component during server-side rendering (SSR). It provides a loading state and maintains layout consistency while the client-side chart component hydrates.

## Reusability
This component demonstrates the server/client component pattern that's essential for Nuxt applications with heavy client-side visualizations. The pattern can be reused for any component that requires client-side libraries or extensive JavaScript interactions.

## Props
- None

## Emits
- None

## Slots
- `header` - Contains the revenue title and placeholder display

## Nuxt UI Pro Integration
This component utilizes:
- `UCard` - Same container structure as client component
- Maintains consistent styling and layout with client version
- Uses Nuxt UI Pro design tokens for consistent appearance

## Usage Examples
```vue
<!-- This component is automatically used during SSR -->
<!-- When the client version is not available, this renders instead -->

<!-- In Nuxt, both components work together: -->
<!-- components/home/HomeChart.client.vue (client-side) -->
<!-- components/home/HomeChart.server.vue (server-side) -->

<!-- Usage in pages or other components: -->
<HomeChart :period="'daily'" :range="dateRange" />
```

## Component Relationships
- **Companion to**: `HomeChart.client.vue`
- **SSR Pattern**: Part of Nuxt's client/server component system
- **Layout Consistency**: Maintains same card structure and dimensions

## State Management
- No reactive state required
- Static placeholder content
- Minimal JavaScript footprint for SSR performance

## Key Features

### SSR Optimization
- **Lightweight**: Minimal HTML for fast server rendering
- **Layout Preservation**: Same dimensions as client component
- **Loading Placeholder**: Clear indication of loading state

### Design Consistency
```vue
<template>
  <UCard :ui="{ body: '!px-0 !pt-0 !pb-3' }">
    <template #header>
      <div>
        <p class="text-xs text-muted uppercase mb-1.5">
          Revenue
        </p>
        <p class="text-3xl text-highlighted font-semibold">
          ---
        </p>
      </div>
    </template>
    
    <div class="h-96" />
  </UCard>
</template>
```

### Performance Benefits
- **Fast Initial Load**: Renders immediately on server
- **Reduced CLS**: Prevents layout shift during hydration
- **Progressive Enhancement**: Works without JavaScript

## Client/Server Pattern Benefits

### SEO Optimization
- **Server Rendering**: Initial HTML includes chart container
- **Fast First Paint**: Users see layout immediately
- **Graceful Degradation**: Functional without JavaScript

### User Experience
- **Smooth Transition**: Seamless handover to client component
- **Loading States**: Clear visual feedback during hydration
- **Consistent Layout**: No layout jumps or shifts

## Extension Points
This pattern can be enhanced with:
- More sophisticated loading animations
- Static chart thumbnails or previews
- Skeleton loading states
- Error boundary handling
- Progressive data loading indicators

## Best Practices Demonstrated

### Component Architecture
- **Clear Separation**: Server vs client responsibilities
- **Consistent Interface**: Same props and styling approach
- **Minimal Server Code**: Only essential HTML for layout

### Performance
- **SSR-first**: Server rendering for fast initial loads
- **Client Enhancement**: Rich interactivity after hydration
- **Resource Optimization**: Minimal server-side resources

### Accessibility
- **Progressive Enhancement**: Functional at every loading stage
- **Screen Reader**: Meaningful placeholder content
- **Loading Communication**: Clear loading state indicators

## Implementation Notes
- **File Naming**: Uses `.server.vue` suffix for server-only rendering
- **Automatic Selection**: Nuxt automatically chooses appropriate component
- **Hydration Ready**: Prepared for client component takeover
- **Layout Matching**: Critical for preventing layout shifts

This component exemplifies the client/server pattern that's crucial for modern Nuxt applications requiring both SEO optimization and rich client-side interactions.
