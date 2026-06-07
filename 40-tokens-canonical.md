# Canonical Token API (proposed)

This file proposes the **design-system token names** — the public API — and maps the source
variables and hardcoded literals onto them. It builds on the measured values in
[19-foundations-tokens.md](19-foundations-tokens.md) (the evidence layer) and the directions
in [16-foundations-draft.md](16-foundations-draft.md) (the rules layer).

Direction (from [00-source-led-design-system-plan.md](00-source-led-design-system-plan.md)):
v8 tokens are the future semantic layer; legacy maps into it; source naming defects are not
carried into the API.

---

## 1. Naming scheme

```
color.<group>.<role>[.<state>]      e.g. color.surface.base, color.action.primary.hover
radius.<step>                       xs sm md lg xl pill full
space.<step>                        1..20 (×, see §7)
font.size.<step> / font.weight.<role>
shadow.<layer>                      subtle card menu dialog
motion.<role>                       fast base slow
z.<layer>
```

All color tokens carry a light and dark value (see [19-foundations-tokens.md](19-foundations-tokens.md) §1).

## 2. Color — canonical map (existing v8 tokens)

| Canonical | Source var | Notes |
|---|---|---|
| `color.surface.base` | `--surface-surface` | |
| `color.surface.raised` | `--surface-surface-02/04` | collapse near-duplicates |
| `color.surface.sunken` | `--surface-surface-05` | app background |
| `color.surface.header` | `--surface-surface-06` | table/section header |
| `color.surface.menu` | `--surface-surface-08` | dropdown/popup |
| `color.surface.dialog` | `--surface-surface-10/11` | |
| `color.text.primary` | `--text-text-primary` | |
| `color.text.secondary` | `--text-text-secondary` | |
| `color.text.tertiary` | `--text-text-tetriary` | **fix typo** `tetriary→tertiary` |
| `color.text.quaternary` | `--text-text-quaternary` | |
| `color.text.error` | `--text-text-error` | |
| `color.text.inverse` | `--text-text-inverse` | |
| `color.text.onColor` | `--text-text-on-color` | |
| `color.text.link` | `--link-link` / `--text-link-upgrade` | one link family |
| `color.text.selected` | `--text-text-selected` | + `.hover`/`.active` variants |
| `color.action.primary` | `--background-background-primary` | brand `#2979ff`/`#448aff` |
| `color.action.primary.disabled` | `--background-background-primary-disabled` | |
| `color.action.hover` | `--button-background-hover` | |
| `color.action.pressed` | `--button-background-pressed` | |
| `color.background.destructive` | `--background-background-destructive` | |
| `color.background.information` | `--background-background-information` | also = list row hover (see §4) |
| `color.background.neutralHover` | `--background-neutral-hovered` | |
| `color.background.neutralPressed` | `--background-neutral-pressed` | |
| `color.border.primary` | `--border-border-primary` | **drop duplicate** `--border-primary` |
| `color.border.secondary` | `--border-border-secondary` | dividers |
| `color.border.focused` | `--border-border-focused` | focus ring (see §5) |
| `color.border.selected` | `--color-border-color-border-selected` | **rename** the verbose source name |
| `color.input.*` | `--input-*` | text/title/border/disabled |
| `color.icon.muted` | `--icon-icon-tetriary` | **fix typo** |
| `color.icon.faint` | `--icon-icon-quinary` | |
| `color.rating.weak/medium/good/strong` | `--rag-rating-rag-*` | see §3 |
| `color.item.palette.<hue>` | `--customizable-palette-*` | see §3 |
| `color.menu.hover` | `--menu-hover` / `--filter-menu-hover` | merge (identical) |
| `color.tab.selected` | `--tab-button-selected` | filled tab/nav active |
| `color.checkbox.border` | `--checkbox-border` | |
| `color.scrollbar` | `--scrollbar-color` | |

## 3. Resolutions (close existing gaps)

- **Strength → rating.** The hardcoded strength hexes (`#eb5757/#ffb800/#9ac73a/#5bb254`)
  equal the **light** values of `--rag-rating-rag-weak/medium/good/strong`. The legacy
  `--password-*-strength-color` family resolves onto `color.rating.*`. (See
  [32-spec-strength-badge.md](32-spec-strength-badge.md).)
- **Avatar slots → item palette.** Avatar/initials triads
  (`#639dff/#2e45c3`, `#f88/#b54040`, `#9d8fe6/#5343a9`, `#9bd596/#278722`, `#ffcd29/#cb8434`)
  match the **dark** values of `--customizable-palette-*`. Source avatar slots from
  `color.item.palette.*`; unify legacy `color-1..5` vs v8 `bg-color-1..4`. (See
  [36-spec-avatar.md](36-spec-avatar.md).)

## 4. New tokens (name the unnamed literals)

These values are **hardcoded with no source variable** but are reused across components, so
they need names.

