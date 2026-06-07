# Component Candidates

This file tracks repeated UI parts that are promising design-system components but are not final APIs yet.

Candidate maturity:

- `Seed`: seen once or lightly documented.
- `Growing`: seen across multiple contexts, anatomy mostly clear.
- `Nearly ready`: enough evidence to write a final spec after one focused review.
- `Blocked`: important missing states prevent promotion.

## Candidate Index

| Candidate | Maturity | Evidence surfaces | Main missing evidence | Likely DS priority |
|---|---|---|---|---|
| Button | **Promoted → [20-spec-button.md](20-spec-button.md)** | dialogs, editors, sharing, emergency access, password generator, login Save enabled/disabled, unsaved-changes dialog | (in spec) destructive styling, legacy color values, compact loading | High |
| Icon Button / Command Button | **Promoted → [21-spec-icon-button.md](21-spec-icon-button.md)** | editor headers, instance commands, row actions, generator actions, table manage columns | (in spec) focus-visible, disabled opacity, size consolidation | High |
| Dropdown / Context Menu | **Promoted → [22-spec-dropdown-menu.md](22-spec-dropdown-menu.md)** | account menu, create menu, commands menu, popup menu, permission menu | (in spec) hover/focus/disabled, Help submenu, keyboard spec | High |
| Dialog | **Promoted → [23-spec-dialog.md](23-spec-dialog.md)** | New Login, duplicate confirmation, unsaved changes confirmation, sharing, emergency access, manage columns | (in spec) validation/error, loading, destructive styling, light surface | High |
| Toast / Notification | Seed | common notification create/unpin, settings information notification | visible position, dismissed, severity variants, placement rules across app/settings | High |
| Copyable Field | **Promoted → [27-spec-copyable-field.md](27-spec-copyable-field.md)** | Login detail fields, Identity fields, TOTP, Security table behavior | (in spec) button-level copied feedback, keyboard copy, identity pane geometry | High |
| Text Input | **Promoted → [24-spec-text-input.md](24-spec-text-input.md)** | New Login, editor inputs, inline rename, emergency email, identity edit fields | (in spec) dark hover border, base box geometry, legacy `--edit-*` values | High |
| Password Input / Reveal | **Promoted → [25-spec-password-input.md](25-spec-password-input.md)** | New Login reveal, Login detail masked, Security Center behavior described | (in spec) reveal timeout, column mass-reveal gating, copy feedback | High |
| Select / Combobox | Growing | search type, account selector, folder selector, emergency timeout closed | timeout menu opened, disabled/error states | Medium |
| Segmented Selector / Tabs | Growing | vault sort/order, password generator, security center, sharing, emergency access, devices/activity tabs | keyboard focus, overflow, badge behavior | Medium |
| Radio Group | Seed | AutoFill and AutoSave settings | focus/keyboard, disabled styling, validation if any | Medium |
| Sheet Shell | Seed | Settings sheets and Devices & Activity | sheet navigation, mobile sizing, scroll behavior | Medium |
| Settings Row | Seed | AutoFill, AutoSave, Advanced, License & Billing | focus/disabled details, long copy, opened selects | Medium |
| Table | **Promoted → [26-spec-table.md](26-spec-table.md)** | Security Center tables, vault list, manage columns | (in spec) active sort capture, loading/empty, column resize, selected-row token | High |
| Provider Picker | Seed | Import provider picker and Chrome instruction page | hover/focus/disabled, app/CSV next step, visible progress/result/error | Medium |
| Vault Item / Row | **Promoted → [28-spec-vault-item.md](28-spec-vault-item.md)** | grid/list, Security Center rows, Sharing sections | (in spec) clean grid default capture, active border/shadow tokens, per-type grid anatomy | High |
| Command Bar | Growing | editor header, instance commands, multiselect, row cmdbar | responsive/icon-only and disabled command states | High |
| Empty State | Growing | emergency access, vault no-items, hidden security no-items | visible variants across product areas | Medium |
| Security Strength Badge | Growing | Login detail, Security Center tables, password generator score colors | all strength/risk combinations in one place | High |
| Sharing Indicator | Growing | vault items, folders, Sharing Center, identity nav | hover/focus and no-permission variants | Medium |
| Identity Group Navigator | Seed | Identity detail view | edit-mode, group menu, drag/reorder if any | Medium |
| Identity Field Display | Growing | Person, address, card, bank, business, passport, car | empty/warning, copied feedback | Medium |
| Identity Field Editor | Growing | Person, address, card, bank, business, passport, car edit-mode | save enabled/loading, validation, opened select menus | Medium |
| Activity List | Seed | Devices & Activity Activities tab | Devices tab, row detail view, warning dismissed | Medium |
| Backup List | Seed | Settings Backups list | expanded row/details, restore/create flow | Medium |

