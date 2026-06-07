# Import Flow

## Sources

Captured from focused `dialog-view import-dialog` fragment.

States captured:

- Import provider picker.
- Browser importer section.
- Google Chrome instruction/import-from-file page.
- Password-manager/app importer section.
- CSV importer option.

## Component Role

Import Flow lets users bring passwords from browsers, password managers, or CSV. The captured state is the first provider-selection step, not the import execution or result state.

## Dialog Shell

| Part | Source class | Purpose |
|---|---|---|
| Dialog | `dialog-view import-dialog` | Import flow modal/dialog. |
| Header | `header` | Dialog title, captured as `Import passwords`. |
| Close | `close-button` | Dismiss import dialog. |
| Provider page root | `choose-importer import-page` | Provider-picker page. |
| Instructions page root | `show-instructions-and-import import-page` | Provider-specific instructions and import action page. |
| Section wrapper | `importers-section` | Provider list content. |

Design implications:

- Import uses a specific `import-dialog` class, not only the generic `dialog-modal` shell.
- This state has no visible footer buttons; provider items are the primary actions.

## Provider Picker

| Part | Source class | Purpose |
|---|---|---|
| Provider list | `importers-list` | Group of provider tiles/rows. |
| Apps provider list | `importers-list apps-list` | Password-manager/app providers. |
| Provider item | `importer-item` | One selectable import source. |
| Provider icon | `icon icon-*` | Provider logo/icon slot. |
| Provider title | `title` | Provider label. |
| Separator | `separator` | Section divider. |

Captured provider groups:

| Group | Captured providers / items |
|---|---|
| Browsers | Google Chrome, Mozilla Firefox, Opera, Microsoft Edge |
| Password managers/apps | LastPass, Bitwarden, Dashlane, Myki, 1Password, Keeper, NordPass, True Key, Enpass, Kaspersky, Proton Pass, More... |
| File import | CSV |

Captured icon classes:

- `icon-chrome`
- `icon-firefox`
- `icon-opera`
- `icon-edge`
- `icon-lastpass`
- `icon-bitwarden`
- `icon-dashlane`
- `icon-myki`
- `icon-1password`
- `icon-keeper`
- `icon-nordpass`
- `icon-truekey`
- `icon-enpass`
- `icon-kaspersky`
- `icon-proton-pass`
- `icon-more`
- `icon-csv`

Design requirements:

- Provider items are action choices and need hover/focus/selected/disabled states even though only default is captured.
- Provider icon assets should be treated as branded/product icons with fallback behavior.
- `More...` is a navigation/expansion item, not a provider.
- CSV is a file-import route and likely needs a distinct next-step pattern from browser/app importers.

## Provider Instructions Page

Captured after selecting Google Chrome:

| Part | Source class | Purpose |
|---|---|---|
| Page root | `show-instructions-and-import import-page` | Provider instructions and import action page. |
| Instructions title | `instructions-title` | Provider logo plus provider name. |
| Provider logo | `logo logo-chrome` | Selected provider logo. |
| Provider name | `text` | Selected provider label. |
| Instructions body | `instructions` | Step-by-step export instructions. |
| Error section | `error-section hidden` | Import error slot, hidden in capture. |
| Error icon | `error-section .icon` | Error visual. |
| Error description | `error-section .description` | Error copy slot. |
| Progress section | `progress-section hidden` | Import progress slot, hidden in capture. |
| Progress bar | `progress` | Progress track with inner width value. |
| Buttons | `buttons-bar` | Page actions. |
| Import action | `button button-default` | `Import from File`. |
| Back action | `button button-non-essential` | Returns to provider picker. |

Design implications:

- Browser importers can require external export instructions before file import.
- `Import from File` is the primary action; `Back` is non-essential/secondary.
- Hidden error/progress sections confirm slots, not visible states. Progress/result/error still need capture.
- Instruction copy includes browser-specific product navigation; keep provider-specific content outside generic component API.

## States To Document

| State | Captured | Notes |
|---|---:|---|
| Provider picker | Yes | `dialog-view import-dialog`, `choose-importer import-page`. |
| Browser providers | Yes | Chrome, Firefox, Opera, Edge. |
| App providers | Yes | Password-manager/app provider list captured. |
| More providers | Partial | `More...` item captured; expanded more list not captured. |
| CSV import option | Yes | `icon-csv` item captured. |
| Provider hover/focus | No | Need interactive state if possible. |
| Provider selected/next step | Yes | Google Chrome instructions/import-from-file page captured. Need app/CSV variants. |
| Error slot | Partial | `error-section hidden` captured. Need visible error. |
| Progress slot | Partial | `progress-section hidden`, progress width `0%` captured. Need visible progress. |
| Import progress/result | No | Need import execution, success, partial success, and error if reachable. |
