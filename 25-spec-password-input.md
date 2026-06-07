# Component Spec: Password Input / Reveal

**Status:** Ready for DS (final spec) · **Tier:** Core primitive (security-sensitive)
**Evidence:** New Login reveal, login detail masked, security center reveal behavior,
identity secret fields, TOTP. Builds on [24-spec-text-input.md](24-spec-text-input.md).

Tokens: [19-foundations-tokens.md](19-foundations-tokens.md). Source classes:
[02-component-state-matrix.md](02-component-state-matrix.md). Security rules:
[16-foundations-draft.md](16-foundations-draft.md) §Sensitive Data.

---

## 1. Anatomy

A Text Input whose value is masked, plus a **reveal toggle** (eye) trailing affordance.
Often paired with a **copy** action and, in tables, a column-level reveal toggle.

```
  ┌ Label ───────────────────────────────────┐
  │ ••••••••••••              👁  [⧉ copy]     │   trailing reveal eye + optional copy
  └───────────────────────────────────────────┘
```

The field reserves space for the trailing eye via `with-hint input { padding-right: 28px }`.

## 2. Reveal toggle (measured)

Two source affordances exist:

| Source | Box | Icon | Hover |
|---|---|---|---|
| `toggle-pwd` | `20×20`, `bg-size:16px`, `margin-left:5px` | `toggle-pwd-show` = masked (closed-eye), `toggle-pwd-hide` = revealed (open-eye) | swaps to higher-contrast asset (`black70`→`black87`, `white70`→`dark2`) |
| `eye-icon` | `20×20`, `border-radius:100%` | `eye-icon` (open) / `eye-icon.hide-pwd` (closed) | bg `rgba(41,121,255,.16)` + blue icon `password-input-eye-blue.svg` |

> Naming in source is inverted-looking: `toggle-pwd-show` is shown while the value is
> **hidden** (it offers the "show" action); `toggle-pwd-hide` is shown while **revealed**.
> The DS state should be modeled as `revealed: boolean`, not by the source class name.

Native browser reveal is suppressed: `input[type=password]::-ms-reveal { display:none }` —
the product uses **only its custom eye**. The DS keeps a single custom reveal control.

## 3. States

| State | Treatment |
|---|---|
| Masked (default) | value rendered as dots; eye offers "Show password" |
| Revealed | plaintext value; eye offers "Hide password"; consider auto-hide timeout |
| Hover (eye) | higher-contrast / blue icon + `rgba(41,121,255,.16)` hover fill (`eye-icon`) |
| Focus (eye) | ⚠️ add focus-visible ring (`--border-border-focused`); source has none |
| Disabled | inherits Text Input disabled tokens; reveal disabled |
| Error | inherits `--input-border-error` + `rf-field-error` |
| Copied | copy feedback must **not** echo the secret (see §5) |

All Text Input border/label/disabled states from
[24-spec-text-input.md](24-spec-text-input.md) apply unchanged.

## 4. Variants

- **Field reveal** — single input with trailing eye (New Login, editors).
- **Detail reveal** — read-only masked value in login/identity detail with hover copy
  (`onhover-button-copy`) and reveal.
- **Table column reveal** — Security Center `toggle-pwd` in the `c_password` column header
  toggles reveal for the whole masked column (`password-header-column`). Mass reveal is a
  higher-risk action and should warn/audit per product rules.
- **Generated password** — read display in the password generator (monospace), see
  [08-pattern-password-generator.md](08-pattern-password-generator.md).

## 5. Security rules (binding, not optional)

From the foundations sensitive-data rules:

- **Never store real secrets** in fixtures/docs — synthetic/redacted only.
- **Copy feedback must not echo the full secret** value in a toast, tooltip, or accessible
  name (e.g. say "Password copied", not the value).
- Reveal states need a clear hide control; consider a **reveal timeout** that re-masks.
- Reveal/copy controls must be **keyboard-reachable** even when they are hover-only in the
  detail view.
- Table/column mass-reveal needs a strong masking contract and should re-mask on blur.
- The accessible name of the eye announces the **action and target field**, never the value.

## 6. Tokens

| Role | Token |
|---|---|
| Eye hover fill | `rgba(41,121,255,.16)` → `--button-background-pressed` / menu hover family |
| Eye icon | `--icon-icon-tetriary`; active/hover brand blue |
| Field border/label/disabled | inherit `--input-*` (see Text Input) |
| Error | `--input-border-error` / `--text-text-error` |
| Focus ring (to add) | `--border-border-focused` |

## 7. Accessibility

- Reveal toggle is a real button with `aria-pressed` reflecting `revealed`, and an
  `aria-label` like "Show password" / "Hide password".
- Add a visible focus-visible ring (source provides none).
- When revealed, announce the change politely without reading out the secret.
- Respect the copy-feedback rule above for screen-reader output.
- Masked value should not be exposed in plaintext via the accessibility tree while masked.

## 8. Source → DS mapping

| Source class | DS |
|---|---|
| `toggle-pwd` / `toggle-pwd-show` / `toggle-pwd-hide` | `PasswordInput` reveal toggle (`revealed` state) |
| `eye-icon` / `eye-icon.hide-pwd` | reveal toggle (rounded variant) |
| `with-hint` | trailing-eye spacing |
| `password-header-column` / `toggle-pwd` (table) | column reveal toggle |
| `onhover-button-copy` | copy affordance (see Copyable Field candidate) |
| `secret-field` | masked detail value |

## 9. Open questions

1. Does the product enforce a **reveal timeout** / auto-re-mask? (capture if reachable)
2. Is column mass-reveal gated by a confirmation or audit? (Security Center)
3. Captured revealed-state source classes with values redacted (still on the backlog).
4. Copy feedback exact copy/placement and whether it differs from the common toast.
