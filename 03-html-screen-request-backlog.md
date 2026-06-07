# HTML Screen Request Backlog

Use this list to request targeted HTML dumps from Inspector. The goal is to collect states, not pretty screenshots. For each screen, the most useful export is the HTML of the active `#v8` subtree after the UI state is opened.

## How to provide each dump

Open the state in RoboForm, then copy the relevant HTML from Chrome DevTools `Elements`.

Simple method:

1. Open the state in RoboForm.
2. Open Chrome DevTools.
3. In `Elements`, click the visible block you want to send.
4. Right click the highlighted HTML node.
5. Choose `Copy` -> `Copy outerHTML`.
6. Paste it into the chat.

For small menus/dialogs, copying only that visible block is enough. For full screens, copy the biggest parent block you can identify, usually something like `rf-password-generator`, `rf-security-center`, `rf-data-view`, or `rf-editor`.

When possible, include whether the app is in light or dark mode and what action opened the state.

## Priority 1: App shell and navigation

1. **Main vault grid, clean default state**
   - Need: full `#v8`.
   - Why: baseline layout, active nav, grid item anatomy.

2. **Main vault list view**
   - Status: captured with login and safenote rows.
   - Why: list row anatomy and columns differ from grid.

3. **Collapsed sidebar**
   - Need: sidebar collapsed but not hovered.
   - Why: responsive/collapsed nav spec.

4. **Collapsed sidebar hovered/expanded**
   - Need: collapsed sidebar while hover-expanded.
   - Why: transition and partial-expanded state.

5. **Search dropdown opened**
   - Need: click search options arrow, copy `#v8`.
   - Why: advanced search component.

6. **Search results with query**
   - Need: typed search with results and no-results if easy.
   - Why: search result list and empty state.

## Priority 2: Menus and dialogs

7. **Account menu opened** - captured
   - Need: menu visible.
   - Why: account dropdown, submenu, toggle, enterprise account anatomy.

8. **Help submenu opened**
   - Need: account menu with Help submenu visible.
   - Why: nested menu behavior.

9. **Create-new menu opened** - captured
   - Need: `rf-new-dropdown` visible.
   - Why: creation menu variants and disabled states.

10. **New Login dialog, first step** - captured
   - Need: dialog root and overlay.
   - Why: editor/dialog design foundation.

11. **New Login dialog with validation/error**
   - Status: inline validation not reachable so far; duplicate overwrite confirmation captured instead.
   - Why: validation and confirmation states.

12. **New Login fill fields** - captured, including enabled save and password reveal
   - Need: selected template with fields.
   - Why: fields, folder picker, note, pin, save states.

13. **Existing Login editor/detail**
   - Status: view-mode, edit-mode, changed-field Save enabled, hidden loading slot, unsaved changes confirmation, visible critical warning, inline rename, context menu, and notification variants captured. User confirmed no visible loading after Save in this flow.
   - Why: password fields, copy/reveal, commands.

14. **Folder selector dropdown inside editor** - captured in New Login
   - Need: editor with folder dropdown opened.
   - Why: folder selector component.

14a. **Create folder inline in New Login** - captured
   - Need: inline folder creation row.
   - Why: nested creation/editing pattern.

14b. **Common notification visible and severity variants**
   - Status: offscreen/transitioning messages captured: `Login has been created`, `Login removed from Pinned`.
   - Need: visible toast position, dismissed state if reachable, and any warning/error variants.
   - Why: toast motion, stacking, close affordance, and severity token rules.

14c. **Identity detail and instances**
   - Status: view-mode and edit-mode captured for Person, Address, Credit Card, Bank Account, Business, Passport, and Car; groups pane, section groups, missing optional groups, instance commands, field display variants, edit fields, note states, and disabled save captured.
   - Need: visible identity inline rename, visible `security-warning`, visible `instance-no-fields`, group context menu, save enabled/loading, edit validation if reachable.
   - Why: identity is a separate multi-instance editor pattern, not a login detail variant.

## Priority 3: Security and password tools

15. **Password Generator, random password**
   - Status: captured.
   - Why: score-colored panel, settings rows, copy/generate actions.

16. **Password Generator, passphrase**
   - Status: captured.
   - Why: generator mode variant.

17. **Security Center overview**
   - Status: captured with populated summary pane.
   - Why: security dashboard and RAG patterns.

