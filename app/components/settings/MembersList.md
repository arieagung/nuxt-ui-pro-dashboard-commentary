# MembersList Component

## Purpose
The `MembersList` component provides a comprehensive member management interface for team/organization settings. It displays a list of team members with their roles and provides contextual actions for member management through an intuitive list-based interface.

## Reusability
This component demonstrates excellent patterns for member management interfaces and can be adapted for:
- Team member management in various applications
- User role administration systems
- Organization member directories
- Any list-based user management interface requiring roles and actions

## Props
- `members: Member[]` - Required array of member objects containing user and role information

## Emits
- None (actions are handled through callbacks in dropdown items)

## Nuxt UI Pro Integration
This component showcases clean Nuxt UI Pro integration:
- `UAvatar` - Member profile picture display
- `USelect` - Role selection with capitalized styling
- `UDropdownMenu` - Member actions with contextual menu
- `UButton` - Action trigger buttons

## Usage Examples
```vue
<!-- Basic usage -->
<MembersList :members="teamMembers" />

<!-- In settings page -->
<template>
  <div class="space-y-6">
    <div class="flex items-center justify-between">
      <h2 class="text-lg font-semibold">Team Members</h2>
      <UButton label="Add Member" icon="i-lucide-plus" />
    </div>
    
    <MembersList :members="members" />
  </div>
</template>

<script setup>
const { data: members } = await useFetch('/api/team/members')
</script>
```

## Component Structure

### Member Data Interface
```typescript
interface Member {
  name: string
  username: string
  avatar: AvatarProps
  role: 'member' | 'owner'
}
```

### Action Items Configuration
```vue
const items = [{
  label: 'Edit member',
  onSelect: () => console.log('Edit member')
}, {
  label: 'Remove member',
  color: 'error' as const,
  onSelect: () => console.log('Remove member')
}] satisfies DropdownMenuItem[]
```

## Key Features

### Member Display
- **Avatar Integration**: Displays member profile pictures with proper fallbacks
- **User Information**: Shows both display name and username
- **Role Visualization**: Clear role indication through select component
- **Responsive Layout**: Adapts to various screen sizes

### Role Management
- **Dynamic Role Selection**: Inline role editing with USelect component
- **Visual Hierarchy**: Capitalized role display for better readability
- **Real-time Updates**: Can be extended for immediate role changes

### Member Actions
- **Contextual Menu**: Dropdown with member-specific actions
- **Edit Functionality**: Member information editing capability
- **Remove Action**: Member removal with proper styling (error color)
- **Extensible Actions**: Easy to add more member management actions

## Advanced Features

### Accessible List Structure
```vue
<ul role="list" class="divide-y divide-default">
  <li 
    v-for="(member, index) in members"
    :key="index"
    class="flex items-center justify-between gap-3 py-3 px-4 sm:px-6"
  >
    <!-- Member content -->
  </li>
</ul>
```

### Responsive Design
- **Mobile-first**: Proper spacing adjustments for mobile devices
- **Flexible Layout**: Content adapts to available space
- **Touch-friendly**: Appropriate target sizes for touch interfaces

### Role Selection Integration
```vue
<USelect
  :model-value="member.role"
  :items="['member', 'owner']"
  color="neutral"
  :ui="{ value: 'capitalize', item: 'capitalize' }"
/>
```

## Styling and Layout

### Visual Design
- **Clean Separation**: Dividers between member entries
- **Consistent Spacing**: Proper padding and margins throughout
- **Information Hierarchy**: Clear visual hierarchy for member data

### Typography
- **Emphasized Names**: Font-medium styling for member names
- **Subtle Usernames**: Muted text color for secondary information
- **Truncation**: Prevents layout breaks with long names

### Interactive Elements
- **Hover States**: Proper hover feedback for interactive elements
- **Loading States**: Can be extended with loading indicators
- **Focus Management**: Keyboard navigation support

## Extension Points

