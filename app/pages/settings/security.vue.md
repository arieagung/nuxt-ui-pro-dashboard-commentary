# Security Settings - `pages/settings/security.vue`

## Overview

The security settings page provides a comprehensive interface for managing account security, including password changes and account deletion. It demonstrates advanced form validation, custom validation rules, and destructive action patterns using Nuxt UI Pro components.

## Route Information

- **Path**: `/settings/security`
- **File**: `pages/settings/security.vue`
- **Layout**: Inherits from `pages/settings.vue` parent layout
- **Page Name**: `settings-security`

## Key Features

### 1. Password Management
- **Current Password Verification**: Ensures user authentication before changes
- **New Password Setup**: Secure password entry with validation
- **Custom Validation**: Advanced validation beyond schema rules
- **Type-Safe Forms**: Full TypeScript integration with Zod

### 2. Account Deletion
- **Destructive Action UI**: Clear visual indication of dangerous operations
- **Warning Presentation**: Comprehensive information about consequences
- **Gradient Styling**: Visual emphasis for critical actions
- **Confirmation Patterns**: Best practices for destructive operations

### 3. Form Validation
- **Schema Validation**: Zod-based validation rules
- **Custom Validation**: Additional business logic validation
- **Error Handling**: User-friendly error messages
- **Real-time Feedback**: Immediate validation responses

## Component Structure

```vue
<script setup lang="ts">
import * as z from 'zod'
import type { FormError } from '@nuxt/ui'

// Password validation schema
const passwordSchema = z.object({
  current: z.string().min(8, 'Must be at least 8 characters'),
  new: z.string().min(8, 'Must be at least 8 characters')
})

type PasswordSchema = z.output<typeof passwordSchema>

// Reactive password state
const password = reactive<Partial<PasswordSchema>>({
  current: undefined,
  new: undefined
})

// Custom validation function
const validate = (state: Partial<PasswordSchema>): FormError[] => {
  const errors: FormError[] = []
  if (state.current && state.new && state.current === state.new) {
    errors.push({ name: 'new', message: 'Passwords must be different' })
  }
  return errors
}
</script>
```

## Form Validation System

### Zod Schema Definition
```typescript
const passwordSchema = z.object({
  current: z.string().min(8, 'Must be at least 8 characters'),
  new: z.string().min(8, 'Must be at least 8 characters')
})
```

**Validation Rules:**
- **Current Password**: Minimum 8 characters, required for verification
- **New Password**: Minimum 8 characters, enforced security standard
- **String Type**: Ensures password fields are text-based
- **Required Fields**: Both passwords must be provided

### Custom Validation Logic
```typescript
const validate = (state: Partial<PasswordSchema>): FormError[] => {
  const errors: FormError[] = []
  if (state.current && state.new && state.current === state.new) {
    errors.push({ name: 'new', message: 'Passwords must be different' })
  }
  return errors
}
```

**Custom Validation Features:**
- **Business Logic**: Enforces passwords must be different
- **Error Targeting**: Associates errors with specific fields
- **Multiple Errors**: Supports array of validation errors
- **Type Safety**: Full TypeScript support for error handling

### Type Safety Implementation
```typescript
type PasswordSchema = z.output<typeof passwordSchema>
const password = reactive<Partial<PasswordSchema>>({
  current: undefined,
  new: undefined
})
```

**Type Safety Benefits:**
- **Compile-time Validation**: TypeScript checks at build time
- **IntelliSense**: Full IDE support and autocompletion
- **Runtime Safety**: Zod validation at form submission
- **Partial State**: Allows undefined values during form entry

## Password Form Implementation

### Form Structure
```vue
<UPageCard
  title="Password"
  description="Confirm your current password before setting a new one."
  variant="subtle"
>
  <UForm
    :schema="passwordSchema"
    :state="password"
    :validate="validate"
    class="flex flex-col gap-4 max-w-xs"
  >
    <!-- Password fields -->
    <UButton label="Update" class="w-fit" type="submit" />
  </UForm>
</UPageCard>
```

### Form Fields
```vue
<UFormField name="current">
  <UInput
    v-model="password.current"
    type="password"
    placeholder="Current password"
    class="w-full"
  />
</UFormField>

<UFormField name="new">
  <UInput
    v-model="password.new"
    type="password"
    placeholder="New password"
    class="w-full"
  />
</UFormField>
```

**Form Features:**
- **Password Fields**: Proper input type for security
- **Placeholders**: Clear field identification
- **Full Width**: Responsive input sizing
- **Two-way Binding**: Reactive state management

## Account Deletion Section

### Destructive Action UI
```vue
<UPageCard
  title="Account"
  description="No longer want to use our service? You can delete your account here. This action is not reversible. All information related to this account will be deleted permanently."
  class="bg-gradient-to-tl from-error/10 from-5% to-default"
>
  <template #footer>
    <UButton label="Delete account" color="error" />
  </template>
</UPageCard>
```

**Destructive Action Features:**
- **Visual Warning**: Gradient background with error color
- **Clear Messaging**: Explicit warning about irreversibility
- **Footer Placement**: Action button in dedicated footer area
- **Error Color**: Red button to indicate danger

### Gradient Styling
```css
class="bg-gradient-to-tl from-error/10 from-5% to-default"
```

**Styling Details:**
- **Gradient Direction**: Top-left diagonal gradient
- **Error Transparency**: 10% opacity error color
- **Gradient Start**: 5% position for subtle effect
- **Default Background**: Fallback to theme default

## Nuxt UI Pro Components Used

### Form Components
- **`UForm`**: Main form container with validation
- **`UFormField`**: Field wrapper for consistent layout
- **`UInput`**: Password input fields with security
- **`UButton`**: Submit and action buttons

