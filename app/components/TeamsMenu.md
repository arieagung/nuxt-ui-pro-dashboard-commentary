# TeamsMenu Component

## Purpose
The `TeamsMenu` component provides a dropdown menu interface for team selection and management. It allows users to switch between different teams and access team management options.

## Reusability
This component is highly reusable for any multi-team application where users need to switch between different organizations or projects. The dynamic team loading makes it adaptable to various team structures.

## Props
- `collapsed?: boolean` - Controls whether the menu is displayed in a collapsed state (shows only avatar vs full team info)

## Emits  
- None

## Slots
- None (uses default dropdown menu slots)

## Nuxt UI Pro Integration
This component leverages several Nuxt UI Pro components:
- `UDropdownMenu` - Main dropdown functionality
- `UButton` - Team selection trigger button
- Utilizes proper Nuxt UI Pro styling patterns and design tokens

## Usage Examples
```vue
<!-- Normal expanded view -->
<TeamsMenu />

<!-- Collapsed view (sidebar minimized) -->
<TeamsMenu :collapsed="true" />

<!-- In a responsive sidebar -->
<TeamsMenu :collapsed="sidebarCollapsed" />
```

## Component Relationships
- Integrates with Nuxt UI Pro's dropdown system
- Uses computed properties for dynamic menu items
- Follows Nuxt UI Pro patterns for button variants and styling

## State Management
- Uses local `ref()` for teams array and selected team
- Manages team selection state internally
- Could be extended to integrate with global team state management

## Key Features
- **Dynamic Team Loading**: Teams are stored in a reactive array that can be easily modified
- **Team Selection**: Click handlers for switching between teams
- **Responsive Design**: Adapts UI based on collapsed prop
- **Team Management Actions**: Includes options for creating and managing teams
- **Avatar Support**: Each team can have an associated avatar image

## Styling Notes
- Uses CSS custom properties for dynamic width calculation
- Implements hover states and focus management
- Follows Nuxt UI Pro color scheme patterns
