# Login Detail And Editor

## Sources

Captured from full `#v8` dumps after creating and opening a Google login item.

States captured:

- Existing login detail in `view-mode`.
- Existing login editor in `edit-mode`.
- Existing login editor with changed field and enabled Save.
- Unsaved changes confirmation after changed login edit.
- Context commands menu.
- Common success notification: `Login has been created`.
- Common notification variant: `Login removed from Pinned`.
- Inline rename state in detail header.
- Compact login header with empty content body.
- Password strength details panel, hidden by default.
- Password strength details panel opened for strong password.
- Visible security warning for critical breaches.

## Component Role

Login Detail is the read and action surface for a saved credential. Login Editor is the inline edit surface for the same item. They are related, but should be treated as separate states of one credential-detail pattern rather than as part of the New Login creation flow.

The component is security-sensitive because it exposes reveal/copy controls, password strength information, URL actions, and destructive item commands.

## Shared Container

| Part | Source class | Purpose |
|---|---|---|
| Editor shell | `rf-editor` | Right-side or inline credential detail panel. |
| Resize handle | `rf-resize-handle` | Width resizing affordance, captured in edit-mode. |
| Details view | `details-view` | Hosts item-specific detail view. |
| Login view root | `editor-view editor-inline editor-login view-mode` | Read/action state. |
| Login edit root | `editor-view editor-inline editor-login edit-mode` | Inline editing state. |
| Content | `editor-content` | Header plus body. |

Captured layout signal: edit-mode included inline style `width: 630px; left: 661px;`, so the design system should support a resizable detail panel rather than a fixed modal-only editor.

## View Mode Header

| Part | Source class | Purpose |
|---|---|---|
| Header | `content-header` | Top area for logo, title, actions, command icons. |
| Compact login header | `content-header login` | Dense header variant without actionbar/container wrapper. |
| Header container | `header-container` | View-mode header layout. |
| Own background header | `content-header rf-has-own-background rf-light/rf-dark` | Branded/custom item header background. |
| Site logo | `header-image rf-item-imgLogo` | Site/app icon, e.g. Google logo. |
| Small site logo | `header-image rf-item-img32` | 32px favicon-style icon in compact header. |
| Title pane | `header-title-pane` | Holds item title and optional folder. |
| Title | `title-name` | Saved login name, e.g. `Google - 1`. |
| Folder label | `title-folder` | Optional folder path/name; captured visible with `folder-shown`. |
| Rename pane | `inplace-edit-pane`, `inplace-edit-pane hidden` | Inline rename input and OK/cancel buttons, hidden by default. |
| Action bar | `header-actionbar` | Primary credential actions. |
| Command bar | `editor-cmdbar cmdbar-main` | Icon commands: edit, more, close. |

Primary action buttons:

| Action | Source class | Captured label | Tooltip |
|---|---|---|---|
| Log In | `action-button action-login` | `Log In` | `Just click and we will do the rest` |
| Go Fill | `action-button action-gofill` | `Go Fill` | `We will take you to the form page and pre-populate the fields, but not submit` |
| Go To | `action-button action-goto` | `Go To` | `We will take you to the form page` |

Header icon commands:

- `editor-cmd editor-cmd-edit`, title `Edit`.
- `editor-cmd editor-cmd-more`, title `More Actions`.
- `editor-cmd editor-cmd-close`, title `Close`.

Captured branded header variants:

- `content-header rf-has-own-background rf-light`, inline Airbnb background.
- `content-header rf-has-own-background rf-dark`, inline Vercel black background.
- `header-title folder-shown` with `title-folder`, e.g. `Home/ClearNorth`.

### Compact Login Header

Captured source:

```html
<div class="editor-content">
  <div class="content-header login">
    <div class="header-image rf-item-img32"></div>
    <div class="header-title-pane">
      <div class="header-title">
        <div class="title-name">Vercel</div>
      </div>
    </div>
    <div class="editor-cmd editor-cmd-close" title="Close"></div>
  </div>
  <div class="content-data"></div>
</div>
```

State signals:

