# Identity Detail And Instances

## Sources

Captured from full `#v8` dumps and focused `rf-editor` / `details-view` / `editor-content` fragments.

States captured:

- Identity detail in `view-mode`.
- Inline editor shell with resize handle and collapsed navigator context.
- Identity header with initials avatar.
- Groups pane with Person, Address, Credit Card, Bank Account, Business, Passport, and Car.
- Section groups with expanded child instances.
- Current group and current child instance states.
- Groups with no information shown as add prompts.
- Instance command bar for add, edit, rename, copy, delete.
- Field layouts for person, address/location, credit card, bank account, business, passport, and car.
- Identity edit-mode for person, address/location, credit card, bank account, business, passport, and car.
- Edit actions pane with group icon, group title, cancel/save buttons.
- Edit field groups, selects, textarea notes, add-note collapsed state, and disabled save state.

Sensitive-data rule: identity records contain personal, address, financial, passport, vehicle, and business data. Do not store real values in design-system fixtures. Use synthetic values or redacted examples only.

## Component Role

Identity Detail is the read/action surface for a form-fill identity. It differs from Login Detail because the main content is split into a left group navigator and a right instance pane. A single identity can contain multiple instances of some groups, such as several addresses or cards.

## Shell And Header

| Part | Source class | Purpose |
|---|---|---|
| Editor shell | `rf-editor` | Right-side or inline identity detail panel. |
| Resize handle | `rf-resize-handle` | Width resizing affordance. |
| Detail host | `details-view` | Hosts the identity editor view. |
| Identity view root | `editor-view editor-inline editor-identity view-mode` | Read/action state for identities. |
| Identity edit root | `editor-view editor-inline editor-identity edit-mode` | Field-editing state for identity group/instance. |
| Content | `editor-content` | Header plus body. |
| Header | `content-header` | Identity title and commands. |
| Avatar | `header-image bg-color-*` | Initials avatar, e.g. two-letter initials. |
| Title | `header-title`, `title-name` | Identity display name. |
| Rename pane | `inplace-edit-pane hidden` | Hidden inline rename state, same source pattern as login detail. |
| Main command bar | `editor-cmdbar cmdbar-main` | Header-level commands. |

Captured main commands:

| Command | Source class | Tooltip |
|---|---|---|
| View in new tab | `editor-cmd editor-cmd-view-in-new-tab` | `View In New Tab` |
| More actions | `editor-cmd editor-cmd-more` | `More Actions` |
| Close | `editor-cmd editor-cmd-close` | `Close` |

Design implications:

- Identity uses an initials/color avatar rather than a website logo.
- Header command order is different from compact login headers and should not be inferred from login detail.
- View-mode header includes view-in-new-tab, more, and close commands.
- Edit-mode header only captured close command in `cmdbar-main`.
- Inline rename is available structurally but only hidden state is captured for Identity so far.

## Body Layout

| Part | Source class/style | Purpose |
|---|---|---|
| Body | `content-body` | Two-pane identity content area. |
| Groups pane | `groups-pane`, inline `width: 200px` | Left navigation for identity groups and instances. |
| Groups list | `groups-list noselect` | Vertical list of group rows. |
| Instance pane | `instance-pane` | Right detail pane for selected group/instance. |
| Instance command bar | `editor-cmdbar cmdbar-instance` | Context actions for selected group/instance. |
| Instance content | `instance-content` | Fields and warnings. |
| Security warning | `security-warning hidden` | Warning slot, hidden in captures. |
| Fields list | `instance-fields-list` | Field stack for selected instance. |
| No fields state | `instance-no-fields hidden` | Empty state slot, hidden in populated captures. |

Design requirements:

- The groups pane has a stable narrow width and should not collapse field content unpredictably.
- Instance pane is the primary reading area; copy affordances need to align per field type.
- `security-warning` and `instance-no-fields` are real slots even though hidden in current captures.

## Group Navigator

