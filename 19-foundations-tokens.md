# Foundations: Extracted Token Values (Source Truth)

This file records the **real values** extracted from the product source, paired by theme.
Unlike `16-foundations-draft.md` (which captures observed *directions* and rules), this
file is the measured evidence layer: every value below comes directly from
`start-page-v8.css` (v8 semantic layer) or `common-ui.css` (legacy layer).

- Extraction date: 2026-06-07
- Sources: `start-page-v8.css` (512 KB), `common-ui.css` (21 KB)
- Themes: values are defined in `.light-theme{…}` and `.dark-theme{…}` blocks. 80 unique
  CSS custom properties carry a light/dark pair.

> Naming is **not** finalized here. These are the source variable names. The canonical
> design-system API names are proposed in the last section.

---

## 1. Color tokens (paired light / dark)

`—` means the token is defined only for that one theme in this export.

### Surfaces

| Source token | Light | Dark | Role |
|---|---|---|---|
| `--surface-surface` | `#fff` | `#2f2f2f` | Base app surface |
| `--surface-surface-0` | `—` | `#000` | Deepest surface (dark only) |
| `--surface-surface-02` | `#fff` | `#343434` | Raised surface |
| `--surface-surface-03` | `#fafafa` | `#2c2c2c` | Subtle alt surface |
| `--surface-surface-04` | `#fff` | `#343434` | Card/panel surface |
| `--surface-surface-05` | `#fafafa` | `#121212` | App background / deep |
| `--surface-surface-06` | `#f0f0f0` | `#2a2a2a` | Table header / inset |
| `--surface-surface-08` | `#fff` | `#353535` | Menu/popup surface |
| `--surface-surface-09` | `rgba(0,0,0,.02)` | `#333` | Hover surface tint |
| `--surface-surface-10` | `#fff` | `#2f2f2f` | Dialog surface |
| `--surface-surface-11` | `#fff` | `#1c1c1c` | Input title backdrop |
| `--surface-surface-12` | `rgba(0,0,0,.02)` | `#373737` | Subtle fill |

### Text

| Source token | Light | Dark | Role |
|---|---|---|---|
| `--text-text-primary` | `rgba(0,0,0,.87)` | `hsla(0,0%,100%,.87)` | Primary text |
| `--text-text-secondary` | `rgba(0,0,0,.54)` | `hsla(0,0%,100%,.54)` | Secondary text |
| `--text-text-secondary-hover` | `rgba(0,0,0,.87)` | `hsla(0,0%,100%,.87)` | Secondary text hover |
| `--text-text-secondary-disabled` | `rgba(0,0,0,.24)` | `hsla(0,0%,100%,.24)` | Disabled secondary |
| `--text-text-tetriary` *(sic)* | `rgba(0,0,0,.4)` | `hsla(0,0%,100%,.4)` | Tertiary / muted text |
| `--text-text-quaternary` | `rgba(0,0,0,.7)` | `hsla(0,0%,100%,.7)` | Quaternary text |
| `--text-text-hover` | `rgba(0,0,0,.8)` | `hsla(0,0%,100%,.8)` | Generic text hover |
| `--text-text-error` | `#eb5757` | `#fa7171` | Error text |
| `--text-text-inverse` | `#fff` | `#000` | Inverse text |
| `--text-text-selected` | `#2979ff` | `#639dff` | Selected/active label |
| `--text-text-selected-hover` | `#448aff` | `#448aff` | Selected label hover |
| `--text-text-selected-active` | `#2962ff` | `#2962ff` | Selected label active |
| `--text-link-upgrade` | `#2979ff` | `#639dff` | Upgrade/link emphasis |

### Background (fills)

| Source token | Light | Dark | Role |
|---|---|---|---|
| `--background-background-primary` | `#2979ff` | `#448aff` | **Brand primary fill** |
| `--background-background-primary-disabled` | `rgba(0,0,0,.08)` | `hsla(0,0%,100%,.08)` | Disabled primary |
| `--background-background-bold` | `rgba(0,0,0,.87)` | `hsla(0,0%,100%,.87)` | Bold fill |
| `--background-background-white` | `#fff` | `hsla(0,0%,100%,.04)` | White/elevated fill |
| `--background-background-disabled` | `rgba(0,0,0,.04)` | `hsla(0,0%,100%,.04)` | Disabled fill |
| `--background-background-destructive` | `rgba(240,72,63,.2)` | `rgba(240,72,63,.2)` | Destructive tint |
| `--background-background-information` | `#eef4ff` | `#383838` | Info banner fill |
| `--background-background-import` | `rgba(0,0,0,.05)` | `hsla(0,0%,100%,.05)` | Import surface |
| `--background-background-input-bolder` | `rgba(0,0,0,.04)` | `hsla(0,0%,100%,.04)` | Input fill |
| `--background-background-modal-header` | `rgba(41,121,255,.04)` | `hsla(0,0%,100%,.04)` | Modal header tint |
| `--background-login-template` | `#fff` | `#555` | Login template chip |
| `--background-neutral-hovered` | `rgba(41,121,255,.08)` | `hsla(0,0%,100%,.08)` | Neutral hover |
| `--background-neutral-pressed` | `rgba(41,121,255,.16)` | `hsla(0,0%,100%,.16)` | Neutral pressed |

