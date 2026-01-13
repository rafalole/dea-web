# Hugo Translation System (i18n)

See [i18n/](../../i18n/)

Hugo's system for managing UI text translations across languages.

## Two Translation Systems

### 1. Language Configuration (`hugo.toml`)

Site-level settings and metadata.

**Built-in fields:**
```toml
[languages.pl]
  languageCode = 'pl'           # ISO code
  languageName = 'Polski'       # Display name
  weight = 1                    # Sort order
  title = 'Site Title'          # Site title
  contentDir = 'content/pl'     # Content directory
```

**Custom parameters:**
```toml
[languages.pl.params]
  description = '...'           # Must be in params
  author = 'Kasia'
```

**Access:**
```html
{{ .Language.LanguageCode }}  → "pl"
{{ .Site.Title }}             → "DeaPrint - Profesjonalna Fotografia"
{{ .Site.Params.description }} → "Profesjonalne usługi..."
```

### 2. i18n Files (`i18n/*.toml`)

UI text in templates.

```toml
# i18n/pl.toml
[home]
other = "Strona główna"

[contact]
other = "Kontakt"
```

**Access:**
```html
{{ i18n "home" }}     → "Strona główna"
{{ i18n "contact" }}  → "Kontakt"
```

### When to Use Which?

| Use Case | System | Example |
|----------|--------|---------|
| Site title | `hugo.toml` built-in | `title = 'DeaPrint'` |
| Meta description | `hugo.toml` params | `[languages.pl.params]` |
| Navigation links | `i18n/*.toml` | `[home]` |
| Button text | `i18n/*.toml` | `[submit]` |
| Form labels | `i18n/*.toml` | `[formName]` |

## File Structure

```
i18n/
├── pl.toml    # Polish translations
├── en.toml    # English translations
└── uk.toml    # Ukrainian translations
```

## TOML Format

```toml
[translationKey]
other = "Translated text"
```

**Example:**
```toml
# i18n/pl.toml
[home]
other = "Strona główna"

[about]
other = "O nas"
```

## Using in Templates

```html
{{ i18n "home" }}
<a href='/'>{{ i18n "home" }}</a>
<button title='{{ i18n "contact" }}'>Click</button>
```

## Advanced Features

**With variables:**
```toml
[greeting]
other = "Witaj, {{ .Name }}!"
```

```html
{{ i18n "greeting" (dict "Name" "Kasia") }}
```

**Pluralization:**
```toml
[readingTime]
one = "{{ .Count }} minuta"
few = "{{ .Count }} minuty"
other = "{{ .Count }} minut"
```

```html
{{ i18n "readingTime" .ReadingTime }}
```

## Adding Translations

1. Add to all language files:
```toml
# i18n/pl.toml
[newKey]
other = "Polish text"

# i18n/en.toml
[newKey]
other = "English text"
```

2. Use in template:
```html
{{ i18n "newKey" }}
```

3. Restart Hugo:
```bash
hugo server -D
```

## Best Practices

**Use descriptive keys:**
```toml
[contactFormSubmit]
other = "Wyślij wiadomość"
```

**Group related translations:**
```toml
# Navigation
[home]
other = "Strona główna"

# Forms
[formName]
other = "Imię"
```

**Keep keys consistent** across all language files.

## Common Patterns

**Buttons:**
```toml
[learnMore]
other = "Dowiedz się więcej"

[contactUs]
other = "Skontaktuj się"
```

**Forms:**
```toml
[formName]
other = "Imię"

[formEmail]
other = "Email"

[formSubmit]
other = "Wyślij"
```

## Troubleshooting

**Translation not showing:**
1. Key exists in all language files
2. Key spelling matches (case-sensitive)
3. TOML syntax correct
4. Hugo server restarted

**Wrong language:**
- Check URL has correct prefix (`/en/`, `/uk/`)
- Check `defaultContentLanguage` in hugo.toml

## Adding New Language

```bash
touch i18n/de.toml
```

```toml
# i18n/de.toml
[home]
other = "Startseite"
```

Configure in hugo.toml (see hugo-multilingual.md).
