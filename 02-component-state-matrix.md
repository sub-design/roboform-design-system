# Component And State Matrix

This matrix translates current source classes into design-system components. It is intentionally source-led: names can be cleaned up later, but each row starts from what exists today.

## Core primitives

| Component | Source classes/selectors | States found | Notes |
|---|---|---|---|
| Button | `button`, `normal-button`, `wide-button`, `regular-button`, `important-button`, `low-emphasis-button`, `default-button`, `non-essential-button`, `white`, `blue` | default, hover, active, focus, disabled | Needs unified variants: primary, secondary, tertiary, destructive, neutral, link. |
| Icon Button | `rf-cmdbutton`, `rf-image-button`, `icon-button`, `manage-icon`, `rf-new-button`, `more-actions-button` | default, hover, active, focus-visible, disabled, auto-hide | Icon button sizes vary: 20, 24, 28, 30, 32, 34, 48, 60. Needs size scale. |
| Link | `flat-link`, `link`, `restore-link`, `password-generator-history-button` | default, hover/focus, active, disabled | Link colors should map to `color.link.default/hover`. |
| Text Input | `extension-normal-input`, `input-field`, `number-input`, `chars-input-identity`, `inplace-input`, `rf-input-box` | default, hover, focus, disabled, error, with hint, with placeholder | Existing source has both legacy and v8 input systems. |
| Password Input | `password-eye-icon`, `hide-pwd`, `toggle-pwd`, `toggle-pwd-hide` | hidden, revealed, hover, focus, disabled | Needs security-specific behavior: masking, reveal timeout, copy/reveal audit if product requires it. |
| Checkbox | `checks-box`, `check-label`, `radio-check`, `search_word`, `match_case`, `match_words`, `search_in` | unchecked, checked, radio-like, hover, disabled | Search options use checkbox inputs with custom visual labels. |
| Radio Group | `radio-group`, `role="radio"`, `option-checked`, `radio-button`, `radio-text` | selected, unselected, disabled option captured | Settings AutoFill/AutoSave use label-based custom radios. |
| Toggle | `toggle`, `switcher`, `switcher-disabled`, `left-button-selected`, `right-button-selected` | off, on, disabled, selected left/right | There are two concepts: binary toggle and two-way switcher. Keep separate. |
| Select | `custom-select`, `select-title`, `select-option`, `select-upper-title`, `selector-with-dropdown` | closed, opened, selected, disabled | Used for search type and account/nav selectors. |
| Dropdown/Menu | `dropdown-popup`, `dropdown-popup-item`, `dropdown-popup-separator`, `dropdown-popup-submenu`, `context-menu-popup` | closed, opened, hover, disabled, submenu | Needs keyboard behavior and positioning rules. |
| Message / Confirmation Dialog | `dialog-view rf-message rf-modal-dialog`, `dialog-view dialog-modal`, `modal-dialog-screen`, `content`, `confirmation-text`, `rf-body`, `rf-text`, `rf-buttons-bar`, `buttons-bar` | duplicate overwrite confirmation captured, unsaved changes confirmation captured | Generic confirmation/message dialog pattern; includes overwrite and save/discard decisions. |
| Tooltip | `rf-tooltip` | hidden, visible | Need placement, delay, max width, keyboard behavior. |
| Progress | `rf-progress`, `rf-progress-runner`, `progress`, `marquee`, `rf-main-progress`, `rf-loading` | determinate, indeterminate, loading overlay | Multiple loading concepts should be normalized. |
| Badge | `badge`, `category-badge`, `rf-bandge-count`, `rf-events-count`, `counter` | default, active, hidden | Typo `bandge` exists in source; do not preserve typo in design-system API. |
| Avatar | `avatar`, `initials`, `bg-color-*`, `icon-by-data-item-path` | color variants 1-5, identity favorites | Map to avatar initials and item-generated color tokens. |

## App shell

