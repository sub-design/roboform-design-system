# Component Spec: Icon Button / Command Button

**Status:** Ready for DS (first final spec) · **Tier:** Core primitive
**Evidence:** editor headers, instance commands, row actions, generator actions, table
manage-columns, copy/reveal controls, sharing badges. Appears in 5+ surfaces.

Tokens: [19-foundations-tokens.md](19-foundations-tokens.md). Source classes:
[02-component-state-matrix.md](02-component-state-matrix.md).

---

## 1. Anatomy

A square/circular control containing only an icon, no visible label. The icon is a
background SVG centered in the box (`background-position: 50%; background-repeat: no-repeat`).

## 2. Size scale (measured)

| DS size | Box | Icon (bg-size) | Radius | Source basis |
|---|---|---|---|---|
| `xs` | 20×20 | 16 | circle | inline row actions |
| `sm` | 24×24 | 16–20 | `50%` | `rf-cmdbutton.rf-item-sharing`, `cmd-learn-more` |
| `md` | 30×30 | 24 | `50%` | `rf-cmdbutton` (30 variant) |
| `md+` | 32×32 | 20 | `100%`, padding 6 | `show-password-button` |
| **`lg` (default)** | **34–36×34–36** | **24** | `100%` (or `4px` framed) | `.icon-button` (34), `.rf-cmdbutton` (36) |
| `xl` | 48×48 | — | `100%` | password generator generate button |
| `2xl` | 60×60 | — | — | large create/empty-state affordance |

Two shapes exist: **circular** (`border-radius:100%`/`50%`, default) and **framed**
(`border-radius:4px; padding:6px`, used in tables / command bars). Final API exposes
`shape="circular" | "framed"`.

## 3. States

| State | Treatment | Source |
|---|---|---|
| Default | transparent bg, icon at full opacity | `background-color:transparent; opacity:1` |
| Hover | icon swaps to `-hover` SVG; framed gets `--menu-hover` fill | `cmd-*-hover.svg`, `cmd-*-hover-dark.svg` |
| Active / pressed | `-hover`/active SVG; `--button-background-pressed` for framed | `:active` rules |
| Focus-visible | ⚠️ not in source — must be added (see §6) | — |
| Disabled | reduced opacity / muted icon, `rf-cmd-disabled` | `rf-cmd-disabled` |
| Auto-hide | hidden until row/item hover or focus | `auto-hide` |

The icon has **light, dark, hover, and hover-dark** asset variants (e.g.
`cmd-account-theme.svg` / `-dark.svg` / `-hover.svg` / `-hover-dark.svg`). The DS should
prefer a single SVG recolored via `currentColor`/`--icon-*` tokens instead of four assets.

## 4. Tokens

| Role | Token |
|---|---|
| Icon default | `--icon-icon-tetriary` (muted) / `--text-text-primary` for prominent |
| Icon faint | `--icon-icon-quinary` |
| Framed hover fill | `--menu-hover` |
| Framed pressed fill | `--button-background-pressed` |
| Focus ring (to add) | `--border-border-focused` |
| Radius | `radius.sm (4)` framed · `full (50%)` circular |

## 5. Behavior

- Default container is transparent; emphasis comes from the icon, not a fill, except in
  framed/table contexts.
- Auto-hide actions appear on hover/focus of the parent row or item; they must still be
  keyboard-reachable (see a11y).
- Spacing between adjacent icon buttons in a command bar: `margin-right` 3–8px.

## 6. Accessibility

- **Every icon-only button requires an accessible name** (`aria-label` or visually-hidden
  text) describing the action, e.g. "Copy password", "Reveal password", "More actions".
- **Add a visible focus-visible ring** using `--border-border-focused`; source provides none.
- **Hover-only / auto-hide controls must be focusable via keyboard** so copy/reveal actions
  are reachable without a pointer.
- Sensitive actions (reveal/copy password, TOTP) must not echo the secret value in the
  accessible name or any tooltip.
- Minimum 24×24 target; pad smaller visual icons to a 24px+ hit area.

## 7. Source → DS mapping

| Source class | DS |
|---|---|
| `icon-button` | `IconButton size=lg shape=circular` |
| `rf-cmdbutton` | `IconButton size=lg` (circular default; framed when `4px`/padding 6) |
| `rf-image-button` | `IconButton` |
| `manage-icon` | `IconButton` (table settings) |
| `rf-new-button` | `IconButton size=xl` (create) |
| `show-password-button` | `IconButton size=md+ shape=circular` (reveal) |
| `rf-cmd-disabled` | `state=disabled` |
| `auto-hide` | `behavior=auto-hide` |

## 8. Open questions

1. Confirm focus-visible treatment and whether framed vs circular differ.
2. Exact disabled opacity/color (legacy values undefined in export).
3. Consolidate the 4-asset hover/dark icon model into token-recolored single SVGs.
4. Which sizes are truly distinct vs incidental (20 vs 24, 30 vs 32 vs 34 vs 36)?
