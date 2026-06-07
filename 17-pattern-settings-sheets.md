# Settings Sheets And Account Activity

## Sources

Captured from focused `sheet-view` fragments and full settings-related dumps.

States captured:

- Generic settings sheet shell.
- AutoFill settings.
- AutoSave settings.
- Advanced settings.
- License & Billing settings.
- Devices & Activity sheet with Activities tab selected.
- Backups list surface.
- Settings information notification after saving a setting.
- Settings rows with radio groups, toggles, selects, actions, disabled policy-managed form, and license/status rows.
- Activity list with warning banner and device/activity rows.

Sensitive-data rule: Devices & Activity can include approximate location, IP address, timestamps, and device names. License & Billing can include company names/logos. Do not store real values in design fixtures.

## Component Role

Settings Sheets are account/product configuration surfaces opened as sheet-like pages. They use a dedicated shell with a title and `Done` action, then rows of setting-specific controls. Devices & Activity uses the same sheet surface but has a tabbed layout and list/table-like activity feed.

## Sheet Shell

| Part | Source class | Purpose |
|---|---|---|
| Sheet root | `sheet-view` | Main settings/sheet surface, `role="main"`. |
| Sheet | `sheet` | Inner sheet container. |
| Title bar | `sheet-title` | Title and Done action for regular settings pages. |
| Title text | `sheet-title-text` | Page title, focusable with `tabindex="-1"`. |
| Done action | `done-button` | Closes sheet, `role="button"` in captures. |
| Content | `sheet-content` | Body content area. |
| Settings list | `settings-list` | Vertical list of settings rows. |

Captured sheet titles:

- `AutoFill`
- `AutoSave`
- `Advanced`
- `License & Billing`

Design implications:

- `Done` is part of the sheet chrome, not a row action.
- Sheet titles should support programmatic focus for navigation/return-to-page behavior.
- Regular settings pages and Devices & Activity share `sheet-view`, but Devices & Activity replaces `sheet-title` with an internal tab selector.

## Settings Rows

| Row type | Source class | Purpose |
|---|---|---|
| Base row | `settings-row` | Generic setting row. |
| Vertical row with title | `vertical-setting-with-title` | Title/hint followed by larger control block, such as radio group. |
| Horizontal row with title | `horizontal-setting-with-title` | Title, inline setting content, hint. |
| Toggle row | `setting-toggle` | Title/hint plus switch. |
| Toggle on | `setting-toggle on` | Switch enabled state at row level. |
| Action row | `setting-action` | Title/hint plus command button. |
| Disabled/policy row | `disabled` | Row disabled or policy-managed. |
| License type row | `setting-license-type` | Displays license type. |
| License company row | `setting-license-company` | Displays company logo/title. |
| License status row | `setting-license-status` | Displays subscription state. |

Shared row anatomy:

| Part | Source class | Purpose |
|---|---|---|
| Title | `title` | Setting label. |
| Hint | `hint` | Explanatory helper copy. |
| Setting title group | `setting-title` | Title/hint group in toggle rows. |
| Setting content | `setting-content` | Inline control/action area. |

Design requirements:

- Settings rows are dense but explanatory. Hints are part of the setting, not tooltips.
- Disabled/policy-managed rows need a visible reason and noninteractive controls.
- Long titles, especially Advanced settings, need wrapping rules without breaking control alignment.

## Controls In Settings

### Radio Group

Captured in AutoFill and AutoSave:

| Part/state | Source class/attribute | Notes |
|---|---|---|
| Group | `radio-group` | Contains label-based radio options. |
| Option | `label`, `role="radio"` | Radio option wrapper. |
| Checked option | `option-checked`, `aria-selected="true"` | Selected state. |
| Unchecked option | `aria-selected="false"` | Unselected state. |
| Disabled option | `input disabled` | Disabled native input inside option. |
| Visual control | `radio-button` | Custom radio visual. |
| Text | `radio-text` | Main label, can include secondary inline span. |

### Toggle / Switch

Captured in AutoFill, AutoSave, Advanced:

| Part/state | Source class/attribute | Notes |
|---|---|---|
| Switch | `toggle`, `role="switch"` | Interactive switch control. |
| On row | `settings-row setting-toggle on` | Row-level on state. |
| On switch | `aria-checked="true"` | Semantic on state. |
| Off switch | `aria-checked="false"` | Semantic off state. |

### Select

Captured in Advanced:

| Part | Source class/attribute | Purpose |
|---|---|---|
| Select wrapper | `custom-select` | Closed combobox. |
| Combobox semantics | `role="combobox"`, `aria-haspopup="true"`, `aria-expanded="false"` | Accessibility state. |
| Selected value | `select-option selected` | Current value. |

### Action Button

Captured in Advanced and License & Billing:

| Part/state | Source class | Notes |
|---|---|---|
| Action button | `action-button unselectable` | Button inside setting row. |
| Disabled action | `action-button disabled` | Noninteractive/policy-disabled action. |