### Borders

| Source token | Light | Dark | Role |
|---|---|---|---|
| `--border-border-primary` | `rgba(0,0,0,.16)` | `hsla(0,0%,100%,.16)` | Default border |
| `--border-border-secondary` | `rgba(0,0,0,.08)` | `hsla(0,0%,100%,.08)` | Subtle divider |
| `--border-border-focused` | `#2979ff` | `#639dff` | Focus border |
| `--border-primary` | `rgba(0,0,0,.16)` | `hsla(0,0%,100%,.16)` | Alias of border-primary |

### Inputs

| Source token | Light | Dark | Role |
|---|---|---|---|
| `--input-text-primary` | `rgba(0,0,0,.87)` | `hsla(0,0%,100%,.87)` | Input text |
| `--input-text-disabled` | `rgba(0,0,0,.7)` | `hsla(0,0%,100%,.7)` | Disabled input text |
| `--input-title-primary` | `rgba(0,0,0,.4)` | `hsla(0,0%,100%,.4)` | Floating label |
| `--input-title-background` | `#fff` | `#1c1c1c` | Floating label backdrop |
| `--input-background-disabled` | `rgba(0,0,0,.04)` | `hsla(0,0%,100%,.04)` | Disabled input fill |
| `--input-border-primary` | `rgba(0,0,0,.32)` | `hsla(0,0%,100%,.32)` | Input border |
| `--input-border-hovered` | `rgba(0,0,0,.54)` | `—` | Input border hover |
| `--input-border-focused` | `#2979ff` | `hsla(0,0%,100%,.87)` | Input border focus |
| `--input-border-error` | `#eb5757` | `#fa7171` | Input border error |
| `--input-border-disabled` | `rgba(0,0,0,.16)` | `hsla(0,0%,100%,.16)` | Disabled input border |

### Interaction (hover / pressed / selected surfaces)

| Source token | Light | Dark | Role |
|---|---|---|---|
| `--button-background-hover` | `rgba(41,121,255,.04)` | `hsla(0,0%,100%,.04)` | Button hover fill |
| `--button-background-pressed` | `rgba(41,121,255,.16)` | `hsla(0,0%,100%,.16)` | Button pressed fill |
| `--menu-hover` | `rgba(41,121,255,.08)` | `hsla(0,0%,100%,.08)` | Menu item hover |
| `--filter-menu-hover` | `rgba(41,121,255,.08)` | `hsla(0,0%,100%,.08)` | Filter menu hover |
| `--tab-button-selected` | `rgba(41,121,255,.16)` | `hsla(0,0%,100%,.16)` | Selected tab fill |
| `--color-background-color-background-neutral-subtle-hovered` | `rgba(0,0,0,.04)` | `hsla(0,0%,100%,.16)` | Subtle neutral hover |
| `--color-border-color-border-selected` | `#2979ff` | `#fff` | Selected border |

### Link, Icon, Checkbox, Scrollbar

| Source token | Light | Dark | Role |
|---|---|---|---|
| `--link-link` | `#2979ff` | `#639dff` | Link color |
| `--icon-icon-quinary` | `rgba(0,0,0,.24)` | `hsla(0,0%,100%,.24)` | Faint icon |
| `--icon-icon-tetriary` *(sic)* | `rgba(0,0,0,.4)` | `hsla(0,0%,100%,.4)` | Muted icon |
| `--checkbox-border` | `rgba(0,0,0,.2)` | `hsla(0,0%,100%,.2)` | Checkbox border |
| `--scrollbar-color` | `rgba(0,0,0,.22)` | `hsla(0,0%,100%,.22)` | Scrollbar thumb |

### RAG / password strength (risk rating)

