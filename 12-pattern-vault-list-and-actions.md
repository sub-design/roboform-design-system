# Vault List And Item Actions

## Sources

Captured from full `#v8` dumps and an item clone dialog.

Captured states:

- Vault list view with login rows.
- Vault list view with safenote rows.
- List headers and managed/hidden columns.
- Row command bars.
- Icon-based `popup-menu` item actions.
- Clone dialog with folder tree.

## Vault List View

| Part | Source class | Purpose |
|---|---|---|
| Data root | `rf-data rf-folder-breadcrumbs-shown rf-view-list` | List mode data surface with breadcrumbs. |
| List section | `rf-items-section rf-items-section-main-list-view` | Main list view section. |
| Virtual list | `rf-items rf-virt-list rf-list-type-logins` | Virtualized list rows. |
| Viewport | `rf-virt-list-viewport` | Rendered visible rows. |
| Scroll height | `rf-virt-list-scroll-height` | Virtual scrolling spacer. |
| Header | `rf-list-item-header` | Column header row. |
| Row | `rf-list-item-row` | Item row. |
| Cell | `rf-list-item-field` | Column cell. |
| Commands cell | `rf-field-commands` | Row actions. |
| Header manage button | `manage-icon` | Manage visible columns. |

Captured column classes:

- `rf-field-name`
- `rf-field-folder`
- `rf-field-username`
- `rf-field-password`
- `rf-field-security`
- `rf-field-url`
- `rf-field-age`
- `rf-field-creator`
- `rf-field-permission`
- `rf-field-modified`
- `rf-field-files`
- `rf-field-verification-code`
- `rf-field-auth-commands`
- `rf-field-commands`

Design requirements:

- Visible/hidden columns are controlled per field with `hidden`.
- Column order is inline via `style="order: ..."`; design system should expose ordered columns declaratively.
- Header and rows share the same field class names.
- Password header can include `toggle-pwd toggle-pwd-hide`; do not put real password values into docs.

## Row Types

### Login row

Captured row root:

```html
<div class="rf-list-item-row rf-item-login rf-item-sec-warning cmd-assigned">
```

Captured cells:

| Cell | Example content |
|---|---|
| Name | Nested `rf-item rf-item-login`, logo/default image, title, sharing, multiselect. |
| Folder | Folder path, e.g. `ClearNorth`. |
| Username | Username/email. |
| Password | Hidden in capture. |
| Password Strength | `security-password-strength weak`. |
| URL | Host, e.g. `clearnorth.app`. |
| Commands | Log In, View, Rename, Move, Delete, More Actions. |

### Safenote row

Captured row root:

```html
<div class="rf-list-item-row rf-item-safenote cmd-assigned">
```

Captured behavior:

- Name cell nests `rf-item rf-item-safenote`.
- Username/security/URL cells show em dash where not applicable.
- Command bar omits `Log In`; includes `View`, `Rename`, `Move`, `Delete`, `More Actions`.

Design requirements:

- List rows should preserve item-type identity through icon and available commands.
- Not-applicable values use an em dash, not empty cells.
- `hover-action-cmd` marks commands that may appear on hover.

## Row Command Bar

| Command | Source class | Captured title |
|---|---|---|
| Log In | `rf-item-log-in rf-cmdbutton` | `Log In` |
| View | `rf-item-view rf-cmdbutton` | `View` |
| Sharing Settings | `rf-item-sharing-btn rf-cmdbutton` | `Sharing Settings` |
| Rename | `rf-item-rename rf-cmdbutton` | `Rename` |
| Move | `rf-item-move rf-cmdbutton` | `Move` |
| Delete | `rf-item-delete rf-cmdbutton` | `Delete` |
| More Actions | `rf-item-menu rf-cmdbutton rf-image-button auto-hide` | `More Actions` |
| Separator | `rf-cmd-separator` | Groups primary row actions from overflow. |

## Icon Popup Menu

Captured root:

```html
<div role="menu" tabindex="-1" class="unselectable popup-menu">
```

This is a separate menu system from `dropdown-popup context-menu-popup commands-menu`.

| Part | Source class | Purpose |
|---|---|---|
| Menu | `unselectable popup-menu` | Icon-based item overflow menu. |
| Item | `item` | Menu command row. |
| Item role | `role="button"` | Command activation. |
| Separator | `separator` | Groups commands. |
| Left icon | `icon icon-left cmd-*` | Command icon. |
| Text | `text` | Command label. |

Captured commands:

| Group | Commands |
|---|---|
| View | `View In New Tab` |
| Share/export | `Share`, `Send`, `Print` |
| Location | `Open File Location` |
| Manage | `Rename`, `Move`, `Clone`, `Add to Pinned`, `Delete` |
| History | `Restore` |

Design requirements:

- This menu has visible command icons; it should share spacing/keyboard behavior with other menus while preserving icon slots.
- Destructive `Delete` should map to destructive intent.
- `Open File Location` is a navigation/location command and should be grouped separately.
- `View In New Tab` is distinct from inline `View`.

## Clone Dialog

Captured root:

```html
<div class="dialog-view rf-move-or-clone rf-clone rf-modal-dialog">
```

| Part | Source class | Purpose |
|---|---|---|
| Dialog | `dialog-view rf-move-or-clone rf-clone rf-modal-dialog` | Clone item dialog. |
| Header | `header` | Captured `Clone`. |
| Close | `close-button` | Dismiss dialog. |
| Body | `rf-body` | Form content. |
| Name input | `rf-input rf-item-name` | New/cloned item name. |
| Folder label | `rf-title folder` | Folder selection section. |
| Folder path | `rf-title folder-path` | Current selected folder, e.g. `ClearNorth`. |
| Folder tree | `rf-folders-tree active` | Select target folder. |
| Highlighted folder | `rf-item-folder highlighted` | Current/selected target folder. |
| Sharing info-only | `rf-item-sharing ... rf-info-only` | Displays sharing state without command behavior. |
| Action bar | `rf-buttons-bar` | Cancel/OK. |
| OK disabled | `rf-ok-button default-button disabled-button` | Clone unavailable until valid/change conditions are met. |

Design requirements:

- Move and Clone can reuse this dialog shell with variant title/action.
- Folder tree must show sharing indicators as informational overlays.
- The selected folder is visible both in path text and highlighted tree row.

## States To Document

| State | Captured | Notes |
|---|---:|---|
| List view login rows | Yes | Includes security warning rows and visible columns. |
| List view safenote rows | Yes | Non-applicable cells captured as em dash. |
| Icon popup menu | Yes | `popup-menu` with command icons. |
| Clone dialog | Yes | Folder tree and disabled OK captured. |
| Move dialog | No | Likely same shell without `rf-clone`; capture if behavior differs. |
| Popup hover/disabled states | No | Need item hover/disabled if available. |
