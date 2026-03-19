---
description: Guide for replacing Argo's mock data with real Supabase queries. Covers ArgoContext, auth, and each data type (spaces, chats, artifacts, files, members). Use when a frontend engineer is ready to connect the backend.
---

# Connect Argo to Supabase

This guide replaces all mock data in `src/context/ArgoContext.tsx` with real Supabase queries.

Read `src/context/ArgoContext.tsx` and `src/integrations/supabase/client.ts` first.

---

## Step 1 — Remove the isLoading demo toggle

Find this block (marked `// DEMO ONLY`) and remove it:
```typescript
// DEMO ONLY — simulates data loading for prototype. Remove when connecting Supabase.
const [isLoading, setIsLoading] = useState(true);
useEffect(() => {
  const timer = setTimeout(() => setIsLoading(false), 1200);
  return () => clearTimeout(timer);
}, []);
```

Replace with real async loading state that reflects actual data fetching.

---

## Step 2 — Auth

Replace the hardcoded user (`Pranav Nagrani`) with Supabase auth:

```typescript
import { supabase } from '@/integrations/supabase/client';

const [user, setUser] = useState(null);
useEffect(() => {
  supabase.auth.getUser().then(({ data }) => setUser(data.user));
  const { data: listener } = supabase.auth.onAuthStateChange((_, session) => {
    setUser(session?.user ?? null);
  });
  return () => listener.subscription.unsubscribe();
}, []);
```

---

## Step 3 — Spaces (Projects)

Replace `useState([...mock spaces])` with:

```typescript
const [spaces, setSpaces] = useState<Space[]>([]);
useEffect(() => {
  supabase
    .from('spaces')
    .select('*')
    .order('created_at', { ascending: false })
    .then(({ data }) => setSpaces(data ?? []));
}, []);
```

Update `createSpace`, `updateSpace` to call Supabase mutations and update local state optimistically.

---

## Step 4 — Chats

Replace mock chats with:
```typescript
supabase.from('chats').select('*, messages(*)').eq('space_id', activeSpaceId)
```

Update `createChat`, `sendMessage`, `renameChat` to write to Supabase.

---

## Step 5 — Artifacts

Replace mock artifacts with:
```typescript
supabase.from('artifacts').select('*').order('created_at', { ascending: false })
```

---

## Step 6 — Files

Replace `MOCK_PROJECT_FILES` in `RightPanel.tsx` with:
```typescript
supabase.storage.from('project-files').list(`${spaceId}/`)
```

Update upload handler to call `supabase.storage.from('project-files').upload(...)`.

---

## Step 7 — Members

Replace `shareMembers` mock array in `SpaceWorkspaceView.tsx` with:
```typescript
supabase.from('space_members').select('*, profiles(*)').eq('space_id', spaceId)
```

---

## Step 8 — Real-time (optional, post-MVP)

Add Supabase Realtime subscriptions for live chat updates:
```typescript
supabase.channel('chat-updates')
  .on('postgres_changes', { event: 'INSERT', schema: 'public', table: 'messages' }, handleNewMessage)
  .subscribe();
```

---

## Checklist

- [ ] `isLoading` DEMO toggle removed
- [ ] Auth connected
- [ ] Spaces CRUD connected
- [ ] Chats + messages connected
- [ ] Artifacts connected
- [ ] Files connected (Supabase Storage)
- [ ] Members connected
- [ ] Error states wired (show error UI when queries fail)
- [ ] Loading states wired to actual async state
- [ ] All `// DEMO ONLY` comments removed