| Part | Source class | Captured state |
|---|---|---|
| Header | `content-header login` | Compact login-specific header. |
| Logo | `header-image rf-item-img32` | Small favicon/logo treatment. |
| Title | `header-title`, `title-name` | Single title, no folder label in capture. |
| Close | `editor-cmd editor-cmd-close` | Close icon is directly in header, not inside `editor-cmdbar`. |
| Body | `content-data` | Empty body captured. |

Design implications:

- This is not the same header density as the full detail header with `header-container`, `header-actionbar`, and `editor-cmdbar`.
- Empty `content-data` should preserve layout without showing placeholder content unless the product explicitly does so.
- Small-logo and large-logo header variants need shared alignment rules so title baseline and close button do not jump across states.

### Inline Rename State

Captured header source:

```html
<div class="header-title hidden">
  <div class="title-name" title="">Live</div>
  <div class="title-folder hidden"></div>
</div>
<div class="inplace-edit-pane">
  <input type="text" class="inplace-input" spellcheck="false">
  <div class="inplace-ok-btn" tabindex="-1"></div>
  <div class="inplace-cancel-btn" tabindex="-1"></div>
</div>
```

State signals:

| Part | Source class | Captured state |
|---|---|---|
| Static title | `header-title hidden` | Title is kept in DOM but hidden while renaming. |
| Rename pane | `inplace-edit-pane` | Visible when rename is active. |
| Rename input | `inplace-input` | Editable item name field. |
| Confirm | `inplace-ok-btn` | Icon-only commit action. |
| Cancel | `inplace-cancel-btn` | Icon-only cancel action. |

Design implications:

- Inline rename is a header substate, not a modal.
- The logo, Log In / Go Fill / Go To actionbar, and edit/more/close command bar remain visible during rename.
- OK and cancel are icon actions and need focus-visible states even though captured source uses `tabindex="-1"`.

## View Mode Fields

| Part | Source class | Purpose |
|---|---|---|
| Body | `content-data` | Read-mode credential fields. |
| URL field | `login-field field-url` | Site URL/host. |
| Field caption | `field-caption` | Label such as `URL`, `User ID`, `Password`, `Note`. |
| Field value | `field-value` | Value container with optional actions. |
| Value text | `text-value` | Visible field value. |
| Data fields | `data-fields` | Credential data stack. |
| User ID field | `login-field field-id-0` | Username/email. |
| Password wrapper | `secret-field medium password-details-enabled` | Password field plus strength disclosure. |
| Password field | `login-field field-id-1` | Masked password row. |
| TOTP add row | `login-field field-totp-key add-totp` | Add authenticator setup key. |
| Note field | `login-field field-note` | Editable note textarea in detail view. |

Hover/action controls:

| Control | Source class | Purpose |
|---|---|---|
| Expand URL | `onhover-button-expand` | Show full address. |
| Collapse URL | `onhover-button-collapse hidden` | Return to hostname-only view. |
| Copy | `onhover-button-copy`, `visible-copy` | Copy field value. |
| Show password | `onhover-button-show-secrete` | Reveal password. Source typo should not become design API. |
| Hide password | `onhover-button-hide-secrete hidden` | Hide revealed password. |

Design rule: copy and reveal controls are part of the field anatomy, not generic trailing icons. They need keyboard focus, clear accessible names, and behavior rules for sensitive data.

## TOTP And Passkeys

Captured TOTP code state:

| Part | Source class | Purpose |
|---|---|---|
| TOTP row | `login-field field-totp-key` | Authenticator code field. |
| TOTP section | `totp-section show-code half-filled` | Code visible, timer partially elapsed. |
| Code section | `code-section` | Code label and controls. |
| Code title | `title` | `Verification Code`. |
| Code pane | `code-pane` | Code, timer, actions. |
| Code | `code` | Two grouped spans, click to copy. |
| Timer | `totp-timer` | Circular conic-gradient countdown. |
| Buttons | `buttons` | Copy/more actions. |
| Copy action | `cmd-copy icon-button` | Copy verification code. |
| More action | `cmd-more-options icon-button hidden-option` | Hidden more actions. |

