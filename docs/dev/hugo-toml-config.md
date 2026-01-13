# Hugo Configuration (hugo.toml)

See [hugo.toml](../../hugo.toml)

Hugo's main configuration file that controls site settings, custom parameters, and build configuration.

## Base Configuration

```toml
baseURL = 'https://deaprint.pl/'     # Root URL (must end with /)
languageCode = 'pl'                  # Primary language (ISO 639-1)
title = 'DeaPrint - Professional Photography'
theme = ''                           # Empty = custom theme in layouts/
```

### disableKinds

```toml
disableKinds = ['taxonomy', 'term', 'RSS']
```

Disables categories, tags, and RSS feeds. Not needed for portfolio site.

**Keep enabled:** sitemap, robotsTXT, 404 (critical for SEO)

## Multilingual Configuration

```toml
defaultContentLanguage = 'pl'

[languages.pl]
  languageCode = 'pl'
  languageName = 'Polski'
  weight = 1
  contentDir = 'content/pl'
  title = 'DeaPrint - Profesjonalna Fotografia'
  [languages.pl.params]
    description = 'Profesjonalne usługi fotograficzne...'

[languages.en]
  languageCode = 'en'
  languageName = 'English'
  weight = 2
  contentDir = 'content/en'
  title = 'DeaPrint - Professional Photography'
  [languages.en.params]
    description = 'Professional photography services...'
```

**URL structure:**
- `/` → Polish (default)
- `/en/` → English
- `/uk/` → Ukrainian

## Custom Parameters

```toml
[params]
company = 'DeaPrint'
owner = 'Kasia'
address = 'Warmińska 20/47, 54-110 Wrocław'
phone = '882947647'                  # Numbers only
email = 'deaprint.kasia@gmail.com'
```

**Access in templates:**
```html
{{ .Site.Params.company }}
{{ .Site.Params.phone }}
<a href='tel:+48{{ .Site.Params.phone }}'>{{ .Site.Params.phone }}</a>
<a href='mailto:{{ .Site.Params.email }}'>{{ .Site.Params.email }}</a>
```

### Shared vs Language-Specific

**Shared** (same for all languages):
```toml
[params]
company = 'DeaPrint'
phone = '882947647'
email = 'deaprint.kasia@gmail.com'
```

**Language-specific** (translated):
```toml
[languages.pl]
title = 'DeaPrint - Profesjonalna Fotografia'
[languages.pl.params]
description = 'Profesjonalne usługi...'
```

## Markup Configuration

```toml
[markup.goldmark.renderer]
unsafe = true  # Allow HTML in markdown
```

**Use when:** Need to embed forms, videos, maps in markdown.

**Security:** Only enable if you control all content.

## Using in Templates

```html
{{ .Site.Title }}
{{ .Site.LanguageCode }}
{{ .Site.BaseURL }}
{{ .Site.Params.company }}
{{ .Language.Lang }}
{{ .Language.LanguageName }}
```

## Common Additions

**Social media:**
```toml
[params]
facebook = 'https://facebook.com/deaprint'
instagram = 'https://instagram.com/deaprint'
```

**Feature flags:**
```toml
[params]
showGallery = true
enableBooking = false
```

**Menu:**
```toml
[[menu.main]]
  name = 'Home'
  url = '/'
  weight = 1
```

## Validation

```bash
hugo config          # Display configuration
hugo server -D       # Test changes
```

## Troubleshooting

- `baseURL` must end with `/`
- Parameter names are case-sensitive
- Restart server after config changes
- Check syntax: `hugo config`
