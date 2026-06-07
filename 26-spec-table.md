# Component Spec: Table

**Status:** Ready for DS (final spec) · **Tier:** Data component
**Evidence:** Security Center tables (compromised/weak/reused/all/duplicates/excluded),
vault list view, Manage Columns dialog, multiselect command bar. High-frequency.

Tokens: [19-foundations-tokens.md](19-foundations-tokens.md). Source classes:
[02-component-state-matrix.md](02-component-state-matrix.md). Pattern detail:
[09-pattern-security-center.md](09-pattern-security-center.md),
[12-pattern-vault-list-and-actions.md](12-pattern-vault-list-and-actions.md).

---

## 1. Anatomy

```
┌ table-outer (position:relative, height:100%) ───────────────────┐
│ table-inner (flex, surface bg, height: calc(100vh - 400px)) ─── │
│ ┌ table-items (border-collapse, table-layout:fixed, width 100%) │
│ │ thead  (sticky top:54px, z2)   Name 35% │ … │ Strength │ ⋮    │
│ │ ────────────────────────────────────────────────────────────  │
│ │ tbody  row (height 50px)   value cells …        [hover cmds]   │
│ │        group row (duplicate grouping)                          │
│ └───────────────────────────────────────────────────────────────│
└──────────────────────────────────────────────────────────────────┘
```

## 2. Container (measured)

| Part | Property | Value | Token |
|---|---|---|---|
| `table-outer` | layout | `position:relative; height:100%` | — |
| `table-inner` | bg | `#fafafa` / `#1a1a1a` | `--surface-surface-05` |
| `table-inner` | height | `calc(100vh - 400px)` (scroll region) | — |
| `table-items` | model | `border-collapse:collapse; table-layout:fixed; width:100%` | — |
| `table-items` | bg | `#fff` / `#343434` | `--surface-surface` / `-04` |

## 3. Header (measured)

| Property | Value | Token |
|---|---|---|
| Background | `#f0f0f0` / `#2a2a2a` | `--surface-surface-06` |
| Sticky | `position:sticky; top:54px; z-index:2` | — |
| Cell height | `44px` | — |
| Text | `rgba(0,0,0,.54)` / `.54` white, `white-space:nowrap` | `--text-text-secondary` |
| Column widths | `name-header-column: 35%`, `password-header-column: 20%`, others fluid | fixed layout |

### Sort (measured)

- `header-sort { cursor:pointer }`; arrow is **hidden by default** (`sort-arrow { display:none }`)
  and appears only when sorted.
- `header-sort-asc .sort-arrow` → up arrow (`arrow-up2-black40` / `white40`).
- `header-sort-desc .sort-arrow` → down arrow (`arrow-down2` / `-dark`).
- Arrow box `20×20`, `margin-left:2px`. → Active sort = direction class on the header cell.

## 4. Row (measured)

| Property | Value | Token |
|---|---|---|
| Height | `50px` | — |
| Text | `rgba(0,0,0,.54)` / `.54` white | `--text-text-secondary` |
| Cursor | `pointer` (rows are clickable) | — |
| Padding | `0 12px 0 0`; last cell `padding-right:0` | — |
| Row divider | `box-shadow: inset 0 -1px 0 0 rgba(0,0,0,.08)` | `--border-border-secondary` |
| Vertical align | `middle`; `white-space:nowrap` | — |

### Row states (measured, light → dark)

| State | Source | Light | Dark | Token |
|---|---|---|---|---|
| Hover | `tr:hover td` | `#eef4ff` | `hsla(0,0%,100%,.08)` | `--background-background-information` (light) / `--background-neutral-hovered` |
| Menu open | `tr.shown-popup-menu td` | `#eef4ff` | `.08` | same as hover |
| Multiselect hover/selected | `tr.rf-multiselect-hovered/selected td` | `#ddeaff` | `hsla(0,0%,100%,.16)` | selected fill (deeper) |
| Cmdbar revealed cell | `tr.rf-list-view-cmdbar-hovered td` | `#fff` | `#343434` | surface (so commands read clearly) |

### Group row (duplicate grouping)

`tbody td.group { font-weight:400; padding-bottom:24px; padding-left:30px; background:inherit }`
— used for Complete Duplicates grouping headers.

## 5. Columns & cells

- Column set varies by tab: `c_name`, `c_url`, `c_username`, `c_password`, `c_strength`,
  `c_age`, `c_location`, `c_commands`.
