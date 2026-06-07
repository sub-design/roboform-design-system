# New Login Flow

## Sources

Captured two states:

1. `rf-new-dialog rf-new-login select-account`
   - Initial Add New Login state.
   - Website/Application input.
   - Learn more link.
   - `Create your own` item.
   - Large known-login template list: Google, Amazon, PayPal, Facebook, Microsoft, etc.

2. `rf-new-dialog fill-fields rf-new-login`
   - Google selected.
   - Name field with suggestions dropdown.
   - Email and Password fields.
   - Folder picker with sharing overlays.
   - Pin toggle.
   - Note section.
   - TOTP help prompt.
   - Back/Cancel/Save action bar.

## Flow Role

New Login is a guided credential creation workflow. It supports two creation paths:

- Template-assisted login creation from known sites/apps.
- Manual login creation through `Create your own`.

The flow is security-sensitive because it collects username/email, password, folder placement, pinned state, notes, and optional TOTP setup context.

## State 1: Select Account

### Container

| Part | Source class | Purpose |
|---|---|---|
| Modal screen | `modal-dialog-screen` | Focusable modal backdrop/container. |
| Dialog | `dialog-view rf-new-dialog rf-new-login select-account rf-modal-dialog` | Add New Login dialog in account selection mode. |
| Header | `header` | Dialog title: `Add New Login`. |
| Close button | `close-button` | Dismiss dialog. |
| Body | `body` | Contains search/select content. |

### Account selector

| Part | Source class | Purpose |
|---|---|---|
| Selector pane | `account-selector-pane` | Holds Website/Application input. |
| Field wrapper | `account-selector field-with-title field-border field-frame` | Floating-label input container. |
| Input | `input-field` | Website or Application query. |
| Floating label | `title` | `Website or Application`. |
| Clear action | `clear-text hidden` | Clear input when query exists. |
| Help block | `account-creation-help` | Supporting docs link. |
| Help link | `account-creation-help-link` | `Learn more`, external help article. |

### Known login list

| Part | Source class | Purpose |
|---|---|---|
| List | `accounts-list noselect` | Scrollable/selectable templates. |
| Manual item | `rf-item rf-create-manually` | Starts manual login creation. |
| Known login item | `rf-item rf-known-login` | Template item for a specific service. |
| Logo | `rf-item-image rf-item-imgLogo` | External site/app logo. |
| Default logo | `rf-item-default-image` | Fallback for missing/unsupported logo. |
| Title | `rf-item-title` | Service/template name. |
| Custom background | `rf-has-own-background` + inline `background-color` | Service-branded tile background. |
| Contrast class | `rf-dark` / `rf-light` | Declares whether item needs light/dark foreground treatment. |
| Loading overlay | `rf-loading-overlay hidden` | Async state. |

### Design implications

- Template items are compact selectable cards/rows with optional brand background.
- Contrast is explicitly managed with `rf-dark` and `rf-light`.
- Logo URLs come from `https://etc.roboform.com/icons/...`; design system should support remote logo, missing logo, and custom brand background.
- `Create your own` is structurally an `rf-item`, not a button. It should visually stand apart as an action item.

## State 2: Fill Fields

### Container

| Part | Source class | Purpose |
|---|---|---|
| Dialog | `dialog-view rf-new-dialog fill-fields rf-new-login rf-modal-dialog` | Add New Login dialog in fields mode. |
| Body | `body rf-doc` | Document-like editor layout. |
| Doc header | `rf-doc-header` | Internal header for selected template. |
| Header close | `close-button` | Close from document header. |
| Title | `rf-title` | `Add New Login`. |
| Service image | `rf-doc-image rf-item-image rf-item-imgLogo` | Selected service logo. |

### Fields

| Part | Source class | Purpose |
|---|---|---|
| Fields wrapper | `rf-doc-fields` | Main form area. |
| Instruction | `rf-doc-fields-title` | `Enter your username and password for Google.` |
| Name field wrapper | `field-input-with-dropdown doc-field field-name handler-shown` | Name input with suggestions. |
| Floating field | `field-with-title field-border field-frame` | Shared input frame. |
| Name input | `input-field` | Login name. |
| Dropdown handler | `dropdown-handler-icon` | Opens suggestions. |
| Suggestions popup | `dropdown-popup-list noselect` | Name suggestions and replacement candidates. |
| Disabled suggestion header | `item disabled` | Section label inside popup. |
| Account fields | `rf-new-account-fields` | Credential fields. |
| Email field | `doc-field field-with-title field-border field-0` | Username/email. |
| Password field | `doc-field field-with-title field-border field-frame field-1` | Password input. |
| Password reveal | `eye-icon` | Show password as text. |

### Name suggestions

Captured suggestions:

- Section label: `Name suggestions:`
- Suggested name: `Google - 1`
- Suggested name after filled state: `Google - 2`
- Suggested name with user-entered suffix: `Google (ыывываыва)`
- Section label: `Replace existing Login:`
- Existing login candidates:
  - `Google - 1`
  - `civicmarketplace\Google - alex@civicmarketplace.com`
  - `Google`
  - `Google-soft-machine`

