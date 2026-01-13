# Hugo Multilingual Configuration

See [hugo.toml](../../hugo.toml)

## URL Structure

```toml
defaultContentLanguage = 'pl'
```

**Result:**
- Polish (default): `/` (no `/pl/` prefix)
- English: `/en/`
- Ukrainian: `/uk/`

**To show `/pl/` prefix:**
```toml
defaultContentLanguageInSubdir = true
```

## Content Structure

```
content/
├── pl/                # Polish content
│   ├── _index.md
│   ├── services/
│   └── about/
├── en/                # English content
└── uk/                # Ukrainian content
```

**Don't create** `content/_index.md` at root.

## Language Configuration

```toml
[languages.pl]
  languageCode = 'pl'           # ISO code for browsers/SEO
  languageName = 'Polski'       # Display name for users
  weight = 1                    # Sort order
  contentDir = 'content/pl'     # CRITICAL - ensures correct language context
  title = 'DeaPrint - Profesjonalna Fotografia'
  [languages.pl.params]
    description = 'Profesjonalne usługi fotograficzne...'
```

### Language Identifier

`[languages.pl]` controls:
- URL: `/pl/`
- Content dir: `content/pl/`
- i18n file: `i18n/pl.toml`
- Template: `{{ .Language.Lang }}` → `pl`

### languageCode vs languageName

| Field | Value | Used For | Example |
|-------|-------|----------|---------|
| Identifier | `pl` | URLs, directories | `/pl/about/` |
| `languageCode` | `pl` | `<html lang="">`, SEO | `<html lang="pl">` |
| `languageName` | `Polski` | Language switcher | Button shows "Polski" |

### contentDir - CRITICAL

```toml
contentDir = 'content/pl'
```

Without it, Hugo may use wrong language context. **Always include** for each language.

### Built-in vs Custom Fields

**Built-in** (`.Site.Title`, `.Language.LanguageCode`):
- `languageCode`, `languageName`, `weight`, `contentDir`, `title`, `copyright`

**Custom** (`.Site.Params.description`):
- `description`, `author`, `keywords`

## translationKey

Links translations across languages.

```markdown
# content/pl/about.md
---
title: "O DeaPrint"
translationKey: about
---

# content/en/about.md
---
title: "About DeaPrint"
translationKey: about
---
```

**When needed:** Filenames differ (`o-nas.md` ↔ `about-us.md`)

**Best practice:** Always use for explicit linking.

## Language Switching

```html
{{ range .Site.Languages }}
  <a href='{{ .Lang | relLangURL }}'>{{ .LanguageName }}</a>
{{ end }}

{{ if .IsTranslated }}
  {{ range .Translations }}
    <a href='{{ .Permalink }}'>{{ .Language.LanguageName }}</a>
  {{ end }}
{{ end }}
```

## Adding New Language

1. Add to hugo.toml:
```toml
[languages.de]
  languageCode = 'de'
  languageName = 'Deutsch'
  weight = 4
  contentDir = 'content/de'
  title = 'DeaPrint - Professionelle Fotografie'
  [languages.de.params]
    description = '...'
```

2. Create directories:
```bash
mkdir -p content/de
touch i18n/de.toml
```
