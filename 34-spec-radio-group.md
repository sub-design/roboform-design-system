# Component Spec: Radio Group

**Status:** Ready for DS (final spec) · **Tier:** Core primitive
**Evidence:** settings AutoFill and AutoSave mode selectors.

Tokens: [19-foundations-tokens.md](19-foundations-tokens.md). Source classes:
[02-component-state-matrix.md](02-component-state-matrix.md). Pattern:
[17-pattern-settings-sheets.md](17-pattern-settings-sheets.md).

---

## 1. Anatomy

A `radio-group` of label rows; each row has a `radio-button` (custom circle) + `radio-text`.
Selection marked by `option-checked` and `role="radio"` / `aria-selected`.

```
( ) Option one
(•) Option two   ← selected
( ) Option three (disabled)
```

## 2. Control (measured)

| Property | Value | Token |
|---|---|---|
| Radio circle | `radio-button`: `18×18`, `border:2px solid #777`, `border-radius:100%`, `margin-right:10px` | — |
| Selected ring | border → `#2979ff` | `--background-background-primary` |
| Selected dot | `:before` `8×8`, `#2979ff`, circular | `--background-background-primary` |
| Disabled | border `#777`, `opacity:.54`; dot `#777` | — |
| Label text | `radio-text span`: `font-size:13px`, `margin-left:.5em` | caption |

## 3. States

| State | Treatment |
|---|---|
| Unselected | grey ring, no dot |
| Selected | brand ring + brand dot (`option-checked`) |
| Disabled | grey ring at `opacity:.54`; native input may be disabled |
| Focus | add focus-visible ring (source has none) |

## 4. Accessibility

- Real radio semantics: `role="radiogroup"` + `role="radio"` with `aria-checked` (source uses
  `role="radio"` + `aria-selected` — prefer `aria-checked`).
- **Keyboard:** `↑`/`↓` (or `←`/`→`) move and select within the group; `Tab` enters/exits.
- Add a visible focus-visible ring; disabled options use `aria-disabled` and are skipped.
- Selection not color-only: the filled dot + label convey state.
- Label clickable selects the option.

## 5. Source → DS mapping

| Source class | DS |
|---|---|
| `radio-group` | `RadioGroup` |
| `radio-button` | radio control |
| `radio-button:before` | selected dot |
| `radio-text` | label |
| `option-checked` / `role=radio` | `Radio state=selected` |

## 6. Open questions

1. Focus ring and exact disabled treatment beyond `opacity:.54`.
2. Inline secondary text in option labels (noted in candidate) — confirm layout.
3. Validation/error state if any radio group is required.
