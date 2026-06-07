# Component Spec: Copyable Field

**Status:** Ready for DS (final spec) · **Tier:** Composite (security-sensitive)
**Evidence:** login detail fields, identity fields, TOTP, security table cells, password
generator history. Composes [21-spec-icon-button.md](21-spec-icon-button.md) and
[25-spec-password-input.md](25-spec-password-input.md).

Tokens: [19-foundations-tokens.md](19-foundations-tokens.md). Source classes:
[02-component-state-matrix.md](02-component-state-matrix.md). Patterns:
[07-pattern-login-detail-editor.md](07-pattern-login-detail-editor.md),
[13-pattern-identity-detail.md](13-pattern-identity-detail.md). Security rules:
[16-foundations-draft.md](16-foundations-draft.md) §Sensitive Data.

---

## 1. Anatomy

A read row showing a label + value, with **hover-revealed action buttons** (copy, and for
secrets reveal/expand):

```
┌ field (flex row, border-bottom .08, padding 8) ──────────────────┐
│  field-column: Caption                                            │
│  field-column: value text (font 15, word-break)   [⧉] [👁] [⤢]   │  ← .buttons (hover)
└───────────────────────────────────────────────────────────────────┘
```

## 2. Field row (measured)

| Property | Value | Token |
|---|---|---|
| Layout | `display:flex; flex-direction:row; padding:8px` | — |
| Divider | `border-bottom: 1px solid rgba(0,0,0,.08)` | `--border-border-secondary` |
| Text | `rgba(0,0,0,.54)` | `--text-text-secondary` |
| Value column | `font-size:15px; padding:0 8px; word-break:break-word` | body |
| Selected field | `.field.selected` bg `#ddeaff` / `#444` | selected fill |

## 3. Hover action pane (measured)

| Property | Value | Token |
|---|---|---|
| Default | `.buttons { display:none }` (hidden until hover) | — |
| Background | `#fff` / `#1f1f1f`; selected `#ddeaff` / `#444` | surface / selected |
| Expanded position | `.field.expanded .buttons { position:absolute; right:8px; top:0 }` | — |
| Copy button | `copy-button .copy-icon` 20×20, asset `button-copy.svg`, hover `-hover` | — |
| Reveal button | `show-password-button` 32×32 circular padding 6 (secrets only) | see Password Input |
| Expand button | `expand-button .expand-icon` 20×20 (multiline/long values) | — |

The button pane appears on row hover; for expanded rows it pins to the top-right. Selected
rows tint the pane to match the selected-row fill (same `#ddeaff` family as the table).

## 4. Variants

- **Simple text value** — copy only (username, URL, email).
- **Masked secret** — copy + reveal eye; never copies the visible mask
  (see [25-spec-password-input.md](25-spec-password-input.md)).
- **Grouped / composite value** — name/date/card parts in one row
  (`rf-group-fields`, `rf-date-fields`).
- **Country / flag value** — `rf-field-country` with flag glyph.
- **Multiline note** — `rf-note`, expandable; uses the expand button.
- **TOTP code** — code + copy + timer (`totp-section`, `cmd-copy`); copy text key
  `Login_OneTimeCode_Copied_Text`.
- **Previous-version value** (backup history) — `field-column.previous-version` with a
  `restore` action; collapsed values ellipsize until expanded.

## 5. Copy feedback (measured)

Copy feedback is delivered by the **common toast**, not an inline class. Localized keys in
source:

- `Notification_Copied_ToClipboard_Text` (generic)
- `StartPage_Editor_CopiedToClipboard` (editor field)
- `Login_OneTimeCode_Copied_Text`, `Login_OneTimeCode_SetupKey_Copied_Text` (TOTP)
- `PassGen_History_Copied` (generator history)

> The toast announces the **action**, never the copied value. See the Toast/Notification
> candidate for placement/severity.

**Audit:** the data model tracks `m_time_last_copied_utc_secs` (per-field last-copied time).
The DS should treat copy as an auditable event for secrets, not a silent action.

## 6. Tokens

| Role | Token |
|---|---|
| Row divider | `--border-border-secondary` |
| Caption/value text | `--text-text-secondary` (value `--text-text-primary` where prominent) |
| Pane surface | `--surface-surface` / dark `#1f1f1f` |
| Selected fill | selected-row token (`#ddeaff` / `#444`) — shared with Table |
| Copy/expand icon | `--icon-icon-tetriary`; hover brand |
| Focus ring (to add) | `--border-border-focused` |

## 7. Behavior

- Single click on the value (or copy button) copies; row is `cursor`-interactive.
- Hover reveals actions without layout shift (pane is `display:none` → shown; absolute when
  expanded).
- Secrets stay masked after copy; copy does not reveal.
- Expand toggles multiline/long values and pins the action pane.

## 8. Accessibility

- **Hover-only action panes must be keyboard-reachable** — copy/reveal/expand are real
  buttons in the tab order, focusable without a pointer.
- Each action has an accessible name naming the field: "Copy username", "Reveal password",
  "Expand note" — **never** include the value.
- Copy success announced politely (toast `aria-live=polite`) without echoing the secret.
- Add a visible focus-visible ring (source provides none).
- Value text should be selectable for sighted users where appropriate, but secrets are
  masked until revealed.

## 9. Source → DS mapping

| Source class | DS |
|---|---|
| `fields-list .field` | `CopyableField` row |
| `.field-column` (caption/value) | `CopyableField.Label` / `.Value` |
| `.field .buttons` | hover action pane |
| `copy-button` / `copy-icon` | copy action |
| `show-password-button` | reveal action (secrets) |
| `expand-button` | expand action |
| `.field.selected` | row `selected` |
| `.field.expanded` | row `expanded` |
| `field-column.previous-version` / `restore` | backup-history value + restore |
| `inline-onhover-buttons-pane` (identity) | identity field action pane |
| `onhover-button-copy` (login detail) | login field copy action |

## 10. Open questions

1. Copied-state micro-feedback on the **button** itself (vs only the toast) — confirm if
   the copy icon briefly changes.
2. Exact selected-fill token name (shared with Table `#ddeaff` / dark variants).
3. Keyboard copy affordance confirmation (currently hover-driven in source).
4. Whether copy of a secret triggers any reveal/audit confirmation beyond the timestamp.
5. Identity `inline-onhover-buttons-pane` exact geometry (login `.buttons` measured; identity
   pane assumed equivalent — verify).
