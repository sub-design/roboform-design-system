# Password Generator

## Source

Captured from full `#v8` dump with Password Generator active in the main content area.

Captured state:

- Random Password tab active.
- Passphrase tab active.
- Generated password panel with `data-score="strong"`.
- Settings list with number stepper and toggles.
- Restore defaults, Show history, and bit strength text.

History/opened copy feedback is not captured yet.

## Component Role

Password Generator is a utility workspace for generating secure passwords and passphrases. It combines a mode selector, generated secret output, strength/status display, copy/generate actions, and a settings form.

The component is security-sensitive because it displays a generated secret and offers direct copy to clipboard.

## Container

| Part | Source class | Purpose |
|---|---|---|
| Root | `rf-password-generator` | Main password generator view. |
| Header | `rf-data-view-header` | Holds mode selector. |
| Mode selector | `password-generator-order-selector order-selector noselect` | Segmented tabs for generator type. |
| Active tab | `selector-item active` | Current mode. |
| Progress separator | `separator rf-progress` | Progress/transition runner area. |
| Progress runner | `rf-progress-runner hidden` | Hidden async progress indicator. |
| Content section | `rf-password-generator-container` | Generated secret and settings. |

Captured tabs:

- Random Password state: `Random Password` active, `Passphrase` inactive.
- Passphrase state: `Passphrase` active, `Random Password` inactive.

Design rule: this selector should reuse the same segmented selector pattern as other data-view headers, but its labels and density belong to the generator utility context.

## Generated Password Panel

| Part | Source class | Purpose |
|---|---|---|
| Panel | `rf-password-view-and-actions` | Output and primary actions. |
| Score state | `data-score="strong"` | Semantic strength state. |
| Password/generate group | `rf-password-view-and-generate` | Generated text plus regenerate action. |
| Generated password | `rf-generated-password` | Secret output text. |
| Generate button | `rf-generate-password-button` | Generate new password, title `Generate new password`. |
| Separator | `rf-password-view-and-actions-separator` | Divides output from score/copy. |
| Score/copy group | `rf-password-score-and-copy` | Strength and copy action. |
| Score | `rf-password-score` | Strength indicator wrapper. |
| Score icon | `rf-password-score-image` | Strength icon. |
| Score text | `rf-password-score-value` | Captured `Strong`. |
| Copy button | `rf-copy-password-button` | Captured label `Copy`, title `Copy generated password to clipboard`. |

Captured generated value:

```text
#!vSVWTXsz@7rn4mcx%Knt
```

Captured passphrase value:

```text
Pruning-Utopia-Strike-Murky-Nape-Stardust
```

Design requirements:

- Generated secret must support long strings up to at least the configured max length.
- Use stable wrapping/truncation rules so long passwords do not resize the full layout unpredictably.
- Copy and generate actions need clear focus states and should expose success feedback after activation.
- Strength color must be paired with text/icon, not color alone.

## Settings

| Part | Source class | Purpose |
|---|---|---|
| Settings root | `rf-password-generator-settings` | Settings area. |
| Container | `password-generator-settings-container` | Layout wrapper. |
| Settings list | `password-generator-settings-list` | Vertical list of settings. |
| Setting label | `setting-name-text` | Row label. |
| Toggle row | `toggle-option` | Boolean setting row. |
| Toggle with input | `toggle-option toggle-option-with-input` | Boolean setting plus custom input. |
| Toggle | `toggle`, `toggle checked` | Off/on switch. |
| Toggle control | `control` | Knob. |

Captured settings:

| Setting | Source signal | Captured state |
|---|---|---|
| Number of characters | `chars-number`, `number-input`, `min="1" max="512" step="1"`, `aria-valuenow="16"` | Number field with stepper. |
| 0-9 | `toggle checked` | On. |
| A-Z | `toggle checked` | On. |
| a-z | `toggle checked` | On. |
| Hexadecimal 0-9, A-F | `toggle` | Off. |
| Exclude similar | `toggle checked` | On. |
| Special characters | `toggle-option-with-input`, `input maxlength="30"`, `toggle checked` | On, editable character set. |

## Passphrase Settings

Passphrase mode reuses the same generator shell and output panel, but swaps the settings list.

| Setting | Source signal | Captured state |
|---|---|---|
| Number of words | `chars-number`, label title `Number of words in the generated passphrase`, `number-input` | Value `6`, min `2`, max `8`, step `1`. |
| Word separator | `toggle-option-with-input`, title `Use any character as words delimeter in the generated passphrase`, `input maxlength="1"` | Toggle on, custom one-character separator input. |
| Capitalize first letters | `toggle checked` | On. |
| Add numbers | `toggle` | Off. |

Design implications:

- The numeric setting label changes from `Number of characters` to `Number of words`; the control component is shared, but copy and min/max are mode-specific.
- The passphrase separator input permits one character only.
- Source tooltip includes typo `delimeter`; product copy should normalize to `delimiter` in design documentation unless preserving exact source copy.
- Passphrase output is word-like text with separators, so wrapping rules differ from random-password symbol strings.

## Number Stepper

| Part | Source class | Purpose |
|---|---|---|
| Wrapper | `edit-box-with-spin` | Number input with custom step buttons. |
| Input | `number-input` | Numeric field. |
| Buttons wrapper | `buttons-container` | Holds increment/decrement buttons. |
| Increment | `button-up` | Increase value. |
| Decrement | `button-down` | Decrease value. |

Accessibility notes from source:

- Label has `for="chars-number-input-startpage"`.
- Input has `aria-labelledby="chars-number-label-startpage"`.
- Input has `aria-valuenow="16"` and `aria-live="polite"`.

Design requirement: numeric settings should define min/max/step visibly only when helpful, but always enforce and expose them programmatically.

## Footer Actions And Strength

| Part | Source class | Captured text |
|---|---|---|
| Restore defaults | `restore-link flat-link` | `Restore defaults` |
| Show history | `password-generator-history-button` | `Show history` |
| Bit strength | `password-generator-bitstrength` | `Bit Strength = 96` |

Design requirements:

- Restore defaults is a low-emphasis link action.
- Show history behaves like an action; decide whether it opens an inline panel, popup, or separate view after capture.
- Bit strength should use a stable numeric metric style and should remain readable when settings change.

## States To Document

| State | Captured | Notes |
|---|---:|---|
| Random Password active | Yes | Primary captured state. |
| Passphrase active | Yes | Generated passphrase and settings captured. |
| Strong score | Yes | `data-score="strong"` and text `Strong`. |
| Weak/medium/good scores | No | Need generated examples or state changes. |
| Toggle on | Yes | `toggle checked`. |
| Toggle off | Yes | Hexadecimal captured off. |
| Number stepper default | Yes | Value `16`, min/max/step captured. |
| Special characters input | Yes | Empty input with max length. |
| Restore defaults | Yes | Link captured. |
| History closed | Yes | Button captured. Need opened history state. |
| Progress/loading | No | Progress runner hidden only. |
| Copy feedback | No | Need post-copy state/notification if any. |