| Component | Source classes/selectors | States found | Notes |
|---|---|---|---|
| App Root | `#v8`, `light-theme`, `dark-theme` | light, dark, navigator collapsed, payment page shown, settings shown, editor inline shown | Theme and mode classes drive large layout changes. |
| Header | `rf-main-header`, `rf-header-logo`, `rf-flex-gap`, `rf-start-guide`, `rf-account` | normal, collapsed nav, payment/settings hidden search | Header is a persistent workspace toolbar, not a landing header. |
| Search Box | `rf-search`, `rf-search-box`, `rf-search-box-edit`, `rf-search-box-count`, `rf-search-box-eraser`, `rf-search-box-arrow` | empty, typed, eraser shown, options open, progress | Needs typeahead/search-results contract. |
| Advanced Search Dropdown | `rf-search-dropdown`, `select-box`, `checks-box`, `buttons-box`, `reset-btn` | hidden/open, selected type, checkbox states | Should be a documented composite component. |
| Account Menu | `rf-account-box`, `rf-account-menu`, `company-logo`, `initials`, `enterprise`, `dropdown-image`, `rf-account-theme-item`, `rf-account-settings-item`, `rf-account-admin-center-item`, `rf-import-item`, `rf-account-help-item`, `rf-account-logout-item` | closed/open captured, enterprise, submenu hidden captured, dark mode toggle off captured | Detailed spec started in `04-components-account-menu.md`. Need Help submenu open and dark mode on captures. |
| Sidebar Navigator | `rf-navigator`, `navigator-section`, `selector-item`, `active`, `separator-top` | active, hover, collapsed, hovered collapsed, hidden section | Sidebar is the main IA component. |
| Folder Tree | `rf-folders-pane`, `rf-folders-tree`, `rf-item-folder`, `expand-handler`, `collapsed`, `rf-folder-end-marker` | collapsed/expanded, filtered, active, shared roles | Needs indentation, icons, sharing overlays, keyboard navigation. |
| Breadcrumbs | `rf-folder-breadcrumbs`, `container`, `separator` | normal, collapsed sidebar spacing | Need overflow and folder path behavior. |

## Vault and data views

| Component | Source classes/selectors | States found | Notes |
|---|---|---|---|
| Data View Header | `rf-data-view-header`, `order-selector`, `sort-order-selector` | popular, recent, alphabet, hidden for specific sections | Segment/tabs component reused across product areas. |
| View Style Selector | `rf-view-style-selector`, `rf-view-style-popup`, `rf-view-style-large`, `rf-view-style-condensed`, `rf-view-style-list` | large, condensed, list, popup below 650px | Needs responsive mode. |
| Vault Grid | `rf-data`, `rf-view-large`, `rf-items-section-main`, `rf-items`, `rf-sortable` | sortable, disabled sorting, virtualized | Primary vault browsing surface. |
| Vault List | `rf-view-list`, `rf-items-section-main-list-view`, `rf-virt-list`, `rf-list-item-header`, `rf-list-item-row`, `rf-list-item-field` | login rows captured, safenote rows captured, list headers captured, hidden/ordered columns captured | Detailed spec in `12-pattern-vault-list-and-actions.md`. |
| Empty State | `rf-no-items`, `rf-no-items-icon`, `rf-no-items-text`, `rf-no-items-btn` | hidden, shown, product-specific icon/copy | Empty states exist for pinned, search, sharing, logins, safenotes, authenticator, identities. |
| Vault Item | `rf-item`, `item-content`, `rf-item-image`, `rf-item-title` | default, hover, active/focus, multiselect, warning, shared, own background | Core product component. |
| Login Item | `rf-item-login`, `rf-item-imgLogo`, `rf-item-default-image` | logo/default image, security warning, custom background | Needs variants for site favicon/logo vs default. |
| Item Hover Overlay | `rf-item-hover-overlay`, `rf-item-hover-overlay-text`, `rf-item-hover-overlay-icon`, `rf-item-hover-overlay-title` | hidden, visible on hover | Current copy includes `Click to Log In`. |
| Sharing Indicator | `rf-item-sharing`, `shared-none`, `shared-group-regular`, `shared-group-manager`, `shared-folder-regular`, `shared-folder-manager`, `shared-received`, `shared-login-only` | none, limited, regular, manager, granted, received, login-only | Product-specific status component. |
| Multiselect | `rf-item-multiselect`, `rf-item-multiselect-overlay`, `rf-item-multiselect-hovered-overlay`, `multiselect-cmd-select-all` | hidden, hover, selected, checked | Needs command bar pairing. |
| Command Bar | `rf-multiselect-cmdbar`, `rf-multiselect-cmd-item`, `rf-multiselect-cmd-item-image`, `rf-multiselect-cmd-item-text` | hidden, visible, responsive icon-only | Reused in vault and security center. |

## Product workflows

