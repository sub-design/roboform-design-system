# Sharing Center And Sharing Settings

## Sources

Captured from Sharing Center full `#v8` dumps and sharing dialogs.

Captured states:

- Sharing Center with `Shared With Me` active.
- Sharing Center with `Shared By Me` active.
- Shared folders, groups, and shared files sections.
- Folder tree sharing indicators.
- Group Settings dialog.
- Shared Folder settings dialog for creator/owner.
- Received Shared Folder settings dialog with `Remove Me`.
- Create New Shared Folder dialog.
- Recipient autocomplete list, permission dropdown shell, recipient rows, owner/manager/admin/not-received statuses.

## Sharing Center

| Part | Source class | Purpose |
|---|---|---|
| Navigator item | `sharing-center-select active` | Sharing Center selected in RoboForm Tools. |
| Header selector | `sharing-center-order-selector order-selector noselect` | Switches shared direction. |
| Shared with me tab | `sharing-center-shared-with-me active` | Items shared to current user. |
| Shared by me tab | `sharing-center-shared-by-me active` | Items current user shares with others. |
| Data root | `rf-data rf-view-large sharing-data-view` | Sharing-specific grid view. |
| Shared folders section | `rf-items-section rf-items-section-shared-folders` | Shared folder cards. |
| Groups section | `rf-items-section rf-items-section-groups` | Shared group cards. |
| Shared files section | `rf-items-section rf-items-section-shared-files` | Shared login/file cards. |
| Section header | `rf-section-header` | Section title wrapper. |
| Section title | `rf-section-title` | Captured `Shared Folders`, `Groups`, `Shared Files`. |
| Create trigger | `rf-new-button plus-image` | Title changes to `Create new Shared Folder`. |

Captured section behavior:

- In `Shared With Me`, `Shared Files` can be hidden/empty while Shared Folders and Groups are visible.
- In `Shared By Me`, Shared Folders and Shared Files can both be visible.
- Sharing Center still reuses vault item cards: `rf-item`, `rf-item-folder`, `rf-item-login`, `rf-item-menu`, `rf-item-multiselect-overlay`.

## Sharing Indicators

| Source class | Captured meaning |
|---|---|
| `rf-item-sharing` | Sharing status/action marker. |
| `rf-item-sharing-shown` | Item displays sharing indicator. |
| `shared-none` | Base/no direct overlay state in class stack. |
| `shared-folder-regular` | Shared folder, regular access. |
| `shared-folder-manager` | Shared folder, manager/creator-like access. |
| `shared-group-regular` | Shared group, regular access. |
| `shared-group-manager` | Shared group, manager access. |
| `shared-granted` | Granted shared login/item access. |
| `rf-info-only` | Informational sharing indicator; captured in identity/navigation context. |

Design requirements:

- Sharing indicator is both status and command when `cmd-assigned` is present.
- Indicators appear in folder tree, vault cards, security table rows, and Sharing Center cards.
- The design API should separate object type (`folder`, `group`, `login`) from permission level (`regular`, `manager`, `granted`, `info-only`).

## Shared Item Cards

| Part | Source class | Purpose |
|---|---|---|
| Folder card | `rf-item rf-item-folder` | Shared folder/group item. |
| Login card | `rf-item rf-item-login` | Shared login/file item. |
| Title | `rf-item-title` | Item name. |
| Count | `rf-item-count` | Folder/group count placeholder. |
| Sharing control | `rf-item-sharing rf-cmdbutton ...` | Opens settings or displays status. |
| Multiselect overlay | `rf-item-multiselect-overlay` | Selection overlay. |
| More actions | `rf-item-menu rf-cmdbutton rf-image-button auto-hide` | Row/card actions. |
| Security warning | `rf-sec-warning-button` | Captured on shared login with compromised password warning. |

## Sharing Dialog Shell

| Part | Source class | Purpose |
|---|---|---|
| Modal screen | `modal-dialog-screen` | Dialog backdrop/focus root. |
| Dialog | `dialog-view rf-sharing-dialog rf-sharing-folder rf-modal-dialog` | Sharing settings dialog. |
| Create dialog | `rf-create-sharing-folder` | Create New Shared Folder variant. |
| Received folder dialog | `rf-received-folder` | Received shared folder variant. |
| Header | `header` | Dialog title. |
| Close | `close-button` | Dismiss dialog. |
| Body | `sharing-dialog-body` | Dialog content area. |
| Content | `rf-sharing-content` | Editable sharing settings content. |
| Footer | `rf-buttons-bar` | Dialog actions. |
| Loading overlay | `rf-loading-overlay hidden` | Hidden async state. |