| Part/state | Source class | Notes |
|---|---|---|
| Group row | `instance-group-item` | Base row. |
| Current row | `instance-group-current` | Selected group or selected child instance. |
| Section title | `instance-group-section-title` | Expandable parent for repeated instances. |
| Section title current | `instance-group-section-title-current` | Parent section highlighted while one child is active. |
| Expanded section | `instance-group-section section-shown` | Child instance list visible. |
| Dropdown affordance | `instance-group-dropdown-image` | Expand/collapse indicator. |
| Group icon | `instance-group-image` | Group type icon. |
| Group title | `instance-group-title` | Group or instance label. |
| Count | `instance-group-count hidden` | Count exists but was hidden in captures. |
| Row menu | `instance-group-menu` | Per-group menu affordance. |
| No information row | `no-information` | Add prompt for missing optional groups. |

Captured group types:

| Group | Source class | Captured states |
|---|---|---|
| Person | `instance-group-person` | normal, current |
| Address | `instance-group-address` | normal, current, section title with child instances |
| Credit Card | `instance-group-creditcard` | section title, section title current, child current |
| Bank Account | `instance-group-bank` | normal, current |
| Business | `instance-group-business` | normal, current |
| Passport | `instance-group-passport` | no-information add row, populated current row |
| Car | `instance-group-car` | no-information add row, populated current row |

Design implications:

- Some groups are single-instance rows; others become expandable section headers with child instance rows.
- Missing optional groups display action copy such as `Add Passport` and `Add Car`, not an empty field panel.
- Parent section and child instance can both need selected/active visual treatment.

## Instance Commands

| Command | Source class | Tooltip pattern |
|---|---|---|
| Add new | `editor-cmd editor-cmd-add-new` | `Add new "{Group}"` |
| Edit | `editor-cmd editor-cmd-edit` | `Edit "{Group}"` |
| Rename | `editor-cmd editor-cmd-rename` | `Rename "{Group}"` |
| Copy | `editor-cmd editor-cmd-copy` | `Copy "{Group}"` |
| Delete | `editor-cmd editor-cmd-delete` | `Delete "{Group}"` |

Captured command variants:

- Person, Address, Bank Account, Business, Passport, and Car show add/edit/copy/delete.
- Credit Card child instance additionally shows rename.

Design requirements:

- Delete is a destructive command even when source only exposes a neutral icon class.
- Rename is instance-specific and may not appear for every group.
- Tooltips interpolate the selected group label; long labels need clipping rules.

## Field Anatomy

| Part | Source class | Purpose |
|---|---|---|
| Field row | `rf-field` | Standard single field. |
| Field caption | `rf-field-caption` | Field label. |
| Field value | `rf-field-value` | Display value. |
| Inline copy pane | `inline-onhover-buttons-pane` | Hover/focus copy affordance. |
| Copy icon | `onhover-button onhover-button-copy` | Copy selected field/group value. |
| Row fields | `rf-row-fields`, `rf-row-field` | Multiple related fields in one row. |
| Grouped field value | `rf-group-fields`, `rf-field-content-item`, `rf-field-content-item-caption` | Composite values like names, dates, expiry, or plate/state. |
| Date fields | `rf-date-fields`, `rf-date-field`, `rf-date-field-value` | Structured date display. |
| Country field | `rf-field-country`, `rf-country-image`, `rf-field-content-item-value` | Country with flag asset. |
| Note field | `rf-note` | Multiline note display. |

Design requirements:

- Copy controls are field-level hover actions and must be available by keyboard.
- Composite fields need consistent separators and copy behavior: copy the whole logical value, not an arbitrary visual fragment.
- Country flags are content icons; fallback state is needed when flag asset fails.
- Notes can contain line breaks and need long-text wrapping rules.

## Edit Mode

Edit-mode replaces the two-pane `content-body` with an action header and a single editable field stack.

### Edit Shell

