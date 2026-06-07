# Component Spec: Text Input

**Status:** Ready for DS (final spec) · **Tier:** Core primitive
**Evidence:** New Login, login/identity editors, inline rename, emergency email, identity
edit fields, folder selector, number stepper, settings. Appears across the whole product.

Tokens: [19-foundations-tokens.md](19-foundations-tokens.md). Source classes:
[02-component-state-matrix.md](02-component-state-matrix.md).

---

## 1. Anatomy

The v8 input is a **floating-label field** (`input-with-title` / `editor-field`):

```
  ┌───────────────────────────────────────────┐
  │ ⌖ Label  ← .title sits on the top border    │   .title: absolute, font 13px,
  │  ┌─────────────────────────────────────┐   │           translateY(-50%), top 0,
  │  │ value text          [trailing icon] │   │           bg = field bg, padding 0 4px
  │  └─────────────────────────────────────┘   │
  └───────────────────────────────────────────┘
  Helper / error line (rf-field-error)
```

The label background matches the field surface (`--input-title-background`) so it visually
"cuts" the border. Optional leading icon (folder selector) and trailing affordance (hint,
reveal eye, dropdown arrow).

## 2. Two input systems in source

| System | Source | Radius | Height | Notes |
|---|---|---|---|---|
| **v8 floating-label (canonical)** | `input-with-title`, `editor-field`, `field-with-title` | per scale | content | Label on border, `--input-*` tokens |
| **Legacy** | `extension-normal-input` | `3px` | `min-height: 52px`, `min-width: 315px`, `text-indent: 10px` | placeholder 15px; map into v8 |

The DS standardizes on the **v8 floating-label** input and maps legacy into it.

## 3. Floating label (measured)

| Property | Value | Token |
|---|---|---|
| Font size | `13px` | caption |
| Position | `position:absolute; top:0; left:4px; transform:translateY(-50%); padding:0 4px` | — |
| Backdrop | field surface | `--input-title-background` (`#fff` / `#1c1c1c`) |
| Default color | muted | `--input-title-primary` (`rgba(0,0,0,.4)`) / `--text-text-tetriary` |
| Active/focus color | brand / high-contrast | `#2979ff` (light) / `hsla(0,0%,100%,.87)` (dark) |
| Overflow | `max-width: calc(100% - 8px)`, ellipsis, nowrap | — |

## 4. States (measured)

| State | Border token | Other |
|---|---|---|
| Default | `--input-border-primary` (`rgba(0,0,0,.32)` / `.32` white) | text `--input-text-primary` |
| Hover | `--input-border-hovered` (`rgba(0,0,0,.54)`; ⚠️ **no dark value in export**) | |
| Focus | `--input-border-focused` (`#2979ff` / `hsla(0,0%,100%,.87)`) | `outline:none`; label → active color |
| Error | `--input-border-error` (`#eb5757` / `#fa7171`) | `rf-field-error` text `#eb5757`, `field-with-error` wrapper |
| Disabled | `--input-border-disabled` (`.16`) | bg `--input-background-disabled`; text `--input-text-disabled` |
| Placeholder | legacy placeholder font 15px | map to `--text-text-tetriary` |
| With hint/affordance | `with-hint input { padding-right: 28px }` | room for trailing icon |

## 5. Variants

- Single-line text input (default).
- **Textarea** (`textarea-with-title`, `.textarea { min-height: 70px; resize: none }`) — see
  note fields in identity/login editors; `show-note` collapsed add-prompt variant.
- **Selector-style** input (read-only, opens a menu): folder selector
  (`field-with-title.folder-selector`, `cursor:pointer`, leading folder icon + trailing
  arrow, `padding-left:40px; padding-right:30px`). Pairs with the attached menu in
  [22-spec-dropdown-menu.md](22-spec-dropdown-menu.md).
- **Number input** (`number-input`, font 15/400, `min-height:25px`, `width:104px`) paired
  with a stepper (`buttons-container`, 36px). Border state lives on the sibling buttons.
- **Inline / in-place edit** (`inplace-edit-pane`, `inplace-input`) for rename flows, with
  OK/cancel affordances.

## 6. Layout

- Field spacing: `field-with-title:not(:last-child) { margin-bottom: 28px }`; tighter
  `8px` when a help link follows (`field-with-help-link`).
- Error line: `rf-field-error { margin-top:-3px; margin-bottom:6px }`, hidden until invalid.

## 7. Tokens

| Role | Token |
|---|---|
| Text | `--input-text-primary` |
| Disabled text | `--input-text-disabled` |
| Label | `--input-title-primary` |
| Label backdrop | `--input-title-background` |
| Border default | `--input-border-primary` |
| Border hover | `--input-border-hovered` |
| Border focus | `--input-border-focused` |
| Border error | `--input-border-error` |
| Border disabled | `--input-border-disabled` |
| Disabled fill | `--input-background-disabled` |
| Error text | `--text-text-error` |
| Radius | `radius.sm (4)` |

## 8. Accessibility

- Floating label must be a real `<label>` associated to the input (not decorative), so it
  is announced even when floated.
- Focus relies on a border-color change with `outline:none` — acceptable for inputs **only
  if** the focus border meets 3:1 contrast against the surface; verify `--input-border-focused`
  in both themes and strengthen if needed.
- Errors: associate `rf-field-error` text via `aria-describedby` and set `aria-invalid`;
  never signal error by color alone (border + text + optional icon).
- Disabled inputs use the disabled attribute; if a value must stay readable, prefer
  read-only over disabled.
- Selector-style inputs expose combobox semantics (`role="combobox"`, `aria-expanded`).
- Placeholder is not a label substitute.

## 9. Source → DS mapping

| Source class | DS |
|---|---|
| `input-with-title` / `editor-field` | `TextInput` (floating label) |
| `field-with-title` | field wrapper |
| `extension-normal-input` | legacy → `TextInput` |
| `textarea-with-title` / `show-note` | `TextInput multiline` / collapsed note |
| `field-with-title.folder-selector` | `TextInput variant=selector` |
| `number-input` + `buttons-container` | `NumberInput` (stepper) |
| `inplace-edit-pane` / `inplace-input` | `TextInput variant=inline` |
| `rf-field-error` / `field-with-error` | error message + wrapper |
| `with-hint` | trailing-affordance modifier |

## 10. Open questions

1. **Dark `--input-border-hovered` is missing** in the export — confirm the dark hover border.
2. Exact input box height/padding/radius for the v8 floating-label input (label and state
   rules captured; base box geometry not isolated — measure from a live field).
3. Legacy `--edit-*` color values are undefined in this export (see
   [19-foundations-tokens.md](19-foundations-tokens.md) §8).
4. Filled vs outlined: confirm the field has no fill in default (only border) across themes.
5. Inline-edit commit/cancel interaction states (focus, commit, cancel) not yet captured.
