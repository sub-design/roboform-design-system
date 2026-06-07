# Account Menu

## Source

Captured from opened account dropdown:

```html
<div class="rf-account-menu rf-fade-in rf-fade-out dropdown-popup shown-popup-menu" style="opacity: 1;">
  <div class="dropdown-popup-item rf-account-theme-item">
    <div class="dropdown-popup-item-image"></div>
    <div class="dropdown-popup-item-text">Dark Mode</div>
    <div class="toggle"><div class="control"></div></div>
  </div>
  <div class="dropdown-popup-item rf-account-settings-item">
    <div class="dropdown-popup-item-image"></div>
    <div class="dropdown-popup-item-text">Settings</div>
  </div>
  <div class="dropdown-popup-item rf-account-admin-center-item">
    <div class="dropdown-popup-item-image"></div>
    <div class="dropdown-popup-item-text">Admin Center</div>
  </div>
  <div class="dropdown-popup-item rf-import-item">
    <div class="dropdown-popup-item-image"></div>
    <div class="dropdown-popup-item-text">Import</div>
  </div>
  <div class="dropdown-popup-separator"></div>
  <div class="dropdown-popup-item rf-account-help-item dropdown-popup-item-with-submenu">
    <div class="dropdown-popup-item-image"></div>
    <div class="dropdown-popup-item-text">Help</div>
    <div class="dropdown-popup dropdown-popup-submenu hidden">...</div>
  </div>
  <div class="dropdown-popup-separator"></div>
  <div class="dropdown-popup-item rf-account-logout-item rf-command-logout">
    <div class="dropdown-popup-item-image"></div>
    <div class="dropdown-popup-item-text">Lock</div>
  </div>
</div>
```

## Component Role

The Account Menu is a workspace-level dropdown opened from the account selector in the app header. It combines account-level preferences, administrative/product entry points, support links, and the lock command.

This is not a generic menu only. It is a high-trust security menu because it contains `Lock`, account/admin navigation, and theme switching.

## Anatomy

| Part | Source class | Purpose |
|---|---|---|
| Menu container | `rf-account-menu dropdown-popup` | Floating menu surface. |
| Open state | `shown-popup-menu`, inline `opacity: 1` | Indicates visible menu. |
| Animation state | `rf-fade-in`, `rf-fade-out` | Entry/exit animation classes. |
| Menu item | `dropdown-popup-item` | Clickable row. |
| Item icon | `dropdown-popup-item-image` | 24px-ish leading icon slot. |
| Item label | `dropdown-popup-item-text` | Visible command label. |
| Separator | `dropdown-popup-separator` | Groups related actions. |
| Submenu item | `dropdown-popup-item-with-submenu` | Parent item for nested popup. |
| Submenu | `dropdown-popup-submenu` | Nested menu, hidden until hover/focus/open. |
| Toggle | `toggle`, `control` | Binary dark mode control. |

## Menu Items

| Item | Source class | Type | Notes |
|---|---|---|---|
| Dark Mode | `rf-account-theme-item` | Toggle row | Contains inline toggle control. |
| Settings | `rf-account-settings-item` | Navigation/action | Account/app settings entry. |
| Admin Center | `rf-account-admin-center-item` | Navigation/action | Enterprise/admin-only item. |
| Import | `rf-import-item` | Navigation/action | Import flow entry. |
| Help | `rf-account-help-item dropdown-popup-item-with-submenu` | Submenu parent | Opens nested support menu. |
| Manual | `rf-account-manual-item` | Submenu action | Documentation/manual link. |
| Help Center | `rf-account-help-center-item` | Submenu action | Support center link. |
| Contact Support | `rf-account-contact-support-item` | Submenu action | Support/contact link. |
| About | `rf-account-about-item` | Submenu action | Product/about dialog. |
| Lock | `rf-account-logout-item rf-command-logout` | Security action | Locks RoboForm. Should be visually distinct enough to scan, but not destructive-danger red. |

## Help Submenu

Captured submenu structure:

```html
<div class="dropdown-popup dropdown-popup-submenu hidden">
  <div class="dropdown-popup-item rf-account-manual-item">
    <div class="dropdown-popup-item-image hidden"></div>
    <div class="dropdown-popup-item-text">Manual</div>
  </div>
  <div class="dropdown-popup-item rf-account-help-center-item">
    <div class="dropdown-popup-item-image hidden"></div>
    <div class="dropdown-popup-item-text">Help Center</div>
  </div>
  <div class="dropdown-popup-separator"></div>
  <div class="dropdown-popup-item rf-account-contact-support-item">
    <div class="dropdown-popup-item-image hidden"></div>
    <div class="dropdown-popup-item-text">Contact Support</div>
  </div>
  <div class="dropdown-popup-separator"></div>
  <div class="dropdown-popup-item rf-account-about-item">
    <div class="dropdown-popup-item-image hidden"></div>
    <div class="dropdown-popup-item-text">About</div>
  </div>
</div>
```

Observed detail: submenu items keep the standard `dropdown-popup-item-image` slot, but each icon slot is marked `hidden`. This means submenu rows are text-only while preserving the same row anatomy as icon-bearing menu items.

Submenu grouping:

| Group | Items |
|---|---|
| Documentation | Manual, Help Center |
| Support | Contact Support |
| Product info | About |

## States To Document

| State | Source signal | Design requirement |
|---|---|---|
| Closed | menu has `hidden` or is absent from visible state | Account selector remains in header. |
| Open | `shown-popup-menu`, `opacity: 1` | Floating surface visible above content. |
| Hover item | CSS hover state needed from stylesheet | Row background changes; icon may switch to hover asset. |
| Keyboard focus | not captured yet | Must be visible and navigate row by row. |
| Disabled item | likely `rf-cmd-disabled` in other menus | Text/icon disabled treatment required. |
| Submenu closed | `dropdown-popup-submenu hidden` | Captured. Parent row shows submenu affordance. |
| Submenu open | need HTML capture | Nested popup visible, aligned to parent row. |
| Dark mode off | `toggle` with no active/on modifier | Control knob off position. |
| Dark mode on | need dark-mode capture | Control knob on position and app root `dark-theme`. |

## Token Mapping

Proposed semantic tokens:

| Use | Token |
|---|---|
| Menu surface | `color.surface.menu` |
| Menu border | `color.border.menu` |
| Menu shadow | `shadow.overlay.menu` |
| Item text | `color.text.primary` |
| Secondary/disabled item text | `color.text.disabled` |
| Item hover background | `color.background.menu.hover` |
| Item active background | `color.background.menu.pressed` |
| Separator | `color.border.separator` |
| Focus ring | `color.border.focus` |
| Toggle active background | `color.action.selected` |
| Toggle off track | `color.background.control` |

Existing source variables likely involved:

- `--surface-surface-*`
- `--menu-hover`
- `--background-neutral-hovered`
- `--background-neutral-pressed`
- `--text-text-primary`
- `--text-text-secondary`
- `--border-border-secondary`

## Behavior Rules

- Clicking outside closes the menu.
- `Esc` closes the menu and returns focus to account selector.
- Arrow keys should move through menu items.
- `Enter`/`Space` activates the focused item.
- Submenu parent should open on hover and keyboard focus/arrow-right.
- `Lock` should not require confirmation; it is reversible by unlocking.
- Admin Center should only be present for accounts with admin/enterprise capability.
- Theme toggle should update the app immediately and persist preference.

## Accessibility Requirements

- Account selector should expose `aria-label="Account menu"` or equivalent.
- Menu container should use menu semantics if implementation allows it.
- Each row must have a keyboard focus state.
- Icon-only meaning must not be required because labels are visible.
- Toggle row needs an accessible checked state for Dark Mode.
- Submenu parent needs `aria-haspopup` / expanded state.

## Open Questions

- What is the exact dark mode toggle on-state class?
- Does `Admin Center` appear only for enterprise accounts?
- Is `Lock` equivalent to logout in all platforms, or only vault lock?
- Does Help submenu open on hover, click, or both?
- Are menu icons 20px or 24px in final rendering?

## Needed Next Capture

Next useful state: Account menu with **Help submenu opened and visible**.

The submenu structure is captured, but the current pasted block still has `hidden`, so we do not yet have the visible/open class state or positioning. Copy the visible account menu HTML again after hovering/clicking `Help`. The useful block starts with:

```html
<div class="rf-account-menu ...
```
