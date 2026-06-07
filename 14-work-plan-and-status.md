# Work Plan And Status

This is the operating plan for building the RoboForm web app design system without falling into an unbounded capture/archive exercise.

## Operating Model

Work moves through four artifact layers:

1. **Source Inventory**
   - Raw evidence from HTML/CSS/assets.
   - Source classes, state classes, token names, icon families, screenshots when useful.

2. **Pattern Specs**
   - Product-level surfaces and workflows.
   - Examples: Login Detail, Identity Detail, Security Center, Sharing, Emergency Access.

3. **Component Candidates**
   - Repeated UI parts that are not final design-system APIs yet.
   - Candidates collect evidence and missing states.

4. **Design System Specs**
   - Finalized foundations/components with API, variants, states, behavior, accessibility, and migration mapping.

Rule: do not jump from one HTML dump directly to a final component unless it meets the evidence threshold.

## Status Model

| Status | Meaning |
|---|---|
| `Not started` | No useful source evidence yet. |
| `Captured` | HTML/source exists. |
| `Mapped` | Anatomy, source classes, and major states are documented. |
| `State-complete` | Main default/hover/focus/disabled/loading/error/empty states are covered where relevant. |
| `Componentized` | Repeated parts have been promoted to component candidates. |
| `Ready for DS` | Stable enough to write final design-system spec/API. |
| `Blocked` | Missing source state is known and blocks progress. |

## Evidence Threshold

A component can move from candidate to final design-system spec when at least one of these is true:

- It appears in 3+ product surfaces.
- It appears in 2 product surfaces and is a core primitive, such as Button, Input, Menu, Dialog, Toast.
- Its state coverage is sufficient for implementation: default, hover/focus, active, disabled, loading/error/empty when applicable.
- Light/dark behavior is understood.
- Responsive/overflow behavior is understood.
- Sensitive-data behavior is understood where applicable.

Until then, keep it in `15-component-candidates.md`.

## Per-Dump Processing Pipeline

For each pasted HTML dump:

1. Identify surface and pattern.
2. Extract root classes, state classes, and command/action classes.
3. Avoid storing real sensitive values in docs or fixtures.
4. Update the relevant pattern spec.
5. Update `02-component-state-matrix.md`.
6. Update `03-html-screen-request-backlog.md`.
7. If repeated UI evidence is confirmed, update `15-component-candidates.md`.
8. If token/foundation evidence is confirmed, update `16-foundations-draft.md`.

## Area Status

| Area | Current status | Evidence | Main gaps | Next best capture |
|---|---|---|---|---|
| App Shell / Navigation | Captured | Header, account shell, folder tree, categories, collapsed context in full dumps | clean default grid, collapsed hover, search results | Main vault grid clean default |
| Account Menu | Mapped | Account menu, dark toggle off, admin/settings/import/help shell | Help submenu open, dark mode on | Help submenu opened |
| Create Menu | Mapped | Create menu open, hidden group/shared-folder items, overlay | disabled/permission variants | disabled create item state |
| New Login Flow | Mapped | select account, fill fields, known-login templates, manual copy, folder picker, create folder, duplicate confirmation | validation/error, loading/save status, pin on | validation or save/loading if reachable |
| Login Detail / Editor | Mapped | view, edit, changed-field Save enabled, hidden loading slot, unsaved changes confirmation, compact header, inline rename, context menu, strength details, visible critical warning, passkey, TOTP, notification variants | password revealed source classes, visible validation/error if reachable | password revealed with value redacted |
| Identity Detail | Mapped | view-mode, edit-mode, group navigator, repeated sections, no-information rows, instance commands, field display and editable field sets | visible inline rename, visible warning/no-fields, group menu, save enabled/loading, validation | Identity save enabled/loading or group menu |
| Vault List / Actions | Mapped | list rows, columns, icon popup menu, clone dialog | grid clean default, hover/focus/selected variants, sorting/empty | Main vault grid or selected row states |
| Password Generator | Mapped | random password, passphrase, settings, score panel | copied feedback, weak/medium/generated variants, history if any | Copy feedback after copy |
| Security Center | Mapped | summary, tabs, tables, manage columns, multiselect, context menu, breach detail | active sorting, table loading/empty, password revealed source classes | table sort state or empty/loading |
| Sharing Center | Mapped | shared with/by me, settings dialogs, recipients, permission menu, destructive confirmations | Remove Me confirmation, disabled permissions, errors | Remove Me confirmation |
| Emergency Access | Captured | empty states, invite dialog, timeout select closed | populated rows, timeout menu, request/accept/grant states | populated My Emergency Contacts |
| Settings / Import / Locked | Captured | Settings sheets captured: AutoFill, AutoSave, Advanced, License & Billing, Devices & Activity Activities tab, Backups list, settings information notification; Import provider picker and Chrome instruction page captured | settings landing/navigation if separate, Devices tab, opened settings selects, expanded backup row, import app/CSV next step, import progress/result, locked/start/login | Devices tab, expanded backup row, or import result/progress |
| Common Toast / Notification | Captured | offscreen create/unpin messages | visible position, dismissed, warning/error variants | visible warning/error toast |

