# Security Center

## Sources

Captured from full `#v8` dump with Security Center visible.

Captured states:

- Security Center overview selected.
- Overall score state `good`.
- Summary chart with password strength distribution and reused/unique inner chart.
- Security category tabs with counts.
- Compromised table populated with rows.
- Weak, Reused, All, Complete Duplicates, and Excluded tables populated.
- Masked password column and weak strength badges.
- Multiselect mode in Compromised table with 4 selected rows.
- Data Breach Monitoring tab active.
- Breach email detail view with breach list and selected breach info.

## Component Role

Security Center is the risk review workspace for saved logins. It summarizes password health, groups logins by risk category, and exposes table-level commands for viewing, changing, revealing, and managing affected credentials.

It is a high-density operational surface, not a marketing dashboard. The design-system treatment should prioritize scanability, table ergonomics, and clear risk semantics.

## Root And Top Tabs

| Part | Source class | Purpose |
|---|---|---|
| Root | `rf-security-center good` | Security Center with overall score state. |
| Section header | `rf-section-data-view-header` | Hosts top-level tabs. |
| Top tab selector | `security-center-tab-order-selector order-selector noselect` | Switches Security Overview/Data Breach Monitoring. |
| Active top tab | `selector-item active` | Captured `Security Overview`. |
| Data breach tab | `selector-item` | Captured `Data Breach Monitoring`. |
| Breach notification | `data-breach-mon-notification hidden` | Hidden badge/indicator. |

Captured top tabs:

- `Security Overview` active.
- `Data Breach Monitoring` inactive.
- `Data Breach Monitoring` active, with `Security Overview` hidden.

## Summary Pane

| Part | Source class | Purpose |
|---|---|---|
| Overview root | `rf-security-center-security-overview` | Main overview content. |
| Summary pane | `summary-pane` | Chart and score summary. |
| Chart pane | `chart-pane` | Distribution chart and legend. |
| Chart | `chart` | Donut/radial chart built from clipped chunks. |
| Chart segment | `chart-chunk` | Segment of score distribution. |
| Segment gap | `chart-gap` | Visual gap between segments. |
| Inner reused chart | `reused-chart` | Nested reused/unique distribution. |
| Chart title | `chart-title` | Captured initials `AS`. |
| Legend | `legend` | Strength distribution legend. |
| Reused legend | `reused-legend` | Unique/reused distribution. |
| Score column | `score-column` | Overall score and recommendation. |
| Score pane | `score-pane good` | Overall score card/status. |
| Score result | `score-result` | `Your Security Score:` and score value. |
| Progress bar | `progressbar` | Score scale. |
| Score marker | `score` | Inline position, captured `left: 89%;`. |
| Recommendation | `recomendation` | Source typo; contains recommendation copy. |

Captured distribution:

| Legend item | Percent |
|---|---:|
| Strong | 83% |
| Good | 9% |
| Medium | 3% |
| Weak | 3% |
| Compromised | 2% |
| Unique | 95% |
| Reused | 5% |

Captured score:

- Score value: `89`.
- Score label: `Good`.
- Recommendation: majority of passwords are unique and complex; review breakdown to replace weak or duplicated passwords.

Design requirements:

- The chart needs semantic labels beyond visual clip-path segments.
- Overall score state should map to the same semantic strength/risk token scale used by login detail and generator, but the meaning is account-level rather than password-level.
- The source typo `recomendation` should not become the design-system API.

## Category Tabs

| Part | Source class | Purpose |
|---|---|---|
| Header | `rf-data-view-header` | Hosts category selector and multiselect command bar. |
| Selector | `security-center-order-selector order-selector noselect` | Risk category tabs. |
| Active tab | `selector-item active` | Captured `Compromised`. |
| Count | `badge` | Parenthesized count. |
| Multiselect command bar | `rf-multiselect-cmdbar hidden` | Hidden until rows selected. |

Captured categories:

