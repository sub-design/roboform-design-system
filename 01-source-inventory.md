# Source Inventory

## Current source set

| Source | Status | Design-system value |
|---|---:|---|
| `/Users/aleksandrsubbotin/Downloads/start-page-v8.css` | Primary | Main v8 UI layer: app shell, password generator, security center, emergency access, editor, new item flows, responsive rules, z-index layers, semantic tokens. |
| `/Users/aleksandrsubbotin/Downloads/common-ui.css` | Primary legacy | Shared/older UI primitives: buttons, inputs, lists, text styles, password strength, avatar, logo, switches. |
| `/Users/aleksandrsubbotin/Downloads/start-page.css` | Supporting | Loading/error shell and basic start page fallback. |
| `/Users/aleksandrsubbotin/Downloads/start-page.js` | Supporting | Rendered component class names and dialog/flow hints. Useful for states and copy keys. |
| `/Users/aleksandrsubbotin/Downloads/1.js` | Supporting/noisy | Large bundle. Useful only for targeted searches; too broad for manual design extraction. |
| `/Users/aleksandrsubbotin/Downloads/content.js` | Product behavior | Autofill/content-script behavior and field detection rules. Useful later for in-page RoboForm controls. |
| `/Users/aleksandrsubbotin/Downloads/inject.js` | Low design value | Browser credential API interception. Not a primary UI design-system source. |
| Inspector HTML dump | Primary for structure | Real DOM structure for main app shell, navigation, vault grid/list, security center, dialogs, menus. |

Empty or near-empty files in this export:

- `commands-ui.css`
- `controls.css`
- `dialog-view.css`
- `user-consent-page.css`

These should stay on the missing-source list. If a later export contains real content for them, they can fill gaps around command menus, dialogs, extension controls, and consent flows.

## Product surfaces discovered

### App Shell

- `#v8`
- `light-theme`
- `dark-theme`
- `body-v8`
- `rf-main-header`
- `rf-header-logo`
- `main-frame`
- `rf-view-frame`
- `rf-tooltip`
- `rf-notification`
- `rf-modal-screen`

### Header Search

- `rf-search`
- `rf-search-box`
- `rf-search-box-edit`
- `rf-search-box-count`
- `rf-search-box-eraser`
- `rf-search-box-arrow`
- `rf-search-dropdown`
- `custom-select`
- `checks-box`
- `check-label`
- `radio-check`
- `reset-btn`

### Account Menu

- `rf-account`
- `rf-account-box`
- `selector-with-dropdown`
- `dropdown-handler`
- `company-logo`
- `initials`
- `rf-account-menu`
- `dropdown-popup`
- `dropdown-popup-item`
- `dropdown-popup-submenu`
- `toggle`

### Navigation

- `rf-navigator`
- `navigator-container`
- `navigator-section`
- `navigator-section-with-separator`
- `selector-item`
- `select-image`
- `select-title`
- `rf-folders`
- `rf-folders-pane`
- `rf-folders-tree`
- `rf-items-list`
- `rf-item-folder`
- `category-pinned`
- `category-all`
- `category-logins`
- `category-bookmarks`
- `category-safenotes`
- `rf-identities`
- `rf-more-categories`

### Vault Data Views

- `rf-data-view`
- `rf-folder-breadcrumbs`
- `rf-data-view-header`
- `order-selector`
- `sort-order-selector`
- `rf-view-style-selector`
- `rf-view-style-large`
- `rf-view-style-condensed`
- `rf-view-style-list`
- `rf-data`
- `rf-items-section`
- `rf-items`
- `rf-virt-list`
- `virtual-list-scroller`
- `rf-no-items`

### Vault Items

- `rf-item`
- `rf-item-login`
- `rf-item-bookmark`
- `rf-item-safenote`
- `rf-item-folder`
- `rf-item-sec-warning`
- `rf-item-sec-warning-shown`
- `rf-item-sharing-shown`
- `rf-has-own-background`
- `rf-dark`
- `rf-light`
- `item-content`
- `rf-item-image`
- `rf-item-imgLogo`
- `rf-item-default-image`
- `rf-item-title`
- `rf-item-hover-overlay`
- `rf-item-sharing`
- `rf-item-multiselect`
- `rf-item-menu`
- `rf-sec-warning-button`

### Creation Menu

- `rf-new-menu`
- `rf-new-button`
- `rf-new-dropdown`
- `new-group`
- `new-shared-folder`
- `new-folder`
- `new-identity`
- `new-contact`
- `new-safenote`
- `new-bookmark`
- `new-login`

### Password Generator