| Component | Source classes/selectors | States found | Notes |
|---|---|---|---|
| Create FAB/Menu | `rf-new-menu`, `rf-new-button`, `rf-new-dropdown`, `plus-image`, `new-group`, `new-shared-folder`, `new-folder`, `new-identity`, `new-contact`, `new-safenote`, `new-bookmark`, `new-login`, `rf-new-dropdown-screen` | open captured, hidden item captured, modal screen shown captured; disabled item still needed | Detailed spec started in `05-components-create-menu.md`. |
| New Login Flow | `modal-dialog-screen`, `dialog-view`, `rf-new-dialog`, `rf-new-login`, `select-account`, `fill-fields`, `account-selector`, `accounts-list`, `rf-known-login`, `rf-create-manually`, `rf-doc-fields`, `field-input-with-dropdown`, `folders-tree`, `folders-tree active`, `create-folder-section`, `eye-icon hide-pwd`, `rf-buttons-bar` | select-account captured, fill-fields captured, known login template list captured, suggestions dropdown captured, folder picker captured, folder picker active captured, save disabled/enabled captured, password reveal captured, create-folder inline captured, manual path described, duplicate overwrite dialog captured | Detailed spec in `06-pattern-new-login-flow.md`. Need Pin on and saved item editor/detail. |
| Login Detail/Editor | `rf-editor`, `details-view`, `editor-view editor-inline editor-login view-mode`, `editor-view editor-inline editor-login edit-mode`, `content-header`, `content-header login`, `content-data`, `fields-list`, `secret-field`, `security-score`, `password-strength-details`, `strength-details-shown`, `security-warning`, `inplace-edit-pane` | view-mode captured, edit-mode captured, changed-field Save enabled captured, unsaved changes confirmation captured, compact login header captured, empty `content-data` captured, password hidden captured, medium hidden strength captured, strong opened strength captured, visible critical warning captured, inline rename captured, save disabled captured | Detailed spec in `07-pattern-login-detail-editor.md`. Need password revealed and validation/error if reachable. |
| Login Action Bar | `header-actionbar`, `action-button action-login`, `action-button action-gofill`, `action-button action-goto` | Log In, Go Fill, Go To captured | Product-specific primary action cluster for saved login detail. |
| Identity Detail | `editor-view editor-inline editor-identity view-mode`, `editor-view editor-inline editor-identity edit-mode`, `content-body`, `actions-pane`, `groups-pane`, `groups-list`, `instance-pane`, `instance-content`, `instance-fields-list`, `fields-list fields-*`, `instance-no-fields`, `security-warning` | view-mode shell captured, edit-mode shell captured, initials header captured, groups pane captured, instance pane captured, populated fields captured, editable fields captured, hidden warning/no-fields slots captured | Detailed spec in `13-pattern-identity-detail.md`. Need visible inline rename, visible warning, no-fields state, save enabled/loading. |
| Identity Group Navigator | `instance-group-item`, `instance-group-current`, `instance-group-section-title`, `instance-group-section-title-current`, `instance-group-section section-shown`, `instance-group-dropdown-image`, `instance-group-count`, `no-information` | single group, repeated section, current parent, current child, expanded section, missing optional group captured | Groups include Person, Address, Credit Card, Bank Account, Business, Passport, Car. |
| Identity Instance Commands | `editor-cmdbar cmdbar-instance`, `editor-cmd-add-new`, `editor-cmd-edit`, `editor-cmd-rename`, `editor-cmd-copy`, `editor-cmd-delete` | add, edit, rename, copy, delete captured | Contextual command bar for selected identity group/instance. Delete should map to destructive intent. |
| Identity Field Display | `rf-field`, `rf-field-caption`, `rf-field-value`, `rf-row-fields`, `rf-row-field`, `rf-group-fields`, `rf-date-fields`, `rf-field-country`, `rf-note`, `inline-onhover-buttons-pane` | standard field, row field, composite field, date, country/flag, note, copy action captured | Sensitive data fields must use synthetic/redacted fixtures. |
| Identity Field Editor | `input-with-title editor-field`, `field-id-*`, `field-group`, `field-select`, `custom-select selected`, `select-upper-title`, `field-section-separator`, `textarea-with-title`, `show-note`, `has-value` | text inputs, grouped fields, selects, note textarea, add-note prompt, populated value state, disabled save captured | Field sets captured for Person, Address, Credit Card, Bank Account, Business, Passport, Car. Need save enabled/loading and validation. |
| Commands Context Menu | `dropdown-popup context-menu-popup commands-menu`, `rf-menu-cmd-login`, `rf-menu-cmd-gofill`, `rf-menu-cmd-goto`, `rf-menu-cmd-copy`, `rf-menu-cmd-view`, `rf-menu-cmd-edit`, `rf-menu-cmd-delete`, `rf-menu-cmd-share`, `rf-menu-cmd-send`, `rf-menu-cmd-view-backup-history` | full command list captured hidden | Same menu anatomy as dropdowns, but command grouping and destructive/sensitive actions need separate rules. |
| Common Notification | `rf-notification rf-common-notification`, `rf-notification-msg`, `rf-close-btn` | multiple message variants captured offscreen/transitioning | Captured copy: `Login has been created`, `Login removed from Pinned`. Need visible and severity variants. |
| Folder Selector | `field-with-title folder-selector`, `folders-dropdown`, `active`, `icon-arrow-down`, `icon-folder` | closed, open, active, dark, selected folder | Appears in editor/new flows. |
| In-place Edit | `inplace-edit-pane`, `inplace-input`, `inplace-ok-btn`, `inplace-cancel-btn` | hidden, visible editing captured; focus, commit, cancel still need interaction states | Used for rename flows in Login Detail header. |
| TOTP Panel | `totp-section`, `code-pane`, `totp-timer`, `cmd-copy`, `cmd-more-options` | normal, warning timer, hover, copy, more menu | Security-sensitive component. |
| Passkey Row | `passkey-fields`, `passkey-field`, `passkey-icon`, `passkey-info`, `passkey-title`, `passkey-text` | visible passkey row captured in login detail | Credential subrecord in Login Detail. |
| Icon Popup Menu | `popup-menu`, `item`, `separator`, `icon-left`, `cmd-*-icon` | open captured, icon commands captured | Detailed spec in `12-pattern-vault-list-and-actions.md`. Separate from `dropdown-popup`. |
| Move/Clone Dialog | `rf-move-or-clone`, `rf-clone`, `rf-item-name`, `rf-folders-tree active`, `highlighted`, `rf-ok-button disabled-button` | clone dialog captured, folder tree captured, disabled OK captured | Detailed spec in `12-pattern-vault-list-and-actions.md`. |
| Import Flow | `dialog-view import-dialog`, `choose-importer import-page`, `show-instructions-and-import import-page`, `importers-section`, `importers-list`, `apps-list`, `importer-item`, `icon icon-*`, `instructions-title`, `instructions`, `error-section`, `progress-section`, `button-default`, `button-non-essential` | provider picker captured, browser/app/CSV groups captured, Chrome instructions page captured, hidden error/progress slots captured | Detailed spec in `18-pattern-import-flow.md`. Need provider hover/focus, app/CSV next-step variants, visible progress/result/error states. |

