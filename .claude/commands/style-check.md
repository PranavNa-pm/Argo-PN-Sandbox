---
description: Check that a component uses only design system tokens — no hardcoded colors, correct font usage, proper spacing scale, right border radius. Use before committing any new component or after a Lovable sync.
argument-hint: [file path or component name]
---

# Argo Style System Check

Check $ARGUMENTS for design token compliance. If no argument given, check recently changed files using `git diff --name-only HEAD~1`.

Read `src/index.css` first to see the full token set. Then read the target file(s) and check every rule.

---

## Color Tokens — NEVER hardcode hex, rgb, or hsl values

Allowed patterns:
- `text-foreground`, `text-muted-foreground`, `text-primary`
- `bg-background`, `bg-card`, `bg-secondary`, `bg-accent`, `bg-muted`
- `border-border`, `ring-ring`
- `text-destructive`, `bg-destructive`
- Opacity variants: `bg-black/5`, `bg-white/40`, `bg-accent/50`

**Flag immediately:**
- Any `#` hex color in className
- Any `rgb(` or `hsl(` in className (outside of index.css)
- Any `bg-blue-*`, `bg-gray-*`, `bg-slate-*`, `text-gray-*` etc. — use tokens instead
- Any `text-[#...]` or `bg-[#...]` arbitrary Tailwind values

**Exceptions (allowed in index.css only):**
- CSS variable definitions: `--background: 40 10% 98%;`

---

## Typography Tokens

- Font: `font-family` must come from `body` in index.css (Geist). No inline font overrides.
- Sizes: use Tailwind scale — `text-xs`, `text-sm`, `text-base`, `text-lg`, `text-xl`
- Custom sizes only for small metadata: `text-[10px]`, `text-[11px]` — nothing else arbitrary
- Weights: `font-normal`, `font-medium`, `font-semibold`, `font-bold` only
- Line height: use Tailwind defaults. No arbitrary `leading-[...]` values.
- Tracking: `tracking-tight` for headings, `tracking-wide` for uppercase labels only

---

## Spacing Scale

- Use Tailwind spacing scale: `p-1`, `p-2`, `p-3`, `p-4`, `p-6`, `p-8` etc.
- Gaps: `gap-1`, `gap-1.5`, `gap-2`, `gap-3`, `gap-4`
- Flagged: arbitrary values like `p-[13px]`, `gap-[7px]` — find nearest scale value

---

## Border Radius

- Buttons, inputs, list rows: `rounded-lg`
- Modals, panels: `rounded-xl`
- Small badges/chips: `rounded-full` or `rounded`
- Never arbitrary: `rounded-[6px]`

---

## Shadows

- List rows: NO shadow (they blend into background)
- Modals: `shadow-xl`
- Dropdowns: default shadcn shadow
- Never arbitrary shadow values

---

## Primary Color Usage

- Primary color is teal: `hsl(193, 75%, 42%)` — accessed via `bg-primary`, `text-primary`
- Active states, selected filters: `bg-accent`
- Destructive/error: `text-destructive`, `bg-destructive`
- Never use a color for decoration — color must communicate meaning

---

## CSS Variable Cross-Check

Compare usage in file against tokens defined in `src/index.css`:
- Every `bg-*` token used should have a corresponding `--*` variable
- Flag any Tailwind class that looks like a design token but isn't in `index.css`

---

## Output Format

List each violation as:
`LINE [N]: [class or value] — [why it's wrong] → [correct replacement]`

Then summary: X violations found, Y files clean.
