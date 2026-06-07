# Component Spec: Badge / Count

**Status:** Ready for DS (final spec) · **Tier:** Core primitive
**Evidence:** Security Center tab counts, navigation category badge, event counts,
Emergency Access count, legacy item counters.

Tokens: [19-foundations-tokens.md](19-foundations-tokens.md). Source classes:
[02-component-state-matrix.md](02-component-state-matrix.md).

> Source typo `bandge` (in `rf-bandge-count`) must **not** be carried into the DS API.

---

## 1. Variants (measured)

| Variant | Source | Treatment |
|---|---|---|
| **Outlined count** | `badge` | outlined pill: `font-weight:400` (→ `600` active), `line-height:20px`, `margin-left:4px`; border `--border-border-primary` faded, text `--text-text-tetriary`; active border → `--text-text-primary` |
| **Notification pill** | `category-badge` / `data-breach-mon-notification` | filled brand pill: bg `#2979ff`, text on-color, `border-radius:12px`, `height:16px`, `padding:2px 10px`, `font-weight:600` |
| **Circular count** | `rf-events-count` | `20×20`, `border-radius:100%`, bg `#2962ff`, white text |
| **Outlined count (EA)** | `rf-bandge-count` *(typo)* | outlined, text `--text-text-secondary-hover`, themed border |
| **Legacy counter** | `counter` | `border-radius:24px`, `min:24×24`, bordered, `opacity:.9`, `--items-counter-*` |

## 2. Anatomy

A small inline label showing a number (or short text), either outlined (neutral counts) or
filled brand (notifications/alerts).

## 3. States

| State | Treatment |
|---|---|
| Default | outlined neutral |
| Active (in active tab) | weight `600`, border → primary text |
| Alert / notification | filled brand pill |
| Zero/empty | confirm hidden vs shown as 0 (open question) |

## 4. Tokens

| Role | Token |
|---|---|
| Outlined border | `--border-border-primary` (active → `--text-text-primary`) |
| Outlined text | `--text-text-tetriary` / `--text-text-secondary-hover` |
| Filled pill bg | `--background-background-primary` |
| Filled pill text | `--text-text-on-color` |
| Circular count bg | `#2962ff` (selected-active blue) → `--text-text-selected-active` family |
| Radius | pill `12px`, circular `50%`, legacy `24px` |

## 5. Behavior

- Counts update live; pairs with tabs (Segmented Selector), nav items, and toolbar buttons.
- Notification/alert variant draws attention (breach, pending) with the filled brand pill.

## 6. Accessibility

- The count/label must be in the host's accessible name (e.g. "Weak, 12 items"), not a
  decorative visual.
- Alert badges need a text equivalent ("has alerts"), not color-only.
- Sufficient contrast for filled and outlined variants in both themes.

## 7. Source → DS mapping

| Source class | DS |
|---|---|
| `badge` | `Badge variant=outlined` |
| `category-badge` / `data-breach-mon-notification` | `Badge variant=notification` |
| `rf-events-count` | `Badge variant=circular` |
| `rf-bandge-count` | `Badge variant=outlined` (rename, drop typo) |
| `counter` / `items-counter` | legacy counter → `Badge` |

## 8. Open questions

1. Consolidate the five badge families into 2–3 DS variants (outlined / filled / circular).
2. Zero/empty behavior (hidden vs `0`).
3. Max count behavior (e.g. `99+`).
4. Name the circular `#2962ff` against the selected-active token.