## Security center

| Component | Source classes/selectors | States found | Notes |
|---|---|---|---|
| Security Tabs | `security-center-order-selector`, `selector-item`, `badge` | compromised, weak, reused, all, duplicates, excluded, active | Badges likely contain counts. |
| Security Summary | `rf-security-center good`, `summary-pane`, `chart-pane`, `chart-chunk`, `legend`, `score-pane good`, `progressbar`, `recomendation` | populated overview captured, score 89 Good, distribution chart captured | Detailed spec in `09-pattern-security-center.md`. |
| Security Category Tabs | `security-center-order-selector`, `selector-item`, `badge` | compromised, weak, reused, all, complete duplicates, excluded active states captured | Category tabs are risk filters with long explanatory tooltips. |
| Security Table | `table-items`, `table-scrollable`, `table-outer`, `table-inner`, `rf-security-center-item`, `c_name`, `c_url`, `c_password`, `c_strength`, `c_age`, `c_location`, `c_username`, `c_commands` | all main category table variants captured; masked passwords, strength badges, row actions, duplicate group rows captured | Columns vary by tab. Detailed spec in `09-pattern-security-center.md`. |
| Sortable Header | `header-sort`, `sort-arrow`, `real-fixed-header-content` | default, sorted asc/desc likely class-driven | Need active sorted state. |
| Password Column | `password-header-column`, `toggle-pwd`, `toggle-pwd-hide`, `c_password` | password hidden captured | Need revealed state. |
| Strength Column | `strength-header-column`, `security-password-strength`, `weak`, `medium`, `good`, `strong` | weak table badges captured; medium and strong detail badges captured elsewhere | Map to risk/strength tokens. |
| Manage Columns | `manage-icon` | default, hover, active, focus-visible | Reused in tables. |
| Manage Columns Dialog | `dialog-view dialog-modal manage-columns-dialog`, `columns-list`, `item draggable`, `locker-icon`, `close-icon` | open captured, locked Name column captured, draggable optional columns captured, Save disabled captured | Detailed spec in `09-pattern-security-center.md`. |
| Security Multiselect | `rf-multiselect-cmdbar medium`, `rf-multiselect-cmd-item`, `cmd-framed`, `rf-multiselect-in-process`, `rf-multiselect-selected`, `rf-menu-cmd-deselect` | active captured, 4 selected rows, batch commands captured | Batch commands include exclude, batch log in, move, clone, delete, select all, deselect/status. |
| Data Breach Monitoring | `email-details-view`, `email-info-header`, `email-info-sidebar`, `breach-item`, `breach-item selected`, `critical-breach-icon`, `breach-info`, `breach-info-header`, `compromised-data-section` | active tab captured, breach list captured, selected breach detail captured, critical/non-critical data captured | Detailed spec in `09-pattern-security-center.md`. Do not store real revealed passwords in docs or fixtures. |