Captured titles:

- `secretShare - Group Settings`
- `'Shopping' - Sharing Settings`
- `'New Folder' - Sharing Settings`
- `Create New Shared Folder`

## Status Messages

| Source class | Captured copy |
|---|---|
| `rf-group-status` | `You are a Group Manager.` |
| `rf-sharing-status` | `You are the Creator of this Shared Folder.` |
| `rf-sharing-status` | `You have full access to the data in this Shared Folder.` |

Design requirement: these are permission explanations, not errors. They should be visually calm but prominent enough to clarify why actions are available or missing.

## Add Recipient

| Part | Source class | Purpose |
|---|---|---|
| Add row | `rf-sharing-add-recipient` | Recipient email, permission, Add/Share action. |
| Email field | `rf-email field-with-title field-border` | Recipient input. |
| Email input | `input-field` | Required email input. |
| Field title | `title` | `Recipient Email`. |
| Autocomplete | `rf-email-autocomplete-list hidden` | Contact suggestions. |
| Suggestion | `mail-suggestion` | Contact row. |
| Suggestion avatar | `mail-suggestion-icon bg-color-*` | Initials avatar. |
| Suggestion text | `mail-suggestion-text` | Name/email display. |
| Permission wrapper | `rf-perm` | Recipient permission selector. |
| Permission trigger | `input-wrapper` | Button-like readonly dropdown. |
| Permission input | `input-dropdown` | Readonly displayed permission value. |
| Share/Add button | `dialog-button default-button rf-share-btn disabled-button` | Disabled until valid. |
| Error | `rf-sharing-error` | Add-recipient error area. |

Captured button labels:

- Existing sharing settings: `Share`.
- Create shared folder: `Add`.

## Permission Menu

Captured opened permission dropdown:

```html
<div role="menu" tabindex="-1" class="unselectable no-icons popup-menu permission-menu">
```

| Part | Source class | Purpose |
|---|---|---|
| Menu | `unselectable no-icons popup-menu permission-menu` | Permission options popup. |
| Menu role | `role="menu"` | Popup menu semantics. |
| Item | `item` | Permission option. |
| Selected item | `item selected` | Current permission. |
| Item role | `role="button"` | Option activation. |
| Icon placeholder | `icon icon-left` | Left icon slot, visually no-icons variant. |
| Label | `text` | Permission name. |
| Description | `permission-tip` | Inline help text. |

Captured options:

| Permission | Description |
|---|---|
| Log in only | Access for logins only, no editing or sharing. |
| Read and write | View and edit items, changes affect others. |
| Full control | Full access, manage permissions, and recipients. |

Design requirements:

- Permission names need descriptions because the risk/trust difference is material.
- Selected option should be explicit and not rely only on row background.
- `Log in only` is captured as selected.
- Menu is positioned absolutely and must stay anchored to the permission trigger.

## Recipients Table

| Part | Source class | Purpose |
|---|---|---|
| Wrapper | `rf-sharing-recipients` | Access list region. |
| Title | `title` | `Who has access:` |
| Table | `rf-sharing-recipients-table` | Recipient rows. |
| Recipient row | `rf-sharing-recipiant` | Source typo; do not expose as API spelling. |
| Recipient avatar | `recipiant-icon bg-color-*` | Initials avatar. |
| Recipient info | `recipiant-info` | Name/email stack. |
| Recipient name | `recipiant-name` | Display name. |
| Self marker | `rf-self` | `(you)`. |
| Recipient email | `recipiant-email` | Email line. |
| Recipient status | `recipiant-status` | Owner/Manager/Admin/pending status. |
| Permission dropdown | `input-wrapper` + `input-dropdown` | Editable permission for some recipients. |
| Error | `error-msg` | Dialog-level recipient/table error. |

Captured statuses:

- `Owner`
- `Manager`
- `Admin`
- `Not received yet`
- Empty status with editable permission dropdown

## Dialog Variants

### Group Settings

Root:

