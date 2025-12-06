# Hugo Configuration (hugo.toml)

This document explains the `hugo.toml` file - Hugo's main configuration file that controls your entire website.

**File Location**: `/home/devlin/src/github/dea-web/hugo.toml`

---

## What is hugo.toml?

The **brain of your Hugo site** that defines:
1. Site settings (URL, language, title)
2. Custom parameters (business info, contact details)
3. Build configuration (markdown processing, themes)

**Format**: TOML (Tom's Obvious, Minimal Language)
**Comments**: Use `#` for comments

---

## Base Configuration

### baseURL

```toml
baseURL = 'https://deaprint.pl/'
```

**Purpose**: Root URL of your website

**Used for**:
- Generating absolute URLs
- Sitemaps and RSS feeds
- Social media sharing
- Canonical URLs

**Important**:
- Must end with `/`
- Change when deploying to different domain
- Affects all generated links

**Examples**:

```toml
baseURL = 'https://deaprint.pl/'           # Production
baseURL = 'https://staging.deaprint.pl/'   # Staging
baseURL = 'http://localhost:1313/'         # Local (auto-set by hugo server)
```

**In templates**:
```html
{{ .Site.BaseURL }}  → https://deaprint.pl/
```

---

### languageCode

```toml
languageCode = 'pl'
```

**Purpose**: Primary language of the site (ISO 639-1 code)

**Used for**:
- HTML `lang` attribute
- SEO and search engines
- Browser behavior
- Screen readers

**Options**:
- `pl` - Polish
- `en` - English
- `uk` - Ukrainian

**Impact**:
```html
<html lang="pl">  <!-- Generated from languageCode -->
```

**In templates**:
```html
{{ .Site.LanguageCode }}  → pl
```

---

### title

```toml
title = 'DeaPrint - Professional Photography'
```

**Purpose**: Site title

**Used for**:
- Browser tab title
- Search results
- Social media shares
- Navigation
- Footer

**SEO Impact**: High - appears in search results

**In templates**:
```html
<title>{{ .Site.Title }}</title>
<!-- Output: <title>DeaPrint - Professional Photography</title> -->
```

**Page-specific titles**:
```html
<title>{{ .Title }} - {{ .Site.Title }}</title>
<!-- Output: <title>About - DeaPrint - Professional Photography</title> -->
```

---

### theme

```toml
theme = ''  # We'll build custom theme
```

**Purpose**: Name of Hugo theme to use

**Current value**: Empty (custom theme)

**How it works**:
- Empty = Use `layouts/` directory for templates
- `theme = 'ananke'` = Use `themes/ananke/` directory

**For DeaPrint**: Building custom theme, so empty

**If using third-party theme**:
```toml
theme = 'ananke'
```
Then install theme:
```bash
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
```

---

## Multilingual Configuration

### defaultContentLanguage

```toml
defaultContentLanguage = 'pl'
```

**Purpose**: Sets the default language for the site

**Used for**:
- Fallback when language not specified
- Root URL content (`/` instead of `/pl/`)
- Default language in templates

### Languages Configuration

```toml
[languages]
  [languages.pl]
    languageCode = 'pl'
    languageName = 'Polski'
    weight = 1
    title = 'DeaPrint - Profesjonalna Fotografia'
    [languages.pl.params]
      description = 'Profesjonalne usługi fotograficzne we Wrocławiu. Zdjęcia do dokumentów, wydruki, fotoksiążki i kalendarze.'

  [languages.en]
    languageCode = 'en'
    languageName = 'English'
    weight = 2
    title = 'DeaPrint - Professional Photography'
    [languages.en.params]
      description = 'Professional photography services in Wrocław. Document photos, prints, photobooks, and calendars.'

  [languages.uk]
    languageCode = 'uk'
    languageName = 'Українська'
    weight = 3
    title = 'DeaPrint - Професійна Фотографія'
    [languages.uk.params]
      description = 'Професійні фотопослуги у Вроцлаві. Фото на документи, друк, фотокниги та календарі.'
```

**How it works**:

### languageCode
- ISO 639-1 code for the language
- Used in HTML `lang` attribute
- Affects SEO and browser behavior

### languageName
- Display name for language switcher
- Shown to users in navigation

### weight
- Order in language switcher (lower = first)
- Polish (1), English (2), Ukrainian (3)

### title (language-specific)
- Different title for each language
- Automatically used based on current language

### description (language-specific)
- Different description for each language
- Used in meta tags and SEO

**URL Structure**:
```
https://deaprint.pl/          → Polish (default)
https://deaprint.pl/en/       → English
https://deaprint.pl/uk/       → Ukrainian
```

**Content Structure**:
```
content/
├── pl/
│   ├── _index.md
│   ├── about/
│   └── services/
├── en/
│   ├── _index.md
│   ├── about/
│   └── services/
└── uk/
    ├── _index.md
    ├── about/
    └── services/
```

**In templates**:
```html
<!-- Current language title -->
<title>{{ .Site.Title }}</title>
<!-- Polish: DeaPrint - Profesjonalna Fotografia -->
<!-- English: DeaPrint - Professional Photography -->
<!-- Ukrainian: DeaPrint - Професійна Фотографія -->

<!-- Current language description -->
<meta name="description" content="{{ .Site.Params.description }}">

<!-- Language switcher -->
{{ range .Site.Languages }}
  <a href="{{ .Lang }}/">{{ .LanguageName }}</a>
{{ end }}

<!-- Current language code -->
<html lang="{{ .Site.LanguageCode }}">
```

---

## Custom Parameters [params]

```toml
[params]
```

This section holds **custom variables** you define. Access with `{{ .Site.Params.variableName }}`.

**Why use params?**
- Centralize configuration
- Change once, updates everywhere
- No hardcoding in templates
- Easy maintenance

---

### company

```toml
company = 'DeaPrint'
```

**Purpose**: Company/business name

**Used in**:
- Footer
- Contact forms
- Legal pages
- Schema.org structured data

**In templates**:
```html
<p>© {{ now.Year }} {{ .Site.Params.company }}</p>
<!-- Output: © 2024 DeaPrint -->
```

---

### owner

```toml
owner = 'Kasia'
```

**Purpose**: Business owner name

**Used in**:
- About page
- Contact information
- Personal bio

**In templates**:
```html
<p>Owner: {{ .Site.Params.owner }}</p>
<!-- Output: Owner: Kasia -->
```

---

### address

```toml
address = 'Warmińska 20/47, 54-110 Wrocław'
```

**Purpose**: Physical business address

**Used in**:
- Contact page
- Footer
- Google Maps integration
- Schema.org structured data
- Local SEO

**In templates**:
```html
<address>{{ .Site.Params.address }}</address>
<!-- Output: <address>Warmińska 20/47, 54-110 Wrocław</address> -->
```

**For Google Maps**:
```html
<iframe src="https://maps.google.com/maps?q={{ .Site.Params.address | urlize }}"></iframe>
```

---

### phone

```toml
phone = '882947647'
```

**Purpose**: Contact phone number

**Format**: Numbers only (no spaces, dashes, parentheses)

**Why no formatting?**
- Easier to process in templates
- Add formatting when displaying
- Works with tel: links

**Used in**:
- Contact page
- Click-to-call links
- Schema.org structured data

**In templates**:
```html
<!-- Click-to-call link -->
<a href="tel:+48{{ .Site.Params.phone }}">
  {{ .Site.Params.phone }}
</a>
<!-- Output: <a href="tel:+48882947647">882947647</a> -->

<!-- Formatted display -->
<a href="tel:+48{{ .Site.Params.phone }}">
  +48 {{ substr .Site.Params.phone 0 3 }} {{ substr .Site.Params.phone 3 3 }} {{ substr .Site.Params.phone 6 3 }}
</a>
<!-- Output: +48 882 947 647 -->
```

---

### email

```toml
email = 'deaprint.kasia@gmail.com'
```

**Purpose**: Contact email address

**Used in**:
- Contact page
- Mailto links
- Schema.org structured data
- Contact forms

**In templates**:
```html
<a href="mailto:{{ .Site.Params.email }}">
  {{ .Site.Params.email }}
</a>
<!-- Output: <a href="mailto:deaprint.kasia@gmail.com">deaprint.kasia@gmail.com</a> -->
```

---

### Shared vs Language-Specific Parameters

**Shared parameters** (same across all languages):
```toml
[params]
company = 'DeaPrint'
owner = 'Kasia'
address = 'Warmińska 20/47, 54-110 Wrocław'
phone = '882947647'
email = 'deaprint.kasia@gmail.com'
```

**Language-specific parameters** (different per language):
```toml
[languages.pl]
title = 'DeaPrint - Profesjonalna Fotografia'
[languages.pl.params]
description = 'Profesjonalne usługi fotograficzne...'

[languages.en]
title = 'DeaPrint - Professional Photography'
[languages.en.params]
description = 'Professional photography services...'
```

**Why this approach?**
- Contact info (phone, email, address) stays consistent
- Marketing text (title, description) is translated
- Easy to maintain
- No duplication of contact details

**In templates**:
```html
<!-- Language-specific (auto-translated) -->
<title>{{ .Site.Title }}</title>
<meta name="description" content="{{ .Site.Params.description }}">

<!-- Shared (same for all languages) -->
<p>{{ .Site.Params.company }}</p>
<a href="tel:+48{{ .Site.Params.phone }}">{{ .Site.Params.phone }}</a>
<a href="mailto:{{ .Site.Params.email }}">{{ .Site.Params.email }}</a>
```

---

## Markup Configuration [markup]

```toml
[markup]
[markup.goldmark]
[markup.goldmark.renderer]
unsafe = true  # Allow HTML in markdown
```

### What is Goldmark?

**Goldmark** = Hugo's markdown processor (converts .md to HTML)

### unsafe Setting

**`unsafe = true`** (current setting):
- ✅ HTML in markdown files will render
- ✅ Can use `<div>`, `<iframe>`, `<form>`, custom HTML
- ⚠️ Security risk if users can submit content

**`unsafe = false`** (default):
- ❌ HTML in markdown is escaped (shows as text)
- ✅ More secure
- ❌ Can't embed forms, videos, custom HTML

### When to Use unsafe = true

**Use when**:
- You control all content
- Need to embed forms, videos, maps
- Want custom HTML in markdown
- Building custom layouts

**Example with unsafe = true**:
```markdown
# My Page

<div class="custom-box bg-blue-500 p-4">
  This HTML will render with Tailwind classes
</div>

<iframe src="https://maps.google.com/..."></iframe>
```

**Don't use when**:
- Users can submit content (blog comments, etc.)
- Security is critical
- Don't need HTML in markdown

### Other Markup Options

```toml
[markup.goldmark.renderer]
unsafe = true
hardWraps = false  # Don't convert line breaks to <br>

[markup.highlight]
style = 'monokai'  # Code syntax highlighting theme
lineNos = true     # Show line numbers in code blocks
```

---

## TOML Syntax

### Comments

```toml
# Full line comment
key = 'value'  # Inline comment
```

### Strings

```toml
title = 'Single quotes'
title = "Double quotes"
title = '''Multi-line
string'''
```

### Numbers

```toml
port = 1313
timeout = 30.5
```

### Booleans

```toml
unsafe = true
draft = false
```

### Arrays

```toml
keywords = ['photography', 'wroclaw', 'portraits']
```

### Tables (Sections)

```toml
[params]
company = 'DeaPrint'

[markup]
defaultMarkdownHandler = 'goldmark'
```

---

## Using Configuration in Templates

### Access Site Config

```html
{{ .Site.Title }}           → DeaPrint - Professional Photography
{{ .Site.LanguageCode }}    → pl
{{ .Site.BaseURL }}         → https://deaprint.pl/
```

### Access Custom Params

```html
{{ .Site.Params.company }}      → DeaPrint
{{ .Site.Params.owner }}        → Kasia
{{ .Site.Params.phone }}        → 882947647
{{ .Site.Params.email }}        → deaprint.kasia@gmail.com
{{ .Site.Params.address }}      → Warmińska 20/47, 54-110 Wrocław
{{ .Site.Params.description }}  → Professional photography...
```

### Example: Footer Template

```html
<footer class="footer footer-center p-10 bg-base-200">
  <div>
    <p class="font-bold">{{ .Site.Params.company }}</p>
    <p>{{ .Site.Params.address }}</p>
    <p>Phone: <a href="tel:+48{{ .Site.Params.phone }}">{{ .Site.Params.phone }}</a></p>
    <p>Email: <a href="mailto:{{ .Site.Params.email }}">{{ .Site.Params.email }}</a></p>
  </div>
  <div>
    <p>© {{ now.Year }} {{ .Site.Params.company }}. All rights reserved.</p>
  </div>
</footer>
```

---

## Common Additions

### Social Media Links

```toml
[params]
facebook = 'https://facebook.com/deaprint'
instagram = 'https://instagram.com/deaprint'
twitter = 'https://twitter.com/deaprint'
```

### Feature Flags

```toml
[params]
showGallery = true
enableBooking = false
showPricing = true
```

### SEO Settings

```toml
[params]
googleAnalytics = 'G-XXXXXXXXXX'
googleSiteVerification = 'xxxxx'
```

### Menu Configuration

```toml
[menu]
[[menu.main]]
  name = 'Home'
  url = '/'
  weight = 1

[[menu.main]]
  name = 'About'
  url = '/about/'
  weight = 2

[[menu.main]]
  name = 'Services'
  url = '/services/'
  weight = 3

[[menu.main]]
  name = 'Contact'
  url = '/contact/'
  weight = 4
```

**In templates**:
```html
<nav>
  {{ range .Site.Menus.main }}
    <a href="{{ .URL }}">{{ .Name }}</a>
  {{ end }}
</nav>
```

---

## Environment-Specific Configuration

### Development vs Production

Hugo automatically detects environment. Override with:

```bash
# Development
hugo server -D

# Production
hugo --environment production
```

### Multiple Config Files

Create environment-specific configs:

```
hugo.toml                    # Base config
config/
├── production/
│   └── hugo.toml           # Production overrides
└── development/
    └── hugo.toml           # Development overrides
```

**Example production override**:
```toml
# config/production/hugo.toml
baseURL = 'https://deaprint.pl/'
[params]
googleAnalytics = 'G-XXXXXXXXXX'
```

**Example development override**:
```toml
# config/development/hugo.toml
baseURL = 'http://localhost:1313/'
```

---

## Configuration Validation

### Check Configuration

```bash
# Display current configuration
hugo config

# Check for errors
hugo --debug
```

### Test Configuration Changes

```bash
# Start server and check for errors
hugo server -D

# Build and check for errors
hugo
```

---

## Best Practices

### 1. Use Comments

```toml
# Production URL - change for staging/dev
baseURL = 'https://deaprint.pl/'

# Allow HTML in markdown for:
# - Contact forms
# - Google Maps embeds
# - Custom styling
unsafe = true
```

### 2. Centralize Values

**Bad** (hardcoded):
```html
<p>DeaPrint</p>
<p>882947647</p>
```

**Good** (centralized):
```html
<p>{{ .Site.Params.company }}</p>
<p>{{ .Site.Params.phone }}</p>
```

### 3. Group Related Settings

```toml
# Business Information
[params]
company = 'DeaPrint'
owner = 'Kasia'

# Contact Details
address = 'Warmińska 20/47, 54-110 Wrocław'
phone = '882947647'
email = 'deaprint.kasia@gmail.com'

# Social Media
facebook = 'https://facebook.com/deaprint'
instagram = 'https://instagram.com/deaprint'
```

### 4. Document Non-Obvious Settings

```toml
# Phone number without formatting for easier processing
# Format in templates when displaying
phone = '882947647'
```

### 5. Backup Before Major Changes

```bash
git add hugo.toml
git commit -m "config: backup before changes"
```

---

## Troubleshooting

### Site Not Loading Correctly

**Check**:
- `baseURL` ends with `/`
- `baseURL` matches deployment URL

### Templates Not Finding Parameters

**Check**:
- Parameter name spelling (case-sensitive)
- Parameter is in `[params]` section
- Using correct syntax: `{{ .Site.Params.paramName }}`

### HTML Not Rendering in Markdown

**Solution**:
- Set `unsafe = true` in `[markup.goldmark.renderer]`
- Restart Hugo server after config changes

### Changes Not Appearing

**Solution**:
- Restart Hugo server: `Ctrl+C` then `hugo server -D`
- Clear browser cache
- Check for syntax errors: `hugo config`

---

## Summary

### hugo.toml is:

1. **Site settings**: URL, language, title
2. **Custom variables**: Business info, contact details
3. **Build configuration**: Markdown processing, themes

### Key Concepts:

- **Centralize configuration** instead of hardcoding
- **Access in templates**: `{{ .Site.Title }}` or `{{ .Site.Params.phone }}`
- **Use comments** to document settings
- **Test changes** before deploying

### Related Files:

- `package.json` - npm configuration
- `postcss.config.js` - CSS processing
- `layouts/` - HTML templates that use these values

---

## Related Documentation

- [configuration.md](../admin/configuration.md) - Detailed parameter documentation
- [hugo-structure.md](hugo-structure.md) - Hugo directory structure
- [project-manifest.md](project-manifest.md) - package.json explanation
