# RoboForm Web App Design System: Source-Led Plan

## 1. Source strategy

Primary source should be the actual extension/web app code, not screenshots. Screenshots are still useful for visual QA, but the source files reveal the real implementation model: classes, states, tokens, dimensions, responsive rules, icon inventory, and product-specific UI patterns.

Current inspected files:

- `/Users/aleksandrsubbotin/Downloads/start-page-v8.css`
- `/Users/aleksandrsubbotin/Downloads/start-page.css`
- `/Users/aleksandrsubbotin/Downloads/common-ui.css`
- `/Users/aleksandrsubbotin/Downloads/start-page.js`
- `/Users/aleksandrsubbotin/Downloads/1.js`
- `/Users/aleksandrsubbotin/Downloads/content.js`
- `/Users/aleksandrsubbotin/Downloads/inject.js`
- `/Users/aleksandrsubbotin/.codex/attachments/2af2055c-f338-49a9-ada2-e7d6c1fc8c33/pasted-text.txt`

Several CSS files listed in the attachment are empty or near-empty in this export: `commands-ui.css`, `controls.css`, `dialog-view.css`, `user-consent-page.css`. That does not mean the real product lacks those layers; it means this export should be treated as partial.

## 2. What the source already tells us

The app is not a simple marketing web page. It is a dense productivity/security application with an extension-like shell. The main visible page is loaded through `start-page.html#start-page` inside a Safari extension iframe, and the captured DOM uses a `#v8` root with `light-theme` / `dark-theme` variants.

Important UI surfaces found in the source:

- Main app shell: `rf-main-header`, `main-frame`, `rf-navigator`, `rf-view-frame`
- Search: `rf-search`, `rf-search-box`, `rf-search-dropdown`, match mode checkboxes, type selector
- Account menu: dark mode toggle, settings, import, help submenu, lock
- Navigation: folders, pinned, all, logins, bookmarks, safenotes, identities, password generator, authenticator, security center, sharing center, emergency access
- Vault data views: grid, condensed, list, sortable sections, virtualized list
- Item cards/rows: login, bookmark, safenote, folder, identity, sharing overlays, multiselect, warning states, hover actions
- Creation menu: group, shared folder, folder, identity, contact, safenote, bookmark, login
- Password generator: random password/passphrase tabs, generated password panel, strength scoring, copy/generate actions, length controls, toggles
- Security center: tabs for compromised, weak, reused, all, duplicates, excluded; table layouts with sortable headers and password reveal controls
- Emergency access: contact tables, invite/access statuses, pending action controls
- Editor/modal system: modal screen, notifications, contextual dropdowns, command bars
- TOTP/authenticator surface: code display, timer, copy/more actions, add-key dialog

## 3. Existing token model

There are two token generations in the source.

Legacy/common tokens from `common-ui.css`:

- Buttons: `--regular-button-*`, `--important-button-*`, `--low-emphasis-button-*`, `--white-button-*`
- Inputs: `--edit-*`
- Lists: `--list-item-*`, `--items-counter-*`
- Text: `--main-text-color`, `--hint-text-color`, `--error-text-color`
- Toggle/progress: `--toggle-*`, `--progress-*`
- Password strength: `--password-weak-strength-color`, `--password-medium-strength-color`, `--password-good-strength-color`, `--password-strong-strength-color`

Newer v8 semantic tokens from `start-page-v8.css`:

- Surfaces: `--surface-surface-*`
- Text: `--text-text-primary`, `--text-text-secondary`, `--text-text-error`, `--text-text-on-color`, `--text-text-selected`
- Backgrounds: `--background-background-primary`, `--background-background-brand`, `--background-neutral-hovered`, `--background-neutral-pressed`, `--background-background-destructive`
- Borders: `--border-border-primary`, `--border-border-secondary`, `--border-border-focused`
- Inputs: `--input-*`
- Links: `--link-link`, `--text-link-upgrade`
- Icons: `--icon-icon-quinary`, `--icon-icon-tetriary`
- RAG/security ratings: `--rag-rating-rag-weak`, `--rag-rating-rag-medium`, `--rag-rating-rag-good`, `--rag-rating-rag-strong`
- Custom item palettes: green, blue, red, light-green, light-purple, yellow, plus transparent variants

Design-system direction: treat v8 tokens as the future semantic layer and map legacy tokens into it, instead of preserving both as equal public APIs.

## 4. Product taxonomy for the design system

The component library should be organized around RoboForm product surfaces, not only atomic UI.

Foundations:

- Color
- Typography
- Spacing
- Radius
- Elevation/shadow
- Icon sizes
- Focus and keyboard states
- Motion/duration
- Z-index/layering
- Light/dark theme mapping