## Sprint Plan

### Sprint A: Vault + Login

Goal: close the core password-manager loop.

Priority captures:

- Password revealed source classes with value redacted.
- Main vault grid clean default and item hover/selected.

Exit criteria:

- Login Detail/Editor can become `State-complete`.
- Button/IconButton/Menu/Toast/Copyable Field candidates have enough evidence to start final specs.

### Sprint B: Identity

Goal: close the multi-instance identity editor.

Priority captures:

- Identity group menu.
- Visible identity inline rename.
- Visible `security-warning` or `instance-no-fields` if reachable.
- Identity save enabled/loading after changing a field.

Exit criteria:

- Identity Detail can become `State-complete`.
- Identity Group Navigator and Field Display can become component candidates with clear state model.

### Sprint C: Security Center

Goal: close table/risk workflows.

Priority captures:

- Active sorted column state.
- Table empty/loading.
- Password revealed source classes with values redacted.
- Resolved/empty Data Breach Monitoring.

Exit criteria:

- Table, Manage Columns, Security Badge, Risk Detail can be promoted.

### Sprint D: Sharing + Emergency Access

Goal: close permission and access-management workflows.

Priority captures:

- Remove Me confirmation.
- Disabled permission options.
- Emergency Access populated rows.
- Timeout dropdown opened.

Exit criteria:

- Permission Menu, Recipient Row, Emergency Status, Destructive Confirmation can be promoted.

### Sprint E: Settings / Import / Auth

Goal: cover system/account surfaces.

Priority captures:

- Settings Devices tab and device detail if reachable.
- Expanded backup row/details or new-backup flow.
- Settings landing/navigation if separate from sheets.
- Import app/CSV next step or import progress/result.
- Import result/error.
- Locked/start/login state.

Exit criteria:

- App Shell and Settings patterns can be mapped.

## Design System Specs Progress

Eight components promoted from candidates to final specs, grounded in extracted tokens and
measured source CSS rules:

- Button → `20-spec-button.md`
- Icon Button / Command Button → `21-spec-icon-button.md`
- Dropdown / Context Menu → `22-spec-dropdown-menu.md`
- Dialog → `23-spec-dialog.md`
- Text Input → `24-spec-text-input.md`
- Password Input / Reveal → `25-spec-password-input.md`
- Table → `26-spec-table.md`
- Copyable Field → `27-spec-copyable-field.md`

Each spec carries a recurring cross-cutting gap to resolve product-wide: **no focus-visible
ring exists in source** (must be added via `--border-border-focused`), and **destructive
intent reuses neutral source styling** (must map to error treatment). A third recurring gap
emerged from the input specs: **the dark-theme `--input-border-hovered` value is missing**
from the export and several legacy color values (`--edit-*`, `--*-button-*`,
`--password-*-strength-color`) are referenced but undefined — both need the base theme file
or live computed values. A recurring dependency also surfaced: a shared **selected-row fill
token** (`#ddeaff` light / `#444`/`.16` dark) is reused by Table and Copyable Field but is
not yet named in the token layer. Next candidates by readiness: Vault Item / Row, Toggle,
Select/Combobox, Security Strength Badge, Segmented Selector / Tabs.

## Foundations Progress

Real token values have been extracted from `start-page-v8.css` and `common-ui.css` into
`19-foundations-tokens.md` (color light/dark pairs, typography, radius, spacing, shadow,
motion). Foundation color/type/radius/spacing/shadow/motion areas moved from `Seed/Draft`
to `Extracted`. Remaining foundation gap: legacy color values (referenced but not defined
in this export) and final canonical naming.

## Current Next Best Captures

1. Login password revealed with value redacted.
2. Identity group menu or visible inline rename.
3. Visible toast, ideally warning/error.
4. Security Center table sorting or empty/loading.
5. Settings Devices tab or settings navigation.
6. (Unblocked, no capture needed) Base theme file with legacy `--*-button-*` /
   `--password-*-strength-color` definitions, or computed values from a live DOM.
