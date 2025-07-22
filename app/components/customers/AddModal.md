# Customer AddModal Component

## Purpose
The `AddModal` component provides a modal dialog for adding new customers to the database. It includes form validation, user feedback, and integration with the application's toast notification system.

## Reusability
This component serves as an excellent template for creating form-based modal dialogs. The validation schema, form structure, and submission handling patterns can be reused for other entity creation scenarios.

## Props
- None

## Emits
- None

## Slots
- `body` - Contains the form content and action buttons

## Nuxt UI Pro Integration
This component utilizes several Nuxt UI Pro components:
- `UModal` - Main modal dialog functionality
- `UForm` - Form handling with validation
- `UFormField` - Form field wrapper with labels
- `UInput` - Text input fields
- `UButton` - Action buttons with variants

## Usage Examples
```vue
<!-- Basic usage -->
<CustomersAddModal />

<!-- Can be triggered programmatically -->
<CustomersAddModal ref="addModal" />
<UButton @click="$refs.addModal.open = true" label="Add Customer" />
```

## Component Relationships
- Integrates with `useToast()` composable for user feedback
- Uses Zod schema validation for form validation
- Follows Nuxt UI Pro form patterns and styling

## State Management
- Uses local reactive state for form data
- Manages modal open/close state internally
- Integrates with toast notifications for user feedback

## Key Features

### Form Validation
- **Zod Schema**: Validates name (minimum 2 characters) and email format
- **Real-time Validation**: Provides immediate feedback on form errors
- **Type Safety**: Full TypeScript integration with schema types

### User Experience
- **Toast Notifications**: Success message with customer name
- **Modal State Management**: Automatic close on successful submission
- **Form Reset**: Cleans up form state after submission

### Validation Schema
```typescript
const schema = z.object({
  name: z.string().min(2, 'Too short'),
  email: z.string().email('Invalid email')
})
```

### Form Handling
```typescript
async function onSubmit(event: FormSubmitEvent<Schema>) {
  toast.add({ 
    title: 'Success', 
    description: `New customer ${event.data.name} added`, 
    color: 'success' 
  })
  open.value = false
}
```

## Accessibility Features
- Modal dialog with proper ARIA attributes
- Form labels and field associations
- Keyboard navigation support
- Focus management on open/close

## Extension Points
This component can be enhanced with:
- Additional form fields (phone, address, etc.)
- File upload for customer avatars
- Integration with backend APIs
- Form field dependencies
- Custom validation rules
- Bulk import functionality

## Best Practices Demonstrated
- **Separation of Concerns**: Schema definition separate from component logic
- **Type Safety**: Full TypeScript integration throughout
- **User Feedback**: Clear success/error messaging
- **Validation**: Client-side validation with immediate feedback
- **Accessibility**: Proper modal and form accessibility patterns