- `rf-password-generator`
- `password-generator-order-selector`
- `rf-password-generator-container`
- `rf-password-view-and-actions`
- `rf-generated-password`
- `rf-generate-password-button`
- `rf-password-score`
- `rf-copy-password-button`
- `password-generator-settings-container`
- `number-input`
- `buttons-container`
- `toggle-option`
- `chars-input-identity`

### Security Center

- `rf-security-center`
- `security-center-tab-order-selector`
- `security-center-order-selector`
- `summary-pane`
- `security-center-compromised`
- `security-center-weak`
- `security-center-reused`
- `security-center-all`
- `security-center-duplicates`
- `security-center-excluded`
- `table-outer`
- `table-inner`
- `table-items`
- `table-scrollable`
- `header-sort`
- `sort-arrow`
- `toggle-pwd`
- `manage-icon`

### Emergency Access

- `rf-emergency-access`
- `emergency-access-order-selector`
- `rf-ea-data`
- `rf-ea-accounts`
- `rf-ea-account`
- `rf-ea-no-accounts`
- `rf-ea-pending-action`
- `rf-ea-pending-action-btn`
- `rf-ea-new-contact`
- `invited`
- `requested`
- `accepted`
- `access-granted`
- `rejected`

### Editor, Dialogs, Inputs

- `field-with-title`
- `folder-selector`
- `input-field`
- `field-input-with-dropdown`
- `dropdown-popup-list`
- `inplace-edit-pane`
- `inplace-edit-input`
- `buttons-bar`
- `button`
- `default-button`
- `non-essential-button`
- `rf-new-dialog`
- `rf-titlebar`
- `rf-disable-curtain`

### TOTP/Auth

- `totp-section`
- `code-section`
- `code-pane`
- `totp-timer`
- `totp-add-key-section`
- `add-totp-key-dialog`
- `rf-add-totp-key-input`
- `rf-add-totp-paste`

## Semantic token groups observed

### Recommended canonical families

- `color.surface.*`
- `color.text.*`
- `color.background.*`
- `color.border.*`
- `color.link.*`
- `color.icon.*`
- `color.status.*`
- `color.rating.*`
- `color.input.*`
- `color.action.*`
- `shadow.*`
- `radius.*`
- `size.*`
- `space.*`
- `z.*`
- `motion.*`

### Existing v8 variables to preserve or map

- `--surface-surface`, `--surface-surface-0`, `--surface-surface-02`, `--surface-surface-03`, `--surface-surface-04`, `--surface-surface-05`, `--surface-surface-06`, `--surface-surface-08`, `--surface-surface-09`, `--surface-surface-10`, `--surface-surface-11`, `--surface-surface-12`
- `--text-text-primary`, `--text-text-secondary`, `--text-text-secondary-hover`, `--text-text-secondary-disabled`, `--text-text-quaternary`, `--text-text-tetriary`, `--text-text-error`, `--text-text-selected`, `--text-text-inverse`, `--text-text-on-color`
- `--background-background-primary`, `--background-background-brand`, `--background-background-destructive`, `--background-background-disabled`, `--background-background-information`, `--background-neutral-hovered`, `--background-neutral-pressed`
- `--border-border-primary`, `--border-border-secondary`, `--border-border-focused`
- `--input-border-primary`, `--input-border-focused`, `--input-border-error`, `--input-border-hovered`, `--input-title-primary`, `--input-title-background`, `--input-text-primary`, `--input-text-disabled`
- `--rag-rating-rag-weak`, `--rag-rating-rag-medium`, `--rag-rating-rag-good`, `--rag-rating-rag-strong`
- `--link-link`, `--text-link-upgrade`
- `--icon-icon-quinary`, `--icon-icon-tetriary`

### Legacy variables to map

- `--regular-button-*`
- `--important-button-*`
- `--low-emphasis-button-*`
- `--white-button-*`
- `--edit-*`
- `--list-item-*`
- `--items-counter-*`
- `--toggle-*`
- `--progress-*`
- `--password-weak-strength-color`
- `--password-medium-strength-color`
- `--password-good-strength-color`
- `--password-strong-strength-color`

## Icon asset families

The CSS references a large icon set. The design system should document icons by product action, not by raw filename.

Core item types:

- Login
- Bookmark
- Safenote
- Identity
- Contact
- Folder
- Searchcard/blocking passcard

Identity detail/editor source families:

- `editor-identity`
- `groups-pane`
- `groups-list`
- `instance-pane`
- `instance-content`
- `instance-fields-list`
- `actions-pane`
- `group-icon`
- `fields-list`
- `fields-person`
- `fields-location`
- `fields-credit-card`
- `fields-bank-account`
- `fields-business`
- `fields-passport`
- `fields-car`
- `instance-group-item`
- `instance-group-section-title`
- `instance-group-section`
- `instance-group-current`
- `instance-group-section-title-current`
- `no-information`
- `cmdbar-instance`
- `rf-identity-person-fields`
- `rf-identity-location-fields`
- `rf-identity-card-fields`
- `rf-identity-bank-fields`
- `rf-identity-buisness-fields`
- `rf-identity-passport-fields`
- `rf-identity-car-fields`
- `rf-field`
- `rf-row-fields`
- `rf-group-fields`
- `rf-date-fields`
- `rf-field-country`
- `rf-note`
- `input-with-title editor-field`
- `field-group`
- `field-select`
- `field-section-separator`
- `textarea-with-title`
- `show-note`

Actions:

- Login/fill
- Go to
- Copy
- View
- Edit
- Rename
- Move
- Clone
- Delete
- Pin/unpin
- Share
- Send
- Print
- Include/exclude security score
- Manage columns
- Select/multiselect

Navigation:

- All
- Pinned
- Logins
- Bookmarks
- Safenotes
- Identities
- Sharing Center
- Security Center
- Authenticator
- Password Generator
- Emergency Access
- Why RoboForm

Security:

- Shield weak/medium/good/strong
- Compromised mark
- Compromised chart/list icons
- Security warning
- Password reveal/hide

Creation:

- Create group
- Create shared folder
- Create folder
- Create identity
- Create contact
- Create safenote
- Create bookmark
- Create login

Account/settings/import:

- Account
- Theme
- Settings
- Admin center
- Import

Import flow source families:

- `import-dialog`
- `choose-importer`
- `show-instructions-and-import`
- `import-page`
- `importers-section`
- `importers-list`
- `apps-list`
- `importer-item`
- `icon-chrome`
- `icon-firefox`
- `icon-opera`
- `icon-edge`
- `icon-lastpass`
- `icon-bitwarden`
- `icon-dashlane`
- `icon-myki`
- `icon-1password`
- `icon-keeper`
- `icon-nordpass`
- `icon-truekey`
- `icon-enpass`
- `icon-kaspersky`
- `icon-proton-pass`
- `icon-more`
- `icon-csv`
- `instructions-title`
- `instructions`
- `logo-chrome`
- `error-section`
- `progress-section`
- `button-default`
- `button-non-essential`

Settings sheet source families:

- `sheet-view`
- `sheet`
- `sheet-title`
- `sheet-title-text`
- `sheet-content`
- `done-button`
- `settings-list`
- `settings-row`
- `vertical-setting-with-title`
- `horizontal-setting-with-title`
- `setting-toggle`
- `setting-action`
- `setting-license-type`
- `setting-license-company`
- `setting-license-status`
- `sponsored-account-setting`
- `radio-group`
- `option-checked`
- `radio-button`
- `radio-text`
- `devices-and-activity-view`
- `tab-selector`
- `unusial-activity-warning`
- `activities-list`
- `activity`
- `show-device`
- `backup`
- `column-value`
- `column-inner-value`
- `creation-time`
- `creation-type`
- `creation-method-text`
- `storage-location`
- `expand-icon`
- `new-backup-button`
- `notification information`
- Help
- Logout/lock
- Provider/import logos

## Responsive and layer rules found

Responsive breakpoints visible in current source:

- `1900px`
- `1750px`
- `1650px`
- `1500px`
- `1370px`
- `1350px`
- `1220px`
- `1100px`
- `950px`
- `900px`
- `750px`
- `650px`

Key responsive behavior:

- Sidebar collapses to `56px`.
- Hovered collapsed sidebar expands to `235px`.
- Header logo switches to icon-only when collapsed.
- Account title is hidden below `650px`.
- View-style selector becomes a popup below `650px`.
- Command bar text is progressively hidden at narrow widths.
- Editor-inline mode changes width behavior below `1220px`.

Layering:

- `rf-main-header`: `z-index: 4`
- `rf-navigator`: `z-index: 3`
- `rf-editor`: `z-index: 4`
- `rf-search-dropdown`: `z-index: 2`
- `dropdown-popup`: `z-index: 10`
- `context-menu-popup`: `z-index: 20`
- `rf-modal-screen`: `z-index: 5`
- `rf-notification`: `z-index: 100`
- `rf-editor .rf-cmdbar`: `z-index: 100`
- Loading overlays can reach `z-index: 1000`

## Gaps to fill with additional HTML dumps

- Open account menu in light and dark mode.
- Search dropdown opened with options.
- Create-new dropdown opened.
- New Login dialog/editor.
- Existing Login detail/editor.
- Folder selector dropdown inside editor.
- Security Center with populated rows.
- Password Generator with settings visible.
- Sharing Center populated state.
- Emergency Access populated and empty states.
- Settings/import/account setup pages.
- Locked/logged-out/startup/login states.
