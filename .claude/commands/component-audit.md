---
description: Audit a React component for structure, naming, props, state management, and handoff readiness. Use before handing off to a frontend engineer or before a Lovable sync.
argument-hint: [file path or directory]
---

# Argo Component Audit

Audit $ARGUMENTS for React best practices and handoff readiness. If no argument given, audit all files in `src/components/argo/`.

Read each file fully before auditing. Check every rule below.

---

## 1. File Structure

- [ ] One component per file — no multiple exported components in one file (exception: tiny helper sub-components under 30 lines that are only used by the same file)
- [ ] File name matches component name exactly (PascalCase): `ChatView.tsx` exports `ChatView`
- [ ] No component defined inline inside another component's render function
- [ ] File has a clear single responsibility — name describes what it does

## 2. Exports

- [ ] Named export: `export function ComponentName()` — NOT `export default`
- [ ] Exception: page-level components in `src/pages/` may use default export
- [ ] Skeleton components in `src/components/argo/skeletons/` use named exports

## 3. Props

- [ ] Props are TypeScript typed with an inline interface or type above the component
- [ ] No `any` types on props
- [ ] Optional props have `?` and sensible defaults
- [ ] No prop drilling more than 2 levels — use context instead
- [ ] Props are descriptive: `onClose` not `handler`, `spaceName` not `name` if ambiguous

## 4. State Management

- [ ] Local UI state (open/closed, selected tab) → `useState` in component ✓
- [ ] Shared app state (spaces, chats, artifacts, user) → `useArgo()` context ✓
- [ ] No mock data defined inside components — mock data lives in `ArgoContext.tsx` only
- [ ] `isLoading` comes from `useArgo()` — NOT a local `useState(false)` in the component
- [ ] NOTE: `isLoading` in ArgoContext is DEMO ONLY — comment says "remove when connecting Supabase"

## 5. Hooks Usage

- [ ] `useEffect` has correct dependency arrays — no missing deps, no empty `[]` used to suppress warnings
- [ ] IntersectionObserver in `useEffect` returns a cleanup function (`return () => observer.disconnect()`)
- [ ] No `useEffect` used just to set state from props — use derived values instead
- [ ] `useRef` used for DOM references and scroll containers, not for storing state that should trigger re-renders

## 6. Naming Conventions

- [ ] Components: PascalCase (`WorkspaceDashboard`)
- [ ] Functions/handlers: camelCase, verb-first (`handleSend`, `openSpaceWorkspace`, `createChat`)
- [ ] Boolean state: `is` or `has` prefix (`isLoading`, `showModal`, `hasError`)
- [ ] Event handlers: `on` prefix in props (`onClose`, `onSave`), `handle` prefix in implementation
- [ ] Constants: SCREAMING_SNAKE_CASE for module-level (`PAGE_SIZE`, `MOCK_PROJECT_FILES`)

## 7. Comments & Documentation

- [ ] Complex logic has a brief comment explaining WHY (not what)
- [ ] Any DEMO/prototype code has a prominent comment: `// DEMO ONLY — remove when connecting Supabase`
- [ ] No commented-out code left in
- [ ] No TODO comments left without a ticket reference

## 8. Accessibility (Desktop App Minimum)

- [ ] All icon-only buttons have `title` attribute (shows tooltip on hover)
- [ ] Interactive elements are `<button>` not `<div onClick>`
- [ ] Inputs have associated labels or `placeholder` + `aria-label`
- [ ] Modal overlays trap focus (or use shadcn Dialog which handles this)

## 9. Import Hygiene

- [ ] No unused imports
- [ ] Lucide icons imported individually, not as `* as Icons`
- [ ] Path aliases used: `@/components/...`, `@/context/...` — NOT relative `../../`
- [ ] `cn` utility used for conditional classes — NOT string concatenation

## 10. Component Size

- [ ] Under 300 lines — if larger, suggest what to extract
- [ ] No function longer than 60 lines — suggest extracting helpers

---

## Output Format

For each file audited:

**File:** `src/components/argo/ComponentName.tsx`
- PASS / FAIL for each rule that applies
- For FAIL: exact line number + what to fix

End with overall score and top 3 priority fixes.
