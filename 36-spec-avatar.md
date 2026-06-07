# Component Spec: Avatar / Initials

**Status:** Ready for DS (final spec) · **Tier:** Core primitive
**Evidence:** account menu, identity favorites, recipients, enterprise company logo.

Tokens: [19-foundations-tokens.md](19-foundations-tokens.md). Source classes:
[02-component-state-matrix.md](02-component-state-matrix.md).

---

## 1. Anatomy

A circular badge showing initials (or a company logo) on a tonal color fill, deterministic
per identity/item.

## 2. Two sizes in source (measured)

| Component | Source | Size | Radius | Font |
|---|---|---|---|---|
| Legacy avatar | `avatar` | `24×24` | `50%` | `11px / 500` |
| v8 initials | `initials` | `32×32` | `100%` | `12.8px / 500`, `margin-left:5px` |

Both center the text, `user-select:none`.

## 3. Color slots (measured)

Each slot is a **tonal triad**: light fill + same-hue darker border + same-hue dark text.

| Slot | Fill | Border / Text |
|---|---|---|
| 1 (blue) | `#639dff` | `#2e45c3` |
| 2 (red) | `#f88` | `#b54040` |
| 3 (purple) | `#9d8fe6` | `#5343a9` |
| 4 (green) | `#9bd596` | `#278722` |
| 5 (yellow) | `#ffcd29` | `#cb8434` |

> These triads **match the dark values of the customizable item palette**
> (`--customizable-palette-*`, see [19-foundations-tokens.md](19-foundations-tokens.md) §1).
> The DS should source avatar slots from one `color.item.palette.*` family rather than
> duplicate hexes. Legacy `avatar` uses `color-1..5`; v8 `initials` uses `bg-color-1..4` —
> unify the naming and slot count.

## 4. Variants

- **Initials avatar** — 1–2 letters, color slot by hash of name/id.
- **Company logo** (`company-logo`) — enterprise account image instead of initials.
- **Identity favorite / item** — same color-slot system as items.

## 5. States

| State | Treatment |
|---|---|
| Default | tonal fill + initials |
| Image | logo replaces initials |
| (No hover/focus unless interactive) | if used as a button, add focus-visible ring |

## 6. Accessibility

- Accessible name = the full identity/account name, not the initials.
- Company logo has alt text = company name.
- Color slot is decorative; do not encode meaning in slot color alone.
- Contrast: dark text on light tonal fill should meet AA (verify slot 5 yellow especially).

## 7. Source → DS mapping

| Source class | DS |
|---|---|
| `avatar` / `initials` | `Avatar` (size sm/md) |
| `avatar.color-1..5` / `initials.bg-color-1..4` | `Avatar colorSlot` |
| `company-logo` | `Avatar variant=image` |

## 8. Open questions

1. Unify slot count (legacy 5 vs v8 4) and naming (`color-*` vs `bg-color-*`).
2. Initials derivation rule (1 vs 2 letters; which characters).
3. Slot assignment rule (hash function) for stable per-identity color.
4. AA contrast check for dark text on each tonal fill in both themes.