| Category | Count | Captured tooltip role |
|---|---:|---|
| Compromised | 6 | Password seen in a breach, checked with Have I Been Pwned. |
| Weak | 20 | Not sufficiently complex. |
| Reused | 21 | Same password used on more than one site. |
| All | 440 | No tooltip captured. |
| Complete Duplicates | 1 | Same username/password/domain stored more than once. |
| Excluded | 2 | Logins excluded from Security Score. |

Design requirements:

- Counts are part of tab labels and need stable alignment for three-digit counts.
- Tooltips are long explanatory copy; they need max-width and readable line length.
- The category selector is a segmented control with potentially overflowing labels.

Captured active category variants:

| Active category | Source root | Visible table | Notes |
|---|---|---|---|
| Compromised | `selector-item active` on Compromised | `rf-items security-center-compromised` | Captured both normal and multiselect mode. |
| Weak | `selector-item active` on Weak | `rf-items security-center-weak` | Adds `Age, days` column. |
| Reused | `selector-item active` on Reused | `rf-items security-center-reused` | Adds `URL` and `Age, days` columns. |
| All | `selector-item active` on All | `rf-items security-center-all` | Adds `Age, days` column. |
| Complete Duplicates | `selector-item active` on Complete Duplicates | `rf-items security-center-duplicates` | Adds grouping rows and extra identity columns. |
| Excluded | `selector-item active` on Excluded | `rf-items security-center-excluded` | Includes `Include in Security Score` row command. |

## Compromised Table

| Part | Source class | Purpose |
|---|---|---|
| Data root | `rf-data` | Table and empty state wrapper. |
| Empty state | `rf-no-items rf-no-items-security-center-tab hidden` | Hidden because rows exist. |
| Items root | `rf-items security-center-compromised` | Active category table. |
| Table outer | `table-outer` | Scroll/table container. |
| Table inner | `table-inner` | Table layout wrapper. |
| Virtual scroller | `_scroller` | Virtualized scroll spacer. |
| Table | `table-items table-scrollable` | Security table. |
| Row | `_row rf-security-center-item cmd-assigned` | Login row. |

Captured columns:

| Column | Source class | Header |
|---|---|---|
| Name | `name-header-column header-sort` | `Name` |
| Password | `password-header-column header-sort` | `Password`, includes `toggle-pwd toggle-pwd-hide`. |
| Password Strength | `strength-header-column header-sort` | `Password Strength`. |
| Commands | `commands-header-column` | Contains `manage-icon`. |

Captured column variants by tab:

| Table | Columns |
|---|---|
| `security-center-compromised` | Name, Password, Password Strength, Commands |
| `security-center-weak` | Name, Password, Password Strength, Age, days, Commands |
| `security-center-reused` | Name, URL, Password, Password Strength, Age, days, Commands |
| `security-center-all` | Name, Password, Password Strength, Age, days, Commands |
| `security-center-duplicates` | Name, Folder, Username, Password, Password Strength, Commands |
| `security-center-excluded` | Name, Password, Password Strength, Commands |

Additional cell classes captured:

| Cell | Source class | Meaning |
|---|---|---|
| URL | `c_url` | Reused table URL/domain column. |
| Age | `c_age` | Password age in days. |
| Folder | `c_location` | Folder/location in duplicate rows. |
| Username | `c_username` | Username in duplicate rows; can show an em dash. |

Captured row anatomy:

| Part | Source class | Purpose |
|---|---|---|
| Name cell | `c_name` | Login item cell. |
| Login item | `rf-item rf-item-login` | Reuses vault item anatomy. |
| Sharing shown | `rf-item-sharing-shown` | Sharing indicator visible on captured Airbnb row. |
| Logo | `rf-item-image rf-item-img32` | 32px site icon. |
| Title | `rf-item-title` | Login name. |
| Sharing action | `rf-item-sharing rf-cmdbutton shared-none shared-granted` | Sharing state/action. |
| Multiselect action | `rf-item-multiselect rf-cmdbutton` | Select row. |
| Password cell | `c_password` | Masked password, captured bullets. |
| Strength cell | `c_strength` | Strength badge. |
| Strength badge | `security-password-strength weak` | Captured `Weak`. |
| Commands cell | `c_commands` | Row command bar. |
| Row command bar | `rf-item-cmdbar` | View, log in/change, more actions. |
| View command | `rf-item-view hover-action-cmd rf-cmdbutton` | `View`. |
| Login/change command | `rf-item-log-in rf-cmdbutton` | `Log in and change your ... account password`. |
| More command | `rf-item-menu rf-cmdbutton rf-image-button auto-hide` | `More Actions`. |