| Source token | Light | Dark | Role |
|---|---|---|---|
| `--rag-rating-rag-weak` | `#eb5757` | `#fa7171` | Weak / compromised |
| `--rag-rating-rag-medium` | `#ffb800` | `#ffcd29` | Medium |
| `--rag-rating-rag-good` | `#9ac73a` | `#b0dc52` | Good |
| `--rag-rating-rag-strong` | `#5bb254` | `#67bf60` | Strong |

### Customizable item palette (per-item backgrounds)

Each color has a solid `-light` value and a 32% `-light-transparent` value, paired by theme.

| Source token | Light | Dark |
|---|---|---|
| `--customizable-palette-blue-light` | `#eaf2ff` | `#2e45c3` |
| `--customizable-palette-blue-light-transparent` | `rgba(234,242,255,.32)` | `rgba(46,69,195,.32)` |
| `--customizable-palette-green-light` | `#ebf6eb` | `#247621` |
| `--customizable-palette-green-light-transparent` | `rgba(235,246,235,.32)` | `rgba(36,118,33,.32)` |
| `--customizable-palette-light-green-light` | `#f3f8e8` | `#608f1f` |
| `--customizable-palette-light-green-light-transparent` | `hsla(79,53%,94%,.32)` | `rgba(96,143,31,.32)` |
| `--customizable-palette-light-purple-light` | `#e7e3fc` | `#5343a9` |
| `--customizable-palette-light-purple-light-transparent` | `rgba(231,227,252,.32)` | `rgba(83,67,169,.32)` |
| `--customizable-palette-red-light` | `#feebef` | `#b54040` |
| `--customizable-palette-red-light-transparent` | `rgba(254,235,239,.32)` | `rgba(181,64,64,.32)` |
| `--customizable-palette-yellow-light` | `#fff9e1` | `#cb8434` |
| `--customizable-palette-yellow-light-transparent` | `rgba(255,249,225,.32)` | `rgba(203,132,52,.32)` |

### Brand color summary

The product's brand blue is **`#2979ff`** (light) / **`#448aff`** (dark). It anchors:
primary fill, focus borders, links, selected labels. A small ladder of selected-state
blues exists: `#2962ff` (active) → `#2979ff` (base) → `#448aff` (hover/dark) → `#639dff`
(dark base/link). Error red is `#eb5757`/`#fa7171` and is shared across text, input border,
and RAG-weak — these should resolve to one semantic `status.error` token.

---

## 2. Typography

| Property | Value(s) | Notes |
|---|---|---|
| Primary family | `Segoe UI, Helvetica, Arial, sans-serif` | Dominant (17 declarations) |
| Mono family | `Segoe UI Mono, monospace` / `Consolas, Lucida Console, Monaco, monospace` | Generated password / code |
| Size scale (px) | **15** (default), 14, 13, 18, 16, 20, 24, 12, 11, 28, 22 | 15px is the body default (99 uses) |
| Display sizes | 36, 48, 64 | Rare; empty states / large numerics |
| Weights | **600** (dominant, 110 uses), 400, 500, 700 | UI leans semibold for labels |

Proposed type ramp (validate against context): `caption 11–12`, `body-sm 13`, `body 14`,
**`body-md 15` (default)**, `subtitle 16`, `title 18`, `heading 20`, `heading-lg 24`,
`display 28+`. Weight roles: `regular 400`, `medium 500`, **`semibold 600` (default UI)**,
`bold 700`.

---

## 3. Radius

| Value | Uses | Likely role |
|---|---|---|
| `6px` | 50 | **Default control radius** (buttons, inputs) |
| `4px` | 34 | Small controls / chips |
| `3px` | 24 | Tight inset |
| `8px` | 25 | Cards / popups |
| `12px` | 18 | Dialogs / large cards |
| `50%` / `100%` | 34 / 43 | Circular (avatars, icon buttons) |
| `2px`, `5px`, `14px`, `20px`, `24px`, `100px` | rare | Edge cases / pills |

Proposed scale: `xs 3`, `sm 4`, **`md 6` (default)**, `lg 8`, `xl 12`, `pill 100px`,
`full 50%`.

---

## 4. Spacing

Most common px steps from `gap` and `padding`: **4, 6, 8, 11, 16, 24, 32, 40** (plus 2, 3,
10, 12, 20, 30). `16px` dominates `gap`; `8px`/`6px`/`4px` dominate small padding.

