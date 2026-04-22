# GW2-Loadouts i18n (Translations)

This project supports UI translations via **JSON locale files**.

Locale files are version-controlled and distributed in the releases repo [`tgk247/ldtrlss`](https://github.com/tgk247/ldtrlss) under an `i18n/` folder.

## How it works

- The addon **scans the `Loadouts/i18n/` folder** for `*.json` files.
- Each file name (without extension) is treated as the **locale code**:
  - `en.json` → `en`
  - `de.json` → `de`
  - `pt-BR.json` → `pt-BR`
  - `zh-CN.json` → `zh-CN`
- In the options UI, locales are shown using:
  - `meta.name` from the JSON, if present (recommended), otherwise
  - a built-in fallback label (usually the locale code)

## File format (important)

We intentionally use a **flat JSON object** (a single map of `"key": "value"`).

- **No nested objects.**
- **No arrays.**

Example:

```json
{
  "meta.name": "Deutsch",
  "ui.options.language": "Sprache",
  "ui.panels.armor": "Rüstung"
}
```

### Special keys

- **`meta.name`**: Human-friendly language name shown in the dropdown.
  - Example: `"meta.name": "Français"`

## Adding a new language

1. Copy `en.json` to a new file named by your locale code:
   - Example: `pl.json`, `nl.json`, `pt-BR.json`
2. Set `meta.name` to the label you want displayed.
3. Translate values (keep keys unchanged).

Notes:

- **Do not remove keys**. Missing keys fall back to English or the key itself.
- Keep punctuation and meaning consistent with the English source.
- Save files as **UTF-8**.

## “Short names” (compact UI abbreviations)

The compact UI can display abbreviated slot/weapon labels using `ui.short.*` keys, for example:

- `ui.short.equipmentSlot.chest`
- `ui.short.weaponType.greatsword`

These should be **short and readable** in the target language (often 2–5 chars).

Note:

- `ui.short.*` is **our addon UI text**. It follows the language the user selects in the addon options.

## Upgrade name shortening (`upgradeShorten.*`)

When “Short names” is enabled, the compact UI can shorten upgrade names (runes, sigils/cachets, infusions, etc.) using **locale-defined rewrite rules**.

Important:

- `upgradeShorten.*` operates on **item names decoded from the Guild Wars 2 client** (read from game memory), so the patterns must match the **game client language** strings for that locale.

Because our JSON is flat, rules are defined using numbered keys:

### Replace rules

For each upgrade kind, you can define a list of replacements:

- `upgradeShorten.<kind>.replaceFrom.N`
- `upgradeShorten.<kind>.replaceTo.N`

Where:

- `<kind>` is one of:
  - `rune`
  - `sigil`
  - `infusion`
  - `agonyLens`
  - `enrichment`
- `N` starts at `0` and increments by 1.
- The addon reads replacements **in order**, starting at `0`, and stops at the first missing index.

Example (English runes):

```json
{
  "upgradeShorten.rune.replaceFrom.0": "Superior Rune of ",
  "upgradeShorten.rune.replaceTo.0": "",
  "upgradeShorten.rune.replaceFrom.1": "Rune of ",
  "upgradeShorten.rune.replaceTo.1": ""
}
```

### Prefix-strip rules

Optionally, you can strip prefixes after replacements:

- `upgradeShorten.<kind>.stripPrefix.N`

Example (English):

```json
{
  "upgradeShorten.rune.stripPrefix.0": "the "
}
```

### Tips for good rules

- Prefer **official naming patterns** from the Guild Wars 2 wiki for your language.
- Keep rules **as specific as needed** (avoid accidental matches in unrelated names).
- Order matters: put more specific patterns first.
- Test with a few known items (e.g. “Superior Rune of …”, “Überlegenes Sigill …”, “Cachet … supérieur”, “Infusión de …”).

## Style guidelines

- **Keys are stable identifiers**: never translate keys.
- **Values are translated UI strings**: translate values.
- Keep capitalization consistent with the UI.
- Keep short strings short (especially `ui.short.*`).

## Validation / parity

It’s recommended that every locale keeps the same set of keys as `en.json`.

If you add a new key to `en.json`, please add it to other locales (even if the value is temporarily the English text) so translators can find it easily later.