Additional row/table states:

| State | Source class | Notes |
|---|---|---|
| Security warning item | `rf-item-sec-warning` | Appears in broader security tables. |
| Group row | `_row_group` | Used in Complete Duplicates table. |
| Group expanded | `icon-expand icon-expand-shown` | Duplicate group disclosure state. |
| Group domain | `group_url` | Captured example: `Successfactors.eu`. |
| Default site icon | `rf-item-default-image` | Fallback logo in rows. |
| 16px site icon | `rf-item-img16` | Some rows use smaller favicon asset. |
| 32px site icon | `rf-item-img32` | Common security table icon size. |
| Include in score | `rf-item-include-to-security-score rf-cmdbutton` | Excluded tab row command. |

Design requirements:

- Rows mix table behavior with vault item anatomy; do not create a visually unrelated table row pattern.
- Passwords are masked by default; header includes a global reveal/hide toggle.
- Row actions include both always-visible and auto-hide commands.
- Weak/compromised rows require urgent but scannable risk styling.
- Manage columns is an icon-only table settings button and needs a visible focus state.

## Manage Columns Dialog

Captured source root:

```html
<div class="dialog-view dialog-modal manage-columns-dialog">
```

| Part | Source class | Purpose |
|---|---|---|
| Dialog | `dialog-view dialog-modal manage-columns-dialog` | Modal for choosing table columns. |
| Header | `header` | Title: `Manage columns`. |
| Close | `close-button` | Dismiss dialog. |
| Content | `content` | Column list and footer actions. |
| Column list | `columns-list` | Ordered list of table columns. |
| Column item | `item` | Non-draggable required column. |
| Draggable item | `item draggable` | Reorderable/removable column. |
| Item text | `text` | Column name. |
| Locked icon | `locker-icon` | Required column cannot be removed. |
| Remove button | `close-icon` | Removes optional column. |
| Footer | `buttons-bar` | Reset, Cancel, Save. |
| Spacer | `flex-cell` | Pushes final actions right. |

Captured columns:

| Column | Source signal | Behavior |
|---|---|---|
| Name | `item`, `draggable="false"`, `locker-icon`, `order: 0` | Required/locked. |
| Password | `item draggable`, `draggable="true"`, `close-icon`, `order: 1` | Optional and reorderable. |
| Password Strength | `item draggable`, `draggable="true"`, `close-icon`, `order: 2` | Optional and reorderable. |

Footer buttons:

- `Reset`
- `Cancel`
- `Save`, captured as `button default-button disabled`

Design requirements:

- Locked columns should communicate required status with icon and accessible text.
- Drag handles are implicit in the row; if redesigned, use an explicit grip icon for discoverability.
- Save stays disabled until column order/visibility changes.
- Reset is a secondary action and should not visually compete with Save.

## Security Center Context Menu

Captured visible commands menu from a Security Center row:

| Group | Items |
|---|---|
| Security score | `Exclude from Security Score` |
| Open/fill | `Log In`, `Go Fill`, `Go To` |
| Copy | `Copy Username`, `Copy Password` |
| Item management | `View`, `Edit`, `Rename`, `Move`, `Clone`, `Add to Pinned`, `Delete` |
| Sharing/export | `Sharing Settings`, `Send` |
| History | `Restore` |

Source details:

- Root: `dropdown-popup context-menu-popup commands-menu shown-popup-menu`.
- Position is inline: `left: 1057px; top: 197px;`.
- Security score action: `rf-menu-cmd-disable-user-data-breach-warning`.
- Standard item actions reuse `rf-menu-cmd-*` command classes.

