# Base-Theme Token Extraction (console snippet)

The legacy variables (`--regular-button-*`, `--edit-*`, `--toggle-*`, `--list-item-*`,
`--password-*-strength-color`, etc.) are **referenced** in `common-ui.css` but **defined in a
base theme file that is not in the export**. Rather than hunt for that file, resolve their
**computed values** directly from the running app.

## How to run

1. Open RoboForm with the vault visible (so `#v8` / `light-theme` is mounted).
2. Make sure the app is in **light mode**.
3. Open DevTools → Console.
4. Paste the snippet below and run it. It copies a JSON blob to your clipboard and prints a
   table.
5. Paste the result into chat, labeled **light**.
6. Switch the app to **dark mode**, run again, paste labeled **dark**.

> Reads only CSS variable values (colors). No vault data, no secrets.

## Snippet

```js
(() => {
  const NAMES = [
    "--main-text-color","--hint-text-color","--error-text-color",
    "--regular-button-background-color","--regular-button-border-color","--regular-button-text-color",
    "--regular-button-hover-color","--regular-button-hover-border-color",
    "--regular-button-active-color","--regular-button-active-border-color",
    "--regular-button-disabled-color","--regular-button-disabled-border-color","--regular-button-disabled-text-color",
    "--important-button-background-color","--important-button-border-color","--important-button-text-color",
    "--important-button-hover-color","--important-button-active-color",
    "--important-button-disabled-color","--important-button-disabled-border-color","--important-button-disabled-text-color",
    "--low-emphasis-button-text-color","--low-emphasis-button-hover-color","--low-emphasis-button-active-color",
    "--low-emphasis-button-focused-color","--low-emphasis-button-disabled-text-color",
    "--white-button-background-color","--white-button-border-color","--white-button-text-color",
    "--white-button-hover-background-color","--white-button-focus-border-color",
    "--white-button-active-background-color","--white-button-active-border-color",
    "--white-button-disabled-border-color","--white-button-disabled-text-color",
    "--edit-background-color","--edit-border-color","--edit-text-color","--edit-caret-color",
    "--edit-placeholder-color","--edit-hover-border-color","--edit-focused-border-color",
    "--edit-disabled-background-color","--edit-disabled-border-color","--edit-disabled-text-color","--edit-disabled-placeholder-color",
    "--toggle-on-button-background-color","--toggle-on-button-text-color",
    "--toggle-on-button-disabled-background-color","--toggle-on-button-disabled-text-color",
    "--toggle-off-button-background-color","--toggle-off-button-border-color","--toggle-off-button-text-color",
    "--toggle-off-button-disabled-border-color",
    "--toggle-disabled-background-color","--toggle-disabled-border-color",
    "--password-weak-strength-color","--password-medium-strength-color","--password-good-strength-color","--password-strong-strength-color",
    "--list-item-text-color","--list-item-bold-text-color","--list-item-folder-text-color",
    "--list-item-hover-background-color","--list-item-round-border-color","--list-item-show-all-text-color",
    "--list-item-accented-background-color","--list-item-accented-text-color","--list-item-accented-bold-text-color","--list-item-accented-folder-text-color",
    "--list-item-addnew-circle-background-color","--list-item-addnew-circle-border-color","--list-item-addnew-circle-plus-color",
    "--items-counter-background-color","--items-counter-border-color","--items-counter-text-color",
    "--progress-background-color","--progress-border-color","--progress-indicator-color"
  ];
  // Resolve against the themed root if present, else <html>
  const root = document.querySelector("#v8") || document.documentElement;
  const cs = getComputedStyle(root);
  const theme = root.className.match(/(light|dark)-theme/)?.[0] || "unknown-theme";
  const out = {};
  for (const n of NAMES) {
    const v = cs.getPropertyValue(n).trim();
    if (v) out[n] = v;
  }
  const blob = JSON.stringify({ theme, values: out }, null, 2);
  console.table(out);
  console.log(blob);
  try { copy(blob); console.log("✓ copied to clipboard"); } catch {}
  return blob;
})();
```

## What this unblocks

Once both outputs are pasted, these get finalized:

- `40-tokens-canonical.md` §9 — the blocked legacy color values resolved.
- `19-foundations-tokens.md` §8 — legacy values filled in (currently "unknown").
- Exact hover/active/disabled colors for **Button** tiers (20), **Text Input** (24,
  `--edit-*`), and **Switch** (29, `--toggle-*`).
- Confirms whether `--password-*-strength-color` equals the RAG tokens (expected per
  [32-spec-strength-badge.md](32-spec-strength-badge.md)).

## If the snippet returns mostly empty

That means the legacy vars are not defined on `#v8` either (they may be scoped to a specific
component subtree). In that case, run it again with `root` pointed at a legacy element, e.g.
replace the `root` line with:

```js
const root = document.querySelector(".regular-button, .extension-normal-input, .switcher") || document.documentElement;
```

and re-run in both themes.