Design requirement: this dropdown mixes disabled section headers and selectable rows. It is not a generic flat autocomplete.

### Folder picker

| Part | Source class | Purpose |
|---|---|---|
| Container | `folders-tree-container` | Folder selection region. |
| Picker | `folders-tree` | Custom folder picker. |
| Picker active/open state | `folders-tree active` | Indicates active picker and visible list. |
| Selected folder | `choosen-folder` | Current value. Source typo should not propagate to design API. |
| Selected text | `text` | `Home`. |
| Folder icon | `folder-icon` | Folder visual. |
| Arrow icon | `arrow-icon` | Opens folder list. |
| Floating label | `title` | `Folder`. |
| List wrapper | `folders-list-wrapper` | Dropdown/panel wrapper. |
| List | `folders-list`, `list` | Folder options. |
| Folder row | `folders-item`, `folder` | Folder option. |
| Indentation | `margin-left` | Tree depth spacing. |
| Empty state marker | `folder-empty-state` | Leaf/no-children marker. |
| Expand arrow | `folder-arrow-icon` | Collapsible folder. |
| Folder name | `folder-name` | Visible folder label. |
| Sharing overlay | `icon-overlay icon-overlay-*` | Sharing state on folder icon. |
| Create folder action | `create-folder-container`, `create-folder-btn` | Inline create folder entry. |

Sharing overlays captured in folder picker:

- `icon-overlay-regular`
- `icon-overlay-received`
- `icon-overlay-manager`
- `icon-overlay-granted`

### Create folder inline state

Captured hidden and visible states:

| Part | Source class | Purpose |
|---|---|---|
| Create section hidden | `create-folder-section hidden` | Inline folder creation mode inactive. |
| Create section visible | `create-folder-section` | Inline folder creation mode active. |
| Folder input | `folder-input` | Input row. |
| Input icon | `folder-input-icon` | Folder icon. |
| Text input wrapper | `input-with-title` | Floating-label input. |
| Text input | `input[type="text"]` | New folder name input. |
| Filled input | `input.has-value` | Keeps label raised when value exists. |
| Input label | `title noselect` | `New Folder Name`. |
| Save action | `create-folder-btn save-btn` | Save new folder. |
| Cancel action | `create-folder-btn cancel-btn` | Cancel create folder. |
| Error | `create-folder-error error-text hidden` | Validation error. |

Design requirement: inline create folder should preserve context inside the folder picker rather than opening a second modal. Save/cancel are icon-only controls and need explicit accessible labels.

### Additional controls

| Part | Source class | Purpose |
|---|---|---|
| Pin toggle | `toggle` with `label` and `control` | Pinned login state. |
| Note container | `field-note shown` | Optional note section. |
| Add note affordance | `show-note`, `image-plus`, `text` | Opens/shows note field. |
| Note field | `field-with-title field-border textarea.input-field` | Multiline note. |
| TOTP prompt | `totp-prompt rf-link` | Help link: `How to add two-factor authentication key?` |

### Action bar

| Part | Source class | Purpose |
|---|---|---|
| Bar | `rf-buttons-bar rf-right-align noselect` | Dialog footer actions. |
| Back | `rf-button non-essential-button rf-back-button` | Returns to select-account state. |
| Flexible spacer | `rf-space flexy` | Pushes final actions right. |
| Cancel | `rf-button` | Dismisses dialog. |
| Save | `rf-button default-button disabled-button rf-save-button` | Primary action; disabled until valid. |
| Save enabled | `rf-button default-button rf-save-button` | Primary action after required values are valid. |

## Component Dependencies

This flow uses or extends these design-system components:

- Modal/Dialog
- Dialog header
- Floating-label input
- Input with clear action
- Input with dropdown
- Password input with reveal
- Autocomplete / suggestions menu
- Known login template item
- Folder picker/tree
- Sharing indicator overlay
- Toggle
- Note field
- Link
- Button/action bar
- Loading overlay

## States To Document

| State | Captured | Notes |
|---|---:|---|
| Select account default | Yes | Initial `select-account` state. |
| Search/filter typed | No | Need query state, clear button visible, filtered list/no results. |
| Known login item default | Yes | Multiple examples. |
| Known login item branded background | Yes | `rf-has-own-background rf-dark/rf-light`. |
| Manual creation item | Yes | `rf-create-manually`. |
| Fill fields selected template | Yes | Google template. |
| Name suggestions dropdown | Yes | `handler-shown` with popup list. |
| Replace existing suggestions | Yes | Shows conflict/replacement candidates. |
| Password hidden | Yes | `type="password"`, eye icon title says show. |
| Password revealed | Yes | `type="text"`, `eye-icon hide-pwd`, title `Hide password`. |
| Folder picker open/list visible | Yes | List visible; active state captured as `folders-tree active`. |
| Create folder inline | Yes | `create-folder-section` visible; input `has-value` captured. |
| Pin off | Yes | Toggle no active modifier. |
| Pin on | No | Need on-state class. |
| Note shown | Yes | `field-note shown`. |
| Note collapsed/default | Yes | `field-note` without `shown` captured. |
| Save disabled | Yes | `disabled-button`. |
| Save enabled | Yes | Save button without `disabled-button`. |
| Validation error | No | Need invalid/empty error state. |
| Duplicate/overwrite confirmation | Yes | Message dialog: `Login 'Google' already exists. Do you want to overwrite it?` |
| Loading | No | Overlay hidden only. |