- **Password column** — masked; column-level reveal toggle in the header
  (`password-header-column` + `toggle-pwd`); see
  [25-spec-password-input.md](25-spec-password-input.md).
- **Strength column** — `security-password-strength` badge (weak/medium/good/strong),
  font 15px; maps to `color.rating.*`. See Security Strength Badge candidate.
- **Commands column** (`c_commands`) — `rf-item-cmdbar` absolute right, **hidden until row
  hover** (`display:none` → `flex`); `rf-cmdbutton` 36×36 circular, hover fill
  `--background-neutral-hovered`. See [21-spec-icon-button.md](21-spec-icon-button.md).

## 6. Multiselect

- Row checkbox (`rf-item-multiselect`) appears on row hover or while
  `rf-multiselect-in-process`.
- Selected rows take the deeper selected fill (`#ddeaff` / `.16`).
- Pairs with the multiselect command bar (`rf-multiselect-cmdbar`); see Command Bar
  candidate. Batch actions: exclude, log in, move, clone, delete, select all, deselect.

## 7. Manage Columns (measured)

`manage-columns-dialog` (width `440px`): `columns-list .item` rows `padding:10px 24px`,
`draggable` rows `cursor:grab`/`grabbing` with a `drag-icon-20` handle and
`--background-neutral-hovered` hover; the **Name column is locked** (`locker-icon`) and not
reorderable; Save disabled until a change. A `Dialog variant=management` (see
[23-spec-dialog.md](23-spec-dialog.md)) hosting a reorderable list.

## 8. Tokens

| Role | Token |
|---|---|
| Outer/scroll surface | `--surface-surface-05` |
| Table surface | `--surface-surface` |
| Header surface | `--surface-surface-06` |
| Header / cell text | `--text-text-secondary` |
| Row divider | `--border-border-secondary` |
| Row hover / menu-open | `--background-background-information` (light) / `--background-neutral-hovered` (dark) |
| Row selected | deeper selected fill (`#ddeaff` / `.16`) — needs a named token |
| Strength badge | `color.rating.*` (`--rag-rating-*`) |
| Sort arrow | `--icon-icon-*` (recolor the black40/white40 assets) |

## 9. Behavior

- Header is sticky under the toolbar (`top:54px`); body scrolls within `table-inner`.
- Rows are clickable (open detail); hover reveals the command cluster without shifting
  layout (commands are absolutely positioned).
- Sorting toggles none → asc → desc on a header; only one sorted column at a time; arrow
  shows direction.
- `table-layout:fixed` + percentage widths keep columns stable; long values truncate
  (`white-space:nowrap`) — confirm ellipsis + title tooltip per cell.

## 10. Accessibility

- Use real table semantics (`<table>`/`<th scope>`/`<td>`); sortable headers are buttons
  with `aria-sort` reflecting `none|ascending|descending` — **source conveys sort by an
  arrow image only**, so the DS must add `aria-sort`.
- Row selection exposes `aria-selected`; the select-all is a labelled control.
- Hover-only command buttons must be keyboard-reachable per row and have accessible names.
- Strength/risk must not rely on color alone (badge text + icon).
- Password cells: masked value not exposed in plaintext to AT while masked; column reveal
  follows the password security rules.
- Provide a visible focus indicator for focused rows/cells (`--border-border-focused`).

## 11. Source → DS mapping

| Source class | DS |
|---|---|
| `table-outer` / `table-inner` | `Table` scroll container |
| `table-items` | `Table` element |
| `thead td` / `*-header-column` | `Table.HeaderCell` (+ width) |
| `header-sort` / `header-sort-asc/desc` / `sort-arrow` | sortable header (`aria-sort`) |
| `tbody td` | `Table.Cell` |
| `tr.rf-multiselect-selected/hovered` | row `selected`/`hover` |
| `tr.shown-popup-menu` | row `menu-open` |
| `td.group` | group/section row |
| `c_commands` / `rf-item-cmdbar` | row command cluster |
| `manage-columns-dialog` / `columns-list` | Manage Columns (management dialog) |

## 12. Open questions

1. **Active sorted state** still on the capture backlog (asc/desc confirmed by class, but a
   live capture would confirm header styling and aria).
2. **Empty and loading** table states not captured.
3. Per-cell truncation: ellipsis + title tooltip behavior for long values.
4. The selected-row fill (`#ddeaff` / `.16` white) has no named token yet — add one.
5. Column resize behavior (only reorder via Manage Columns is confirmed).
6. Vault list view vs Security Center table: confirm they share one Table or diverge.
