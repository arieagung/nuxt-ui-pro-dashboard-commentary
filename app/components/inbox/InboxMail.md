# InboxMail Component

## Purpose
The `InboxMail` component provides a detailed email viewing interface with reply functionality. It renders individual email messages in a dashboard panel format with full email metadata, actions, and inline reply capabilities.

## Reusability
This component demonstrates excellent patterns for email/message interfaces and can be adapted for:
- Customer support ticketing systems
- Internal communication tools
- Document review systems
- Any detail view requiring rich content display and response capabilities

## Props
- `mail: Mail` - Required mail object containing email data structure

## Emits
- `close()` - Emitted when user closes the email view

## Nuxt UI Pro Integration
This component showcases extensive Nuxt UI Pro integration:
- `UDashboardPanel` - Main container with proper dashboard styling
- `UDashboardNavbar` - Header with navigation and action buttons
- `UButton` - Various action buttons with tooltips and icons
- `UDropdownMenu` - More actions dropdown with contextual menu
- `UAvatar` - Sender avatar display
- `UCard` - Reply form container with subtle styling
- `UTextarea` - Autoresizing reply input
- `UTooltip` - Action button tooltips
- `UIcon` - Iconography throughout the interface

## Usage Examples
```vue
<!-- Basic usage -->
<InboxMail 
  :mail="selectedMail" 
  @close="selectedMail = null" 
/>

<!-- In a master-detail layout -->
<template>
  <div class="flex">
    <InboxList @select="handleMailSelect" />
    <InboxMail 
      v-if="selectedMail"
      :mail="selectedMail" 
      @close="selectedMail = null" 
    />
  </div>
</template>

<script setup>
const selectedMail = ref(null)

function handleMailSelect(mail) {
  selectedMail.value = mail
}
</script>
```

## Key Features

### Email Display
- **Header Information**: Subject, sender details, timestamp
- **Sender Avatar**: Visual identification with fallback
- **Formatted Date**: Human-readable date formatting using date-fns
- **Rich Content**: Preserves email body formatting with whitespace

### Action Interface
- **Navigation**: Close button to return to mail list
- **Quick Actions**: Archive, Reply buttons with tooltips
- **More Actions**: Dropdown menu with additional mail operations
  - Mark as unread/read
  - Mark as important
  - Star/unstar thread
  - Mute thread

### Reply System
- **Inline Reply**: Integrated reply form within the email view
- **Rich Input**: Autoresizing textarea with proper styling
- **Form Actions**: Save draft and send functionality
- **File Attachment**: Attachment button (UI placeholder)
- **Loading States**: Proper loading indicators during send

## State Management

### Local State
```vue
const reply = ref('')           // Reply content
const loading = ref(false)      // Send operation state
```

### Form Handling
```vue
function onSubmit() {
  loading.value = true
  
  // Simulate API call
  setTimeout(() => {
    reply.value = ''
    toast.add({
      title: 'Email sent',
      description: 'Your email has been sent successfully',
      icon: 'i-lucide-check-circle',
      color: 'success'
    })
    loading.value = false
  }, 1000)
}
```

## Advanced Features

### Toast Integration
The component demonstrates proper toast notification integration for user feedback:
```vue
const toast = useToast()

// Success notification after sending
toast.add({
  title: 'Email sent',
  description: 'Your email has been sent successfully',
  icon: 'i-lucide-check-circle',
  color: 'success'
})
```

### Dropdown Menu Configuration
```vue
const dropdownItems = [[{
  label: 'Mark as unread',
  icon: 'i-lucide-check-circle'
}, {
  label: 'Mark as important',
  icon: 'i-lucide-triangle-alert'
}], [{
  label: 'Star thread',
  icon: 'i-lucide-star'
}, {
  label: 'Mute thread',
  icon: 'i-lucide-circle-pause'
}]]
```

### Date Formatting
```vue
import { format } from 'date-fns'

// In template
{{ format(new Date(mail.date), 'dd MMM HH:mm') }}
```

## Layout and Styling

### Responsive Design
- **Desktop**: Full two-column layout with sidebar
- **Mobile**: Stacked layout with proper spacing adjustments
- **Flexible Content**: Adapts to various email content lengths

