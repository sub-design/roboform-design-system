# Capture Checklist (prioritized worklist)

A focused, ordered list of HTML dumps to collect from RoboForm, ranked by how much each one
unblocks. This supersedes the scattered "needed" notes in
[03-html-screen-request-backlog.md](03-html-screen-request-backlog.md) and the
"Current Next Best Captures" in [14-work-plan-and-status.md](14-work-plan-and-status.md) —
work top to bottom.

## How to capture each one

1. Open the state in RoboForm (note light/dark and what action opened it).
2. DevTools → Elements → click the visible block.
3. Right-click the highlighted node → Copy → **Copy outerHTML**.
4. Paste into chat. For full screens, copy the biggest sensible parent
   (`rf-editor`, `rf-security-center`, `rf-data-view`, `rf-emergency-access`, `sheet-view`).

**Sensitive data:** redact real passwords, TOTP codes, card/CVV/PIN, passport/bank numbers,
addresses, phone numbers, birth dates before pasting. Synthetic values are fine; structure is
what matters. See [16-foundations-draft.md](16-foundations-draft.md) §Sensitive Data.

---

## Tier 1 — biggest unblock (do these first)

Each closes a *spec-level open question* and moves a pattern toward `State-complete`.

- [ ] **1. Main vault grid — clean default**
  - Open the All/Logins grid with items, no hover. Copy `rf-data-view`.
  - Unblocks: Vault Item §13 (clean grid default), App Shell baseline, active nav, grid spacing.

- [ ] **2. Vault item — hover + selected + menu-open**
  - Hover one grid card; then select one (multiselect); then open its row menu. Copy `rf-data` for each.
  - Unblocks: Vault Item states, the **selected-row fill token** (shared with Table/Copyable Field).

- [ ] **3. Login password revealed (value redacted)**
  - Open a saved login, reveal the password. Copy the password field/row.
  - Unblocks: Password Input §9, Copyable Field, Login Detail → `State-complete`.

- [ ] **4. Visible toast / notification (ideally warning or error)**
  - Trigger a copy or an action that shows the toast; capture while visible. Copy `rf-notification`.
  - Unblocks: the **Toast/Notification** spec (currently a Seed candidate, can't be promoted without this), Copyable Field copy-feedback.

- [ ] **5. Security Center table — active sorted column + empty/loading**
  - Click a sortable header (asc then desc); also open a tab with no rows if reachable. Copy `table-items` / `rf-security-center`.
  - Unblocks: Table §12 (active sort `aria-sort`, empty/loading), Security Center → `State-complete`.

---

## Tier 2 — opened menus & select states

Small dumps that close the Select/Dropdown open-state gaps.

- [ ] **6. Any custom-select opened** (settings Advanced, or Emergency Access timeout)
  - Open the dropdown. Copy `custom-select` with its `options-list`.
  - Unblocks: Select §10 (option row height, hover, **disabled option**), Dropdown attached-list states.

- [ ] **7. Account menu — Help submenu open**
  - Open account menu → hover Help. Copy `rf-account-menu`.
  - Unblocks: Dropdown §10 (submenu open), Account Menu.

- [ ] **8. Create menu — disabled item**
  - Open create menu in a context where an item is disabled (e.g. no permission). Copy `rf-new-dropdown`.
  - Unblocks: Dropdown disabled-item state, Create Menu.

---

## Tier 3 — Identity editor states

Moves Identity Detail and its sub-components from `Mapped` to `State-complete`.

- [ ] **9. Identity group context menu open** — copy the open menu over a group.
- [ ] **10. Identity inline rename (visible)** — start renaming an instance; copy `inplace-edit-pane`.
- [ ] **11. Identity save enabled + (if shown) loading** — change a field; copy the editor header/buttons.
- [ ] **12. Identity visible `security-warning` / `instance-no-fields`** — if reachable.
  - Unblocks: Identity Group Navigator, Identity Field Editor (save/loading/validation), Identity Detail → `State-complete`.

---

## Tier 4 — Sharing & Emergency Access populated states

- [ ] **13. Remove Me confirmation** (received shared folder) — copy the confirmation dialog.
- [ ] **14. Disabled permission options** — open a permission menu where some roles are disabled.
- [ ] **15. Emergency Access — populated My Emergency Contacts** (≥1 row: timeout, status, commands).
- [ ] **16. Emergency Access — populated I'm Emergency Contact For** (≥1 incoming/accepted row).
- [ ] **17. Emergency Access — timeout dropdown opened** (in Invite dialog).
  - Unblocks: Permission Menu disabled options, Destructive Confirmation, Emergency Access Table + statuses, EA → `State-complete`.

---

## Tier 5 — Settings / Import / Auth

- [ ] **18. Settings Devices tab** (+ a device detail row if it opens).
- [ ] **19. Settings Backups — expanded row** (+ new-backup flow if reachable).
- [ ] **20. Settings landing/navigation** (if separate from the sheets).
- [ ] **21. Import — app provider + CSV next step**, and **import progress / success / error**.
- [ ] **22. Locked / logged-out / startup / login** screen.
  - Unblocks: Sheet Shell, Settings Row, Activity List (Devices), Backup List, Provider Picker, and the auth shell (currently uncaptured).

---

## Status snapshot (what each tier closes)

| Tier | Pattern areas it advances |
|---|---|
| 1 | Vault List, Login Detail, Security Center, **Toast** (new spec) |
| 2 | Select, Dropdown, Account/Create menus |
| 3 | Identity Detail + 3 identity sub-components |
| 4 | Sharing Center, Emergency Access |
| 5 | Settings, Import, Auth shell |

After Tier 1 alone, Login Detail and Security Center can reach `State-complete`, the Vault
Item selected-fill token can be named, and the Toast component becomes promotable.
