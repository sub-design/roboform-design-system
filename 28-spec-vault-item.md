# Component Spec: Vault Item / Row

**Status:** Ready for DS (final spec) · **Tier:** Product component (core)
**Evidence:** grid (large), condensed, and list views; logins, bookmarks, safenotes,
folders, identities; Security Center rows; Sharing sections. The primary browsing surface.

Tokens: [19-foundations-tokens.md](19-foundations-tokens.md). Source classes:
[02-component-state-matrix.md](02-component-state-matrix.md). Pattern:
[12-pattern-vault-list-and-actions.md](12-pattern-vault-list-and-actions.md).

---

## 1. Anatomy

```
GRID (large)            CONDENSED            LIST
┌──────────────┐        ┌──────────────┐    ┌────────────────────────────┐
│   [logo 48]  │        │ [logo] title │    │ ☐ [img] ⬡ Title        ⋯  │
│              │        └──────────────┘    └────────────────────────────┘
│    Title     │        height 75            height 36
└──────────────┘
 214×135, radius 8
 hover → overlay "Click to Log In"
```

Shared parts: selection checkbox (`rf-item-multiselect`), type image/logo
(`rf-item-image` / `rf-item-imgLogo` / `rf-item-default-image`), title (`rf-item-title`),
sharing indicator (`rf-item-sharing`), security-warning button (`rf-sec-warning-button`),
hover overlay (grid), command bar (list/table).

## 2. View modes (measured)

| Mode | Source | Size | Shape |
|---|---|---|---|
| **Grid / large** | `rf-view-large .rf-item` | `flex:0 0 214px; height:135px; padding:30px 0 3px` | radius `8`, `border:1px transparent` |
| **Condensed** | `rf-view-condensed .rf-item` | `flex:0 0 214px; height:75px` | radius `8`; `item-content` row-reverse |
| **List** | `rf-items .rf-item` | `height:36px; width:100%` | transparent, no border/shadow |

Grid uses fixed `214px` cards wrapping; a fixed-num-per-line mode sets `width: calc(20% - 15px)`
(5 per row) within `880–1180px`.

## 3. Card states — grid (measured)

| State | Border | Shadow |
|---|---|---|
| Default | `1px solid transparent` | `0 1px 2px rgba(0,0,0,.12), 0 1px 4px rgba(0,0,0,.04)` (subtle) |
| Hover / selected / menu-open / can-drop | `1px solid #838789` (light) / `#a7acae` (dark) | `0 5px 10px rgba(44,44,44,.4)` / `hsla(197,4%,67%,.4)` |
| Surface | `#fff` / `#343434` | — |

> One ruleset drives `:hover`, `.selected`, `.shown-popup-menu`, `.rf-multiselect-selected`,
> `.rf-multiselect-can-drop` — they share the elevated/bordered treatment.

List rows are flat: `background:transparent`, no hover elevation (hover is conveyed by the
row command bar and overlays, not a card lift).

## 4. Hover overlay — grid (measured)

`rf-item-hover-overlay`: `display:none` → `flex` on hover; covers the card +2px
(`height/width: calc(100% + 2px)`, offset `-1px`), `border-radius:6px`, `z-index:2`.

| Part | Value |
|---|---|
| Scrim | `rgba(0,0,0,.5)` (light) / `hsla(0,0%,100%,.5)` (dark) |
| Action text | `rf-item-hover-overlay-text` 16px/500 + icon (e.g. "Click to Log In") |
| Title strip | `rf-item-hover-overlay-title` bottom, 14px, ellipsis |

On hover the normal `rf-item-title` is hidden (`:hover .rf-item-title { display:none }`)
except for folders / add-new; the overlay title takes over.

## 5. Sub-elements (measured, list view)

| Element | Source | Geometry |
|---|---|---|
| Type image | `rf-item-image` | `24×24`, abs `left:52px; top:6px` (grid default image `48×48`) |
| Title | `rf-item-title` | font 15, `line-height:36px`, `padding:0 7px 0 97px`, ellipsis nowrap |
| Sharing indicator | `rf-item-sharing` | `16×16`, abs `left:68px; top:24px`, circular, hover `--background-neutral-hovered` |
| Security warning | `rf-sec-warning-button` | `24×24` circular, `compromised-icon-list.svg`, hover tint |
| Multiselect | `rf-item-multiselect` | `20×20`, abs `left:15px; top:9px`; shown on row hover or in-process |

## 6. Custom item background (measured)

Branded items use `rf-has-own-background` with a contrast gradient overlay:

- `:before` = `linear-gradient(180deg, transparent → rgba(255,255,255,.8))` (light) /
  `(transparent → rgba(56,56,56,.8))` (dark) so the title stays legible over any logo color.
- Per-item contrast classes `rf-dark` / `rf-light` flip text/icon contrast locally,
  independent of the global theme.

> **Theme rule:** do not infer per-item contrast from the global theme; custom backgrounds
> carry their own `rf-dark`/`rf-light` contrast logic (see
> [16-foundations-draft.md](16-foundations-draft.md) §Theme).

## 7. Item types & variants

- **Login** (`rf-item-login`) — site logo (`rf-item-imgLogo`) or default image; security
  warning; custom background.
- **Bookmark / Safenote / Folder / Identity** — type-specific icon; folder has
  `has-item-count` (count chip, title shifts up).
- **Add-new item** (`rf-add-new-item`) — circular 50px outlined icon with `+`, muted title,
  darkens on hover.

## 8. States summary

| State | Grid | List |
|---|---|---|
| Default | subtle shadow | flat |
| Hover | elevated + overlay | command bar + multiselect appear |
| Selected / multiselect | elevated, bordered | selected fill (shared token) |
| Menu open | elevated (`shown-popup-menu`) | row tint |
| Drag target | `rf-multiselect-can-drop` elevated | — |
| Security warning | warning button visible | warning button inline |
| Shared | sharing indicator | sharing indicator |

## 9. Tokens

| Role | Token |
|---|---|
| Card surface | `--surface-surface` / `-04` |
| Title text | `--text-text-primary` |
| Card border (rest) | transparent |
| Card border (active) | `#838789` / `#a7acae` — needs a named token |
| Card shadow rest / active | `shadow.subtle` / legacy `0 5px 10px .4` (consolidate) |
| Overlay scrim | `rgba(0,0,0,.5)` / `hsla(0,0%,100%,.5)` → `color.scrim.item` |
| Sharing/warning hover | `--background-neutral-hovered` |
| Selected fill (list) | shared selected-row token (`#ddeaff` / dark) |
| Custom-bg gradient | per-item contrast overlay |

## 10. Behavior

- Grid: hover lifts the card and shows the action overlay; click runs the primary action
  (e.g. log in). Title hidden under overlay (folders excepted).
- List: hover reveals the command bar and multiselect checkbox without layout shift
  (absolutely positioned overlays).
- Long titles truncate with ellipsis; provide a title tooltip.
- Multiselect mode (`rf-multiselect-in-process`) keeps checkboxes visible and pairs with the
  multiselect command bar.

## 11. Accessibility

- Each item is a single activatable control with an accessible name = the item title;
  the type (login/bookmark/folder) is conveyed in the name, not by icon alone.
- **Hover-only affordances (overlay action, command bar, multiselect, warning) must be
  keyboard-reachable and focusable**; the overlay "Click to Log In" needs a real focusable
  control.
- Add a visible focus-visible indicator (`--border-border-focused`); source elevation is
  not a focus signal.
- Custom-background items must meet text contrast via the gradient + `rf-dark`/`rf-light`;
  verify AA in both themes.
- Security-warning and sharing status must not rely on icon/color alone — expose via name.
- Selection state exposed with `aria-selected`/checkbox semantics.

## 12. Source → DS mapping

| Source class | DS |
|---|---|
| `rf-item` | `VaultItem` |
| `rf-view-large` / `rf-view-condensed` / list | `view=grid|condensed|list` |
| `rf-item-image` / `rf-item-imgLogo` / `rf-item-default-image` | item media |
| `rf-item-title` | title |
| `rf-item-hover-overlay*` | grid hover overlay |
| `rf-item-sharing` | sharing indicator (see Sharing Indicator candidate) |
| `rf-sec-warning-button` | security warning (see Strength/Risk) |
| `rf-item-multiselect` | selection checkbox |
| `rf-has-own-background` / `rf-dark` / `rf-light` | custom background + contrast |
| `rf-add-new-item` | add-new affordance |
| `has-item-count` | folder count chip |
| `.selected` / `.shown-popup-menu` / `.rf-multiselect-*` | item states |

## 13. Open questions

1. **Clean grid default capture** still on the backlog (states inferred from CSS; a live
   default-grid dump would confirm spacing/anatomy).
2. Active card border (`#838789`/`#a7acae`) and the legacy `0 5px 10px .4` shadow need
   named tokens (consolidate with elevation scale).
3. List-view selected fill = shared selected-row token (same gap as Table / Copyable Field).
4. Bookmark/safenote/identity grid anatomy differences vs login (only login measured here).
5. Drag-and-drop interaction states beyond `rf-multiselect-can-drop`.