Captured passkey state:

| Part | Source class | Purpose |
|---|---|---|
| Passkey wrapper | `passkey-fields` | Passkey section visible. |
| Passkey row | `login-field passkey-field` | One passkey entry. |
| Passkey icon | `passkey-icon`, `passkey-icon-content` | Passkey visual. |
| Passkey info | `passkey-info` | Title and metadata. |
| Passkey title | `passkey-title` | Account/user identifier. |
| Passkey metadata | `passkey-text` | Created date/time. |

Design requirements:

- Do not store real TOTP values in docs/fixtures beyond synthetic examples. Treat the visible code as sensitive transient data.
- Timer state should be visually and programmatically distinguishable as it approaches expiry.
- Passkey rows are credential subrecords and should visually sit below regular login fields.

## Security Warning

Captured visible warning source pattern:

```html
<div class="security-warning">
  <div class="security-warning-pane">
    <div class="title">
      <div class="icon"></div>
      Critical Breaches Detected
    </div>
    <div class="message">
      ...
      <div class="buttons-bar">
        <button class="button cta-btn">View Full Report</button>
      </div>
    </div>
  </div>
</div>
```

State signals:

| Part | Source class | Purpose |
|---|---|---|
| Warning wrapper | `security-warning` | Visible warning block in login detail. |
| Warning pane | `security-warning-pane` | Warning content container. |
| Warning title | `title` | Includes icon and warning title. |
| Warning icon | `icon` | Critical warning visual. |
| Warning message | `message` | Breach explanation copy. |
| CTA area | `buttons-bar` | Action area inside message. |
| CTA button | `button cta-btn` | Opens full report. |

Design requirements:

- Warning content can reference compromised services and account identifiers; fixtures must use synthetic/redacted values.
- The warning is inline in Login Detail below credential/security/TOTP content, not a modal.
- The CTA is a normal button element with `cta-btn`, not an `rf-button`.
- This warning should map to critical/risk semantic tokens and should not rely on color alone.


## Password Strength

Captured medium strength state:

| Part | Source class | Purpose |
|---|---|---|
| Score wrapper | `security-score` | Strength badge and disclosure control. |
| Badge | `security-password-strength medium` | Visible score, title `Password Strength: Medium`. |
| Disclosure trigger | `security-password-expand-handler` | Opens strength details. |
| Details | `password-strength-details medium hidden` | Hidden details panel. |

Details panel content:

- Title: `Improving is easy!`
- Description recommends changing a not-sufficiently-complex password.
- Inline action link: `Log in`.
- Domain token: `google.com`.
- Factors block: `Factors affecting password strength score`.
- Factors: `Reused` and `Complexity`.
- Complexity slider: width `48%`, value `48`.
- Total score: `48/100`.

Design requirements:

- Strength statuses need semantic tokens: weak, medium, good, strong, compromised/reused when applicable.
- Disclosure must not rely on color alone.
- Factor chart is a product-specific score explanation component, not a generic progress bar.
- Hidden explanatory tip exists inside `.tip.hidden`; it needs a hover/focus trigger and max-width rules.

### Strong Details Opened

Captured source:

```html
<div class="secret-field strong password-details-enabled strength-details-shown">
```

Opened state signals:

| Part | Source class/style | Captured value |
|---|---|---|
| Secret wrapper | `secret-field strong password-details-enabled strength-details-shown` | Strong password with details expanded. |
| Strength badge | `security-password-strength strong` | `Strong`, title `Password Strength: Strong`. |
| Details panel | `password-strength-details strong` | Visible, `style="height: auto;"`. |
| Field caption | `field-caption` | Captured custom caption `passwd`, not always `Password`. |
| Details title | `title .text` | `Great job!` |
| Details description | `description` | Password is unique and sufficiently complex; points to Security Center. |
| Complexity slider | `slider-complexity` | Width `98%`. |
| Complexity value | `value-complexity` | `98`, positioned with `right: 0px`. |
| Total score slider | `total-score .slider` | Width `98%`. |
| Total score value | `total-score .value` | `98/100`. |

