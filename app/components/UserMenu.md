# UserMenu Component

## Purpose
The `UserMenu` component provides a comprehensive user account dropdown menu with profile management, theme customization, navigation links, and account actions. It serves as the primary user interaction hub in the dashboard application.

## Reusability
This component is highly reusable across different dashboard templates and can be adapted for various user management scenarios. The theme switching and profile management features make it suitable for most admin interfaces.

## Props
- `collapsed?: boolean` - Controls whether the menu is displayed in a collapsed state (shows only avatar vs full user info)

## Emits
- None

## Slots
- `chip-leading` - Custom slot for rendering color chips in the theme selection menu

## Nuxt UI Pro Integration
This component heavily integrates with Nuxt UI Pro components:
- `UDropdownMenu` - Main dropdown functionality with nested menus
- `UButton` - User profile trigger button
- Utilizes Nuxt UI Pro's color system and theming capabilities
- Integrates with `useColorMode()` and `useAppConfig()` composables

## Usage Examples
```vue
<!-- Normal expanded view -->
<UserMenu />

<!-- Collapsed view (sidebar minimized) -->
<UserMenu :collapsed="true" />

<!-- In a responsive layout -->
<UserMenu :collapsed="isMobile || sidebarCollapsed" />
```

## Component Relationships
- Integrates with Nuxt's color mode system (`useColorMode`)
- Uses app configuration for theme colors (`useAppConfig`) 
- Links to settings pages and external documentation
- Follows Nuxt UI Pro dropdown patterns

## State Management
- Uses local `ref()` for user data
- Integrates with global color mode state
- Modifies app configuration for theme changes
- Could be extended to integrate with user authentication state

## Key Features

### Theme Management
- **Primary Color Selection**: Choose from 17 predefined colors
- **Neutral Color Selection**: Choose from 5 neutral color schemes  
- **Light/Dark Mode Toggle**: Seamless appearance switching

### Navigation
- **Profile & Settings**: Direct links to user management pages
- **Template Showcase**: Links to different Nuxt UI Pro templates
- **Documentation Access**: Quick access to docs and GitHub

### User Profile
- **Avatar Display**: Shows user avatar with fallback
- **User Information**: Displays user name and details
- **Account Actions**: Logout and account management options

## Advanced Integration Notes

### Color System Integration
```vue
// Theme color selection logic
onSelect: (e) => {
  e.preventDefault()
  appConfig.ui.colors.primary = color
}
```

### Responsive Behavior
- Adapts menu width based on collapsed state
- Shows/hides user name based on available space
- Maintains functionality in both expanded and collapsed modes

## Styling Features
- **Dynamic Color Chips**: Visual color preview using CSS custom properties
- **Checkbox States**: Proper checked/unchecked states for theme options
- **Hover Effects**: Consistent with Nuxt UI Pro design language
- **Multi-level Dropdowns**: Nested menu support for complex options

## Extensibility
This component can be easily extended to include:
- Additional profile settings
- More theme options
- Custom navigation items
- Integration with authentication systems
- Real-time user status indicators