### Visual Hierarchy
- **Header Section**: Clear sender identification and metadata
- **Content Area**: Scrollable email body with preserved formatting
- **Reply Section**: Distinct card-based reply interface

### Accessibility Features
- **Proper ARIA roles**: List semantics and navigation
- **Keyboard Navigation**: All interactive elements are keyboard accessible
- **Screen Reader Support**: Proper labeling and structure
- **Focus Management**: Logical tab order through interface

## Integration Patterns

### Mail Object Structure
```typescript
interface Mail {
  subject: string
  from: {
    name: string
    email: string
    avatar: AvatarProps
  }
  date: string | Date
  body: string
  // Additional mail properties...
}
```

### Component Communication
```vue
<!-- Parent manages selection state -->
<script setup>
const selectedMail = ref(null)

function closeMail() {
  selectedMail.value = null
}
</script>

<template>
  <InboxMail 
    v-if="selectedMail"
    :mail="selectedMail"
    @close="closeMail"
  />
</template>
```

## Performance Considerations

### Efficient Rendering
- **Conditional Rendering**: Only renders when mail is selected
- **Lazy Loading**: Can be wrapped in Suspense for async data
- **Memory Management**: Proper cleanup of refs and watchers

### User Experience
- **Loading States**: Clear feedback during operations
- **Optimistic Updates**: Immediate UI feedback before API calls
- **Error Handling**: Toast notifications for operation results

## Extension Points

### Custom Actions
```vue
// Add custom dropdown items
const customDropdownItems = [
  ...dropdownItems,
  [{
    label: 'Forward',
    icon: 'i-lucide-forward',
    onSelect: () => handleForward(mail)
  }, {
    label: 'Print',
    icon: 'i-lucide-printer',
    onSelect: () => handlePrint(mail)
  }]
]
```

### Rich Text Editor Integration
```vue
<!-- Replace textarea with rich editor -->
<template #reply-editor>
  <QuillEditor 
    v-model="reply"
    :options="editorConfig"
    @ready="onEditorReady"
  />
</template>
```

### Attachment Support
```vue
const attachments = ref([])

function handleFileAttachment(files) {
  attachments.value.push(...files)
}

function removeAttachment(index) {
  attachments.value.splice(index, 1)
}
```

## Best Practices Demonstrated

### Component Design
- **Single Responsibility**: Focused on email display and reply
- **Props Interface**: Clean, typed props interface
- **Event Communication**: Proper parent-child communication
- **State Management**: Local state for component-specific data

### User Experience
- **Immediate Feedback**: Loading states and toast notifications
- **Intuitive Actions**: Contextual buttons and dropdown menus
- **Accessible Interface**: Proper keyboard and screen reader support
- **Responsive Design**: Works across all device sizes

### Code Organization
- **Clear Structure**: Logical separation of concerns
- **Type Safety**: Proper TypeScript integration
- **Consistent Styling**: Follows Nuxt UI Pro design patterns
- **Performance**: Efficient rendering and state management

This component exemplifies modern Vue 3 + Nuxt UI Pro patterns for building rich, interactive email interfaces with comprehensive functionality and excellent user experience.

---