## Candidate Details

### Button

Evidence:

- `button`, `default-button`, `disabled`, `disabled-button` in dialogs and editors.
- Login editor confirms Save disabled as `button default-button disabled` and Save enabled as `button default-button`.
- Unsaved changes confirmation confirms secondary text action `Don't save` beside primary `Save`.
- `rf-button`, `dialog-button`, `normal-button`, `wide-button`, `regular-button`, `important-button`, `low-emphasis-button`.

Known variants:

- primary/default
- secondary/cancel
- disabled
- destructive by action semantics, even when source uses neutral/default styling

Promotion blockers:

- Confirmed loading button state if any flow displays one.
- Final destructive treatment.

### Icon Button / Command Button

Evidence:

- `editor-cmd`, `rf-cmdbutton`, `rf-image-button`, `icon-button`, `manage-icon`, `rf-new-button`.
- Header commands, row commands, instance commands, generator buttons.

Known sizes:

- roughly 20, 24, 28, 30, 32, 34, 48, 60.

Promotion blockers:

- Confirmed focus-visible behavior.
- Disabled command behavior across editor and table contexts.

### Dropdown / Context Menu

Evidence:

- `dropdown-popup` menus: account, create, commands context menu.
- `popup-menu`: icon menu and permission menu.
- `dropdown-popup-submenu` for Help.

Known variants:

- menu with icons
- menu without icons
- nested submenu
- permission menu with descriptions
- separators

Promotion blockers:

- Help submenu open state.
- Hover/focus/disabled item states.
- Keyboard contract.

### Dialog

Evidence:

- `dialog-view`, `rf-modal-dialog`, `dialog-modal`.
- New Login, duplicate overwrite message, Manage Columns, Sharing Settings, Emergency Invite.
- Login unsaved-changes confirmation: generic `dialog-view dialog-modal`, header `Save changes`, actions `Don't save` and `Save`.

Known variants:

- form dialog
- confirmation dialog
- unsaved-changes confirmation
- management dialog
- message dialog

Promotion blockers:

- Validation/error and loading states.
- Destructive action styling.

### Toast / Notification

Evidence:

- `rf-notification rf-common-notification`
- `rf-notification-msg`
- settings `notification information`
- `rf-close-btn`
- Captured messages: `Login has been created`, `Login removed from Pinned`.

Promotion blockers:

- Visible position.
- Dismissed state.
- Warning/error variants.
- Stacking rules.
- Relationship between common notification and settings-scoped notification.

### Copyable Field

Evidence:

- Login detail fields use `onhover-button-copy`.
- Identity fields use `inline-onhover-buttons-pane`.
- TOTP has copy command.

Known variants:

- simple text value
- masked secret
- grouped/composite value
- multiline note
- country/flag value

Promotion blockers:

- Keyboard copy affordance.
- Copied feedback.
- Sensitive-value redaction rules in notifications/tooltips.

### Table

Evidence:

- Security Center `table-items table-scrollable`.
- Vault list rows.
- Manage Columns dialog.

Known variants:

- sortable headers
- command column
- password reveal header toggle
- multiselect
- grouped duplicate rows

Promotion blockers:

- Active sort state.
- Empty/loading states.
- Column resize/reorder behavior.

### Identity Group Navigator

Evidence:

- `groups-pane`, `groups-list`, `instance-group-item`, `instance-group-section-title`, `instance-group-section section-shown`, `no-information`.

Known variants:

- single group
- repeated section
- current row
- current section title
- current child instance
- missing optional group add row

Promotion blockers:

- Group menu opened.
- Edit-mode interaction.
- Visible empty/no-fields state.

### Identity Field Display

Evidence:

- `rf-field`, `rf-row-fields`, `rf-group-fields`, `rf-date-fields`, `rf-field-country`, `rf-note`.

Known variants:

- standard label/value
- composite name/date/expiry/plate
- country with flag
- multiline note
- sensitive financial/identity fields

Promotion blockers:

- Visible validation/error.
- Copied feedback.

### Identity Field Editor

Evidence:

- `editor-identity edit-mode`.
- `actions-pane`, `group-icon group-icon-*`, `group-title`, `buttons-bar`.
- `fields-list fields-person`, `fields-location`, `fields-credit-card`, `fields-bank-account`, `fields-business`, `fields-passport`, `fields-car`.
- `input-with-title editor-field field-id-*`.
- `field-group`.
- `field-select` with `custom-select selected`.
- `textarea-with-title`, `show-note`, `has-value`.

Known variants:

- text input with floating label
- grouped fields
- select/combobox field
- section separator
- populated note textarea
- add-note prompt with hidden textarea
- disabled Save

Promotion blockers:

- Save enabled.
- Save/loading lifecycle.
- Validation/error state.
- Opened select menus.

### Sheet Shell

Evidence:

- `sheet-view`, `sheet`, `sheet-title`, `sheet-title-text`, `sheet-content`, `done-button`.
- Captured settings sheets: AutoFill, AutoSave, Advanced, License & Billing.
- Devices & Activity uses `sheet-view` but swaps the regular title shell for a tab selector.

Known variants:

- regular titled settings sheet
- tabbed sheet
- sheet with settings rows
- sheet with activity list

Promotion blockers:

- Settings landing/navigation if separate.
- Responsive/mobile sheet behavior.
- Scroll behavior and sticky title if any.

### Settings Row

Evidence:

- `settings-list`, `settings-row`, `vertical-setting-with-title`, `horizontal-setting-with-title`.
- `setting-toggle`, `setting-toggle on`.
- `setting-action`, `action-button`.
- License/billing rows and policy-managed disabled sponsored account row.

Known variants:

- vertical explanatory row
- horizontal control row
- toggle row
- select row
- action row
- license/status row
- disabled policy-managed row

Promotion blockers:

- Focus/hover states.
- Opened select menus.
- Settings validation/error if any.
- Long copy overflow on narrow widths.

### Radio Group

Evidence:

- AutoFill and AutoSave mode selectors.
- `radio-group`, label with `role="radio"`, `option-checked`, `radio-button`, `radio-text`.
- Disabled native input captured for unavailable option.

Known variants:

- selected option
- unselected option
- disabled option
- option text with secondary inline span

Promotion blockers:

- Keyboard arrow behavior.
- Focus ring state.
- Disabled visual treatment.

### Activity List

Evidence:

- Devices & Activity Activities tab.
- `activities-list`, `title-bar`, `activity`, `role="listitem"`.
- Warning banner: `unusial-activity-warning`, `change-mp`, `close-button`.

Known variants:

- selected Activities tab
- warning banner visible
- current-device label
- activity row with show-device affordance

Promotion blockers:

- Devices tab.
- Device details view.
- Warning dismissed state.
- Empty/loading activity states.

### Provider Picker

Evidence:

- Import provider picker.
- `dialog-view import-dialog`.
- `choose-importer import-page`, `importers-section`.
- `show-instructions-and-import import-page`.
- `instructions-title`, `logo logo-chrome`, `instructions`.
- `error-section hidden`, `progress-section hidden`.
- `importers-list`, `importers-list apps-list`.
- `importer-item`, `icon icon-*`, `title`, `separator`.

Known variants:

- browser provider group
- password-manager/app provider group
- CSV file-import option
- More providers/navigation item
- provider instruction page
- import-from-file primary action
- hidden error/progress slots

Promotion blockers:

- Provider hover/focus states.
- Disabled/unavailable provider state if any.
- App provider and CSV next-step variants.
- Visible import progress/result/error states.

### Backup List

Evidence:

- Settings backups surface.
- `backup`, `role="group"`, `aria-expanded="false"`.
- `column-value`, `column-inner-value`.
- `creation-time`, `creation-type`, `creation-method-text`, `storage-location`.
- `expand-icon`.
- `new-backup-button`.

Known variants:

- collapsed backup row
- scheduled backup
- manually created backup
- local storage
- server storage
- create new backup action

Promotion blockers:

- Expanded backup row/details.
- Restore/download/delete actions if present.
- New backup flow.
- Empty/loading backup list.