## Password generator

| Component | Source classes/selectors | States found | Notes |
|---|---|---|---|
| Generator Tabs | `password-generator-order-selector`, `selector-item` | random password captured, passphrase captured | Same segmented selector pattern. |
| Generated Password Panel | `rf-password-view-and-actions`, `data-score` | none, weak, medium, good, strong | Panel color changes by score. |
| Generated Password Text | `rf-generated-password` | generated strong password captured | Needs line wrapping and monospace decision. Detailed spec in `08-pattern-password-generator.md`. |
| Generate Button | `rf-generate-password-button` | default captured | 48px circular icon button. Need hover/active/focus. |
| Copy Password Button | `rf-copy-password-button` | default captured | High-salience action inside score-colored panel. Need copied feedback state. |
| Number Stepper | `edit-box-with-spin`, `number-input`, `buttons-container`, `button-up`, `button-down` | value 16, min 1, max 512, step 1 captured | Needs reusable numeric input spec. |
| Option Row | `toggle-option`, `setting-name-text`, `chars-input-identity` | toggles on/off captured, with-input captured | Could become settings row pattern. |

## Settings and sheets

| Component | Source classes/selectors | States found | Notes |
|---|---|---|---|
| Sheet Shell | `sheet-view`, `sheet`, `sheet-title`, `sheet-title-text`, `sheet-content`, `done-button` | settings sheet captured, Done action captured | Detailed spec in `17-pattern-settings-sheets.md`. |
| Settings Row | `settings-list`, `settings-row`, `vertical-setting-with-title`, `horizontal-setting-with-title`, `setting-title`, `setting-content`, `hint`, `disabled` | vertical rows, horizontal rows, disabled/policy rows captured | Settings use explanatory row pattern with inline or block controls. |
| Settings Toggle Row | `settings-row setting-toggle`, `setting-toggle on`, `toggle`, `role="switch"`, `aria-checked` | on and off captured | Switch semantics captured in AutoFill, AutoSave, Advanced. |
| Settings Radio Group | `radio-group`, `option-checked`, `radio-button`, `radio-text`, `role="radio"`, `aria-selected` | selected, unselected, disabled option captured | AutoFill/AutoSave mode selectors. |
| Settings Action Row | `setting-action`, `action-button unselectable`, `action-button disabled` | enabled and disabled action captured | Advanced and License & Billing. |
| License/Billing Rows | `setting-license-type`, `setting-license-company`, `setting-license-status`, `sponsored-account-setting disabled`, `managed-by-policy-icon` | license type, company, active status, policy-managed disabled form captured | Do not store real company/logo fixture data. |
| Devices And Activity | `devices-and-activity-view`, `tab-selector`, `selector-item selected`, `unusial-activity-warning`, `activities-list`, `activity`, `show-device` | Activities tab captured, warning banner captured, activity rows captured | Source typo `unusial` should not become public API naming. Need Devices tab. |
| Backups List | `backup`, `column-value`, `column-inner-value`, `creation-time`, `creation-type`, `creation-method-text`, `storage-location`, `expand-icon`, `new-backup-button` | collapsed rows captured, scheduled/manual labels captured, local/server labels captured, create-new action captured | Need expanded backup details. |
| Settings Notification | `notification information`, `text`, `icon close-icon` | information notification captured | Separate from common `rf-notification` until placement/timing are understood. |

## Sharing and emergency access