## Newly Captured State Details

### Save Enabled

Disabled:

```html
<div class="rf-button default-button disabled-button rf-save-button">Save</div>
```

Enabled:

```html
<div class="rf-button default-button rf-save-button">Save</div>
```

Design rule: enabled/disabled must not rely only on color. The disabled state should be programmatically disabled or removed from tab order.

### Password Revealed

Hidden:

```html
<input class="input-field" type="password">
<div class="eye-icon" title="Show password as text"></div>
```

Revealed:

```html
<input class="input-field" type="text">
<div class="eye-icon hide-pwd" title="Hide password"></div>
```

Design rule: `eye-icon` is the reveal action; `hide-pwd` indicates the password is currently visible and the action will hide it.

### Folder Picker Active

```html
<div class="folders-tree active">
```

Design rule: active picker should show focused/open treatment and expose the folder list panel.

### Inline Create Folder

```html
<div class="create-folder-section">
  <div class="folder-input">
    <div class="folder-input-icon"></div>
    <div class="input-with-title">
      <input type="text" class="has-value">
      <label class="title noselect">New Folder Name</label>
    </div>
    <div class="create-folder-btn save-btn"></div>
    <div class="create-folder-btn cancel-btn"></div>
  </div>
  <div class="create-folder-error error-text hidden"></div>
</div>
```

Design rule: this is an inline editor inside a dropdown/tree, with icon-only save/cancel and validation below.

### Duplicate Login Confirmation

When saving a login whose name already exists, RoboForm shows a message dialog:

```html
<div class="dialog-view rf-message rf-modal-dialog">
  <div class="header"></div>
  <div class="close-button"></div>
  <div>
    <div class="rf-body">
      <div class="rf-text">
        <div>Login 'Google' already exists.</div>
        <div>Do you want to overwrite it?</div>
      </div>
    </div>
    <div class="rf-buttons-bar">
      <div class="rf-button">No</div>
      <div class="rf-button default-button">Yes</div>
    </div>
  </div>
</div>
```

Design rule: overwrite is a confirmation dialog, not inline validation. The default action is `Yes` in the current source, but this should be reviewed because overwriting an existing credential is a high-impact action. A safer system default may be `No` unless product behavior intentionally optimizes replacement.

### Manual Create Your Own

The manual path uses the same fill-fields structure as template-assisted login creation, but with empty fields and without the selected site logo. This should be documented as a variant, not a separate flow.

Design rule: manual creation should not depend on site-specific instruction text or favicon/logo. It needs a neutral icon or no icon state, and field labels must carry the workflow.

## Token Mapping

Proposed tokens:

| Use | Token |
|---|---|
| Modal backdrop | `color.overlay.modal` |
| Dialog surface | `color.surface.dialog` |
| Dialog title | `color.text.primary` |
| Dialog close hover | `color.background.neutral.hover` |
| Input background | `color.input.background` |
| Input border | `color.input.border` |
| Input focused border | `color.input.border.focus` |
| Input error border | `color.input.border.error` |
| Field label | `color.input.label` |
| Dropdown surface | `color.surface.menu` |
| Dropdown item hover | `color.background.menu.hover` |
| Disabled suggestion text | `color.text.disabled` |
| Primary action | `color.action.primary` |
| Primary action disabled | `color.action.primary.disabled` |
| Link | `color.link.default` |
| Branded item foreground dark-bg | `color.text.inverse` |
| Branded item foreground light-bg | `color.text.primary` |

## Accessibility Requirements

- Modal must trap focus while open.
- Close buttons need accessible labels.
- Website/Application, Name, Email, Password, Folder, and Note fields need programmatic labels.
- Suggestions dropdown should expose listbox/option semantics if technically feasible.
- Disabled suggestion section headers should not be announced as selectable options.
- Password reveal button needs checked/pressed state and an accessible name that changes between show/hide.
- Folder picker needs keyboard navigation, expand/collapse semantics, selected state, and clear focus treatment.
- Save disabled state must be programmatically disabled, not only visually disabled.

## Product Decisions To Make

- Whether `Learn more` remains visible in the initial dialog or moves to contextual help.
- Whether branded known-login tiles should keep custom backgrounds in the future design, or normalize to neutral cards with logo only.
- Whether replacement candidates should be visually warning-coded because selecting them may overwrite or modify an existing login.
- Whether notes are expanded by default, as captured, or collapsed until `Add Note`.
- Whether TOTP setup belongs inside new login creation or should appear after save.

## Needed Next Captures

High value next states:

1. Pin on-state.
2. Create folder validation error, for example empty or duplicate folder name, if reachable.
3. Existing Login editor/detail after a saved item.
4. Password Generator random password state.