Design requirements:

- Security-specific actions can appear before standard item actions.
- `Exclude from Security Score` is a risk-management action, not a destructive action, but it affects score visibility and should be clearly reversible in the Excluded tab.
- Sharing command label is context-expanded to `Sharing Settings`.

## Multiselect Mode

Captured in Compromised table with 4 selected rows.

| Part | Source class | Purpose |
|---|---|---|
| Command bar | `rf-multiselect-cmdbar medium` | Batch action toolbar for selected rows. |
| Batch command | `rf-multiselect-cmd-item ... cmd-framed` | Framed action button. |
| Flexible gap | `rf-flex-gap` | Pushes selection actions to the right. |
| Select all command | `rf-menu-cmd-select-all` | Select all visible/eligible rows. |
| Deselect/status command | `rf-menu-cmd-deselect` | Shows selected count and clears selection. |
| Multiselect table state | `rf-items security-center-compromised rf-multiselect-in-process` | Active table is in selection mode. |
| Selected row | `_row rf-security-center-item ... rf-multiselect-selected` | Row selected for batch actions. |

Captured batch commands:

| Command | Source class | Label/title |
|---|---|---|
| Exclude from Security Score | `rf-menu-cmd-disable-user-data-breach-warning` | `Exclude from Security Score` |
| Batch Log In | `rf-menu-cmd-login` | Title `Batch Log In`, label `Batch Log In` |
| Move | `rf-menu-cmd-move` | `Move` |
| Clone | `rf-menu-cmd-clone` | `Clone` |
| Delete | `rf-menu-cmd-delete` | `Delete` |
| Select All | `rf-menu-cmd-select-all` | `Select All` |
| Deselect/status | `rf-menu-cmd-deselect` | Title `4 selected`, label `4 selected` |

Design requirements:

- Batch toolbar replaces the hidden multiselect command bar but keeps category tabs visible.
- Selected row state must be visible across full row width, not only through the small multiselect icon.
- Batch destructive action `Delete` needs destructive styling and confirmation rules.
- The selected count is both status and action; if redesigned, separate count text from clear/deselect control for clarity.

## Data Breach Monitoring

Captured with the top tab `Data Breach Monitoring` active.

| Part | Source class | Purpose |
|---|---|---|
| Top tab | `selector-item active` | Active `Data Breach Monitoring` state. |
| Notification marker | `data-breach-mon-notification hidden` | Hidden indicator inside tab. |
| Overview content | `rf-security-center-security-overview hidden` | Security Overview remains mounted but hidden. |
| Editor shell | `rf-editor` | Right-side detail panel, captured width `680px`. |
| Details view | `details-view` | Hosts email breach detail. |
| Email detail | `email-details-view` | Breach monitoring detail for one email. |
| Email header | `email-info-header` | Email identity and close command. |
| Email icon | `email-icon` | Leading email/avatar icon. |
| Email title | `email-title` | Monitored email address. |
| Email command bar | `email-cmdbar` | Header command area. |
| Close command | `email-command-close` | Close detail panel. |
| Sidebar | `email-info-sidebar` | Breach list. |
| Breach list title | `email-info-breach-title` | Captured `Сritical breaches` with Cyrillic-looking first character in source. |
| Breach item | `breach-item` | Selectable breach row. |
| Selected breach item | `breach-item selected` | Current breach. |
| Breach icon | `breach-icon` | Breach/company logo, may be empty. |
| Critical marker | `critical-breach-icon` | Marks critical breach items. |
| Breach name | `breach-item-name` | Breach title. |
| Detail pane | `breach-info` | Selected breach details. |

Captured breach list examples:

- Online Trade, selected and critical.
- Dropbox, 500px, Disqus, Synthient Credential Stuffing Threat Data, Combolists Posted to Telegram.
- PaySystem.tech, using Have I Been Pwned logo URL.
- Twitter-like and other non-critical examples in pasted snippets.

## Breach Info Detail