## Captured Setting Pages

| Page | Captured controls | Notes |
|---|---|---|
| AutoFill | radio group, off toggle, on toggle | Offer mode selected; contact offer off; basic auth autofill on. |
| AutoSave | radio group, on toggle, off toggles | Offer mode selected; basic auth autosave on; keyboard shortcuts off. |
| Advanced | off toggles, on toggle, closed selects, action buttons | Desktop editor/menu toggles, browser password manager setting, popularity selects, restore/open actions. |
| License & Billing | license rows, company row, status row, disabled sponsored account form | Policy-managed row includes icon, disabled input, disabled action, explanatory list. |

## Devices And Activity

Devices & Activity is a sheet variant with internal tabs and a list/table-style activity feed.

| Part | Source class | Purpose |
|---|---|---|
| Root | `devices-and-activity-view` | Devices/activity sheet content. |
| Tab selector | `tab-selector`, `role="tablist"` | Internal tabs. |
| Tab | `selector-item`, `role="tab"` | Devices / Activities. |
| Selected tab | `selector-item selected` | Active tab. |
| Warning banner | `unusial-activity-warning` | Unusual activity warning. Source typo is preserved only as source evidence. |
| Warning icon | `warning-icon` | Warning visual. |
| Warning text | `warning-text` | Warning copy with inline action. |
| Master Password action | `change-mp`, `role="button"` | Inline action to change Master Password. |
| Banner close | `close-button` | Dismiss warning. |
| Activities container | `activities` | Activity tab content. |
| List | `activities-list`, `role="list"` | Activity rows. |
| Header row | `title-bar` | Column labels. |
| Activity row | `activity`, `role="listitem"` | One activity record. |
| Show device affordance | `show-device`, `show-device-icon` | Opens device details. |

Captured activity columns:

- Date and time
- Action
- Approximate location
- Device

Design requirements:

- Activity rows are interactive list items with keyboard focus.
- Approximate location/IP/device values are sensitive enough for fixtures to use synthetic values.
- The warning banner has an inline action and a close button; both need keyboard behavior.
- Source spelling `unusial-activity-warning` should not become public design-system API naming.

## Backups

Captured backup list surface in settings context.

| Part | Source class | Purpose |
|---|---|---|
| Backup row | `backup`, `role="group"` | One backup entry. |
| Expanded state | `aria-expanded="false"` | Collapsed state captured. |
| Column wrapper | `column-value` | One column value cell. |
| Inner value | `column-inner-value` | Sized/aligned cell content. |
| Created time column | `creation-time` | Date/time plus relative time. |
| Expand affordance | `expand-icon` | Row expansion control. |
| Date | `date` | Backup creation date. |
| Relative time | `time-passed` | Human-readable age. |
| Creation type | `creation-type`, `creation-method-text` | Scheduled/manual creation label. |
| Storage location | `storage-location` | Local/server location label. |
| New backup action | `new-backup-button` | Create new backup action, `role="button"`. |

Captured states:

- Collapsed backup rows.
- Scheduled and manually created backup type labels.
- Local and server storage labels.
- New backup floating/action button.

Design requirements:

- Backup rows are interactive groups with expansion semantics; expanded content still needs capture.
- Date/time values in fixtures should be synthetic.
- Inline widths were captured on cell content; design-system implementation should express layout through column rules, not fixed copied inline widths.

## Settings Notification

Captured settings-specific notification:

```html
<div class="notification information">
  <div class="text">Domains setting changes have been saved</div>
  <div class="icon close-icon" title="Close"></div>
</div>
```

Design implications:

- This is separate from `rf-notification rf-common-notification`.
- The state is an in-settings information notification with close icon.
- Copy confirms a setting save; it should not be merged blindly with vault/editor toasts until placement and timing are confirmed.

## States To Document

| State | Captured | Notes |
|---|---:|---|
| Sheet shell | Yes | `sheet-view`, `sheet`, title/content/done captured. |
| AutoFill settings | Yes | Radio group, toggle on/off captured. |
| AutoSave settings | Yes | Radio group, toggle on/off captured. |
| Advanced settings | Yes | Toggles, selects, action buttons captured. |
| License & Billing | Yes | License rows, company row, disabled sponsored account form captured. |
| Devices & Activity: Activities tab | Yes | Tab selector, warning banner, activity list captured. |
| Devices tab | No | Need Devices selected state and device rows/details. |
| Backups list | Yes | Collapsed backup rows and new-backup action captured. Expanded backup details still needed. |
| Settings notification | Yes | `notification information` with close icon captured. |
| Settings search/nav | No | Need settings landing/sidebar if it exists separately. |
| Open select menu | No | Advanced select opened state still needed. |
| Toggle focus/disabled | Partial | Switch semantics captured; visual focus/disabled variants still needed. |
| Policy-managed disabled field | Yes | Disabled sponsored account row captured. |
| Warning banner dismissed | No | Need dismissed state if persistent. |