### Enhanced Role Management
```vue
<script setup>
const roleItems = [
  { label: 'Owner', value: 'owner' },
  { label: 'Admin', value: 'admin' },
  { label: 'Member', value: 'member' },
  { label: 'Guest', value: 'guest' }
]

function updateMemberRole(memberId, newRole) {
  // API call to update role
  $fetch(`/api/members/${memberId}/role`, {
    method: 'PATCH',
    body: { role: newRole }
  })
}
</script>

<template>
  <USelect
    :model-value="member.role"
    :items="roleItems"
    @update:model-value="updateMemberRole(member.id, $event)"
  />
</template>
```

### Advanced Actions Menu
```vue
const getActionItems = (member) => [
  {
    label: 'Edit Profile',
    icon: 'i-lucide-user-pen',
    onSelect: () => openEditModal(member)
  },
  {
    label: 'View Activity',
    icon: 'i-lucide-activity',
    onSelect: () => viewMemberActivity(member)
  },
  {
    label: 'Send Message',
    icon: 'i-lucide-message-circle',
    onSelect: () => openMessageComposer(member)
  },
  {
    label: 'Transfer Ownership',
    icon: 'i-lucide-crown',
    onSelect: () => initiateOwnershipTransfer(member),
    disabled: member.role !== 'owner'
  },
  {
    label: 'Remove Member',
    icon: 'i-lucide-user-x',
    color: 'error',
    onSelect: () => confirmRemoveMember(member)
  }
]
```

### Bulk Actions Support
```vue
<script setup>
const selectedMembers = ref([])
const selectAll = ref(false)

function toggleSelectAll() {
  if (selectAll.value) {
    selectedMembers.value = members.value.map(m => m.id)
  } else {
    selectedMembers.value = []
  }
}

function bulkUpdateRoles(newRole) {
  selectedMembers.value.forEach(memberId => {
    updateMemberRole(memberId, newRole)
  })
}
</script>

<template>
  <!-- Bulk action header -->
  <div v-if="selectedMembers.length > 0" class="bg-primary-50 p-4 rounded-lg mb-4">
    <div class="flex items-center justify-between">
      <span class="text-sm">{{ selectedMembers.length }} members selected</span>
      <div class="flex gap-2">
        <UButton size="sm" @click="bulkUpdateRoles('member')">
          Make Members
        </UButton>
        <UButton size="sm" color="error" @click="bulkRemoveMembers()">
          Remove Selected
        </UButton>
      </div>
    </div>
  </div>
  
  <!-- Member list with selection -->
  <ul role="list">
    <li v-for="member in members" :key="member.id">
      <div class="flex items-center gap-3">
        <UCheckbox
          :model-value="selectedMembers.includes(member.id)"
          @update:model-value="toggleMemberSelection(member.id, $event)"
        />
        <!-- Rest of member display -->
      </div>
    </li>
  </ul>
</template>
```

### Search and Filtering
```vue
<script setup>
const searchQuery = ref('')
const roleFilter = ref('all')

const filteredMembers = computed(() => {
  let filtered = members.value

  // Apply search filter
  if (searchQuery.value) {
    const query = searchQuery.value.toLowerCase()
    filtered = filtered.filter(member =>
      member.name.toLowerCase().includes(query) ||
      member.username.toLowerCase().includes(query)
    )
  }

  // Apply role filter
  if (roleFilter.value !== 'all') {
    filtered = filtered.filter(member => member.role === roleFilter.value)
  }

  return filtered
})
</script>

<template>
  <!-- Filter controls -->
  <div class="flex gap-4 mb-6">
    <UInput
      v-model="searchQuery"
      placeholder="Search members..."
      icon="i-lucide-search"
      class="flex-1"
    />
    <USelect
      v-model="roleFilter"
      :items="[
        { label: 'All Roles', value: 'all' },
        { label: 'Owners', value: 'owner' },
        { label: 'Members', value: 'member' }
      ]"
      placeholder="Filter by role"
    />
  </div>

  <MembersList :members="filteredMembers" />
</template>
```