| Component | Source classes/selectors | States found | Notes |
|---|---|---|---|
| Sharing Center Tabs | `sharing-center-order-selector`, `sharing-center-shared-with-me`, `sharing-center-shared-by-me` | shared with me active captured, shared by me active captured | Detailed spec in `10-pattern-sharing-center.md`. |
| Sharing Center Sections | `sharing-data-view`, `rf-items-section-shared-folders`, `rf-items-section-groups`, `rf-items-section-shared-files` | shared folders, groups, shared files captured; hidden empty section captured | Uses vault item cards with sharing indicators. |
| Sharing Settings Dialog | `rf-sharing-dialog`, `rf-sharing-folder`, `rf-create-sharing-folder`, `rf-received-folder`, `sharing-dialog-body`, `rf-sharing-status`, `rf-group-status` | group settings, creator folder settings, received folder settings, create shared folder captured | Detailed spec in `10-pattern-sharing-center.md`. |
| Sharing Recipients | `rf-sharing-add-recipient`, `rf-email-autocomplete-list`, `mail-suggestion`, `rf-perm`, `rf-sharing-recipients-table`, `rf-sharing-recipiant`, `recipiant-status` | owner, manager, admin, pending, editable permission shell, autocomplete captured | Source misspells recipient as `recipiant`; do not carry typo into DS API. |
| Sharing Permission Menu | `popup-menu permission-menu`, `item selected`, `permission-tip` | open captured, Log in only selected, Read and write, Full control | Detailed spec in `10-pattern-sharing-center.md`. |
| Sharing Destructive Confirmation | `dialog-view dialog-modal but-licenses-dialog`, `confirmation-text`, `text-part`, `button default-button` | Stop Sharing and Confirm Deletion captured | Source uses normal default button for destructive action; DS should map to destructive intent. |
| Emergency Access Tabs | `emergency-access-order-selector`, `emergency-access-my-contacts`, `emergency-access-im-contact-for`, `rf-bandge-count` | both tabs active states captured, badge hidden captured | Reuses order selector. Detailed spec in `11-pattern-emergency-access.md`. |
| Emergency Access Empty State | `rf-ea-data`, `rf-ea-accounts`, `rf-ea-no-accounts`, `rf-ea-no-accounts-icon`, `rf-ea-no-accounts-text`, `rf-no-items-btn`, `rf-ea-new-contact` | My Contacts empty captured, I'm Contact For empty captured, invite action visible/hidden captured | Detailed spec in `11-pattern-emergency-access.md`. |
| Emergency Access Invite Dialog | `rf-ea-dialog`, `rf-ea-new-contact-dialog`, `rf-ea-description`, `rf-field-error`, `select-wrapper`, `custom-select`, `rf-ok-button disabled-button` | dialog captured, email field captured, timeout select closed with `2 days`, Invite disabled | Detailed spec in `11-pattern-emergency-access.md`. |
| Emergency Access Table | `rf-ea-accounts`, `rf-ea-account`, `rf-title`, `rf-email`, `rf-timeout`, `rf-status`, `rf-commands` | table shell captured empty | Needs populated rows. |
| Emergency Status | `invited`, `requested`, `accepted`, `access-granted`, `rejected` | semantic statuses | Map to status tokens and icon set. |
| Pending Action | `rf-ea-pending-action`, `rf-ea-pending-action-btn`, `rf-ea-pending-action-accept`, `rf-ea-pending-action-deny` | accept, deny, grant, reject | Icon-only action buttons. |

## Cross-cutting states

| State class/pattern | Meaning |
|---|---|
| `hidden` | Component not displayed. |
| `active` | Selected/current item or tab. |
| `selected` | Selected list item or custom select option. |
| `highlighted` | Focus/keyboard/highlighted list state in legacy layer. |
| `disabled`, `rf-cmd-disabled` | Disabled control/action. |
| `auto-hide` | Action hidden until hover/focus/context. |
| `shown-popup-menu` | Menu currently open for row/item. |
| `rf-fade-in`, `rf-fade-out` | Overlay/dropdown animation state. |
| `cmd-assigned` | Command behavior attached by JS. Implementation detail, not a design-system state. |
| `rf-dark`, `rf-light` | Per-item/theme contrast state, especially custom backgrounds. |
| `rf-has-own-background` | Item has a custom background requiring contrast handling. |

## Immediate component spec order

1. App Shell
2. Header Search
3. Sidebar Navigator
4. Dropdown/Menu
5. Button/IconButton
6. Text Input/Password Input
7. Vault Item
8. View Selector/Order Selector
9. Security Table
10. Password Generator Panel
11. Sharing Indicator
12. Emergency Access Table