Core primitives:

- Button
- Icon button
- Link button
- Text input
- Password input
- Select/custom select
- Checkbox
- Radio-like checkbox
- Toggle
- Tabs/segmented selector
- Badge/count badge
- Avatar/initials
- Tooltip
- Progress/loading indicator
- Divider/separator
- Scrollbar

Shell and navigation:

- App header
- Search box and advanced search dropdown
- Account selector/menu
- Sidebar navigator
- Collapsed sidebar
- Folder tree
- Category selector
- Breadcrumbs
- View style selector
- Command bar
- Floating create button/menu

Vault components:

- Vault item card
- Vault item row
- Login item
- Bookmark item
- Safenote item
- Folder item
- Identity/contact item
- Identity detail/editor with group navigator and instance fields
- Shared item overlay
- Security warning mark
- Multiselect control
- Hover action overlay
- More actions menu
- Empty state

Security components:

- Password strength indicator
- Security score/RAG rating
- Compromised/weak/reused status
- Security center tab selector
- Sortable security table
- Password reveal/hide column control
- Manage columns control
- Excluded-from-score state

Credential workflows:

- New login flow
- Folder selector
- Matching dropdown
- Password reveal field
- TOTP code panel
- Add TOTP key dialog
- Copy-to-clipboard feedback
- Duplicate/same-password warnings

Sharing and access:

- Sharing state overlay
- Permission role indicators: limited, regular, manager, granted, received, login-only
- Shared folder/group/file sections
- Emergency contact table
- Invite/request/access-granted/rejected statuses
- Pending action accept/deny controls

Feedback and overlays:

- Modal screen
- Dialog titlebar/body/actions
- Dropdown popup
- Context menu
- Notification/toast
- Inline validation
- Global loading screen
- Progress runner

## 5. Documentation structure

Recommended Figma/documentation pages:

1. `00 Overview`
2. `01 Foundations`
3. `02 Tokens`
4. `03 Core Components`
5. `04 App Shell`
6. `05 Vault Patterns`
7. `06 Security Center`
8. `07 Password Generator`
9. `08 Sharing & Emergency Access`
10. `09 Dialogs & Feedback`
11. `10 Responsive Behavior`
12. `11 Accessibility`
13. `12 Engineering Handoff`
14. `Archive / Legacy Mapping`

## 6. Source audit work plan

Phase 1: inventory

- Extract all CSS custom properties.
- Group variables into semantic categories.
- Extract all `rf-*`, `dropdown-*`, `selector-*`, `table-*`, `button`, `input`, and state classes.
- Build a component/state matrix from class names and DOM structure.
- Identify icon assets referenced by CSS.
- Identify hard-coded values that should become tokens.

Phase 2: normalize

- Define canonical token names.
- Map existing v8 variables into canonical tokens.
- Map legacy variables into canonical tokens.
- Flag duplicate or conflicting concepts.
- Decide which tokens are public design-system API and which remain implementation-private.

Phase 3: component specs

- Start with high-frequency primitives: Button, IconButton, Input, Select, Checkbox, Toggle, Tabs, Dropdown, Tooltip.
- Then app shell: Header, Search, Sidebar, FolderTree, AccountMenu.
- Then product patterns: VaultItem, PasswordGeneratorPanel, SecurityTable, SharingIndicator, EmergencyAccessRow.
- For each component: anatomy, variants, states, token usage, behavior, accessibility, examples.

Phase 4: screen templates

- Main vault grid
- Main vault list
- Search results
- Password generator
- Security center overview/table
- Sharing center
- Emergency access
- New login dialog
- Settings/import/account flows
- Settings sheets and Devices & Activity
- Import provider picker

Phase 5: implementation handoff

- Token JSON/CSS variable export.
- Component API proposals.
- Legacy CSS migration map.
- Storybook or equivalent component examples.
- Visual regression examples for light/dark themes.

## 7. Immediate next steps

Use the working control layer before adding more final specs:

- `14-work-plan-and-status.md` keeps area status, sprint order, evidence thresholds, and next best captures.
- `15-component-candidates.md` tracks repeated UI parts before they become final design-system APIs.
- `16-foundations-draft.md` records token and foundation observations without prematurely freezing names.

Current operating loop:

1. Add each new HTML dump to the relevant pattern spec.
2. Update `02-component-state-matrix.md`.
3. Update `03-html-screen-request-backlog.md`.
4. Promote repeated UI parts to `15-component-candidates.md` only when there is enough source evidence.
5. Add token/foundation observations to `16-foundations-draft.md` as draft evidence, not final naming.
6. Use `14-work-plan-and-status.md` to choose the next best capture and avoid collecting low-value states.
