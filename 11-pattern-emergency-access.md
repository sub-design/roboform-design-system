# Emergency Access

## Sources

Captured from full `#v8` dumps with Emergency Access active.

Captured states:

- `My Emergency Contacts` tab active, empty.
- `I'm Emergency Contact For` tab active, empty.
- Invite New Contact action visible in `My Emergency Contacts`.
- Invite New Contact action hidden in `I'm Emergency Contact For`.
- Invite New Contact dialog.

## Component Role

Emergency Access is an account-recovery and trusted-contact workflow. It lets the user invite trusted contacts who may obtain access under specific conditions, and lets the user accept/manage invitations where they are the emergency contact for someone else.

This surface is security-sensitive and should use clear, calm explanatory copy. Empty states are not decorative; they explain the trust model and next action.

## Container

| Part | Source class | Purpose |
|---|---|---|
| Root | `rf-emergency-access` | Emergency Access surface. |
| Header | `rf-data-view-header` | Contains tab selector. |
| Tab selector | `emergency-access-order-selector order-selector noselect` | Switches between outgoing and incoming access relationships. |
| My contacts tab | `selector-item emergency-access-my-contacts` | Trusted contacts invited by current user. |
| I'm contact for tab | `selector-item emergency-access-im-contact-for` | Invitations sent to current user. |
| Active tab | `active` | Current tab. |
| Count badge | `rf-bandge-count hidden` | Hidden count marker inside tab. Source typo should not become API spelling. |
| Progress separator | `separator rf-progress` | Progress/transition bar area. |
| Progress runner | `rf-progress-runner hidden` | Hidden progress indicator. |
| Data area | `rf-ea-data` | Table plus empty state. |
| Accounts table | `rf-ea-accounts` | Empty in captures. |
| Empty state | `rf-ea-no-accounts` | Empty state content. |
| Empty icon | `rf-ea-no-accounts-icon` | Empty-state icon. |
| Empty copy | `rf-ea-no-accounts-text` | Contextual explanatory text. |
| Empty action | `rf-no-items-btn btn-secondary` | Inline action button, captured only for My Contacts. |
| Floating invite | `rf-ea-new-contact` | Floating create/invite button. |
| Floating plus | `rf-new-button plus-image` | Plus icon inside invite button. |

## Empty States

### My Emergency Contacts

Active state:

```html
<div class="selector-item emergency-access-my-contacts active">
```

Captured copy:

```text
Select a trusted contact to securely obtain access to your RoboForm Data in the event of death, incapacitation, or simply as a method of account recovery.
```

Actions:

- Inline `Invite New Contact` button: `rf-no-items-btn btn-secondary`.
- Floating `rf-ea-new-contact`, title `Invite New Contact`.

Design requirements:

- This state has a primary next step: invite a trusted contact.
- The inline empty-state button and floating plus should not compete; one can be primary depending on layout.
- Copy needs enough line length for serious account-recovery explanation.

### I'm Emergency Contact For

Active state:

```html
<div class="selector-item emergency-access-im-contact-for active">
```

Captured copy:

```text
All Emergency Contact invitations sent to you will be listed here. Once you accept, the timeout periods set by the Grantor will be shown and you will be able to request access at any time.
```

Actions:

- No inline action captured.
- Floating `rf-ea-new-contact hidden`.

Design requirements:

- This state is passive/informational; the user waits for invitations.
- Do not show invite affordances in the incoming-invitation tab.
- The phrase `Grantor` is product-specific and should remain clear in localized copy.

## States To Document

## Invite New Contact Dialog

Captured root:

```html
<div class="dialog-view rf-ea-dialog rf-ea-new-contact-dialog rf-modal-dialog">
```

| Part | Source class | Purpose |
|---|---|---|
| Dialog | `dialog-view rf-ea-dialog rf-ea-new-contact-dialog rf-modal-dialog` | Invite trusted emergency contact. |
| Header | `header` | `Invite New Contact`. |
| Close | `close-button` | Dismiss dialog. |
| Body | `rf-body` | Dialog content and actions. |
| Description | `rf-ea-description` | Explains trusted contact access. |
| Email field | `field-with-title field-border` | Floating-label email input. |
| Email input | `input-field`, `required`, `spellcheck="false"` | Contact email address. |
| Field label | `title` | `Email`. |
| Error area | `rf-field-error` | Email validation error container. |
| Timeout wrapper | `select-wrapper` | Timeout selection. |
| Select | `custom-select` | Combobox trigger. |
| Select title | `select-title unselectable` | Label and selected value. |
| Selected option | `select-option selected` | Captured `2 days`. |
| Action bar | `rf-buttons-bar` | Cancel/invite buttons. |
| Cancel | `rf-button rf-cancel-btn` | Cancel invite. |
| Invite | `rf-button rf-ok-button default-button disabled-button` | Disabled until valid. |

Accessibility details from source:

- Timeout select has `role="combobox"`.
- `aria-haspopup="true"`.
- `aria-expanded="false"`.
- `aria-controls` points to a listbox id.

Design requirements:

- The description is part of the trust/legal context and should not be hidden behind help text.
- Timeout selection is required to communicate delay before access can be obtained.
- Invite stays disabled until required email/validation conditions are satisfied.
- Need opened timeout menu capture for available timeout options.

| State | Captured | Notes |
|---|---:|---|
| My Emergency Contacts empty | Yes | Empty state plus invite actions captured. |
| I'm Emergency Contact For empty | Yes | Empty state captured, invite action hidden. |
| Populated My Emergency Contacts table | No | Need rows with statuses/timeouts. |
| Populated I'm Emergency Contact For table | No | Need incoming invitations/access rows. |
| Invite New Contact dialog | Yes | Email, timeout select, disabled Invite captured. |
| Timeout select opened | No | Need option list. |
| Accept/deny/request/grant flows | No | Need action dialogs/states. |
