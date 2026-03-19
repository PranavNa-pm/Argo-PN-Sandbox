---
description: Review a component or page against Argo's established UI patterns. Flags anything that breaks consistency — lists, hover states, backgrounds, icons, spacing, empty states, loading states. Use after building or editing any UI component.
argument-hint: [component file path or component name]
---

# Argo Design Review

Review $ARGUMENTS against Argo's UI design system. If no argument given, review recently changed files using `git diff --name-only HEAD~1`.

Read the full file(s) before reviewing. Then check every rule below.

---

## 1. List Views (Projects, Artifacts, Chats)

- [ ] Uses borderless rows — NOT `<table>`, NOT card borders, NOT shadows on rows
- [ ] Row base: no background (`bg-transparent`), NOT `bg-card`, NOT `bg-white`
- [ ] Hover: `hover:bg-black/5` only — no other hover background
- [ ] Rows are `<button>` elements (clickable) with `text-left`
- [ ] Actions on rows are `opacity-0 group-hover:opacity-100` (revealed on hover only)
- [ ] No pagination — uses IntersectionObserver infinite scroll
- [ ] Count shown as "Showing X of Y projects" (filtered) or "X projects" (unfiltered)
- [ ] Sortable column headers above the list (small, uppercase, `text-[11px]`)
- [ ] Search bar uses `bg-secondary/50 border border-border rounded-lg px-3 py-2`

## 2. Backgrounds & Surfaces

- [ ] Page/canvas background: `bg-background` (hsl 40 10% 98% — warm off-white). Never hardcoded hex or blue tint.
- [ ] Sidebar: `bg-sidebar-background` (hsl 40 8% 94% — slightly deeper warm gray)
- [ ] Chat area: `bg-card` (pure white) — ONLY the chat view, not other pages
- [ ] Modals/popovers: `bg-background` with `border border-border shadow-xl rounded-xl`
- [ ] NO hardcoded background colors. Only CSS variables via Tailwind tokens.

## 3. Icons

- [ ] Projects (sidebar + detail): `FolderOpen` from lucide-react
- [ ] Artifacts (sidebar): `Layers` from lucide-react
- [ ] General Chat (sidebar): `MessagesSquare` from lucide-react
- [ ] Visibility: `Globe` for shared, `Lock` for private — icon only, NO text badge
- [ ] Action buttons: icon only (no text) unless it's a primary CTA button
- [ ] Icon sizes: `w-3.5 h-3.5` for row icons, `w-4 h-4` for header/nav, `w-5 h-5` for empty states
- [ ] Every icon button has a `title` attribute for tooltip on hover

## 4. Typography & Copy

- [ ] Font family: Geist only (`font-family: 'Geist'`). No system fonts in headings.
- [ ] Page titles: `text-lg font-semibold text-foreground tracking-tight`
- [ ] Section subtitles: `text-sm text-muted-foreground`
- [ ] Row primary text: `text-sm font-medium text-foreground`
- [ ] Row secondary/metadata: `text-xs text-muted-foreground`
- [ ] Column headers: `text-[11px] font-medium text-foreground/60 uppercase tracking-wide`
- [ ] All copy is plain English — no jargon, no ALL CAPS labels, no "N/A"
- [ ] Visibility badges use icon only — NOT "Private" or "Shared" text

## 5. Filters & Search

- [ ] Filters are inline tab pills — NOT a dropdown
- [ ] Active filter: `bg-accent text-foreground font-medium`
- [ ] Inactive filter: `text-muted-foreground hover:text-foreground hover:bg-accent/50`
- [ ] NO type-based filters (Markdown / HTML / PPTX etc.) — these don't scale
- [ ] Changing filter resets display count back to default (20)

## 6. Buttons & CTAs

- [ ] Primary CTA: `bg-primary text-primary-foreground hover:bg-primary/90 rounded-lg`
- [ ] Secondary/outlined: `border border-border text-muted-foreground hover:text-foreground hover:bg-accent`
- [ ] Destructive: `text-destructive hover:bg-destructive/10`
- [ ] Send button in chat: `bg-primary text-primary-foreground rounded-lg p-2 hover:bg-primary/90 disabled:opacity-40`
- [ ] No button uses hardcoded color classes (e.g. `bg-blue-500`) — only token-based

## 7. Layout & Spacing

- [ ] All content pages use `max-w-4xl mx-auto` — NEVER full width
- [ ] Desktop only — NO `sm:`, `md:`, `lg:`, `xl:` breakpoint classes anywhere
- [ ] Page padding: `p-6` on the outer container
- [ ] Gap between rows: `space-y-0.5` or `space-y-1`
- [ ] Header row (title + action button): `flex items-start justify-between`

## 8. Empty States

- [ ] Every list view has an empty state (shown when list is empty and not loading)
- [ ] Empty state: icon (`text-muted-foreground/40`) + primary message + optional helper text
- [ ] Centered with `flex flex-col items-center justify-center py-20 text-center`
- [ ] Different message for "no results" (search/filter) vs "nothing exists yet"

## 9. Loading States

- [ ] Every list view shows skeleton rows while `isLoading` is true
- [ ] Uses `<ListRowSkeleton />` from `src/components/argo/skeletons/ListRowSkeleton.tsx`
- [ ] Chat view uses `<ChatMessageSkeleton />` while loading
- [ ] NOTE: `isLoading` in ArgoContext is a demo toggle — marked "DEMO ONLY — remove when connecting Supabase"

## 10. Hover Rules

- [ ] Every interactive element has a visible hover state
- [ ] List rows: `hover:bg-black/5`
- [ ] Icon buttons: `hover:bg-accent hover:text-foreground`
- [ ] Text links: `hover:underline` or `hover:text-foreground`
- [ ] Sidebar items: `hover:bg-accent/60`
- [ ] No hover state should use a completely different color — same hue, lighter/darker

---

## Output Format

Report findings as:

**PASS** — rule satisfied
**FAIL [rule name]** — rule violated, with the exact line and suggested fix
**WARN [rule name]** — not technically wrong but worth flagging

Group by section. End with a summary count: X passed, Y failed, Z warnings.