Design implications:

- Expanded state is controlled by parent class `strength-details-shown`, not only by removing `hidden` from details.
- Strong detail copy is congratulatory and does not include the medium-state inline `Log in` action.
- Field labels are user/custom-field driven; do not hardcode `Password` in the design-system component.
- High-end score values need edge positioning rules because `98` is pinned at the right edge rather than using `left: calc(...)`.

## Edit Mode

### Header and actions

| Part | Source class | Purpose |
|---|---|---|
| Header | `content-header` | Edit header. |
| Title pane | `header-title-pane` | Logo, title, hidden rename pane. |
| Logo | `header-image rf-item-imgLogo` | Site icon. |
| Title | `title-name` | Item name. |
| Command bar | `editor-cmdbar cmdbar-main` | Captured close-only command bar. |
| Close | `editor-cmd editor-cmd-close` | Close editor. |
| Button bar | `buttons-bar` | Edit actions. |
| Cancel | `button` | Cancel changes. |
| Save | `button default-button disabled` | Save changes, disabled when unchanged/invalid. |
| Save enabled | `button default-button` | Save changes after a field has been modified. |
| Loading | `loading-progress hidden` | Async save/progress state, hidden in capture. |

### Editable fields

| Field | Source class | Notes |
|---|---|---|
| Go To URL | `input-with-title editor-field field-gotourl` | Floating-label text input. |
| Match URL | `input-with-title editor-field field-matchurl` | Floating-label text input. |
| User ID | `input-with-title editor-field field-0` inside `field-with-actions` | Has field actions button. |
| Password | `input-with-title editor-field secret-field field-1` inside `field-with-actions` | Password input with reveal/hide hover buttons. |
| Add field | `editor-field add-field-section` | Adds custom field. |
| Passkeys | `passkey-fields hidden` | Hidden section. |
| TOTP key | `editor-field field-totp-key` | Add authenticator setup key. |
| Note | `textarea-with-title editor-field field-note` | Multiline note. |

Trailing field action:

- `cmdbutton cmd-actions` appears after User ID and Password fields.

Design rule: edit-mode uses floating labels from the editor system (`input-with-title`) while New Login uses `field-with-title field-border field-frame`. These should map to one design-system input component with context-specific density and frame variants.

### Save Enabled State

Captured source signals:

```html
<div class="buttons-bar">
  <div class="button">Cancel</div>
  <div class="button default-button">Save</div>
</div>
<div class="loading-progress hidden"></div>
```

State implications:

- Save enabled is represented by removing `disabled` from `button default-button`.
- The loading/progress node remains present as `loading-progress hidden`.
- User observation: no visible loading state appeared after Save in this flow. Treat loading as a hidden implementation slot, not as confirmed visible behavior.
- The changed-field capture included multiline custom fields and secret fields; fixtures must use synthetic values and should not retain live form field content.

### Unsaved Changes Confirmation

Captured when leaving a changed login editor:

```html
<div class="modal-dialog-screen" tabindex="-1">
  <div class="dialog-view dialog-modal">
    <div class="header">Save changes</div>
    <div class="close-button"></div>
    <div class="content">
      <div class="confirmation-text">Login was changed. Do you want to save changes?</div>
      <div class="buttons-bar">
        <div class="button">Don't save</div>
        <div class="button default-button">Save</div>
      </div>
    </div>
  </div>
</div>
```

Design implications:

- This is an unsaved-changes confirmation, not a destructive delete confirmation.
- Primary action is `Save`; secondary action is `Don't save`.
- Source uses generic `dialog-view dialog-modal`, not `rf-message rf-modal-dialog`.
- The close button needs a defined behavior: equivalent to cancel/dismiss, not silently discard.

## Context Commands Menu

Captured hidden menu in saved-login dump and visible menu in Security Center row context:

| Group | Items |
|---|---|
| Security-specific | `Exclude from Security Score` |
| Open/fill | `Log In`, `Go Fill`, `Go To` |
| Copy | `Copy Username`, `Copy Password` |
| Item management | `View`, `Edit`, `Rename`, `Move`, `Clone`, `Add to Pinned`, `Delete` |
| Sharing/export | `Share` / `Sharing Settings`, `Send` |
| History | `Restore` |

