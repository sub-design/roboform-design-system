# Component Spec: Sharing Indicator

**Status:** Ready for DS (final spec) · **Tier:** Product component
**Evidence:** vault items, folders, Sharing Center sections, identity navigator, recipients
table. Marks the share/permission status of an item.

Tokens: [19-foundations-tokens.md](19-foundations-tokens.md). Source classes:
[02-component-state-matrix.md](02-component-state-matrix.md). Pattern:
[10-pattern-sharing-center.md](10-pattern-sharing-center.md). Related:
[28-spec-vault-item.md](28-spec-vault-item.md), [22-spec-dropdown-menu.md](22-spec-dropdown-menu.md)
(permission menu).

---

## 1. Anatomy

A small circular overlay icon (`rf-item-sharing`) layered on an item/folder, whose glyph
encodes the sharing **role**. Hidden when not shared.

```
[ item media ] ⬡   ← rf-item-sharing overlay (role glyph)
```

## 2. Role variants (measured)

| DS role | Source class | Icon |
|---|---|---|
| Not shared | `shared-none` | hidden (`display:none`) |
| Group · regular | `shared-group-regular` | `sharing-overlay-regular.svg` |
| Group · manager | `shared-group-manager` | `sharing-overlay-manager.svg` |
| Group · limited | `shared-group-limited` | `sharing-overlay-limited.svg` |
| Received · regular | `shared-received` | `sharing-overlay-received-regular.svg` |
| Received · limited | `shared-limited` | `sharing-overlay-received-limited.svg` |
| Granted | `shared-granted` | `sharing-overlay-granted.svg` |

Roles split along two axes: **direction** (shared by me / received / granted) and
**permission** (limited / regular / manager). The full permission vocabulary (limited,
regular, manager, login-only) is selected via the permission menu — see §5.

## 3. Geometry per context (measured)

`rf-item-sharing` is `position:absolute` over the host; its size/anchor varies by view:

| Context | Size | Anchor | Shape |
|---|---|---|---|
| List row | `16×16` (bg 12) | `left:68px; top:24px` | circle |
| List row (alt) | `24×24` (bg 16) | `left:64px; top:20px` | circle |
| Grid card | `28×28` (bg 20) | `left:4px` | circle |
| Folder / large | `36×36` (bg 24) | `left:4px; bottom:3px` | circle |
| Compact | `12×12` | `left:16/40px; top:12/22px` | contain |

Hover fill: `--background-neutral-hovered` (`rgba(0,0,0,.08)` / dark) when interactive.

## 4. States

| State | Treatment |
|---|---|
| Not shared | hidden (`shared-none { display:none }`) |
| Shared (role) | role glyph visible |
| Received | always shown (`shared-received { display:inherit }`) |
| Hover (interactive) | neutral hover fill |
| Focus | add focus-visible ring if interactive (source has none) |

## 5. Relationship to permission controls

- **Sharing button** (`rf-item-sharing-btn`, `cmd-share.svg`) — the action that opens
  sharing settings; an Icon Button, not the indicator
  (see [21-spec-icon-button.md](21-spec-icon-button.md)).
- **Permission menu** (`popup-menu permission-menu`, `item selected`, `permission-tip`) —
  selects the role (Log in only / Read and write / Full control); a Dropdown variant
  (see [22-spec-dropdown-menu.md](22-spec-dropdown-menu.md)).
- **Recipient role/status** in the recipients table: `rf-perm` (clickable role control,
  `margin-left:8px; pointer-events:all`) and `recipiant-status` *(source typo)* status text
  (`text-align:end; white-space:nowrap`). The DS name is **recipient**.

These are separate components; the Sharing Indicator is only the status glyph.

## 6. Tokens

| Role | Token |
|---|---|
| Hover fill | `--background-neutral-hovered` |
| Icon | role SVG (recolor via `--icon-*` where monochrome) |
| Focus ring (if interactive) | `--border-border-focused` |

> Sharing role colors are not in the v8 token block; the role SVGs are currently distinct
> assets. The DS should define a `color.sharing.*` family (regular / manager / limited /
> received / granted / login-only) per
> [16-foundations-draft.md](16-foundations-draft.md) §Sharing.

## 7. Accessibility

- **Not color/icon-only:** the role must be in the accessible name (e.g. "Shared — Manager",
  "Received — Read only"), since the only visual cue is a small glyph.
- If the indicator is purely informational, expose it as text to AT, not a decorative image.
- If interactive (opens settings), it is a button with a clear name and focus-visible ring.
- Distinguish the six role glyphs by shape, not hue alone (verify the SVGs differ in form).

## 8. Source → DS mapping

| Source class | DS |
|---|---|
| `rf-item-sharing` | `SharingIndicator` |
| `shared-none` | `role=none` (hidden) |
| `shared-group-regular/manager/limited` | `role=group-*` |
| `shared-received` / `shared-limited` | `role=received-*` |
| `shared-granted` | `role=granted` |
| `rf-item-sharing-btn` | share action (Icon Button) |
| `permission-menu` / `permission-tip` | permission menu (Dropdown) |
| `rf-perm` | recipient role control |
| `recipiant-status` | recipient status text (rename → recipient) |

## 9. Open questions

1. `login-only` and `pending` indicator glyphs (referenced as roles; not isolated as
   `rf-item-sharing` variants here).
2. Define the `color.sharing.*` token family (no v8 color tokens for roles yet).
3. Confirm the six role SVGs are shape-distinct (color-blind accessibility).
4. Interactive vs informational: which contexts make the indicator clickable.
5. Identity navigator sharing indicator geometry (referenced; verify against item variants).
