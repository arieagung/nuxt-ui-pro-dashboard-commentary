# Main CSS Styling Documentation

## Overview

This file contains the main CSS configuration for the Nuxt UI Pro dashboard application. It establishes the foundation for the application's styling system by integrating Tailwind CSS with Nuxt UI Pro components and defining custom CSS variables.

## CSS Imports and Architecture

### Tailwind CSS Integration
```css
@import "tailwindcss" theme(static);
```

**Purpose:** Imports Tailwind CSS with static theme configuration, enabling utility-first CSS approach.

**Benefits:**
- Provides utility classes for rapid UI development
- Enables responsive design patterns
- Offers consistent spacing, colors, and typography
- Supports custom theme extensions through CSS variables

### Nuxt UI Pro Integration
```css
@import "@nuxt/ui-pro";
```

**Purpose:** Imports Nuxt UI Pro component styles and design system.

**Benefits:**
- Pre-built, accessible component library
- Consistent design language across components
- Dark/light mode support out of the box
- Professional UI components (forms, navigation, layouts)

## Custom Styling Approaches

### 1. CSS Variable-Based Theming
The application uses CSS custom properties (variables) to define a flexible theming system:

```css
@theme static {
  --font-sans: 'Public Sans', sans-serif;
  
  --color-green-50: #EFFDF5;
  --color-green-100: #D9FBE8;
  --color-green-200: #B3F5D1;
  --color-green-300: #75EDAE;
  --color-green-400: #00DC82;
  --color-green-500: #00C16A;
  --color-green-600: #00A155;
  --color-green-700: #007F45;
  --color-green-800: #016538;
  --color-green-900: #0A5331;
  --color-green-950: #052E16;
}
```

### 2. Typography Customization

**Font Family:** `'Public Sans', sans-serif`
- Modern, readable typeface suitable for dashboard interfaces
- Fallback to system sans-serif fonts
- Optimized for both display and body text

### 3. Color System Design

**Primary Color Palette: Green**
The green color palette provides:
- **50-100:** Light tints for backgrounds and subtle accents
- **200-300:** Medium tints for hover states and secondary elements
- **400-500:** Primary brand colors for key actions and highlights
- **600-700:** Darker shades for text and active states
- **800-950:** Deep shades for high contrast elements and dark themes

## CSS Variable Usage Patterns

### Color Variables
```css
/* Usage in components */
.primary-button {
  background-color: var(--color-green-500);
  border-color: var(--color-green-600);
}

.success-badge {
  background-color: var(--color-green-100);
  color: var(--color-green-800);
}
```

### Typography Variables
```css
/* Usage in components */
body {
  font-family: var(--font-sans);
}
```

## Tailwind CSS Customizations

### Static Theme Configuration
The `@theme static` directive allows for:

1. **Design Token Integration:** CSS variables become available as Tailwind utilities
2. **Consistent Naming:** Variables follow Tailwind's naming conventions
3. **Build Optimization:** Static theme improves build performance
4. **Runtime Flexibility:** Variables can be modified via JavaScript for dynamic theming

### Available Utility Classes
With the custom green palette, you can use:
```html
<!-- Background utilities -->
<div class="bg-green-50">Light background</div>
<div class="bg-green-500">Primary background</div>
<div class="bg-green-950">Dark background</div>

<!-- Text utilities -->
<p class="text-green-600">Primary text</p>
<p class="text-green-800">Darker text</p>

<!-- Border utilities -->
<div class="border-green-300">Light border</div>
<div class="border-green-600">Strong border</div>
```

## Nuxt UI Pro Theme Complementation

### Configuration Integration
The styling works in harmony with `app.config.ts`:
```typescript
export default defineAppConfig({
  ui: {
    colors: {
      primary: 'green',    // References our custom green palette
      neutral: 'zinc'      // Uses Tailwind's built-in zinc palette
    }
  }
})
```

### Component Theming Strategy
1. **Primary Actions:** Use green color palette for buttons, links, and key interactions
2. **Neutral Elements:** Use zinc palette for backgrounds, borders, and secondary content
3. **Semantic Colors:** Leverage green shades for success states and positive feedback
4. **Accessibility:** Color combinations maintain WCAG contrast ratios

### Dark Mode Considerations
The CSS variables enable seamless dark mode transitions:
- Light mode uses lighter green shades (50-400)
- Dark mode leverages darker shades (600-950)
- Nuxt UI Pro handles automatic theme switching

## Best Practices

### 1. Variable Usage
- Always use CSS variables instead of hardcoded colors
- Follow the established naming convention
- Test color combinations for accessibility

### 2. Component Styling
- Prefer Tailwind utilities over custom CSS when possible
- Use component-specific CSS variables for complex customizations
- Maintain consistency with the design system

### 3. Theme Extensions
- Add new color variables following the 50-950 scale
- Update both CSS variables and app.config.ts when adding new colors
- Document any new styling patterns

## File Dependencies

**Related Files:**
- `nuxt.config.ts` - Imports this file via `css: ['~/assets/css/main.css']`
- `app.config.ts` - Configures UI color scheme to use 'green' primary
- Component files - Consume these styles through Tailwind classes

**Build Process:**
1. CSS is processed during Nuxt build
2. Tailwind utilities are generated based on theme configuration
3. Unused styles are purged for optimal bundle size
4. Variables are available globally across all components