Source classes:

- Hidden menu root: `dropdown-popup context-menu-popup commands-menu hidden`.
- Visible menu root: `dropdown-popup context-menu-popup commands-menu shown-popup-menu`.
- Item: `dropdown-popup-item rf-menu-cmd-*`.
- Icon: `dropdown-popup-item-image`.
- Text: `dropdown-popup-item-text`.
- Separator: `dropdown-popup-separator`.

Security-specific captured item:

```html
<div class="dropdown-popup-item rf-menu-cmd-disable-user-data-breach-warning">
  <div class="dropdown-popup-item-text">Exclude from Security Score</div>
</div>
```

Design requirements:

- This is a command menu with grouped actions and sensitive commands.
- Destructive command `Delete` should receive destructive token treatment if product allows.
- Commands may be hidden or disabled depending on item type, permissions, and sharing state.
- More menu from Login Detail and row context menu should share the same menu component and command ordering.
- Security Center can prepend a risk-specific command before the standard login commands.
- Label copy can change by context: the same share command appears as `Share` in one dump and `Sharing Settings` in the Security Center menu.

## Notification

Captured common notification variants:

```html
<div class="rf-notification rf-common-notification" data-id="1" style="bottom: -154px;">
  <div class="rf-notification-msg">Login has been created</div>
  <div class="rf-close-btn"></div>
</div>

<div class="rf-notification rf-common-notification" data-id="2" style="bottom: -151px;">
  <div class="rf-notification-msg">Login removed from Pinned</div>
  <div class="rf-close-btn"></div>
</div>
```

Design requirements:

- Common notification has message text and close button.
- Captured position is offscreen/transitioning via inline negative `bottom`; document shown, entering, visible, and dismissed positions separately when captured.
- Current captures confirm neutral/success-like feedback copy but no explicit severity class.
- Need severity variants: success, info, warning, error.

## States To Document

| State | Captured | Notes |
|---|---:|---|
| View mode default | Yes | Existing saved login detail. |
| Edit mode default | Yes | Existing saved login editor. |
| Save disabled | Yes | `button default-button disabled`. |
| Save enabled | Yes | Changed-field edit captured with `button default-button` and no `disabled`. |
| Loading/save progress | Partial | `loading-progress hidden` only. User confirmed no visible loading after Save in this flow. |
| Unsaved changes confirmation | Yes | `dialog-view dialog-modal`, header `Save changes`, actions `Don't save` and `Save`. |
| URL hostname only | Yes | `accounts.google.com`. |
| URL expanded | No | Need after clicking expand. |
| Password hidden | Yes | Masked bullets in view mode, password input in edit mode. |
| Password revealed | No | Need saved-login reveal state if different from New Login. |
| Password strength medium | Yes | Badge and hidden details. |
| Password strength strong | Yes | Opened strong details captured. |
| Password strength details opened | Yes | `strength-details-shown`, `password-strength-details strong`, `height: auto`. |
| Context commands menu | Yes | Hidden root and visible `shown-popup-menu` captured. Need hover/focus/disabled states. |
| Branded header background | Yes | `rf-has-own-background rf-light/rf-dark`, folder path visible. |
| TOTP visible code | Yes | `totp-section show-code half-filled`; sensitive transient state. |
| Passkey visible | Yes | `passkey-fields` with passkey row. |
| Security warning visible | Yes | `security-warning`, `security-warning-pane`, critical breach title, CTA button captured; sensitive copy redacted in docs. |
| Compact login header | Yes | `content-header login`, `rf-item-img32`, direct close icon, empty `content-data` captured. |
| Rename inline | Yes | `header-title hidden`, visible `inplace-edit-pane`, `inplace-input`, OK/cancel captured. |
| Common notification | Yes | Offscreen/transitioning messages: `Login has been created`, `Login removed from Pinned`. Need visible position and severity variants. |
