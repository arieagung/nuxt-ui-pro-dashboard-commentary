# Customer DeleteModal Component

## Purpose
The `DeleteModal` component provides a confirmation dialog for deleting one or multiple customers. It includes dynamic text based on selection count, loading states, and user confirmation patterns.

## Reusability
This component is highly reusable for any delete confirmation scenario. The dynamic count handling and slot-based trigger make it adaptable for different entity types and use cases.

## Props
- `count?: number` - Number of items to be deleted (defaults to 0)

## Emits
- None

## Slots
- Default slot - Contains the trigger button or element

## Nuxt UI Pro Integration
This component utilizes several Nuxt UI Pro components:
- `UModal` - Main modal dialog functionality
- `UButton` - Action buttons with loading states and variants

## Usage Examples
```vue
<!-- Single customer deletion -->
<CustomersDeleteModal :count="1">
  <UButton color="error" variant="outline" label="Delete Customer" />
</CustomersDeleteModal>

<!-- Multiple customers deletion -->
<CustomersDeleteModal :count="selectedCustomers.length">
  <UButton 
    color="error" 
    variant="solid" 
    :label="`Delete ${selectedCustomers.length} customers`" 
  />
</CustomersDeleteModal>

<!-- With custom trigger -->
<CustomersDeleteModal :count="customerCount">
  <UIcon name="i-lucide-trash" class="cursor-pointer text-error" />
</CustomersDeleteModal>
```

## Component Relationships
- Follows Nuxt UI Pro modal patterns
- Uses async/await patterns for deletion operations
- Integrates with loading states and user feedback

## State Management
- Uses local ref for modal open/close state
- Manages loading states during deletion process
- Could be extended to integrate with global state management

## Key Features

### Dynamic Content
- **Pluralization**: Automatically handles singular/plural text based on count
- **Dynamic Titles**: Changes title based on number of items
- **Confirmation Message**: Warns users about irreversible action

### User Experience
- **Loading States**: Shows loading indicator during deletion
- **Auto-loading**: Uses `loading-auto` prop for seamless UX
- **Confirmation Pattern**: Clear Cancel/Delete action buttons

### Deletion Simulation
```typescript
async function onSubmit() {
  await new Promise(resolve => setTimeout(resolve, 1000))
  open.value = false
}
```

## Accessibility Features
- Modal dialog with proper ARIA attributes
- Clear action button labeling
- Keyboard navigation support
- Focus management on open/close

## Extension Points
This component can be enhanced with:
- Integration with actual deletion APIs
- Error handling and rollback scenarios
- Bulk deletion progress indicators
- Confirmation checkboxes for dangerous operations
- Custom deletion reasons or notes
- Soft delete vs hard delete options

## Styling Features
- **Error Colors**: Uses error color scheme for dangerous actions
- **Button Variants**: Proper visual hierarchy with subtle/solid variants
- **Loading Animation**: Built-in loading states for better UX

## Best Practices Demonstrated
- **Destructive Action Patterns**: Clear confirmation for irreversible operations
- **Loading States**: Proper async operation handling
- **Dynamic Content**: Flexible text handling based on context
- **Slot Composition**: Flexible trigger elements through slots
- **Type Safety**: Props with defaults and proper typing
