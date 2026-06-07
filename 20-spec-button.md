# Component Spec: Button

**Status:** Ready for DS (first final spec) · **Tier:** Core primitive
**Evidence:** dialogs, editors, sharing, emergency access, password generator, new login,
login Save enabled/disabled, unsaved-changes dialog. Promotion threshold met (core
primitive, 2+ surfaces, full state coverage, light/dark known).

Tokens referenced here come from [19-foundations-tokens.md](19-foundations-tokens.md).
Source classes are documented in [02-component-state-matrix.md](02-component-state-matrix.md).

---

## 1. Anatomy

```
[ optional leading icon ]  Label  [ optional trailing icon ]
```

A button is a single interactive control with a text label, optional icon, centered
content, `font-weight: 600`, and `cursor: pointer`. Width is either content-driven
(min-width floor) or full-width in form context.

## 2. Two size systems in source

The product has **two distinct button geometries**. The final spec keeps both as sizes,
not as separate components.

| Size | Source basis | Height | Font | Radius | Padding | Min-width |
|---|---|---|---|---|---|---|
| **`md` (compact/dialog)** | `.buttons-bar .button` | ~34px | 14–15px / 600 | 3px → **`radius.xs` (3)** | `9px 16px` | 70–80px |
| **`lg` (form/primary)** | `.button.important-button` | **48px** | 18px / 600 | 6px → **`radius.md` (6)** | content | 80px, often `width:100%` |

> Final API: normalize the compact radius from `3px` to **`radius.sm` (4)** unless the 3px
> is intentional; flag in open questions.

## 3. Variants (intent)

Source has four legacy tiers (`common-ui.css`) plus a neutral dialog button. Map to five
semantic intents:

| DS intent | Source basis | Fill | Border | Text |
|---|---|---|---|---|
| **Primary** | `.important-button` | `--background-background-primary` (brand `#2979ff`/`#448aff`) | none | `--text-text-inverse` / on-color `#fff` |
| **Secondary** | `.regular-button`, dialog `.button` | `--surface-surface` / white | `1px --border-border-primary` | `--text-text-primary` |
| **Tertiary / text** | `.low-emphasis-button` | transparent | none | `--text-text-selected` (link blue) |
| **Neutral** | dialog `.button` | white + `--border-border-primary` | 1px | `--text-text-primary` |
| **Destructive** | *semantic only* | maps to `--background-background-destructive` / error red | per emphasis | per emphasis |

> **Source defect:** destructive confirmations (Stop Sharing, Delete) reuse the neutral
> `default-button` styling. The DS must map destructive **intent** to error treatment, not
> inherit the neutral source class. See `16-foundations-draft.md` dialog rules.

## 4. States

All states are confirmed in source (`:hover`, `:active`, `:disabled`, `:focus`).

| State | Treatment | Token |
|---|---|---|
| Default | as variant | — |
| Hover | fill tint | `--button-background-hover` (primary/neutral); legacy `*-hover-color` |
| Active / pressed | stronger tint | `--button-background-pressed` |
| Focus | **currently `outline:none` + background change** | ⚠️ a11y gap — see §7 |
| Disabled | muted fill + `cursor:default` | `--background-background-primary-disabled` / `--text-text-secondary-disabled` |
| Loading | `button-with-progress.show-progress` hides label (`visibility:hidden; width:0`) and shows a marquee progress bar | confirmed for e.g. change-master-password flow |

Login editor confirms: Save disabled = `button default-button disabled`; Save enabled =
`button default-button`.

## 5. Behavior

- Label never wraps (`white-space: nowrap`).
- Compact buttons sit in a `buttons-bar` (height 56px, top divider shadow, right-aligned,
  `margin: 0 10px`).
- In confirmation dialogs the **primary/continuing action is the rightmost** and matches
  the safest task (e.g. `Save` beside `Don't save`).
- Loading replaces the label in place without resizing the bar (marquee fills the button).

## 6. Tokens

| Role | Token |
|---|---|
| Primary fill | `--background-background-primary` |
| Primary disabled | `--background-background-primary-disabled` |
| Hover fill | `--button-background-hover` |
| Pressed fill | `--button-background-pressed` |
| Secondary/neutral border | `--border-border-primary` |
| Neutral surface | `--surface-surface` |
| Text default | `--text-text-primary` |
| Text on primary | `--text-text-inverse` |
| Text disabled | `--text-text-secondary-disabled` |
| Tertiary text | `--text-text-selected` |
| Radius | `radius.sm (4)` / `radius.md (6)` |
| Font | 600 weight; 14/15px (compact), 18px (form) |

## 7. Accessibility

- **Focus ring is missing in source** (`outline:none`, focus shown only via background).
  The DS **must** add a visible focus indicator using `--border-border-focused`
  (`#2979ff`/`#639dff`), e.g. a 2px focus ring, to meet WCAG 2.4.7.
- Disabled buttons must use `aria-disabled` semantics, not just visual muting, and remain
  discoverable to screen readers when they convey why they are disabled.
- Loading buttons must announce busy state (`aria-busy`) and prevent double-submit.
- Destructive buttons need an accessible name that states the consequence.

## 8. Source → DS mapping

| Source class | DS |
|---|---|
| `important-button` | `Button intent=primary` |
| `regular-button` / dialog `default-button` | `Button intent=secondary` |
| `low-emphasis-button` | `Button intent=tertiary` |
| `white-button` | `Button intent=neutral` |
| `disabled` / `disabled-button` | `state=disabled` |
| `button-with-progress` + `show-progress` | `state=loading` |
| `non-essential-button` | secondary/tertiary depending on context (verify) |

## 9. Open questions

1. Is the compact `3px` radius intentional vs the `6px` form radius? (normalize to scale)
2. Confirmed final destructive treatment (fill vs outline vs text).
3. Legacy button color values are undefined in this export (see
   [19-foundations-tokens.md](19-foundations-tokens.md) §8) — needed to verify hover/active
   exact colors for regular/important/low-emphasis tiers.
4. Does any flow show a loading state on a **compact** dialog button, or only on large
   form buttons?