```vue
<script setup lang="ts">
import { format } from 'date-fns'
import type { Mail } from '~/types'

// Component props - mail object is required for rendering
defineProps<{
  mail: Mail
}>()

// Emit close event to parent component
const emits = defineEmits(['close'])

// Dropdown menu configuration for mail actions
const dropdownItems = [[{
  label: 'Mark as unread',
  icon: 'i-lucide-check-circle'
}, {
  label: 'Mark as important',
  icon: 'i-lucide-triangle-alert'
}], [{
  label: 'Star thread',
  icon: 'i-lucide-star'
}, {
  label: 'Mute thread',
  icon: 'i-lucide-circle-pause'
}]]

// Toast notifications for user feedback
const toast = useToast()

// Reply form state management
const reply = ref('')
const loading = ref(false)

// Handle reply form submission
function onSubmit() {
  loading.value = true

  // Simulate API call with timeout
  setTimeout(() => {
    reply.value = ''

    // Show success toast notification
    toast.add({
      title: 'Email sent',
      description: 'Your email has been sent successfully',
      icon: 'i-lucide-check-circle',
      color: 'success'
    })

    loading.value = false
  }, 1000)
}
</script>

<template>
  <!-- Main dashboard panel container -->
  <UDashboardPanel id="inbox-2">
    <!-- Header navbar with mail subject and actions -->
    <UDashboardNavbar :title="mail.subject" :toggle="false">
      <!-- Close button in leading slot -->
      <template #leading>
        <UButton
          icon="i-lucide-x"
          color="neutral"
          variant="ghost"
          class="-ms-1.5"
          @click="emits('close')"
        />
      </template>

      <!-- Action buttons in right slot -->
      <template #right>
        <!-- Archive action with tooltip -->
        <UTooltip text="Archive">
          <UButton
            icon="i-lucide-inbox"
            color="neutral"
            variant="ghost"
          />
        </UTooltip>

        <!-- Reply action with tooltip -->
        <UTooltip text="Reply">
          <UButton icon="i-lucide-reply" color="neutral" variant="ghost" />
        </UTooltip>

        <!-- More actions dropdown menu -->
        <UDropdownMenu :items="dropdownItems">
          <UButton
            icon="i-lucide-ellipsis-vertical"
            color="neutral"
            variant="ghost"
          />
        </UDropdownMenu>
      </template>
    </UDashboardNavbar>

    <!-- Mail header with sender information and date -->
    <div class="flex flex-col sm:flex-row justify-between gap-1 p-4 sm:px-6 border-b border-default">
      <div class="flex items-start gap-4 sm:my-1.5">
        <!-- Sender avatar -->
        <UAvatar
          v-bind="mail.from.avatar"
          :alt="mail.from.name"
          size="3xl"
        />

        <!-- Sender name and email -->
        <div class="min-w-0">
          <p class="font-semibold text-highlighted">
            {{ mail.from.name }}
          </p>
          <p class="text-muted">
            {{ mail.from.email }}
          </p>
        </div>
      </div>

      <!-- Formatted date display -->
      <p class="max-sm:pl-16 text-muted text-sm sm:mt-2">
        {{ format(new Date(mail.date), 'dd MMM HH:mm') }}
      </p>
    </div>

    <!-- Mail body content area -->
    <div class="flex-1 p-4 sm:p-6 overflow-y-auto">
      <p class="whitespace-pre-wrap">
        {{ mail.body }}
      </p>
    </div>

    <!-- Reply section -->
    <div class="pb-4 px-4 sm:px-6 shrink-0">
      <!-- Reply card with subtle styling -->
      <UCard variant="subtle" class="mt-auto" :ui="{ header: 'flex items-center gap-1.5 text-dimmed' }">
        <!-- Reply header with icon and recipient info -->
        <template #header>
          <UIcon name="i-lucide-reply" class="size-5" />

          <span class="text-sm truncate">
            Reply to {{ mail.from.name }} ({{ mail.from.email }})
          </span>
        </template>

        <!-- Reply form -->
        <form @submit.prevent="onSubmit">
          <!-- Reply textarea with autoresize -->
          <UTextarea
            v-model="reply"
            color="neutral"
            variant="none"
            required
            autoresize
            placeholder="Write your reply..."
            :rows="4"
            :disabled="loading"
            class="w-full"
            :ui="{ base: 'p-0 resize-none' }"
          />

          <!-- Form actions -->
          <div class="flex items-center justify-between">
            <!-- Attachment button -->
            <UTooltip text="Attach file">
              <UButton
                color="neutral"
                variant="ghost"
                icon="i-lucide-paperclip"
              />
            </UTooltip>

            <!-- Submit actions -->
            <div class="flex items-center justify-end gap-2">
              <!-- Save draft button -->
              <UButton
                color="neutral"
                variant="ghost"
                label="Save draft"
              />
              <!-- Send button with loading state -->
              <UButton
                type="submit"
                color="neutral"
                :loading="loading"
                label="Send"
                icon="i-lucide-send"
              />
            </div>
          </div>
        </form>
      </UCard>
    </div>
  </UDashboardPanel>
</template>
```