| Part | Source class | Purpose |
|---|---|---|
| Edit root | `editor-view editor-inline editor-identity edit-mode` | Identity editing state. |
| Header | `content-header` | Same identity avatar/title area as view mode. |
| Close command | `editor-cmd editor-cmd-close` | Only header command captured in edit-mode. |
| Actions pane | `actions-pane` | Edit context title and save/cancel controls. |
| Group icon | `group-icon group-icon-*` | Icon for group being edited. |
| Group title | `group-title` | Copy such as `Edit Business`, `Edit Passport`, `Edit Car`. |
| Buttons bar | `buttons-bar` | Cancel/save actions. |
| Cancel | `button` | Cancel edit. |
| Save | `button default-button disabled` | Save action, disabled in captures. |
| Fields list | `fields-list fields-*` | Group-specific editable field stack. |

Captured group icons and field-list roots:

| Group | Group icon | Fields root |
|---|---|---|
| Person | `group-icon-person` | `fields-list fields-person` |
| Address/location | `group-icon-location` | `fields-list fields-location` |
| Credit Card | `group-icon-creditcard` | `fields-list fields-credit-card` |
| Bank Account | `group-icon-bankaccount` | `fields-list fields-bank-account` |
| Business | `group-icon-business` | `fields-list fields-business` |
| Passport | `group-icon-passport` | `fields-list fields-passport` |
| Car | `group-icon-car` | `fields-list fields-car` |

### Editable Field Anatomy

| Part | Source class | Purpose |
|---|---|---|
| Text field | `input-with-title editor-field field-id-*` | Floating-label text input. |
| Input value state | `input.has-value` | Field has populated value. |
| Floating label | `label.title.noselect` | Field label tied to input `id`. |
| Field group | `field-group` | Horizontal group of related fields. |
| Select field | `editor-field field-select field-id-*` | Select/combobox field. |
| Select control | `custom-select selected` | Closed selected combobox. |
| Select label | `select-upper-title` | Floating select label. |
| Section separator | `field-section-separator` | Separates logical field blocks. |
| Note field | `editor-field field-id-note` | Optional multiline note area. |
| Add note prompt | `show-note` | Collapsed note add state. |
| Hidden add note prompt | `show-note hidden` | Note already present. |
| Textarea wrapper | `textarea-with-title` | Floating-label textarea. |
| Hidden textarea | `textarea-with-title hidden` | Note body hidden while add prompt is shown. |
| Textarea value state | `textarea.has-value` | Note has populated value. |

Design implications:

- Identity edit-mode uses the same `input-with-title editor-field` input family as other editor flows, not the New Login `field-with-title field-border` family.
- `field-group` is the primary layout primitive for paired fields and should define responsive wrapping rules.
- Save disabled is confirmed; save enabled and loading are still needed.
- Note editing has two distinct states: textarea visible with `show-note hidden`, and add-note prompt visible with `textarea-with-title hidden`.
- Selects use closed `custom-select selected`; opened option menus still need capture.

### Captured Edit Field Sets

| Field set | Fields root | Captured field IDs / structures |
|---|---|---|
| Person | `fields-person` | title, first name, middle initial, last name, suffix, sex select, age, birth place, social security number, driver license number/state, income, position, phone/email/Skype-like contact fields, note |
| Address/location | `fields-location` | address line fields, city, state/province select, county, zip/postcode, note |
| Credit Card | `fields-credit-card` | card number, validation code/CVV, PIN, user name, issuing bank, customer-service phones, credit limit, interest rate, note |
| Bank Account | `fields-bank-account` | bank name, branch, address, phone, account owner, account type select, account/routing/SWIFT numbers, PIN code, interest rate, note |
| Business | `fields-business` | company name, department, business type select, employer id, phone, website, stock symbol, note |
| Passport | `fields-passport` | passport number, issue date, expiration date, note |
| Car | `fields-car` | plate number, state select, make, model, year, VIN, add-note prompt |

