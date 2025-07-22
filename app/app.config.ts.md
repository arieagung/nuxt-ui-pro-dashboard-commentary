# App.config.ts - Application Configuration

This configuration file defines the global settings for the Nuxt application, specifically focusing on Nuxt UI Pro theme configuration. It establishes the color scheme that will be used throughout the entire application for consistent theming.

## Overview

The app configuration is a Nuxt-specific file that allows you to define reactive configuration values that can be updated at runtime. This file specifically configures the Nuxt UI Pro component library's theming system.

## Color Scheme Configuration

### Primary Color: Green
- **Value**: `'green'`
- **Purpose**: Defines the primary brand color used across all UI components
- **Impact**: Affects buttons, links, form controls, navigation elements, and accent colors
- **Nuxt UI Pro Integration**: Automatically applies to all Nuxt UI Pro components that use primary color variants

### Neutral Color: Zinc
- **Value**: `'zinc'`
- **Purpose**: Defines the neutral/gray scale color palette
- **Impact**: Used for backgrounds, text colors, borders, and subtle UI elements
- **Design Choice**: Zinc provides a modern, sophisticated gray scale compared to traditional grays

## How It Affects Components

The colors defined here are automatically available throughout the application:

### Primary Color Usage
- **Buttons**: Primary buttons use the green color scheme
- **Links**: Active and hover states for navigation
- **Form Elements**: Focus states, checkboxes, radio buttons
- **Badges**: Primary badge variants
- **Progress Indicators**: Loading bars, progress components
- **Icons**: Accent icons and interactive elements

### Neutral Color Usage
- **Backgrounds**: Card backgrounds, modal overlays
- **Text**: Secondary text, muted content
- **Borders**: Component borders, dividers
- **Shadows**: Subtle shadow colors
- **Disabled States**: Inactive form elements

## Configuration Structure

The configuration follows Nuxt UI Pro's theming convention:

```typescript
ui: {
  colors: {
    primary: string,    // Main brand color
    neutral: string     // Neutral/gray scale
  }
}
```

## Color Customization

To modify the color scheme:

1. **Primary Color Options**: 
   - `'red'`, `'orange'`, `'amber'`, `'yellow'`, `'lime'`, `'green'`, `'emerald'`, `'teal'`, `'cyan'`, `'sky'`, `'blue'`, `'indigo'`, `'violet'`, `'purple'`, `'fuchsia'`, `'pink'`, `'rose'`

2. **Neutral Color Options**:
   - `'slate'`, `'gray'`, `'zinc'`, `'neutral'`, `'stone'`

## Integration with Tailwind CSS

These colors integrate seamlessly with Tailwind CSS:
- Primary color maps to Tailwind's green color palette
- Neutral color maps to Tailwind's zinc color palette
- All Nuxt UI Pro components automatically use these color definitions

## Runtime Configuration

This configuration is:
- **Reactive**: Can be updated at runtime
- **Global**: Available throughout the entire application
- **Type-safe**: Fully typed with TypeScript support
- **SSR-compatible**: Works with server-side rendering

## Best Practices

1. **Consistency**: Use the same color scheme across all components
2. **Accessibility**: Ensure sufficient color contrast for readability
3. **Brand Alignment**: Choose colors that match your brand identity
4. **User Experience**: Consider color psychology and user preferences

---

```typescript
// Application configuration for Nuxt UI Pro theming
export default defineAppConfig({
  ui: {
    colors: {
      // Primary brand color - used for buttons, links, and accent elements
      // Green provides a fresh, positive, and professional appearance
      primary: 'green',
      
      // Neutral color palette - used for backgrounds, text, and subtle elements
      // Zinc offers a modern, sophisticated gray scale for contemporary designs
      neutral: 'zinc'
    }
  }
})
```

## Usage Examples

Once configured, these colors are automatically available in components:

```vue
<!-- Primary color automatically applied -->
<UButton color="primary">Save Changes</UButton>

<!-- Neutral colors in backgrounds and text -->
<UCard class="bg-neutral-50 dark:bg-neutral-900">
  <p class="text-neutral-600 dark:text-neutral-400">
    Content with neutral styling
  </p>
</UCard>

<!-- Color variants automatically use the configured palette -->
<UBadge color="green">Success</UBadge>
<UAlert color="green">Operation completed successfully</UAlert>
```