Proposed scale: `1=2`, `2=4`, `3=6`, `4=8`, `6=12`, `8=16`, `12=24`, `16=32`, `20=40`.
The `11px` value is an oddment (likely 12 − 1px border) and should normalize to the
`12px` step. Validate before freezing.

---

## 5. Elevation (box-shadow)

Distinct shadows observed, grouped into layers:

| Layer | Example shadow | Use |
|---|---|---|
| Subtle | `0 1px 2px rgba(0,0,0,.12), 0 1px 4px rgba(0,0,0,.04)` | Resting cards |
| Card | `0 16px 16px rgba(0,0,0,.04), 0 4px 8px rgba(0,0,0,.08)` | Raised panels |
| Menu | `0 4px 12px rgba(0,0,0,.16), 0 16px 24px rgba(0,0,0,.08)` | Dropdowns / popups |
| Dialog | `0 0 32px rgba(0,0,0,.08), 0 16px 24px rgba(0,0,0,.08), 0 4px 12px rgba(0,0,0,.16)` | Modals |
| Legacy | `0 6px 6px rgba(0,0,0,.3)`, `0 5px 10px rgba(44,44,44,.4)` | Older menus (common-ui) |

Maps to the draft elevation ladder in `16-foundations-draft.md` (layer 0 surface →
layer 4 toast/tooltip). Consolidate the menu/dialog shadows; drop the legacy `.3–.4` alpha
variants in the final API.

---

## 6. Motion

Transition durations observed: **`.3s`** (dominant, 17×), `.5s` (8×), `.17s` (4×),
`.15s`, `.8s`. Matches the `rf-fade-in`/`rf-fade-out` classes noted in the draft.

Proposed: `fast 150ms`, **`base 300ms` (default)**, `slow 500ms`. Reserve `800ms` for
large progress only. Keep menu/dialog motion at `fast`–`base`.

---

## 7. Breakpoints & z-index

Already captured in `01-source-inventory.md`; repeated here for completeness.

- Breakpoints (px): 1900, 1750, 1650, 1500, 1370, 1350, 1220, 1100, 950, 900, 750, 650.
- Key z-index ladder: navigator 3 · header/editor 4 · modal-screen 5 · dropdown-popup 10 ·
  context-menu 20 · notification 100 · editor cmdbar 100 · loading overlay up to 1000.

---

## 8. Legacy tokens (common-ui.css)

The legacy variables (`--regular-button-*`, `--important-button-*`, `--low-emphasis-button-*`,
`--white-button-*`, `--toggle-*`, `--progress-*`, `--list-item-*`, `--main-text-color`,
`--password-*-strength-color`) are **referenced via `var(...)` in `common-ui.css` but their
definitions are not present in this export.** Their values live in a base theme file not
captured here.

Action: request that base file, or read the computed values from a live DOM. Until then,
the legacy color values are unknown and only their *names/roles* are documented. The v8
tokens above are the authoritative color source.

---

## 9. Canonical mapping direction

Direction (from `00-source-led-design-system-plan.md`): treat v8 tokens as the future
semantic layer; map legacy into it; do not preserve both as equal public APIs.

| Canonical family | Source basis | Notes |
|---|---|---|
| `color.surface.*` | `--surface-surface-*` | Renumber to a clean ladder (0–12 is sparse) |
| `color.text.*` | `--text-text-*` | Fix `tetriary`→`tertiary`; collapse selected variants |
| `color.background.*` | `--background-*` | Split brand fill vs neutral hover/pressed |
| `color.border.*` | `--border-*`, `--input-border-*` | Merge duplicate `border-primary` |
| `color.input.*` | `--input-*` | Add missing dark `input-border-hovered` |
| `color.link.*` | `--link-link`, `--text-link-upgrade` | One link family |
| `color.icon.*` | `--icon-icon-*` | Fix `tetriary` typo |
| `color.status.*` | error/info/destructive | Unify `#eb5757` across error uses |
| `color.rating.*` | `--rag-rating-*` | weak/medium/good/strong |
| `color.item.palette.*` | `--customizable-palette-*` | 6 hues × solid/transparent |
| `font.*`, `radius.*`, `space.*`, `shadow.*`, `motion.*`, `z.*` | sections 2–7 | |

### Known source naming defects (do not carry into the API)

- `tetriary` → `tertiary` (in `--text-text-tetriary`, `--icon-icon-tetriary`).
- Redundant `--border-primary` duplicating `--border-border-primary`.
- Verbose `--color-background-color-background-neutral-subtle-hovered`.
- Sparse surface numbering (`surface-0, -02…-12`) with gaps.