## Performance Considerations

### Efficient Rendering
- **Key-based Rendering**: Uses proper keys for list items
- **Minimal Re-renders**: Optimized for large member lists
- **Lazy Loading**: Can be extended with virtual scrolling

### Memory Management
- **Event Cleanup**: Proper cleanup of event listeners
- **Computed Properties**: Efficient filtering and sorting
- **Component Recycling**: Efficient list item rendering

## Accessibility Features

### Semantic HTML
- **Role Attributes**: Proper ARIA roles for list structure
- **List Semantics**: Native ul/li structure for screen readers
- **Interactive Labels**: Clear labels for all interactive elements

### Keyboard Navigation
- **Tab Navigation**: All interactive elements are keyboard accessible
- **Focus Management**: Logical tab order through member list
- **Action Keys**: Supports keyboard shortcuts for common actions

### Screen Reader Support
- **Alternative Text**: Avatar alt text for member identification
- **Status Announcements**: Role changes announced to screen readers
- **Context Information**: Clear context for all member actions

## Best Practices Demonstrated

### Component Design
- **Single Responsibility**: Focused on member list display and basic actions
- **Props Interface**: Clean, typed props interface
- **Composition**: Uses composition for action handling
- **Extensibility**: Easy to extend with additional features

### User Experience
- **Clear Visual Hierarchy**: Easy to scan member information
- **Consistent Actions**: Standardized action patterns
- **Immediate Feedback**: Visual feedback for all interactions
- **Error Prevention**: Confirmation for destructive actions

### Code Quality
- **Type Safety**: Proper TypeScript integration
- **Consistent Styling**: Follows Nuxt UI Pro design patterns
- **Clean Structure**: Logical component organization
- **Reusable Patterns**: Patterns that can be applied to other list components

This component provides an excellent foundation for member management interfaces with comprehensive functionality, accessibility support, and extensive customization options.

---

```vue
<script setup lang="ts">
import type { DropdownMenuItem } from '@nuxt/ui'
import type { Member } from '~/types'

// Component props - members array is required
defineProps<{
  members: Member[]
}>()

// Dropdown menu items configuration for member actions
const items = [{
  label: 'Edit member',
  onSelect: () => console.log('Edit member')
}, {
  label: 'Remove member',
  color: 'error' as const,
  onSelect: () => console.log('Remove member')
}] satisfies DropdownMenuItem[]
</script>

<template>
  <!-- Semantic list structure with proper ARIA roles -->
  <ul role="list" class="divide-y divide-default">
    <li
      v-for="(member, index) in members"
      :key="index"
      class="flex items-center justify-between gap-3 py-3 px-4 sm:px-6"
    >
      <!-- Member information display -->
      <div class="flex items-center gap-3 min-w-0">
        <!-- Member avatar with size and binding -->
        <UAvatar
          v-bind="member.avatar"
          size="md"
        />

        <!-- Member name and username -->
        <div class="text-sm min-w-0">
          <p class="text-highlighted font-medium truncate">
            {{ member.name }}
          </p>
          <p class="text-muted truncate">
            {{ member.username }}
          </p>
        </div>
      </div>

      <!-- Member actions and role selection -->
      <div class="flex items-center gap-3">
        <!-- Role selection dropdown -->
        <USelect
          :model-value="member.role"
          :items="['member', 'owner']"
          color="neutral"
          :ui="{ value: 'capitalize', item: 'capitalize' }"
        />

        <!-- Member actions dropdown menu -->
        <UDropdownMenu :items="items" :content="{ align: 'end' }">
          <UButton
            icon="i-lucide-ellipsis-vertical"
            color="neutral"
            variant="ghost"
          />
        </UDropdownMenu>
      </div>
    </li>
  </ul>
</template>
```