| Proposed token | Light | Dark | Consumers |
|---|---|---|---|
| `color.background.selected` | `#ddeaff` | ⚠️ inconsistent (`#444` vs `hsla(0,0%,100%,.16)`) | Table rows, Copyable Field, Vault list item |
| `color.background.rowHover` | `#eef4ff` (= `background.information`) | `hsla(0,0%,100%,.08)` (= `neutralHover`) | Table row hover / menu-open |
| `color.background.faintHover` | `rgba(0,0,0,.055)` | `hsla(0,0%,100%,.055)` | Segmented Selector tab hover |
| `color.border.itemActive` | `#838789` | `#a7acae` | Vault grid card hover/selected border |
| `color.border.itemOutline` | `#a7acaf` | `#a7acaf` | add-new item / icon outline |
| `color.rating.empty` | `rgba(0,0,0,.28)` | (verify dark) | Score gauge `nodata` |
| `color.badge.circular` | `#2962ff` (= `text.selected.active`) | `#2962ff` | circular count badge |
| `color.scrim.modal` | `rgba(0,0,0,.25)` | `rgba(0,0,0,.25)` | dialog overlay |
| `color.scrim.item` | `rgba(0,0,0,.5)` | `hsla(0,0%,100%,.5)` | grid item hover overlay |

### Decisions to make

1. **`color.background.selected` dark value:** unify `#444` (Copyable Field) vs
   `hsla(0,0%,100%,.16)` (Table). Pick one; recommend the `.16` alpha form for consistency
   with the rest of the neutral interaction family.
2. **`color.background.rowHover`** reuses `background.information` in light but `neutralHover`
   in dark — decide whether row hover is its own token or an alias.
3. **`color.badge.circular`** equals `text.selected.active` — alias rather than a new value.

## 5. Cross-cutting: focus ring (product-wide gap)

No component in source renders a visible focus-visible ring (`outline:none` throughout). The
DS adds one token and applies it everywhere:

- `color.focus.ring` = `--border-border-focused` (`#2979ff` / `#639dff`).
- Recommended: 2px ring (or 2px outline + 1px offset) on all interactive components
  (Button, Icon Button, Menu items, Inputs, Select, Switch, Radio, Tabs, Table rows,
  Vault items, Copyable Field actions). Tracked as an open item in every component spec.

## 6. Radius / shadow / motion / type (from §2–6 of evidence)

| Family | Canonical | Source basis |
|---|---|---|
| Radius | `radius.xs 3 · sm 4 · md 6 (default) · lg 8 · xl 12 · pill 100px · full 50%` | [19](19-foundations-tokens.md) §3 |
| Shadow | `shadow.subtle · card · menu · dialog` | [19](19-foundations-tokens.md) §5 (drop legacy `.3–.4` alphas) |
| Motion | `motion.fast 150 · base 300 (default) · slow 500` | [19](19-foundations-tokens.md) §6 |
| Font size | `11/12 · 13 · 14 · 15 (default) · 16 · 18 · 20 · 24 · 28+` | [19](19-foundations-tokens.md) §2 |
| Font weight | `regular 400 · medium 500 · semibold 600 (default) · bold 700` | [19](19-foundations-tokens.md) §2 |

## 7. Spacing

Proposed `space` scale (px): `1=2 · 2=4 · 3=6 · 4=8 · 6=12 · 8=16 · 12=24 · 16=32 · 20=40`.
Normalize the `11px` oddment → `12`. Confirm the `5px`/`10px` legacy steps map to `4`/`8` or
stay as half-steps.

## 8. Source naming defects (do not ship)

- `tetriary` → `tertiary` (`--text-text-tetriary`, `--icon-icon-tetriary`).
- `bandge` → `badge` (`rf-bandge-count`).
- `recipiant` → `recipient` (`rf-perm` table, `recipiant-status`).
- `unusial` → `unusual` (`unusial-activity-warning`).
- `buisness` → `business` (`rf-identity-buisness-fields`).
- Redundant `--border-primary` duplicating `--border-border-primary`.
- Verbose `--color-background-color-background-neutral-subtle-hovered`.
- Sparse surface numbering (`surface-0, -02 … -12`) with gaps → renumber.

## 9. Blocked (needs the base theme file)

These legacy variables are **referenced but not defined** in the current export (see
[19-foundations-tokens.md](19-foundations-tokens.md) §8). Their values are needed to finalize
exact colors for Button tiers, Text Input (`--edit-*`), and Switch (`--toggle-*`):

- `--regular-button-*`, `--important-button-*`, `--low-emphasis-button-*`, `--white-button-*`
- `--edit-*` (input background/border/text/placeholder, hover/focus/disabled)
- `--toggle-*` (switch track/knob/disabled)
- `--list-item-*`, `--items-counter-*`, `--progress-*`
- Dark `--input-border-hovered` (missing in export)

**Action:** provide the base theme CSS, or paste computed styles from a live DOM for one
button of each tier, one input, and one switch (light + dark).
