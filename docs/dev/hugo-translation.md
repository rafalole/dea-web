# Hugo Translation System (i18n)

This document explains Hugo's i18n (internationalization) feature for managing translations.

**Location**: `/home/devlin/src/github/dea-web/i18n/`

---

## What is i18n?

**i18n** = Internationalization (18 letters between 'i' and 'n')

Hugo's system for managing translations across multiple languages.

---

## Language Configuration vs i18n Files

### Two Translation Systems

Hugo has **two separate systems** for multilingual content:

1. **Language Configuration** (`hugo.toml`) - Site-level settings
2. **i18n Files** (`i18n/*.toml`) - UI text translations

### Language Configuration (hugo.toml)

#### Built-in Fields

Hugo recognizes these fields natively:

```toml
[languages.pl]
  languageCode = 'pl'           # ISO code (required)
  languageName = 'Polski'       # Display name
  languageDirection = 'ltr'     # Text direction: 'ltr' or 'rtl'
  weight = 1                    # Sort order
  title = 'Site Title'          # Site title
  copyright = '© 2024'          # Copyright notice
  contentDir = 'content/pl'     # Content directory
  disabled = false              # Disable language
```

**Access in templates**:
```html
{{ .Language.LanguageCode }}  → "pl"
{{ .Language.LanguageName }}  → "Polski"
{{ .Site.Title }}             → "DeaPrint - Profesjonalna Fotografia"
```

#### Custom Parameters

Anything NOT built-in goes in `[languages.XX.params]`:

```toml
[languages.pl]
  languageCode = 'pl'  # Built-in ✅
  title = 'DeaPrint'   # Built-in ✅
  
  [languages.pl.params]
    description = '...'  # Custom ❌ (must be in params)
    author = 'Kasia'     # Custom ❌
    keywords = '...'     # Custom ❌
```

**Access in templates**:
```html
{{ .Site.Params.description }}  → "Profesjonalne usługi..."
{{ .Site.Params.author }}       → "Kasia"
```

#### Quick Reference

| Field | Built-in? | Access via |
|-------|-----------|------------|
| `languageCode` | ✅ Yes | `.Language.LanguageCode` |
| `languageName` | ✅ Yes | `.Language.LanguageName` |
| `weight` | ✅ Yes | `.Language.Weight` |
| `title` | ✅ Yes | `.Site.Title` |
| `copyright` | ✅ Yes | `.Site.Copyright` |
| `description` | ❌ No | `.Site.Params.description` |
| `author` | ❌ No | `.Site.Params.author` |

### i18n Files (i18n/*.toml)

For **UI text** that appears in templates:

```toml
# i18n/pl.toml
[home]
other = "Strona główna"

[contact]
other = "Kontakt"
```

**Access in templates**:
```html
{{ i18n "home" }}     → "Strona główna"
{{ i18n "contact" }}  → "Kontakt"
```

### When to Use Which?

| Use Case | System | Example |
|----------|--------|----------|
| Site title | `hugo.toml` built-in | `title = 'DeaPrint'` |
| Meta description | `hugo.toml` params | `[languages.pl.params]` |
| Navigation links | `i18n/*.toml` | `[home]` |
| Button text | `i18n/*.toml` | `[submit]` |
| Form labels | `i18n/*.toml` | `[formName]` |
| Footer text | `i18n/*.toml` | `[allRightsReserved]` |

**Rule of thumb**:
- **hugo.toml**: Site configuration and metadata
- **i18n files**: Repeating UI text in templates

---

## File Structure

```
i18n/
├── pl.toml    # Polish translations
├── en.toml    # English translations
└── uk.toml    # Ukrainian translations
```

Each file contains translations for one language.

---

## TOML Format

### Basic Translation

```toml
[translationKey]
other = "Translated text"
```

**Parts**:
- `[translationKey]` - Unique identifier (use in templates)
- `other` - Default translation value

### Example

**i18n/pl.toml**:
```toml
[home]
other = "Strona główna"

[about]
other = "O nas"
```

**i18n/en.toml**:
```toml
[home]
other = "Home"

[about]
other = "About"
```

---

## Using in Templates

### Basic Usage

```html
{{ i18n "home" }}
```

**How it works**:
1. Hugo checks current page language (pl/en/uk)
2. Opens corresponding i18n file (pl.toml/en.toml/uk.toml)
3. Finds `[home]` key
4. Returns `other` value

**Output**:
- Polish page: `Strona główna`
- English page: `Home`
- Ukrainian page: `Головна`

### In Links

```html
<a href="/">{{ i18n "home" }}</a>
```

### In Attributes

```html
<button title="{{ i18n "contact" }}">Click</button>
```

---

## Current Translations

### Navigation

```toml
[home]
other = "Strona główna"  # Home

[about]
other = "O nas"  # About

[services]
other = "Usługi"  # Services

[contact]
other = "Kontakt"  # Contact

[language]
other = "Język"  # Language
```

### Footer

```toml
[phone]
other = "Telefon"  # Phone

[allRightsReserved]
other = "Wszelkie prawa zastrzeżone."  # All rights reserved
```

---

## Advanced Features

### With Variables

**i18n/pl.toml**:
```toml
[greeting]
other = "Witaj, {{ .Name }}!"
```

**Template**:
```html
{{ i18n "greeting" (dict "Name" "Kasia") }}
```