| Part | Source class | Purpose |
|---|---|---|
| Root | `breach-info` | Selected breach detail. |
| Header | `breach-info-header` | Logo, title, optional action. |
| Header icon | `breach-info-header-icon` | Breach logo. |
| Header title | `breach-info-header-title` | Breach name. |
| Resolve action | `button` | `Mark as resolved`, present for some breaches. |
| Content | `breach-info-content` | Date, description, compromised data. |
| Section | `email-info-main-content-section` | Date/description sections. |
| Section title | `email-info-main-content-section-title` | Label plus info icon. |
| Info icon | `section-info` | Help/tooltip affordance. |
| Section text | `email-info-main-content-section-text` | Main text content. |
| Data section | `compromised-data-section` | Critical/non-critical compromised data blocks. |
| Data section title | `compromised-data-section-title` | Section label plus info icon. |
| Critical list | `critical-compromised-data-list` | Critical data with recommended action. |
| Critical item | `critical-compromised-data-item` | Data type and remediation copy. |
| Non-critical list | `non-critical-compromised-data-list` | Non-critical compromised fields. |
| Data title | `compromised_data_title` | Compromised data label. |
| Action link | `action-link` | Inline remediation link, e.g. `Log in`. |

Captured critical data examples:

| Breach | Critical data | Recommended action behavior |
|---|---|---|
| Online Trade | Passwords | Inline `Log in` action link and password-change guidance. |
| PaySystem.tech | Credit cards | Contact card provider, replace card, monitor statements. |

Captured non-critical data examples:

- Dates of birth
- Email addresses
- IP addresses
- Names
- Phone numbers
- Purchases
- Social media profiles
- Usernames

Design requirements:

- Breach detail copy can be long; description sections need readable line length and robust wrapping.
- Critical compromised data should be visually distinct from non-critical data and should support detailed remediation copy.
- `Mark as resolved` is optional by breach/state.
- Some breach icons are empty; the component needs a fallback icon.
- External logos can come from RoboForm icon CDN or Have I Been Pwned logo URLs.
- Inline action links may navigate to affected sites and need external-link behavior.

## Password Reveal Behavior

The table password reveal state was intentionally not captured with real values for security reasons.

Known behavior:

- Header control `toggle-pwd toggle-pwd-hide` toggles the password column.
- Hidden state displays bullets in `c_password`.
- Revealed state replaces bullets with real password plaintext in table cells.

Design/security requirements:

- Do not store real revealed values in design-system documentation, fixtures, screenshots, or examples.
- Treat revealed table passwords as a sensitive transient state.
- Revealed state should be visually and programmatically obvious, with a clear way to hide again.
- Avoid accidental copying or screen persistence in QA fixtures.

## States To Document

| State | Captured | Notes |
|---|---:|---|
| Overview good score | Yes | Score `89`, label `Good`. |
| Distribution chart | Yes | Strong/good/medium/weak/compromised and unique/reused captured. |
| Data Breach Monitoring tab | Yes | Active tab, email breach detail, breach list, selected breach info captured. |
| Compromised tab populated | Yes | Active tab captured, count later captured as `(10)`. |
| Weak tab populated | Yes | Adds `Age, days`. |
| Reused tab populated | Yes | Adds `URL` and `Age, days`. |
| All tab populated | Yes | Adds `Age, days`, includes all strength statuses. |
| Complete Duplicates tab | Yes | Grouped duplicate rows captured. |
| Excluded tab | Yes | Includes `Include in Security Score` command. |
| Passwords hidden | Yes | Bullets in `c_password`. |
| Passwords revealed | Described | Not captured by value for security reasons; behavior documented without sample passwords. |
| Manage columns closed | Yes | Button captured. |
| Manage columns open | Yes | Modal captured. |
| Context menu visible | Yes | Security Center row menu captured. |
| Multiselect hidden | Yes | `rf-multiselect-cmdbar hidden`. |
| Multiselect active | Yes | `rf-multiselect-cmdbar medium`, `rf-multiselect-in-process`, 4 selected rows. |
