# RoboForm Web App Design System

A **source-led** design system for the RoboForm web application, reconstructed from the
real product implementation (extension/web-app CSS, JS, and captured DOM) rather than
from screenshots alone.

## Approach

Work moves through four artifact layers, with an explicit evidence threshold before any
UI part is promoted to a final design-system spec:

1. **Source Inventory** — raw evidence from CSS/JS/HTML (classes, states, tokens, icons).
2. **Pattern Specs** — product-level surfaces and workflows.
3. **Component Candidates** — repeated UI parts not yet final APIs.
4. **Design System Specs** — finalized foundations/components with API, variants, states,
   behavior, accessibility, and legacy migration mapping.

See [00-source-led-design-system-plan.md](00-source-led-design-system-plan.md) for the
full strategy and [14-work-plan-and-status.md](14-work-plan-and-status.md) for the
operating plan, status model, and sprint order.

## Document index

| File | Purpose |
|---|---|
| [00-source-led-design-system-plan.md](00-source-led-design-system-plan.md) | Overall source strategy and product taxonomy |
| [01-source-inventory.md](01-source-inventory.md) | Raw source files, classes, tokens, breakpoints, z-index |
| [02-component-state-matrix.md](02-component-state-matrix.md) | Source classes → components → states |
| [03-html-screen-request-backlog.md](03-html-screen-request-backlog.md) | Targeted HTML dumps still needed (full backlog) |
| [39-capture-checklist.md](39-capture-checklist.md) | Prioritized capture worklist (start here for captures) |
| [14-work-plan-and-status.md](14-work-plan-and-status.md) | Operating model, area status, sprint plan |
| [15-component-candidates.md](15-component-candidates.md) | Repeated UI parts tracked before promotion |
| [16-foundations-draft.md](16-foundations-draft.md) | Draft token and foundation observations (rules) |
| [19-foundations-tokens.md](19-foundations-tokens.md) | Extracted real token values: color light/dark pairs, type, radius, spacing, shadow, motion |
| [40-tokens-canonical.md](40-tokens-canonical.md) | Proposed canonical token API: source→DS name map, new named tokens, defects, blocked legacy |
| [41-base-theme-extraction.md](41-base-theme-extraction.md) | Console snippet to resolve the blocked legacy token values from the live app |
| **Design system specs (final)** | |
| [20-spec-button.md](20-spec-button.md) | Button |
| [21-spec-icon-button.md](21-spec-icon-button.md) | Icon Button / Command Button |
| [22-spec-dropdown-menu.md](22-spec-dropdown-menu.md) | Dropdown / Context Menu |
| [23-spec-dialog.md](23-spec-dialog.md) | Dialog |
| [24-spec-text-input.md](24-spec-text-input.md) | Text Input |
| [25-spec-password-input.md](25-spec-password-input.md) | Password Input / Reveal |
| [26-spec-table.md](26-spec-table.md) | Table |
| [27-spec-copyable-field.md](27-spec-copyable-field.md) | Copyable Field |
| [28-spec-vault-item.md](28-spec-vault-item.md) | Vault Item / Row |
| [29-spec-toggle.md](29-spec-toggle.md) | Toggle / Switch (binary switch + two-way switcher) |
| [30-spec-select.md](30-spec-select.md) | Select / Combobox |
| [31-spec-segmented-selector.md](31-spec-segmented-selector.md) | Segmented Selector / Tabs |
| [32-spec-strength-badge.md](32-spec-strength-badge.md) | Security Strength Badge & Score |
| [33-spec-sharing-indicator.md](33-spec-sharing-indicator.md) | Sharing Indicator |
| [34-spec-radio-group.md](34-spec-radio-group.md) | Radio Group |
| [35-spec-empty-state.md](35-spec-empty-state.md) | Empty State |
| [36-spec-avatar.md](36-spec-avatar.md) | Avatar / Initials |
| [37-spec-badge.md](37-spec-badge.md) | Badge / Count |
| [38-spec-command-bar.md](38-spec-command-bar.md) | Command Bar |
| **Pattern specs** | |
| [04-components-account-menu.md](04-components-account-menu.md) | Account menu |
| [05-components-create-menu.md](05-components-create-menu.md) | Create menu |
| [06-pattern-new-login-flow.md](06-pattern-new-login-flow.md) | New login flow |
| [07-pattern-login-detail-editor.md](07-pattern-login-detail-editor.md) | Login detail / editor |
| [08-pattern-password-generator.md](08-pattern-password-generator.md) | Password generator |
| [09-pattern-security-center.md](09-pattern-security-center.md) | Security Center |
| [10-pattern-sharing-center.md](10-pattern-sharing-center.md) | Sharing Center |
| [11-pattern-emergency-access.md](11-pattern-emergency-access.md) | Emergency Access |
| [12-pattern-vault-list-and-actions.md](12-pattern-vault-list-and-actions.md) | Vault list and actions |
| [13-pattern-identity-detail.md](13-pattern-identity-detail.md) | Identity detail |
| [17-pattern-settings-sheets.md](17-pattern-settings-sheets.md) | Settings sheets |
| [18-pattern-import-flow.md](18-pattern-import-flow.md) | Import flow |

## Sensitive data rule

This repository documents a password manager. Fixtures and examples must use only
synthetic/redacted values — never real passwords, TOTP codes, card numbers, CVV, PIN,
passport/bank numbers, addresses, or birth dates. See the Sensitive Data Rules section
in [16-foundations-draft.md](16-foundations-draft.md).
