---
description: Add a consistent empty state to a list view component. Follows Argo's empty state pattern — centered icon, primary message, optional helper text. Use when adding a new list page or when a list is missing an empty state.
argument-hint: [component file path]
---

# Add Empty State

Add an empty state to $ARGUMENTS.

Read the file first. Identify:
- What the list contains (projects, artifacts, chats, files, members)
- What filter/search state variables exist
- Where the empty list condition should be checked

Then add the following pattern where the list renders (after the skeleton check, before the map):

```tsx
{!isLoading && items.length === 0 && (
  <div className="flex flex-col items-center justify-center py-20 text-center">
    <ICON className="w-8 h-8 text-muted-foreground/40 mb-3" />
    <p className="text-sm font-medium text-muted-foreground">
      {hasActiveFilter ? 'No [items] match your filter' : 'No [items] yet'}
    </p>
    {!hasActiveFilter && (
      <p className="text-xs text-muted-foreground/60 mt-1">
        [Helpful next step or context]
      </p>
    )}
  </div>
)}
```

Icon choices by content type:
- Projects → `FolderOpen` from lucide-react
- Artifacts → `Layers` from lucide-react
- Chats → `MessageSquare` from lucide-react
- Files → `Upload` from lucide-react
- Members → `Users` from lucide-react
- Search results → `Search` from lucide-react

Helper text examples:
- Projects: "Create your first project to get started"
- Artifacts: "Artifacts are generated from your chats"
- Chats: "Start a new chat to begin working on this project"
- Files: "Upload files to share context with your team"

`hasActiveFilter` should be `true` when a search query or non-"all" filter is active.

Run `npm run build` after changes. Report what was added and confirm build passes.