Security implications:

- Edit-mode contains even more sensitive values than view-mode because all fields are directly editable.
- Fixtures for these states must use fake values and should not include real field values copied from source.
- Validation and save lifecycle should be documented from redacted/synthetic data only.

## Captured Field Sets

| Field set | Source class | Captured field types |
|---|---|---|
| Person | `rf-identity-person-fields` | title/name row, birth date, phone, email, sex |
| Address/location | `rf-identity-location-fields` | address lines, postcode, city, country with flag |
| Credit card | `rf-identity-card-fields` | card type, card number, CVV, expiry, cardholder name, PIN |
| Bank account | `rf-identity-bank-fields` | account number |
| Business | `rf-identity-buisness-fields` | company name, website, registration number, note |
| Passport | `rf-identity-passport-fields` | passport number, issue date, expiration date, note |
| Car | `rf-identity-car-fields` | plate composite, make, model, year, VIN |

Source note: `rf-identity-buisness-fields` is misspelled in source. Keep the source spelling in capture docs, but do not carry the typo into design-system public API names.

Security implications:

- Credit card number, CVV, PIN, passport number, bank account, VIN, phone, email, address, and birth date are sensitive or personal data.
- Design fixtures should use fake values and should never include copied production/user data.
- Reveal/copy feedback should avoid echoing full sensitive values in notifications.

## States To Document

| State | Captured | Notes |
|---|---:|---|
| Identity view-mode shell | Yes | `editor-identity view-mode`, `rf-editor`, resize handle. |
| Identity edit-mode shell | Yes | `editor-identity edit-mode`, `actions-pane`, group icon/title, buttons bar, fields list captured. |
| Header initials avatar | Yes | `header-image bg-color-*` with initials. |
| Identity inline rename | Partial | Hidden `inplace-edit-pane` captured; visible edit state still needed. |
| Groups pane default | Yes | Left pane width 200px captured. |
| Current single group | Yes | Person, Address, Bank, Business, Passport, Car variants captured. |
| Section group expanded | Yes | Address and Credit Card sections captured with `section-shown`. |
| Section title current | Yes | Credit Card parent highlighted while child is selected. |
| Child instance current | Yes | Address/card child rows captured. |
| Missing optional group | Yes | `no-information` rows for Add Passport/Add Car captured. |
| Instance commands | Yes | Add/edit/copy/delete captured; rename captured for Credit Card. |
| Security warning | Partial | `security-warning hidden` only. |
| No fields | Partial | `instance-no-fields hidden` only. |
| Person fields | Yes | Composite name and date layouts captured. |
| Address fields | Yes | Country flag field captured. |
| Credit card fields | Yes | Sensitive field set captured; values not retained. |
| Bank fields | Yes | Account-number field captured; value not retained. |
| Business fields | Yes | Note/multiline field captured. |
| Passport fields | Yes | Sensitive field set captured; values not retained. |
| Car fields | Yes | Composite plate field captured. |
| Person edit fields | Yes | `fields-person`, grouped fields/selects/note captured; values not retained. |
| Address edit fields | Yes | `fields-location`, address fields/select/note captured; values not retained. |
| Credit card edit fields | Yes | `fields-credit-card`, sensitive card fields/note captured; values not retained. |
| Bank account edit fields | Yes | `fields-bank-account`, sensitive bank fields/select/note captured; values not retained. |
| Business edit fields | Yes | `fields-business`, select, section separator, note captured; values not retained. |
| Passport edit fields | Yes | `fields-passport`, grouped dates and note captured; values not retained. |
| Car edit fields | Yes | `fields-car`, grouped fields, state select, add-note prompt captured; values not retained. |
| Save disabled | Yes | `button default-button disabled`. |
| Save enabled/loading | No | Need after changing a field and saving if reachable. |
| Note add prompt | Yes | Car edit captured visible `show-note` with hidden textarea; other edits captured populated note. |
