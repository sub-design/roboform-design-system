# Component Spec: Command Bar

**Status:** Ready for DS (final spec) · **Tier:** Composite
**Evidence:** vault row hover commands, Security Center multiselect batch bar, editor /
identity instance command bars. Composes [20-spec-button.md](20-spec-button.md) and
[21-spec-icon-button.md](21-spec-icon-button.md).

Tokens: [19-foundations-tokens.md](19-foundations-tokens.md). Source classes:
[02-component-state-matrix.md](02-component-state-matrix.md).

---

## 1. Variants (measured)

| Variant | Source | Geometry |
|---|---|---|
| **Multiselect batch bar** | `rf-multiselect-cmdbar` | `height:54–56px`, `width:100%`, `padding:0 30px 0 40px`, flex, align center |
| **Row hover cmdbar** | `rf-item-cmdbar` | `height:49px`, absolute `right:0; top:0`, `display:none` → `flex` on row hover, surface bg |
| **Editor / instance cmdbar** | `editor-cmdbar` / `cmdbar-instance` | contextual command cluster in editor headers |

## 2. Command item (measured)

| Property | Value | Token |
|---|---|---|
| Item | `rf-multiselect-cmd-item`: `height:36px`, `font:15/600`, color `--text-text-quaternary` (`.7`), `border-radius:8px`, `min-width:50px` | — |
| Framed item | `cmd-framed`: bg `--surface-surface`, `border:1px --border-border-primary`, `min-width:120px`, `padding:0 18px 2px 16px` | — |
| Framed hover | `--background-neutral-hovered` (`rgba(41,121,255,.08)` / dark `#404040`) | `--background-neutral-hovered` |

Items are either icon+text (framed) or icon-only (compact), per Icon Button sizing.

## 3. States

| State | Treatment |
|---|---|
| Hidden | row cmdbar hidden until row hover/selection |
| Visible | batch bar shown while multiselect in process |
| Item hover | framed: neutral hover fill; icon-only: per Icon Button |
| Disabled | `rf-cmd-disabled` (per Icon Button) |
| Responsive | command **text progressively hidden** at narrow widths → icon-only (see [01-source-inventory.md](01-source-inventory.md)) |

## 4. Behavior

- **Row cmdbar:** appears on row hover (absolutely positioned over the right of the row,
  no layout shift); holds per-item actions (log in, copy, view, edit, share, delete…).
- **Multiselect bar:** appears when ≥1 item is selected; holds batch actions (exclude, log
  in, move, clone, delete, select all, deselect) + a selection count/status.
- Destructive actions (delete) map to destructive intent per
  [20-spec-button.md](20-spec-button.md), not the neutral source styling.
- Progressive disclosure: as width shrinks, labels drop to icons; least-important commands
  collapse first.

## 5. Tokens

| Role | Token |
|---|---|
| Item text | `--text-text-quaternary` |
| Framed surface | `--surface-surface` |
| Framed border | `--border-border-primary` |
| Hover fill | `--background-neutral-hovered` |
| Bar surface (row) | `--surface-surface` / `-04` |
| Radius | `radius.lg (8)` |

## 6. Accessibility

- Group with `role="toolbar"`; arrow-key navigation between commands.
- Icon-only commands need accessible names (per Icon Button).
- Row hover commands must be keyboard-reachable when the row is focused (not hover-only).
- Multiselect bar announces the selection count; batch destructive actions name the
  consequence and the count.
- Maintain a logical focus order as commands collapse responsively.

## 7. Source → DS mapping

| Source class | DS |
|---|---|
| `rf-multiselect-cmdbar` | `CommandBar variant=multiselect` |
| `rf-multiselect-cmd-item` / `cmd-framed` | command item (framed/icon-only) |
| `rf-item-cmdbar` | `CommandBar variant=row` |
| `editor-cmdbar` / `cmdbar-instance` | `CommandBar variant=editor` |
| `rf-cmd-disabled` | item `state=disabled` |

## 8. Open questions

1. Exact responsive thresholds for dropping each command's label (general behavior known,
   per-command order not isolated).
2. Editor/instance cmdbar geometry vs multiselect bar (instance bar not fully measured).
3. Overflow menu for commands that don't fit (vs pure label-drop).
4. Focus/keyboard contract confirmation (toolbar roving tabindex).
