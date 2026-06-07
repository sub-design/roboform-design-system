# Foundations Draft

This is a draft foundation layer. It records observed rules and token directions, but it is not the final design-system token API.

> **Three-layer foundation:** this file keeps the *rules and directions*;
> [19-foundations-tokens.md](19-foundations-tokens.md) holds the *measured evidence* (real
> light/dark pairs, type/radius/spacing/shadow/motion); and
> [40-tokens-canonical.md](40-tokens-canonical.md) proposes the *canonical token API*
> (source→DS name map, newly named literals, naming-defect fixes, and the blocked legacy
> values).

## Draft Status

| Foundation area | Status | Notes |
|---|---|---|
| Color | Extracted | 80 light/dark token pairs measured; see [19-foundations-tokens.md](19-foundations-tokens.md) §1. Brand blue `#2979ff`/`#448aff`. |
| Typography | Extracted | Family `Segoe UI…`; default size 15px, default weight 600; full scale in §2. |
| Spacing / Density | Extracted | Steps 4/6/8/16/24/32/40 measured; proposed scale in §4. |
| Radius | Extracted | Default 6px; scale 3/4/6/8/12 + circular in §3. |
| Shadow / Elevation | Extracted | Menu/dialog/card shadow layers measured in §5. |
| Iconography | Draft | Large source icon families identified; product action taxonomy started. Sizes in §3 of draft below. |
| Motion | Extracted | Durations .15/.17/.3/.5/.8s; default .3s in §6. |
| Sensitive Data | Draft | Security rules must shape reveal/copy/toast/fixtures. |
| Accessibility | Seed | Need keyboard/focus evidence and final contracts. |
| Legacy color values | Blocked | `common-ui.css` references legacy vars but their definitions are not in this export; see §8. |

## Color Token Directions

Do not finalize names yet. Current observed groups:

### Surfaces

- app background
- sidebar background
- header background
- editor panel background
- dialog surface
- dropdown/menu surface
- table header/background
- hover/selected row background
- custom item background with `rf-has-own-background`

### Text

- primary text
- secondary text
- muted/placeholder text
- inverse text for custom/dark backgrounds
- disabled text
- destructive text
- link text

### Borders

- default border
- subtle divider
- input border
- focused border
- error border
- table column divider

### Actions

- primary action
- secondary action
- icon action default/hover/active
- disabled action
- destructive action

### Risk / Security

- password weak
- password medium
- password good
- password strong
- compromised/critical
- warning
- success
- info
- error

### Sharing / Permissions

- shared none
- shared group regular
- shared group manager
- shared folder regular
- shared folder manager
- shared received
- login-only permission
- pending recipient

## Typography Directions

Observed needs:

- compact table cell text
- navigation/sidebar labels
- dialog title
- dialog body copy
- field captions
- field values
- badge text
- tooltip text
- empty-state text
- generated password text, likely monospace or tabular treatment
- floating labels in identity/login editor fields
- compact action-pane titles such as `Edit Business`
- settings sheet titles and explanatory row copy

Rules to preserve:

- Avoid hero-scale type inside operational surfaces.
- Long item titles need truncation with title tooltips.
- Table headers and command labels need stable height and no wrapping unless intentionally multiline.

## Spacing And Density Directions

Observed layout patterns:

- editor panel width often `630px`
- identity groups pane captured at `200px`
- compact command bars use icon-only buttons
- tables are dense and scanning-oriented
- menus use grouped sections with separators
- dialogs use compact but readable body/button spacing
- identity edit-mode uses `field-group` to place related inputs/selects on one row
- identity edit-mode uses `field-section-separator` for larger logical breaks inside a single form
- note fields can switch between visible `textarea-with-title` and compact `show-note` add prompt
- settings sheets use explanatory rows where title/hint/control alignment matters more than card separation
- settings radio groups and toggles need compact but readable vertical spacing

Draft scale candidates to validate from CSS:

- 4px micro-gap
- 8px row/control gap
- 12px compact content padding
- 16px dialog/content padding
- 24px section gap

## Icon Size Directions

Observed icon/button sizes vary by context:

- 20px small inline action
- 24px common command icon
- 28-34px editor/header command
- 48px generator/create/action button
- 60px large empty-state or create affordance

Icon rules:

- Use product-specific icons for item types and commands.
- Prefer semantic action mapping over raw filename names.
- Icon-only actions need accessible names and focus-visible states.
- Branded import provider icons need fallback behavior and should not be treated as generic command icons.

## Radius And Elevation Directions

Current direction:

- Keep operational UI low-radius.
- Avoid nested cards.
- Menus, dialogs, popups, and toasts need consistent elevation layers.

Draft elevation layers:

- layer 0: app surface
- layer 1: sidebar/header/editor panel
- layer 2: dropdown/menu
- layer 3: modal dialog
- layer 4: toast/tooltip

## Motion Directions

Observed classes:

- `rf-fade-in`
- `rf-fade-out`
- progress runner classes
- toast bottom position transitions
- editor `loading-progress hidden` slot

Draft rules:

- Menu/dialog motion should be short and functional.
- Toast enter/exit must not block pointer use.
- Progress/loading should avoid layout shift.
- Hidden progress nodes are not enough evidence for a visible loading state; only document visible loading when captured or explicitly confirmed.

## Sensitive Data Rules

These are design-system rules, not optional documentation notes.

- Never store real passwords, TOTP codes, card numbers, CVV, PIN, passport numbers, bank accounts, phone numbers, addresses, VINs, or birth dates in fixtures.
- Use synthetic/redacted examples only.
- Revealed secret states should be explicitly modeled but value content should be redacted in docs.
- Copy feedback should not echo full sensitive values.
- Password/TOTP reveal states should have clear hide controls and possible timeout rules.
- Sensitive table columns need strong masking/reveal contracts.
- Identity edit-mode contains direct editable PII/financial/identity values, so fixtures must be synthetic even for non-secret-looking fields.
- Devices & Activity fixtures must use synthetic timestamps, locations, IP addresses, and device names.
- License/Billing fixtures must avoid embedding real company logos or base64 image payloads.
- Backup list fixtures should use synthetic dates, relative times, and storage labels when examples are needed.

## Accessibility Draft Rules

To validate:

- All icon-only commands need accessible names.
- Hover-only copy controls need keyboard reachability.
- Menus need arrow-key and escape behavior.
- Dialogs need focus trap and return focus.
- Toasts need non-blocking announcement behavior.
- Password strength and risk states must not rely on color alone.
- Table sorting state needs screen-reader-readable state.

## Dialog Behavior Draft Rules

Observed confirmation types:

- overwrite existing login
- unsaved changes: save vs do not save
- sharing stop/delete confirmations

Draft rules:

- Unsaved-changes dialogs are not destructive delete dialogs, but `Don't save` is discard-adjacent and needs clear copy.
- Primary action should match the safest/continuing task action when product copy does so, e.g. `Save`.
- Close button behavior must be explicit for every dialog type: dismiss, cancel, or equivalent action.
- Destructive confirmations should not rely solely on source `default-button`; intent must be mapped semantically.

## Theme Directions

Observed:

- `light-theme` / `dark-theme` on `#v8`.
- `rf-dark` / `rf-light` per item/custom background.
- `rf-has-own-background` for branded item headers.

Rules:

- Theme tokens should be global, but per-item custom backgrounds need local contrast tokens.
- Do not infer all dark behavior from one global dark class; custom item backgrounds have their own contrast logic.