18. **Security Center compromised tab with rows**
   - Status: captured with populated table.
   - Why: security table columns and row states.

19. **Security Center weak/reused tabs**
   - Status: captured, plus All, Complete Duplicates, and Excluded tabs.
   - Why: different columns and risk states.

20. **Security Center password revealed**
   - Status: behavior described without values for security reasons.
   - Why: sensitive data behavior.

21. **Manage columns menu in Security Center**
   - Status: captured.
   - Why: table settings component.

21a. **Security Center row context menu** - captured
   - Need: hover/focus/disabled variants if easy.
   - Why: row command menu includes Security Center-specific actions.

21b. **Security Center multiselect command bar** - captured
   - Need: selected-row hover/focus variants if easy.
   - Why: batch command ergonomics and selected row state.

21c. **Security Center Data Breach Monitoring** - captured
   - Need: resolved/empty states if easy.
   - Why: breach list, critical/non-critical compromised data, remediation actions.

## Priority 4: Sharing and access

22. **Sharing Center: shared with me**
   - Status: captured.
   - Why: sharing product patterns.

23. **Sharing Center: shared by me**
   - Status: captured.
   - Why: ownership/permission variants.

24. **Sharing settings for item/folder**
   - Status: captured for group, creator shared folder, received shared folder, and create shared folder.
   - Why: role controls: limited, regular, manager.

24a. **Sharing permission dropdown opened**
   - Status: captured.
   - Why: role control values and disabled/available options.

24b. **Sharing destructive confirmations**
   - Status: Stop Sharing and Delete captured. Remove Me still unknown.
   - Why: irreversible or access-removing actions.

25. **Emergency Access: My Emergency Contacts**
   - Status: empty state captured. Populated table still needed.
   - Why: contact/status rows.

26. **Emergency Access: I'm Emergency Contact For**
   - Status: empty state captured. Populated table still needed.
   - Why: inverse access state.

27. **Emergency Access empty state**
   - Status: captured for both tabs.
   - Why: no-data template.

27a. **Emergency Access invite/contact rows**
   - Status: Invite New Contact dialog captured. Populated table still needed later.
   - Why: statuses, timeout periods, accept/request/grant actions.

27b. **Emergency Access timeout dropdown**
   - Need: opened Timeout select in Invite New Contact dialog.
   - Why: timeout option values and combobox menu behavior.

27c. **Emergency Access populated My Emergency Contacts**
   - Need: at least one invited/accepted/contact row, including timeout, status, and row commands.
   - Why: outgoing trusted-contact management states.

27d. **Emergency Access populated I'm Emergency Contact For**
   - Need: at least one incoming invitation or accepted grantor row, including timeout, status, and request/access actions.
   - Why: inverse emergency-access workflow states.

## Priority 5: Account setup, import, settings

28. **Settings main page**
   - Status: settings sheet pages captured: AutoFill, AutoSave, Advanced, License & Billing, Devices & Activity Activities tab, Backups list, information notification.
   - Need: settings landing/navigation if separate from sheets.
   - Why: standalone settings layout.

28a. **Settings Devices tab**
   - Need: Devices tab selected in Devices & Activity, plus device details if a row opens.
   - Why: device management rows likely differ from activity rows.

28b. **Settings select opened**
   - Need: opened `custom-select` menu in Advanced or similar setting.
   - Why: settings combobox menu behavior and option rows.

28c. **Settings Backups expanded row**
   - Need: one expanded backup row/details and new-backup flow if reachable.
   - Why: backup list rows have expansion semantics and restore/create actions.

29. **Import flow provider list**
   - Status: captured.
   - Why: provider cards/list and import states.

29a. **Import provider next step**
   - Status: Chrome instruction/import-from-file page captured with hidden error/progress slots.
   - Need: app provider and CSV next step if possible.
   - Why: provider-specific instructions, file picker/upload, permission, and progress states likely diverge.

30. **Import success/error**
   - Need: visible progress, success, partial success, and error if accessible.
   - Why: feedback and result states.

31. **Locked/start/login state**
   - Need: after lock or logged-out state.
   - Why: auth shell and secure locked UI.

32. **Account setup/password creation**
   - Need: if accessible.
   - Why: password validation checklist and onboarding.

## First request

Please provide **Account menu opened** next. It is a compact state and will let us lock down dropdown/menu, toggle, submenu, account identity, and light/dark menu token behavior before moving into larger editor screens.