**Output**: `Witaj, Kasia!`

---

### Pluralization

**i18n/pl.toml**:
```toml
[readingTime]
one = "{{ .Count }} minuta"
few = "{{ .Count }} minuty"
other = "{{ .Count }} minut"
```

**Template**:
```html
{{ i18n "readingTime" .ReadingTime }}
```

**Output**:
- 1 minute: `1 minuta`
- 2-4 minutes: `2 minuty`
- 5+ minutes: `5 minut`

**Plural forms**:
- `one` - Singular (1)
- `two` - Two items (some languages)
- `few` - Few items (2-4 in Polish)
- `many` - Many items (some languages)
- `other` - Default/all other cases

---

### With Count

**i18n/pl.toml**:
```toml
[itemCount]
one = "{{ .Count }} element"
few = "{{ .Count }} elementy"
other = "{{ .Count }} elementów"
```

**Template**:
```html
{{ i18n "itemCount" (dict "Count" 5) }}
```

**Output**: `5 elementów`

---

## Adding New Translations

### Step 1: Add to All Language Files

**i18n/pl.toml**:
```toml
[newKey]
other = "Polish text"
```

**i18n/en.toml**:
```toml
[newKey]
other = "English text"
```

**i18n/uk.toml**:
```toml
[newKey]
other = "Ukrainian text"
```

### Step 2: Use in Template

```html
{{ i18n "newKey" }}
```

### Step 3: Restart Hugo

```bash
hugo server -D
```

---

## Best Practices

### 1. Use Descriptive Keys

**Good**:
```toml
[contactFormSubmit]
other = "Wyślij wiadomość"
```

**Bad**:
```toml
[btn1]
other = "Wyślij wiadomość"
```

### 2. Group Related Translations

```toml
# Navigation
[home]
other = "Strona główna"

[about]
other = "O nas"

# Forms
[formName]
other = "Imię"

[formEmail]
other = "Email"
```

### 3. Keep Keys Consistent

Use same key across all language files:
```toml
# pl.toml
[submit]
other = "Wyślij"

# en.toml
[submit]
other = "Submit"

# uk.toml
[submit]
other = "Надіслати"
```

### 4. Add Comments

```toml
# Button text for contact form
[contactSubmit]
other = "Wyślij wiadomość"
```

---

## Common Patterns

### Button Text

```toml
[learnMore]
other = "Dowiedz się więcej"

[readMore]
other = "Czytaj więcej"

[contactUs]
other = "Skontaktuj się"
```

### Form Labels

```toml
[formName]
other = "Imię"

[formEmail]
other = "Email"

[formMessage]
other = "Wiadomość"

[formSubmit]
other = "Wyślij"
```

### Status Messages

```toml
[success]
other = "Sukces!"

[error]
other = "Błąd"

[loading]
other = "Ładowanie..."
```

---

## Fallback Behavior

### Missing Translation

If translation key doesn't exist:
```html
{{ i18n "missingKey" }}
```

**Output**: `missingKey` (returns the key itself)

### Missing Language File

If language file doesn't exist, Hugo uses default language (Polish).

---

## File Relationships

### i18n Files → Templates

```
i18n/pl.toml (defines translations)
     ↓
layouts/partials/header.html (uses {{ i18n "home" }})
     ↓
Output: "Strona główna"
```

### Language Detection

```
User visits /en/about/
     ↓
Hugo detects language: en
     ↓
Opens i18n/en.toml
     ↓
{{ i18n "about" }} returns "About"
```

---

## Troubleshooting

### Translation Not Showing

**Check**:
1. Key exists in all language files
2. Key spelling matches exactly (case-sensitive)
3. TOML syntax is correct
4. Hugo server restarted

**Fix**:
```bash
# Restart Hugo
hugo server -D
```

### Wrong Language Displayed

**Check**:
1. URL has correct language prefix (/en/, /uk/)
2. `defaultContentLanguage` in hugo.toml
3. Content file in correct language folder

### TOML Syntax Error

**Error**:
```
Error: failed to load translations: toml: line 5: expected key separator '='
```

**Fix**: Check TOML syntax
```toml
# Wrong
[key]
"value"

# Correct
[key]
other = "value"
```

---

## Adding New Language

### Step 1: Create i18n File

```bash
touch i18n/de.toml
```

### Step 2: Add Translations

**i18n/de.toml**:
```toml
[home]
other = "Startseite"

[about]
other = "Über uns"
```

### Step 3: Configure in hugo.toml

```toml
[languages.de]
  languageCode = 'de'
  languageName = 'Deutsch'
  weight = 4
  title = 'DeaPrint - Professionelle Fotografie'
```

### Step 4: Create Content

```bash
mkdir -p content/de
```

---

## Summary

### i18n is:
- **Translation system** for multilingual sites
- **TOML files** in `i18n/` directory
- **Accessed** via `{{ i18n "key" }}` in templates

### Key Points:
- One file per language (pl.toml, en.toml, uk.toml)
- Use descriptive keys
- Keep keys consistent across languages
- Restart Hugo after changes

### For DeaPrint:
- 3 languages: Polish, English, Ukrainian
- Translations for navigation and footer
- Easy to add new translations

---

## Related Documentation

- [hugo-toml-config.md](hugo-toml-config.md) - Language configuration
- [Hugo i18n Documentation](https://gohugo.io/functions/i18n/)