```html
<div class="dialog-view rf-sharing-dialog rf-sharing-folder  rf-modal-dialog">
```

Captured state:

- Header `secretShare - Group Settings`.
- Status `You are a Group Manager.`
- Add recipient row with permission selector and disabled `Add`.
- Recipients include current user as Manager, another editable recipient, and Admin.

### Creator Shared Folder Settings

Captured state:

- Header `'Shopping' - Sharing Settings`.
- Status `You are the Creator of this Shared Folder.`
- Add recipient row with disabled `Share`.
- Recipients include Owner and pending `Not received yet` rows.
- Footer actions: `Stop Sharing`, `Delete`.

## Destructive Confirmations

Captured generic confirmation dialog shell:

```html
<div class="dialog-view dialog-modal but-licenses-dialog undefined">
```

### Stop Sharing

| Part | Source class | Captured content |
|---|---|---|
| Header | `header` | `Stop Sharing` |
| Body | `confirmation-text`, `text-part` | Explains recipients lose access, personal copy stays. |
| Secondary action | `button` | `Cancel` |
| Primary action | `button default-button` | `Stop` |

Captured message:

- Stop sharing file `testshare\Airbnb`.
- A copy remains in personal storage.
- All recipients lose access.

### Confirm Deletion

| Part | Source class | Captured content |
|---|---|---|
| Header | `header` | `Confirm Deletion` |
| Body | `confirmation-text`, `text-part` | Explains shared file deletion and recipient access loss. |
| Secondary action | `button` | `Cancel` |
| Primary action | `button default-button` | `Delete` |

Captured message:

- Delete shared file `testshare\Airbnb`.
- All recipients lose access.

Design requirements:

- `Stop` and `Delete` are destructive outcomes; design tokens should distinguish them from normal primary actions even though source uses `default-button`.
- Copy must clearly distinguish stop sharing from delete: stop preserves a personal copy; delete removes the shared file.
- File names and paths can be long and should wrap safely inside the confirmation text.

### Received Shared Folder Settings

Captured snippet:

```html
<div class="dialog-view rf-sharing-dialog rf-sharing-folder rf-received-folder rf-modal-dialog">
```

Captured state:

- Header `'New Folder' - Sharing Settings`.
- Status `You have full access to the data in this Shared Folder.`
- Recipients table present but empty in snippet.
- Footer action: `Remove Me`.

Design requirements:

- Received folder variant removes add-recipient controls and exposes self-removal.
- `Remove Me` is a destructive/separating action but less severe than deleting the shared folder for everyone.

### Create New Shared Folder

Root:

```html
<div class="dialog-view rf-sharing-dialog rf-sharing-folder rf-create-sharing-folder rf-modal-dialog">
```

Captured state:

| Part | Source class | Purpose |
|---|---|---|
| Folder name field | `new-shared-folder-field field-with-title field-border` | Shared folder name. |
| Folder name input | `folder-name-input` | Required name input. |
| Existing folder error | `existing-folder-err` | Name conflict/validation area. |
| Add recipient | `rf-sharing-add-recipient field-with-title field-border` | First invite row. |
| Recipients table | `rf-sharing-recipients-table` | Starts with current user as Owner. |
| Cancel | `dialog-button rf-cancel-btn` | Cancel creation. |
| Create | `dialog-button rf-create-btn default-button` | Create folder. |

Design requirements:

- Create flow combines object creation and first sharing invite in one dialog.
- Owner row is always present.
- Loading overlay exists but hidden; capture async state later if possible.

## Open States Still Needed

| State | Captured | Notes |
|---|---:|---|
| Shared With Me | Yes | Folders/groups visible. |
| Shared By Me | Yes | Folders/files visible. |
| Group Settings | Yes | Manager/admin/permission dropdown shell. |
| Creator Shared Folder Settings | Yes | Stop Sharing/Delete actions. |
| Received Shared Folder Settings | Yes | Remove Me action. |
| Create Shared Folder | Yes | Name, autocomplete, owner row. |
| Permission dropdown opened | Yes | `Log in only`, `Read and write`, `Full control` captured with descriptions. |
| Add recipient valid/enabled | No | Button captured disabled. |
| Error states | No | Error containers captured empty. |
| Stop sharing/delete confirmation | Yes | Stop Sharing and Confirm Deletion captured. Need Remove Me confirmation if separate. |