### Layout Components
- **`UPageCard`**: Card containers with variants and styling
- **Form Templates**: Footer template for action placement

### Form Integration
```vue
<UForm
  :schema="passwordSchema"
  :state="password"
  :validate="validate"
  class="flex flex-col gap-4 max-w-xs"
>
```

**Integration Features:**
- **Schema Binding**: Automatic Zod validation
- **State Management**: Reactive form state
- **Custom Validation**: Additional validation rules
- **Layout Control**: Custom form styling

## User Experience Features

### Security Best Practices
- **Current Password Required**: Verifies user identity
- **Password Strength**: Minimum length requirements
- **Different Passwords**: Enforces password changes
- **Clear Warnings**: Explicit account deletion warnings

### Visual Hierarchy
```vue
variant="subtle"    <!-- Password form -->
class="bg-gradient-to-tl from-error/10 from-5% to-default"  <!-- Deletion section -->
```

**Visual Design:**
- **Subtle Cards**: Clean, non-intrusive password form
- **Warning Gradient**: Visual emphasis for destructive actions
- **Color Coding**: Error color for dangerous operations
- **Consistent Spacing**: Uniform gap management

### Form Layout
```vue
class="flex flex-col gap-4 max-w-xs"
```

**Layout Features:**
- **Vertical Stacking**: Logical form field progression
- **Consistent Spacing**: 1rem gaps between elements
- **Width Constraint**: Maximum width for optimal readability
- **Responsive Design**: Adapts to different screen sizes

## Accessibility Features

### Form Accessibility
- **Proper Field Types**: Password inputs for security
- **Clear Labels**: Descriptive placeholder text
- **Error Association**: Field-specific error messages
- **Keyboard Navigation**: Standard form navigation

### Security Accessibility
- **Clear Warnings**: Explicit deletion consequences
- **Visual Indicators**: Color-coded danger levels
- **Focus Management**: Logical tab order
- **Screen Reader Support**: Semantic HTML structure

## Error Handling

### Validation Errors
```typescript
const errors: FormError[] = []
if (state.current && state.new && state.current === state.new) {
  errors.push({ name: 'new', message: 'Passwords must be different' })
}
```

**Error Management:**
- **Field-Specific**: Errors associated with specific fields
- **User-Friendly**: Clear, actionable error messages
- **Multiple Errors**: Support for multiple validation issues
- **Type Safety**: Structured error objects

### Security Considerations
- **Password Masking**: Input type="password" for security
- **Client-Side Only**: Passwords not logged or exposed
- **Validation Rules**: Enforced security standards
- **Clear Requirements**: Explicit password criteria

## Integration Patterns

### API Integration
```typescript
// Example password change integration
async function updatePassword(passwordData: PasswordSchema) {
  try {
    await $fetch('/api/auth/password', {
      method: 'PUT',
      body: {
        currentPassword: passwordData.current,
        newPassword: passwordData.new
      }
    })
    
    // Success feedback
    toast.add({
      title: 'Password updated',
      description: 'Your password has been successfully changed.'
    })
  } catch (error) {
    // Error handling
    toast.add({
      title: 'Update failed',
      description: 'Please check your current password and try again.',
      color: 'error'
    })
  }
}
```

### Account Deletion Flow
```typescript
// Example deletion confirmation
async function deleteAccount() {
  const confirmed = await confirmDialog({
    title: 'Delete Account',
    description: 'This action cannot be undone. All your data will be permanently deleted.',
    confirmText: 'Delete Account',
    confirmColor: 'error'
  })
  
  if (confirmed) {
    await $fetch('/api/account', { method: 'DELETE' })
    // Redirect to goodbye page
    await navigateTo('/goodbye')
  }
}
```

## Performance Considerations

### Form Validation
- **Client-Side First**: Immediate validation feedback
- **Schema Caching**: Reused validation schemas
- **Reactive Updates**: Efficient state management
- **Custom Validation**: Minimal performance impact

### Security Optimization
- **Password Masking**: No performance impact
- **Minimal State**: Only necessary form data stored
- **Clear Memory**: Sensitive data cleanup
- **Efficient Rendering**: Minimal re-renders

## Customization Options

### Validation Rules
```typescript
// Enhanced password validation
const passwordSchema = z.object({
  current: z.string()
    .min(8, 'Must be at least 8 characters')
    .regex(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/, 'Must include uppercase, lowercase, and number'),
  new: z.string()
    .min(12, 'Must be at least 12 characters')
    .regex(/^(?=.*[!@#$%^&*])/, 'Must include special character')
})
```

### Custom Styling
```vue
<!-- Enhanced destructive action styling -->
<UPageCard
  :class="[
    'bg-gradient-to-tl from-error/10 from-5% to-default',
    'border-error/20 border-2',
    'shadow-error/5 shadow-lg'
  ]"
>
```

### Advanced Features
- **Two-Factor Authentication**: Additional security layer
- **Password History**: Prevent password reuse
- **Security Questions**: Additional verification methods
- **Activity Logging**: Track security changes

## Future Enhancements

### Security Features
- **Password Strength Meter**: Visual feedback for password quality
- **Breach Detection**: Check against known compromised passwords
- **Security Audit Log**: Track all security-related changes
- **Multi-Factor Authentication**: Enhanced authentication options

### User Experience
- **Password Generation**: Suggest secure passwords
- **Recovery Options**: Enhanced account recovery methods
- **Security Dashboard**: Overview of account security status
- **Notification Preferences**: Security alert customization

This security settings page provides robust account security management with excellent user experience and comprehensive validation patterns.
