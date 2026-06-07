# Component Spec: Toggle / Switch

**Status:** Ready for DS (final spec) · **Tier:** Core primitive
**Evidence:** settings AutoFill/AutoSave/Advanced (on/off switches), password generator
option rows, legacy two-way switcher. Source has **two distinct concepts** — keep them
separate (per [02-component-state-matrix.md](02-component-state-matrix.md)).

Tokens: [19-foundations-tokens.md](19-foundations-tokens.md). Patterns:
[17-pattern-settings-sheets.md](17-pattern-settings-sheets.md),
[08-pattern-password-generator.md](08-pattern-password-generator.md).

---

## A. Switch (binary on/off)

A single boolean control (`role="switch"`, `aria-checked`) used in settings and generator
option rows.

### A1. Host row (measured)

| Property | Value | Token |
|---|---|---|
| Option row | `toggle-option`: flex, `justify-content:space-between`, `padding:16px 0`, `border-bottom:1px solid rgba(0,0,0,.12)` / `.16` | `--border-border-secondary` |
| With input | `toggle-option-with-input`: `align-items:flex-start`, stacked `input-wrapper` | — |
| Settings toggle row | `settings-row.setting-toggle`, `setting-toggle on` (state) | — |

### A2. Track + knob

> **Geometry not isolated in this export** — the switch track/knob is rendered via
> JS/SVG and does not resolve to a single measurable CSS rule. Colors are known via the
> legacy `--toggle-*` family (also undefined in this export, see
> [19-foundations-tokens.md](19-foundations-tokens.md) §8). **Open: capture the rendered
> switch to confirm track size, knob diameter, and travel.**

Known intent:

| State | Treatment | Token (target) |
|---|---|---|
| Off | neutral track, knob at start | `--toggle-off-button-background-color` |
| On | brand track (`#2979ff`), knob at end | `--toggle-on-button-background-color` |
| Disabled | muted track/knob | `--toggle-disabled-*` |
| Focus | add focus-visible ring | `--border-border-focused` |

### A3. Accessibility

- Real `role="switch"` with `aria-checked` (already in source).
- Add a visible focus-visible ring (source provides none).
- Label the switch via the row title; tapping the row label toggles it.
- State must not rely on color alone (knob position conveys on/off).

---

## B. Switcher (two-way segmented)

A legacy two-position selector (`switcher`, `left-button` / `right-button`), distinct from
the binary switch — used where two labeled options sit side by side.

### B1. Measured

| Part | Value | Token |
|---|---|---|
| Container | `switcher`: `border:1px solid --toggle-on-button-background-color`, `border-radius:10px`, flex row | — |
| Right button | `background:--toggle-off-button-background-color`, `border-radius:8px`, `min-width:40px`, `padding:10px` | — |
| Left button | brand border (`--toggle-on-button-background-color`) | — |
| Selected side | `.highlighted`: `background:#2979ff`, `color:#fff` | `--background-background-primary` / `--text-text-inverse` |
| Disabled | `switcher-disabled`: muted bg/border; `.highlighted` muted fill `#d6d6d6`, text `#9f9f9f` | `--toggle-disabled-*` |

### B2. States

| State | Treatment |
|---|---|
| Left selected | left `.highlighted` (brand fill) |
| Right selected | right `.highlighted` (brand fill) |
| Disabled | `switcher-disabled`, both sides muted |
| Hover/focus | add focus-visible ring (source has none) |

### B3. Accessibility

- Expose as a radio group (`role="radiogroup"` + two `role="radio"`) or a grouped toggle;
  arrow keys move selection.
- Selected side via `aria-checked`/`aria-pressed`; not color-only (the brand fill should be
  paired with text contrast).
- Disabled exposed via `aria-disabled`.

---

## C. Relationship to other components

- The **Switch** is the boolean control; the **Switcher** is a 2-option selector. For 3+
  options use Segmented Selector / Tabs (separate candidate), not Switcher.
- Radio Group (settings AutoFill/AutoSave) is a separate, distinct control — see the Radio
  Group candidate in [15-component-candidates.md](15-component-candidates.md).

## D. Source → DS mapping

| Source class | DS |
|---|---|
| `toggle` (role=switch) / `setting-toggle` / `setting-toggle on` | `Switch` |
| `toggle-option` / `toggle-option-with-input` | switch host row |
| `switcher` / `left-button` / `right-button` / `highlighted` | `Switcher` |
| `switcher-disabled` | `Switcher state=disabled` |

## E. Open questions

1. **Switch track/knob geometry** — capture the rendered settings switch (size, travel,
   knob diameter, transition).
2. Legacy `--toggle-*` color values are undefined in this export (base theme file needed).
3. Switch focus and keyboard behavior confirmation.
4. Whether Switcher is being retired in favor of Segmented Selector in v8 (legacy-only here).
