---
description: Add skeleton loading state to a list view component. Wires up ListRowSkeleton to the isLoading flag from ArgoContext. Use when adding a new list page or when a list view is missing a loading state.
argument-hint: [component file path]
---

# Add Skeleton Loading State

Add a skeleton loading state to $ARGUMENTS.

Read the file first. Then:

1. Check if `isLoading` is already imported from `useArgo()` — if not, add it
2. Check if `ListRowSkeleton` is already imported — if not, add:
   `import { ListRowSkeleton } from '@/components/argo/skeletons/ListRowSkeleton';`
3. Find the list render block (where rows/items are mapped)
4. Wrap it in a conditional:

```tsx
{isLoading ? (
  // Skeleton rows while data loads
  // DEMO ONLY — replace isLoading with real async state when connecting Supabase
  <div className="space-y-1">
    {[...Array(4)].map((_, i) => <ListRowSkeleton key={i} />)}
  </div>
) : (
  // ... existing list render
)}
```

5. For chat views, use `ChatMessageSkeleton` instead:
   `import { ChatMessageSkeleton } from '@/components/argo/skeletons/ChatMessageSkeleton';`

6. Run `npm run build` to verify no TypeScript errors

7. Report: which file was changed, which lines were added, and confirm build passes.

Note: The `isLoading` state in ArgoContext currently simulates a 1.2s load for the Lovable prototype. It is marked `// DEMO ONLY` — a frontend engineer will replace it with real async loading from Supabase.
