# Component Spec: Security Strength Badge & Score

**Status:** Ready for DS (final spec) · **Tier:** Security component
**Evidence:** login detail, Security Center tables, password generator score panel,
Security Center summary score gauge. Reuses the RAG rating tokens.

Tokens: [19-foundations-tokens.md](19-foundations-tokens.md) §RAG. Source classes:
[02-component-state-matrix.md](02-component-state-matrix.md). Patterns:
[08-pattern-password-generator.md](08-pattern-password-generator.md),
[09-pattern-security-center.md](09-pattern-security-center.md).

---

## A. Strength Badge (inline)

A shield icon + label conveying password strength (weak / medium / good / strong).

### A1. Measured

| Property | Value | Token |
|---|---|---|
| Layout | `inline-block`, `height:24px`, `padding:0 0 0 24px` (icon gutter) | — |
| Icon | shield `16px`, `background-position:0`, no-repeat | — |
| Label | `font-size:15px`, `--text-text-secondary`, ellipsis, nowrap | `--text-text-secondary` |
| Weak | `shield-weak.svg` | `--rag-rating-rag-weak` (`#eb5757`) |
| Medium | `shield-medium.svg` | `--rag-rating-rag-medium` (`#ffb800`) |
| Good | `shield-good.svg` | `--rag-rating-rag-good` (`#9ac73a`) |
| Strong | `shield-strong.svg` | `--rag-rating-rag-strong` (`#5bb254`) |
| Loading | `progress-circle.svg` / `ajax-loader.gif`, `height:100%` | — |

> **Token resolution:** the hardcoded strength colors `#eb5757 / #ffb800 / #9ac73a / #5bb254`
> are **identical to the light values of `--rag-rating-rag-weak/medium/good/strong`**. This
> resolves the legacy `--password-*-strength-color` family (undefined in the export, see
> [19-foundations-tokens.md](19-foundations-tokens.md) §8) onto the RAG tokens — one
> `color.rating.*` family covers both.

### A2. Anatomy

```
🛡 Strong          ← shield (color = level) + label (text = level)
```

The level is conveyed by **both** the shield variant and the text label — **not color
alone**, which satisfies the non-color-only requirement.

---

## B. Score Gauge (Security Center summary / generator panel)

A horizontal score bar + numeric value reflecting overall vault or generated-password
strength.

### B1. Measured

| Property | Value | Token |
|---|---|---|
| Progress bar | `progressbar`: `height:10px`, `border-radius:5px`, `width:100%` | `radius.md`-ish (5) |
| Empty track | `nodata`: `--border-border-secondary` | `--border-border-secondary` |
| Overlay | `progressbar-overlay`: absolute, `transition: all .5s ease-out` | `motion.slow (500ms)` |
| Weak band | `weak-overlay`: gradient `rag-weak → rag-medium`, radius left | `--rag-rating-*` |
| Medium band | `medium-overlay`: gradient `rag-good → rag-strong` | `--rag-rating-*` |
| Good band | `good-overlay`: `--rag-rating-rag-strong`, radius right | `--rag-rating-rag-strong` |

### B2. Score levels (score-pane)

| Level | Source | Color |
|---|---|---|
| Excellent | `score-pane.excellent` | `--rag-rating-rag-strong` |
| Good | `score-pane.good` | `--rag-rating-rag-good` |
| Average | `score-pane.average` | `--rag-rating-rag-medium` |
| Low | `score-pane.low` | `--rag-rating-rag-weak` |
| No data | `score-pane.nodata` | `rgba(0,0,0,.28)` (neutral) |
| Error | `score-pane.error` | `#eb5757` |

The numeric `score-value .text` is tinted by level; the bar animates its fill over 500ms.

---

## C. Tokens

| Role | Token |
|---|---|
| Rating weak/medium/good/strong | `--rag-rating-rag-weak/medium/good/strong` |
| Badge label | `--text-text-secondary` |
| Empty/no-data | `--border-border-secondary` / `rgba(0,0,0,.28)` |
| Error | `--text-text-error` |
| Gauge radius | `5px` (→ align to `radius.md (6)` or keep 5) |
| Gauge motion | `motion.slow (500ms)` ease-out |

## D. Behavior

- Badge: static; reflects the computed strength of a single credential; `loading` while the
  score computes.
- Gauge: animates fill on data change (500ms); levels map score ranges to RAG bands.
- Same RAG vocabulary is reused by the Security Center risk tabs and table strength column
  (see [26-spec-table.md](26-spec-table.md)).

## E. Accessibility

- **Never color-only:** badge pairs shield shape + text label; gauge must expose the level
  as text (e.g. "Score 89 — Good"), not just a colored bar.
- Badge: the label is real text; if rendered as an icon+sr-text, the level word must be in
  the accessible name.
- Gauge: use `role="meter"`/`progressbar` semantics with `aria-valuenow`/`min`/`max` and a
  text level; `nodata` and `error` states announced.
- Strength must be perceivable for color-blind users — shield iconography differs per level
  (confirm the four shields are visually distinct beyond hue).
- Loading state announced (`aria-busy`) without flicker.

## F. Source → DS mapping

| Source class | DS |
|---|---|
| `security-password-strength` | `StrengthBadge` |
| `.weak/.medium/.good/.strong` | `level` |
| `security-password-strength.loading` | `state=loading` |
| `score-pane` + `.excellent/.good/.average/.low/.nodata/.error` | `ScoreGauge level` |
| `progressbar` / `progressbar-overlay` / `*-overlay` | gauge track + fill |
| `--rag-rating-rag-*` | `color.rating.*` |

## G. Open questions

1. Confirm the four shield SVGs are distinguishable by shape (not only hue) for color-blind
   accessibility.
2. Exact score-range → level thresholds (which numeric ranges map to low/average/good/excellent).
3. Reconcile the gauge `5px` radius with the radius scale (`4`/`6`).
4. Generator panel score colors vs Security Center score-pane: confirm one shared mapping.
5. `nodata` neutral `rgba(0,0,0,.28)` has no named token — add a neutral/empty rating step.
