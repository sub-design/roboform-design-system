# Create Menu

## Source

Captured opened create menu:

```html
<div class="rf-new-menu">
  <div class="rf-new-button plus-image dropdown-handler shown-popup-menu"
       role="button"
       tabindex="0"
       title="Create new RoboForm file"></div>
  <div class="rf-new-dropdown rf-fade-out dropdown-popup shown-popup-menu">
    <div class="dropdown-popup-item new-group hidden">
      <div class="dropdown-popup-item-image"></div>
      <div class="dropdown-popup-item-text">Group</div>
    </div>
    <div class="dropdown-popup-item new-shared-folder hidden">
      <div class="dropdown-popup-item-image"></div>
      <div class="dropdown-popup-item-text">Shared Folder</div>
    </div>
    <div class="dropdown-popup-item new-folder">
      <div class="dropdown-popup-item-image"></div>
      <div class="dropdown-popup-item-text">Folder</div>
    </div>
    <div class="dropdown-popup-separator"></div>
    <div class="dropdown-popup-item new-identity">
      <div class="dropdown-popup-item-image"></div>
      <div class="dropdown-popup-item-text">Identity</div>
    </div>
    <div class="dropdown-popup-item new-contact">
      <div class="dropdown-popup-item-image"></div>
      <div class="dropdown-popup-item-text">Contact</div>
    </div>
    <div class="dropdown-popup-item new-safenote">
      <div class="dropdown-popup-item-image"></div>
      <div class="dropdown-popup-item-text">Safenote</div>
    </div>
    <div class="dropdown-popup-item new-bookmark">
      <div class="dropdown-popup-item-image"></div>
      <div class="dropdown-popup-item-text">Bookmark</div>
    </div>
    <div class="dropdown-popup-item new-login">
      <div class="dropdown-popup-item-image"></div>
      <div class="dropdown-popup-item-text">Login</div>
    </div>
  </div>
  <div class="rf-modal-screen rf-new-dropdown-screen rf-fade-in rf-fade-out" style="opacity: 1;"></div>
</div>
```

## Component Role

The Create Menu is the primary object-creation entry point for the vault. It combines a floating icon button and a dropdown list of object types.

It should be treated as a product-specific speed menu, not as a generic overflow menu. The order and availability of object types depend on current context and permissions.

## Anatomy

| Part | Source class | Purpose |
|---|---|---|
| Container | `rf-new-menu` | Owns button, dropdown, and modal screen. |
| Trigger | `rf-new-button plus-image dropdown-handler` | Floating create button. |
| Trigger open state | `shown-popup-menu` | Indicates dropdown is open. |
| Dropdown | `rf-new-dropdown dropdown-popup` | Floating list of create actions. |
| Dropdown open state | `shown-popup-menu` | Visible menu surface. |
| Dropdown animation | `rf-fade-out` | Transition class. |
| Menu item | `dropdown-popup-item new-*` | Create action row. |
| Item icon | `dropdown-popup-item-image` | Leading object-type icon. |
| Item text | `dropdown-popup-item-text` | Create action label. |
| Separator | `dropdown-popup-separator` | Groups folder/group actions from item actions. |
| Outside click layer | `rf-modal-screen rf-new-dropdown-screen` | Screen overlay used to close dropdown on outside click. |

## Items

| Item | Source class | Captured visibility | Notes |
|---|---|---:|---|
| Group | `new-group` | hidden | Probably visible in Sharing Center/Admin context. |
| Shared Folder | `new-shared-folder` | hidden | Probably visible when sharing features/context allow it. |
| Folder | `new-folder` | visible | Structural vault object. |
| Identity | `new-identity` | visible | Form-fill identity. |
| Contact | `new-contact` | visible | Identity/contact subtype. |
| Safenote | `new-safenote` | visible | Secure note. |
| Bookmark | `new-bookmark` | visible | URL bookmark. |
| Login | `new-login` | visible | Credential/login item. |

## States

| State | Source signal | Design requirement |
|---|---|---|
| Closed | dropdown hidden/absent, trigger normal | Floating create button remains visible. |
| Open | trigger and dropdown have `shown-popup-menu` | Menu appears above modal screen layer. |
| Hidden item | item has `hidden` | Item should not occupy visual space. |
| Disabled item | source CSS references `rf-cmd-disabled` variants | Disabled item visible but unavailable; needs muted icon/text. |
| Hover item | CSS hover icon variants exist | Row background and icon color/asset change. |
| Outside click | `rf-new-dropdown-screen` visible | Click/tap outside closes dropdown. |
| Keyboard focus | trigger has `role="button"` and `tabindex="0"` | Trigger and menu items need visible focus. |

## Token Mapping

Proposed semantic tokens:

| Use | Token |
|---|---|
| Floating create button background | `color.action.primary` |
| Floating create button hover | `color.action.primary.hover` |
| Floating create button icon | `color.icon.on-action` |
| Menu surface | `color.surface.menu` |
| Menu item text | `color.text.primary` |
| Menu item hover background | `color.background.menu.hover` |
| Menu separator | `color.border.separator` |
| Screen overlay | `color.overlay.transparent-hit-area` |
| Focus ring | `color.border.focus` |

Existing source assets include create icons for each item:

- `cmd-create-group.svg`
- `cmd-create-shared-folder.svg`
- `cmd-create-folder.svg`
- `cmd-create-identity.svg`
- `cmd-create-contact.svg`
- `cmd-create-safenote.svg`
- `cmd-create-bookmark.svg`
- `cmd-create-login.svg`
- disabled and dark variants for several of these.

## Behavior Rules

- Trigger opens the menu and adds outside-click screen layer.
- Clicking the outside layer closes the menu.
- `Esc` closes the menu and returns focus to the create trigger.
- Menu item activation opens the corresponding create flow.
- Hidden items are context-controlled and should not leave gaps.
- Disabled items, when shown, must communicate unavailable state without disappearing.
- The menu should stay anchored to the trigger even when viewport is narrow.

## Accessibility Requirements

- Trigger must have an accessible name. Current `title="Create new RoboForm file"` is useful but should be backed by `aria-label`.
- Trigger is keyboard focusable via `tabindex="0"`.
- Menu items should be reachable by keyboard.
- Hidden items should be removed from the accessibility tree.
- Disabled items should expose disabled state if they remain visible.

## Open Questions

- What contexts show `Group` and `Shared Folder`?
- What class represents disabled create actions in live HTML: `rf-cmd-disabled`, `disabled`, or both?
- Does the trigger title change by current category, such as `Create new Login` versus `Create new RoboForm file`?
- Does the dropdown open upward or sideways near bottom/right viewport edges?

## Needed Next Capture

Next useful state: click `Login` and capture the first New Login dialog/editor.

Copy the visible dialog/editor root. Look for a block starting with one of:

```html
<div class="rf-new-dialog ...
<div class="rf-editor ...
<div class="dialog ...
```
