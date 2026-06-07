# Component Spec: Segmented Selector / Tabs

**Status:** Ready for DS (final spec) · **Tier:** Core primitive (navigation)
**Evidence:** vault sort/order, Security Center category tabs, Sharing Center tabs,
Emergency Access tabs, Password Generator tabs, Devices & Activity tabs. One of the most
reused patterns. The same `selector-item` class also drives the sidebar navigator (filled
variant).

Tokens: [19-foundations-tokens.md](19-foundations-tokens.md). Source classes:
[02-component-state-matrix.md](02-component-state-matrix.md). Patterns:
[08-pattern-password-generator.md](08-pattern-password-generator.md),
[09-pattern-security-center.md](09-pattern-security-center.md),
[10-pattern-sharing-center.md](10-pattern-sharing-center.md),
[11-pattern-emergency-access.md](11-pattern-emergency-access.md).

---

## 1. Anatomy

```
┌ order-selector (flex row) ─────────────────────────────────┐
│  Item        Item (active)        Item · badge   Item(disabled)
│              ▔▔▔▔▔▔▔▔▔ 3px brand underline                  │
└──────────────────────────────────────────────────────────────┘
```

An item is `selector-item`; it can carry a count `badge` or a notification pill. The active
item is marked by a brand underline **and** bolder weight.

## 2. Container (measured)

| Property | Value | Token |
|---|---|---|
| Layout | `order-selector`: `display:flex; align-items:center` | — |
| Background | transparent / dark `#1a1a1a` | `--surface-surface-05` (dark) |
| Responsive | `flex-wrap:wrap; height:auto` (wraps on narrow widths) | — |

## 3. Item — underline tab (measured)

| Property | Value | Token |
|---|---|---|
| Height | `56px` (line-height 56) / `54px` in some hosts | — |
| Min-width | `170px` (vault/generic); `0` inside `order-selector` compact hosts | — |
| Padding | `0 20px` | — |
| Font | `15px`; title `text-transform:capitalize`, `white-space:nowrap` | body |
| Indicator | `border-bottom: 3px solid transparent` | — |
| Text default | `--text-text-primary` | `--text-text-primary` |
| Spacing | `margin-right:4px` | — |

### States (measured)

| State | Treatment | Token |
|---|---|---|
| Default | transparent underline, primary text | — |
| Hover | bg `rgba(0,0,0,.055)` / `.055` white; text → full primary | — |
| Active | `border-bottom-color:#2979ff`; `.title { font-weight:600 }`; `.badge { font-weight:600 }` | `--background-background-primary` (indicator) |
| Disabled | `color: rgba(0,0,0,.4)`; `cursor:auto` | `--text-text-tetriary` |

> The active state uses **underline + weight together**, so it is not color-only — keep both.

## 4. Badge & notification pill (measured)

| Element | Source | Treatment |
|---|---|---|
| Count badge | `selector-item .badge` | outlined pill: `border` (`--border` faded), `line-height:20px`, `margin-left:4px`, weight 400 → **600 when active**, border → text-primary when active |
| Notification pill | `data-breach-mon-notification` | filled brand pill: bg `#2979ff`, text on-color, `border-radius:12px`, `padding:2px 10px`, weight 600 |

Badges carry tab counts (e.g. Security Center category counts). See the Badge primitive in
[02-component-state-matrix.md](02-component-state-matrix.md).

## 5. Variants

- **Underline tabs (default)** — the `order-selector` usage across vault/security/sharing/
  EA/generator/devices. This spec's primary variant.
- **Filled / pill item (navigator)** — the **same** `selector-item` class in the sidebar
  navigator renders active as a filled pill: `background-color: rgba(41,121,255,.16)`
  (`--tab-button-selected`), `font-weight:700`, `transition: font-weight .3s`. Documented
  here for shared styling, but owned by the Sidebar Navigator (App Shell) component.
- **Tabbed sheet selector** — Devices & Activity replaces the sheet title with this selector
  (`tab-selector`); see [17-pattern-settings-sheets.md](17-pattern-settings-sheets.md).

## 6. Tokens

| Role | Token |
|---|---|
| Item text | `--text-text-primary` |
| Hover fill | `rgba(0,0,0,.055)` → add a faint `--background-neutral-*` step |
| Active indicator | `--background-background-primary` (brand) |
| Disabled text | `--text-text-tetriary` |
| Filled active (nav) | `--tab-button-selected` |
| Badge border | `--border-border-primary` (active → `--text-text-primary`) |
| Notification pill | `--background-background-primary` / `--text-text-on-color` |

## 7. Behavior

- Single-select: exactly one active item; clicking switches the view/filter.
- Wraps to multiple rows on narrow widths (`flex-wrap:wrap`); no horizontal scroll/overflow
  menu observed (confirm for very long tab sets).
- Counts update live in badges; the notification pill flags actionable items (e.g. breach).
- Capitalized titles via CSS — source copy may be lower-case.

## 8. Accessibility

- Use `role="tablist"` + `role="tab"` with `aria-selected`; associate each tab to its panel
  via `aria-controls` and the panel back with `aria-labelledby`.
- **Keyboard (must add — source is pointer-driven):** `←`/`→` move between tabs, `Home`/`End`
  jump, activation on focus or `Enter`/`Space` per chosen model.
- Active state already pairs underline + weight (not color-only) — preserve.
- Badge counts must be in the tab's accessible name (e.g. "Weak, 12 items"), not visual-only.
- Disabled tabs use `aria-disabled` and are skipped by arrow navigation.
- Add a visible focus-visible ring distinct from the active underline.

## 9. Source → DS mapping

| Source class | DS |
|---|---|
| `order-selector` | `SegmentedSelector` / `Tabs` (tablist) |
| `selector-item` | `Tab` |
| `selector-item.active` | `Tab state=active` |
| `selector-item.disabled` | `Tab state=disabled` |
| `selector-item .title` | tab label |
| `selector-item .badge` | count badge |
| `data-breach-mon-notification` | notification pill |
| `tab-selector` | tabbed sheet header |
| `selector-item` (navigator, filled active) | Sidebar Navigator item (shared styling) |

## 10. Open questions

1. Overflow behavior for long tab sets (wrap is confirmed; scroll/overflow menu not seen).
2. Activation model: select-on-focus vs select-on-Enter.
3. The `.055` hover fill has no named token — add a faint neutral step.
4. Whether the navigator filled variant should be a `Tabs` variant or fully owned by the
   Sidebar Navigator spec (currently shared).
5. Badge empty/zero behavior (hidden vs shown as 0).
