# Component Spec: Dialog

**Status:** Ready for DS (first final spec) · **Tier:** Core primitive (overlay)
**Evidence:** New Login, duplicate-overwrite confirmation, unsaved-changes confirmation,
Manage Columns, Sharing Settings, Emergency Invite, destructive confirmations. Appears
across the whole product.

Tokens: [19-foundations-tokens.md](19-foundations-tokens.md). Source classes:
[02-component-state-matrix.md](02-component-state-matrix.md).

---

## 1. Anatomy

```
┌─ rf-modal-screen (overlay rgba(0,0,0,.25), fixed, z 5) ───────┐
│        ┌─ dialog-view / dialog-modal (surface) ─────────┐     │
│        │  header / titlebar              [ × close ]     │     │
│        │  ─────────────────────────────────────────     │     │
│        │  content (body / form / confirmation-text)      │     │
│        │  ─────────────────────────────────────────     │     │
│        │  buttons-bar (height 56, right-aligned)         │     │
│        └──────────────────────────────────────────────────┘     │
└──────────────────────────────────────────────────────────────┘
```

## 2. Container (measured)

| Part | Property | Value | Token |
|---|---|---|---|
| Overlay | fill | `rgba(0,0,0,.25)`, fixed, full-screen, `overflow:auto` | — |
| Overlay | z-index | `5` (`rf-modal-screen`) | `z.modal-screen` |
| Surface | bg (dark) | `#1c1c1c`, border `1px #383838` | `--surface-surface-11` |
| Surface | shadow | dialog elevation layer | `shadow.dialog` |
| Surface | radius | `radius.xl (12)` (per radius scale) | `radius.xl` |
| Header | padding | e.g. `16px 45px` (varies by dialog) | `space.*` |
| Body | font | `confirmation-text: 15px` | body default |
| Footer | `buttons-bar` | height `56px`, bg `--surface-surface-09` tint, top divider `0 1px 0 0 --border-border-secondary`, right-aligned, button `margin: 0 10px` | — |
| Width | per dialog | e.g. `480px` (add-email) | sized to content |

## 3. Variants

| Variant | Source basis | Purpose |
|---|---|---|
| **Form dialog** | New Login, Emergency Invite, add-email | Multi-field input + primary/cancel |
| **Confirmation** | duplicate overwrite | Yes/No decision on a message |
| **Unsaved-changes** | login editor `Save changes` | `Don't save` (discard-adjacent) vs `Save` |
| **Management** | Manage Columns, Sharing Settings | Lists/controls + Save/Done |
| **Message** | `rf-message` | Informational, single dismiss |
| **Destructive** | Stop Sharing, Confirm Deletion | Irreversible/access-removing action |

## 4. States

| State | Treatment |
|---|---|
| Open | overlay + surface with `rf-fade-in`, focus moved in |
| Closing | `rf-fade-out`, focus returns to opener |
| Loading | content `loading-view` (e.g. `padding:64px`); some dialogs swap body for a spinner |
| Validation/error | inline field error (`rf-field-error`); primary action disabled until valid (e.g. Invite disabled, `rf-ok-button disabled-button`) |
| Disabled primary | `disabled-button` until form valid / field changed |

## 5. Behavior & rules

- The overlay (`rf-modal-screen`, z 5) blocks the app; dropdowns/menus inside the dialog
  still layer above it (z 10–20).
- **Action order:** primary/continuing action is rightmost in `buttons-bar`. In
  unsaved-changes, primary = `Save` (the safe/continuing action); `Don't save` is the
  discard-adjacent secondary and needs explicit copy.
- **Close button behavior must be explicit per variant:** form/management `×` = cancel
  (no save); confirmation `×` = the safe/cancel option; message `×` = dismiss.
- **Destructive mapping:** source renders destructive confirmations with the neutral
  `default-button`; the DS **must** style destructive intent with error treatment
  (`--background-background-destructive` / error red) and never rely on the source class.
- Width is content-sized; long content scrolls within the surface, header and
  `buttons-bar` stay fixed.

## 6. Tokens

| Role | Token |
|---|---|
| Overlay scrim | `rgba(0,0,0,.25)` → `color.scrim` |
| Surface | `--surface-surface-10` / `--surface-surface-11` |
| Border | `--border-border-secondary` |
| Body text | `--text-text-primary` (15px) |
| Footer divider | `--border-border-secondary` |
| Footer tint | `--surface-surface-09` |
| Destructive | `--background-background-destructive` / `--text-text-error` |
| Radius | `radius.xl (12)` |
| Shadow | `shadow.dialog` |
| Motion | `motion.base (300ms)` fade |

## 7. Accessibility

- `role="dialog"` + `aria-modal="true"`, labelled by the title.
- **Focus trap** within the dialog; **focus returns to the trigger** on close.
- `Esc` closes (mapped to the safe/cancel action), except where data loss needs explicit
  confirmation — then `Esc` triggers the unsaved-changes guard.
- Primary action reachable and the disabled state exposed via `aria-disabled`.
- Validation errors associated to fields via `aria-describedby`; error not color-only.
- Destructive actions need a name that states the consequence.

## 8. Source → DS mapping

| Source class | DS |
|---|---|
| `rf-modal-screen` / `modal-dialog-screen` | `Dialog.Overlay` |
| `dialog-view` / `rf-modal-dialog` / `dialog-modal` | `Dialog.Surface` |
| `rf-titlebar` / `header` | `Dialog.Header` |
| `content` / `rf-body` / `confirmation-text` | `Dialog.Body` |
| `buttons-bar` / `rf-buttons-bar` | `Dialog.Footer` |
| `rf-message` | `variant=message` |
| `rf-ok-button disabled-button` | primary `Button state=disabled` |
| `rf-field-error` | inline validation |
| `rf-disable-curtain` | internal busy/disabled overlay |

## 9. Open questions

1. Light-theme dialog surface values (the captured `dialog-modal` rule shows dark theme;
   confirm light surface = `--surface-surface-10`/`#fff`).
2. Confirmed validation/error layout and a visible loading state for the New Login form.
3. Final destructive styling decision (shared with [20-spec-button.md](20-spec-button.md)).
4. Standard vs per-dialog header padding — define one header scale.
5. Exact dialog corner radius (radius scale says 12; confirm against source surface).
