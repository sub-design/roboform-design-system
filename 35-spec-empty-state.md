# Component Spec: Empty State

**Status:** Ready for DS (final spec) · **Tier:** Pattern component
**Evidence:** pinned, search, logins, safenotes, authenticator, identities, Security Center
(overall + per-tab), Emergency Access (both tabs).

Tokens: [19-foundations-tokens.md](19-foundations-tokens.md). Source classes:
[02-component-state-matrix.md](02-component-state-matrix.md).

---

## 1. Anatomy

```
        ┌──────────────┐
        │   [ icon ]   │   rf-no-items-icon (product-specific, ~90px)
        │   Heading    │   rf-no-items-text
        │   body copy  │
        │  [ Action ]  │   rf-no-items-btn (optional)
        └──────────────┘
   centered in the empty content area
```

## 2. Container (measured)

| Variant | Source | Size |
|---|---|---|
| Default | `rf-no-items` | `width:380px; height:310px`, centered absolute (`margin:auto; inset:0`) |
| Logins | `rf-no-items-login` | `height:400px`, icon `no-items-login.svg` |
| Authenticator | `rf-no-items-authenticator` | `width:400px`, links 20px, `z-index:1` |
| Creation disabled | `rf-no-items-creation-disabled` | `300×290` |
| Security Center | `rf-no-items-security-center` | `height:400px`, icon `no-items-security-center.svg` |
| Security Center tab | `rf-no-items-security-center-tab` | `height:270px`, icon `176px` |
| Shown wrapper | `rf-no-items-shown` | `height: calc(100vh - 346px)`, scrolls |
| Emergency Access | `rf-ea-no-accounts` | icon + text + invite action |

## 3. Parts (measured)

| Part | Source | Value |
|---|---|---|
| Icon | `rf-no-items-icon` | `height:90px; width:100%`, `background-size:contain`, centered; product-specific SVG |
| Text | `rf-no-items-text` | heading + body; stacked, gap ~15px in authenticator |
| Action | `rf-no-items-btn` | optional primary action (e.g. create, invite) |
| Link | `.link` | inline help link (authenticator) |

## 4. Variants

- **Informational** — icon + text only (e.g. no search results).
- **Actionable** — icon + text + `rf-no-items-btn` (logins create, EA invite).
- **Disabled/policy** — `rf-no-items-creation-disabled` when creation is blocked by policy.
- Each product area supplies its own icon and copy.

## 5. Tokens

| Role | Token |
|---|---|
| Heading text | `--text-text-primary` |
| Body text | `--text-text-secondary` |
| Link | `--link-link` |
| Action | Button primary (see [20-spec-button.md](20-spec-button.md)) |

## 6. Behavior

- Shown when a list/section has no items; centered in the available area.
- The wrapper (`rf-no-items-shown`) scrolls if content exceeds the viewport.
- Action button triggers the primary recovery path (create/invite/import).

## 7. Accessibility

- Heading is a real heading; the empty state is announced when it replaces a list.
- Decorative icon is `aria-hidden`; meaning lives in the text.
- Action button has a clear accessible name.
- Sufficient contrast for muted body copy in both themes.

## 8. Source → DS mapping

| Source class | DS |
|---|---|
| `rf-no-items` | `EmptyState` |
| `rf-no-items-icon` | illustration |
| `rf-no-items-text` | heading + body |
| `rf-no-items-btn` | primary action |
| `rf-no-items-*` (login/authenticator/security/creation-disabled) | `variant` |
| `rf-ea-no-accounts*` | Emergency Access empty state |

## 9. Open questions

1. Standard vs per-area sizing — define one responsive container rather than fixed px heights.
2. Heading vs body type scale (icons measured; text scale not isolated per part).
3. Loading vs empty distinction (empty captured; loading state separate).
