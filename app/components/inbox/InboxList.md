# InboxList Component

## Purpose
The `InboxList` component displays a list of emails with selection functionality, keyboard navigation, and smart scrolling. It provides an intuitive email management interface similar to modern email clients.

## Reusability
This component serves as an excellent template for any list-based selection interface. The keyboard navigation, auto-scrolling, and selection patterns can be adapted for file browsers, contact lists, task managers, and other list-based UIs.

## Props
- `mails: Mail[]` - Array of mail objects to display

## Emits
- None (uses `defineModel` for reactive two-way binding)

## Model
- `v-model: Mail | null` - Currently selected mail item

## Slots
- None (uses structured content rendering)

## Nuxt UI Pro Integration
This component utilizes:
- `UChip` - Unread indicators for emails
- Integrates with Nuxt UI Pro styling patterns and color system
- Uses consistent border and background styling

## Usage Examples
```vue
<!-- Basic usage -->
<InboxList v-model="selectedMail" :mails="mailList" />

<!-- In inbox layout -->
<template>
  <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
    <div class="lg:col-span-1">
      <InboxList v-model="selectedMail" :mails="mails" />
    </div>
    <div class="lg:col-span-2">
      <InboxMail v-if="selectedMail" :mail="selectedMail" @close="selectedMail = null" />
    </div>
  </div>
</template>

<script setup>
const selectedMail = ref(null)
const { data: mails } = useFetch('/api/mails')
</script>
```

## Component Relationships
- Integrates with `date-fns` for date formatting (`format`, `isToday`)
- Works with `InboxMail` component for mail display
- Uses template refs for scroll management
- Implements keyboard shortcuts using `defineShortcuts`

## State Management
- Uses `defineModel` for reactive selection state
- Manages template refs array for scroll positioning
- Handles keyboard navigation state
- Maintains visual selection indicators

## Key Features

### Keyboard Navigation
```typescript
defineShortcuts({
  arrowdown: () => {
    const index = props.mails.findIndex(mail => mail.id === selectedMail.value?.id)
    if (index === -1) {
      selectedMail.value = props.mails[0]
    } else if (index < props.mails.length - 1) {
      selectedMail.value = props.mails[index + 1]
    }
  },
  arrowup: () => {
    // Similar logic for up navigation
  }
})
```

### Auto-scroll Functionality
```typescript
watch(selectedMail, () => {
  if (!selectedMail.value) return
  
  const ref = mailsRefs.value[selectedMail.value.id]
  if (ref) {
    ref.scrollIntoView({ block: 'nearest' })
  }
})
```

### Smart Date Display
```vue
{{ isToday(new Date(mail.date)) 
  ? format(new Date(mail.date), 'HH:mm') 
  : format(new Date(mail.date), 'dd MMM') }}
```

### Visual State Management
- **Selection Highlighting**: Border and background changes for selected items
- **Unread Indicators**: Bold text and chip indicators for unread emails
- **Hover Effects**: Interactive feedback on hover
- **Typography Hierarchy**: Clear sender/subject/body structure

## Advanced Features

### Template Refs Management
```typescript
const mailsRefs = ref<Element[]>([])

// In template:
:ref="el => { mailsRefs[mail.id] = el as Element }"
```

### Conditional Styling
```vue
:class="[
  mail.unread ? 'text-highlighted' : 'text-toned',
  selectedMail && selectedMail.id === mail.id 
    ? 'border-primary bg-primary/10' 
    : 'border-(--ui-bg) hover:border-primary hover:bg-primary/5'
]"
```

### Content Truncation
- **Subject Lines**: Truncated with ellipsis for consistent layout
- **Body Preview**: Line-clamped preview text
- **Responsive Text**: Adapts to different screen sizes

## Accessibility Features
- **Keyboard Navigation**: Full arrow key support
- **Screen Reader**: Proper mail structure and labeling
- **Focus Management**: Visual focus indicators
- **Semantic HTML**: Meaningful content structure

## Performance Considerations
- **Efficient Scrolling**: Uses `scrollIntoView` with `block: 'nearest'`
- **Reactive Updates**: Minimal re-renders on selection changes
- **Memory Management**: Proper cleanup of template refs
- **Optimized Rendering**: Efficient list virtualization patterns

## Extension Points
This component can be enhanced with:
- Virtual scrolling for large lists
- Multi-selection support
- Drag and drop functionality
- Context menus
- Email actions (archive, delete, mark as read)
- Search and filtering
- Sorting options
- Pagination support

## Integration Patterns

### Email Client Pattern
```vue
<template>
  <div class="inbox-layout">
    <InboxList 
      v-model="selectedMail" 
      :mails="filteredMails"
      class="inbox-sidebar" 
    />
    <InboxMail 
      v-if="selectedMail"
      :mail="selectedMail"
      @close="selectedMail = null"
      class="inbox-content"
    />
  </div>
</template>
```

## Best Practices Demonstrated
- **Keyboard Accessibility**: Complete keyboard navigation implementation
- **Visual Feedback**: Clear selection and hover states
- **Performance**: Efficient scrolling and rendering
- **User Experience**: Intuitive email client patterns
- **Maintainability**: Clean separation of concerns and reusable patterns

This component exemplifies modern list interface design with excellent accessibility and user experience patterns.
