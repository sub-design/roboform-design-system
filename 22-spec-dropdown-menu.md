# Component Spec: Dropdown / Context Menu

**Status:** Ready for DS (first final spec) · **Tier:** Core primitive
**Evidence:** account menu, create menu, commands context menu, icon popup menu, permission
menu, Help submenu. Appears across the whole product.

Tokens: [19-foundations-tokens.md](19-foundations-tokens.md). Source classes:
[02-component-state-matrix.md](02-component-state-matrix.md). Related pattern detail in
[04-components-account-menu.md](04-components-account-menu.md),
[05-components-create-menu.md](05-components-create-menu.md).

---

## 1. Anatomy

```
┌─ dropdown-popup (surface, radius 8, padding 10, shadow) ─┐
│  ▸ item        [icon 24]  Label                          │
│  ▸ item (hover/selected)                                 │
│  ─ separator ─                                           │
│  ▸ item with submenu                          ▸          │
│      └─ dropdown-popup-submenu (nested)                  │
└──────────────────────────────────────────────────────────┘
```

Two related source families exist:

- **`dropdown-popup`** — floating menu (account, create, commands context menu). Radius 8,
  padding 10.
- **`dropdown-popup-list`** — inline select list (combobox). `max-height: 300px`,
  `overflow-y: auto`, anchored under the field (`top: 36–38px`), `width: 100%`.

The DS treats these as one **Menu** primitive with `mode="floating" | "attached"`.

## 2. Container (measured)

| Property | Value | Token |
|---|---|---|
| Radius | `8px` | `radius.lg (8)` |
| Padding | `10px` (popup) / `3px 0 5px` (list) | `space.*` |
| Surface | `#fff` / `#2f2f2f` (popup); `#fff` / `#2a2a2a` (list) | `--surface-surface-08/10` |
| Shadow | `0 8px 16px rgba(0,0,0,.2)` (list); menu layer for popup | `shadow.menu` |
| Max height | `300px` then scroll | — |
| z-index | `dropdown-popup: 10`, `context-menu-popup: 20` | `z.dropdown` / `z.context-menu` |

## 3. Item (measured)

| Property | Value | Token |
|---|---|---|
| Radius | `8px` | `radius.lg (8)` |
| Padding | `8px 12px` | `space.*` |
| Line height | `24px` | — |
| Text | `rgba(0,0,0,.7)` / `hsla(0,0%,100%,.7)` | `--text-text-quaternary` |
| Leading icon | `24×24`, `margin-right: 12px` | — |

## 4. Separator (measured)

`background: --border-border-secondary`; `margin: 6px 8px`; `width: calc(100% - 16px)`.
→ `Menu.Separator`.

## 5. Variants

- Menu **with icons** (account, create, commands).
- Menu **without icons** (simple lists).
- **Submenu** (`dropdown-popup-submenu`, positioned `right: 25px; top: 36px`) — e.g. Help.
- **Permission menu** with item + description (`permission-menu`, `permission-tip`,
  `item selected`).
- **Attached select list** (`dropdown-popup-list`) for comboboxes/folder selector.

## 6. States

| State | Treatment | Token |
|---|---|---|
| Default item | quaternary text | `--text-text-quaternary` |
| Hover | fill tint | `--menu-hover` (`rgba(41,121,255,.08)` / `hsla(0,0%,100%,.08)`) |
| Selected | check / `item selected`, selected text | `--text-text-selected` |
| Disabled | muted, non-interactive | `--text-text-secondary-disabled` |
| Open/close | `rf-fade-in` / `rf-fade-out` | `motion.base (300ms)` |

## 7. Behavior

- Opens anchored to its trigger; floating popups overlay at z 10–20, above the modal
  screen surfaces they belong to.
- Submenu opens to the side of its parent item; only one submenu path open at a time.
- Attached select lists size to the field width and scroll past 300px.
- Closes on outside click, Escape, and selection.

## 8. Accessibility

- **Keyboard contract (must add — source is pointer-driven):** `↓`/`↑` move between items,
  `→`/`Enter` open submenu, `←`/`Esc` close, `Home`/`End` jump, typeahead optional.
- Use `role="menu"` + `role="menuitem"` (or `listbox`/`option` for the attached select).
- Focus moves into the menu on open and **returns to the trigger on close**.
- Selected/active item exposed via `aria-checked`/`aria-selected`.
- Disabled items use `aria-disabled` and are skipped by arrow navigation.
- Destructive/sensitive command items (Delete, reveal) need clear names; never echo secrets.

## 9. Source → DS mapping

| Source class | DS |
|---|---|
| `dropdown-popup` | `Menu mode=floating` |
| `dropdown-popup-list` | `Menu mode=attached` (combobox list) |
| `dropdown-popup-item` | `Menu.Item` |
| `dropdown-popup-item-image` | `Menu.Item icon` |
| `dropdown-popup-separator` | `Menu.Separator` |
| `dropdown-popup-submenu` | `Menu.Submenu` |
| `context-menu-popup` / `commands-menu` | `Menu` (z 20, command context) |
| `popup-menu` / `permission-menu` | `Menu variant=permission` |
| `item selected` | `Menu.Item state=selected` |

## 10. Open questions

1. Confirmed item hover/focus/disabled visuals for the **icon popup-menu** family vs
   `dropdown-popup` (they may differ).
2. Help submenu open-state capture (currently inferred from positioning rules only).
3. Final keyboard spec and roles (none present in source).
4. Whether `dropdown-popup` and `popup-menu` should remain one component or split.
