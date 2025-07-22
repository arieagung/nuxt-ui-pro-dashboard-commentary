# NotificationsSlideover Component

## Purpose
The `NotificationsSlideover` component provides a sliding panel for displaying notifications. It is designed to be easily accessible and allow users to see their notifications quickly.

## Reusability
This component can be reused in any part of the application where notifications need to be displayed. The ability to fetch notifications dynamically makes it versatile for various notification systems.

## Props
- None

## Emits
- None

## Slots
- `body` (slot): Used to define the content of the slideover.

## Nuxt UI Pro Integration
This component uses `USlideover` from Nuxt UI Pro for creating the slideover interface.

## Usage Examples
```vue
<NotificationsSlideover />
```

## Component Relationships
- Utilizes `UChip` and `UAvatar` for displaying notification details.

## State Management
- Integrates with `useDashboard` composable.
- Fetches notifications using `useFetch`.
