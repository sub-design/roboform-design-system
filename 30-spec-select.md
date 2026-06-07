# Component Spec: Select / Combobox

**Status:** Ready for DS (final spec) · **Tier:** Core primitive
**Evidence:** search type selector, account/nav selector, folder selector, emergency
timeout, identity edit selects, settings selects. Composes
[24-spec-text-input.md](24-spec-text-input.md) (field shell) and
[22-spec-dropdown-menu.md](22-spec-dropdown-menu.md) (attached options list).

Tokens: [19-foundations-tokens.md](19-foundations-tokens.md). Source classes:
[02-component-state-matrix.md](02-component-state-matrix.md).

---

## 1. Anatomy

```
┌ Upper title (floating label) ─────────────┐
│ custom-select                              │
│  select-title: selected value        ▾     │  ← arrow at right
└────────────────────────────────────────────┘
   ┌ select-dropdown / options-list ────────┐   (opened)
   │  select-option                          │
   │  select-option.selected (muted)         │
   └─────────────────────────────────────────┘
```

The closed control reuses the **floating-label input** shell; the open list reuses the
**attached menu** (`dropdown-popup-list` family). One component, two composed parts.

## 2. Trigger / closed state (measured)

| Property | Value | Token |
|---|---|---|
| Title (value) | `select-title`: `font-size:15px; padding:7px 40px 7px 12px` (40px right reserves the arrow) | — |
| Text color | `--input-text-primary` (`rgba(0,0,0,.87)`) | `--input-text-primary` |
| Value overflow | `white-space:nowrap`; `flex-grow:1` | — |
| Upper title (label) | `select-upper-title`: `font-size:13px`, abs `top:0; left:4px; transform:translateY(-50%)`, backdrop `--input-title-background`, color `--input-title-primary` | — |
| Arrow | right-aligned; padding-right `40/32px` variants reserve space | `--icon-icon-*` |

## 3. Open list (measured)

| Property | Value | Token |
|---|---|---|
| Container | `select-dropdown .options-list`: `max-height:210px` (scroll), `max-width:250px` / `calc(100% + 2px)` | — |
| Scrollbar | `scrollbar-color: --icon-icon-quinary` | `--icon-icon-quinary` |
| Option | `select-option`: color `--input-text-primary` | `--input-text-primary` |
| Selected option | `select-option.selected`: muted `--text-text-tetriary` (`rgba(0,0,0,.4)`) | `--text-text-tetriary` |

> Note: the **selected** option text is *muted* (tertiary), not emphasized — it reads as
> "already chosen / inactive in the list". Hover/active option styling follows the attached
> menu rules in [22-spec-dropdown-menu.md](22-spec-dropdown-menu.md) (`--menu-hover`).

## 4. States (measured)

| State | Treatment | Token |
|---|---|---|
| Closed default | input shell, value + arrow | `--input-border-primary` |
| Opened | `custom-select.opened`; **upper title → brand** (`#2979ff`) | `--input-border-focused` |
| Selected value | shown in `select-title` | `--input-text-primary` |
| Error | `custom-select { border: 1px solid --input-border-error }` (`#eb5757`) | `--input-border-error` |
| Disabled | inherits Text Input disabled tokens | `--input-*` disabled |
| Hover | input hover border | `--input-border-hovered` (⚠️ dark missing) |

## 5. Variants

- **Value select** — pick from a list (search type, timeout, identity selects).
- **Folder selector** — `field-with-title.folder-selector`: leading folder icon + arrow,
  read-only input opening a folder **tree** (`rf-folders-tree`, `max-height:200px`); active
  state colors the label/arrow brand and shows `folders-dropdown` bordered in brand. See
  [24-spec-text-input.md](24-spec-text-input.md) §5.
- **Account / nav selector** — `selector-with-dropdown` (composite, with avatar/initials).
- **Search type selector** — compact inline select in the search dropdown.

## 6. Tokens

| Role | Token |
|---|---|
| Value text | `--input-text-primary` |
| Label | `--input-title-primary` (opened → `--input-border-focused`) |
| Label backdrop | `--input-title-background` |
| Border default / focus / error | `--input-border-primary` / `-focused` / `-error` |
| Option text | `--input-text-primary` |
| Selected option (muted) | `--text-text-tetriary` |
| Option hover | `--menu-hover` |
| List scrollbar | `--icon-icon-quinary` |
| Arrow icon | `--icon-icon-*` |
| Radius | `radius.sm (4)` |

## 7. Behavior

- Click/Enter opens the list anchored under the trigger; list scrolls past `210px`.
- Opening colors the floating label brand (the only clear "open" affordance in source) and
  flips the arrow.
- Selecting closes the list and writes the value into `select-title`; the chosen option is
  shown muted in the list.
- Folder selector opens a tree rather than a flat list.

## 8. Accessibility

- Use combobox/listbox semantics: trigger `role="combobox"` + `aria-expanded`/`aria-controls`;
  list `role="listbox"`; options `role="option"` with `aria-selected`.
- **Keyboard (must add — source is pointer-driven):** `↓`/`↑` move, `Enter`/`Space` open &
  select, `Esc` close, typeahead optional, `Home`/`End` jump.
- Focus returns to the trigger on close.
- Add a visible focus-visible ring; opened-label brand alone is not a focus indicator.
- Error associated via `aria-describedby` + `aria-invalid`; not color-only.
- Folder-tree select exposes tree semantics (`role="tree"`/`treeitem`, `aria-expanded`).

## 9. Source → DS mapping

| Source class | DS |
|---|---|
| `custom-select` | `Select` |
| `select-title` | trigger value |
| `select-upper-title` | floating label |
| `custom-select.opened` | `state=open` |
| `select-dropdown` / `options-list` | options list (attached menu) |
| `select-option` / `select-option.selected` | option / selected option |
| `custom-select` + `--input-border-error` | `state=error` |
| `field-with-title.folder-selector` | `Select variant=folder` (tree) |
| `selector-with-dropdown` | composite account/nav select |

## 10. Open questions

1. **Opened menu captures** still on the backlog (timeout select, settings select,
   account select) — confirm option row height, hover, and disabled-option styling.
2. Disabled-option treatment (e.g. unavailable permissions / timeouts).
3. Dark `--input-border-hovered` missing (shared with Text Input).
4. Multi-select / autocomplete variants (sharing recipients use an autocomplete list — may
   be a separate Combobox variant).
5. Arrow icon asset/rotation on open (inferred; confirm).
