# Hugo Multilingual Configuration

This document explains how Hugo handles multiple languages and URL structure.

---

## URL Structure for Languages

### Default Language Behavior

The `defaultContentLanguage` setting in `hugo.toml` determines URL structure:

```toml
defaultContentLanguage = 'pl'
```

**Result:**
- Polish (default): `http://localhost:1313/` (no `/pl/` prefix)
- English: `http://localhost:1313/en/`
- Ukrainian: `http://localhost:1313/uk/`

**Why?** Hugo omits the language prefix for the default language to provide cleaner URLs for your primary audience.

### Show Prefix for All Languages

To include `/pl/` prefix for Polish, add:

```toml
defaultContentLanguage = 'pl'
defaultContentLanguageInSubdir = true
```

**Result:**
- Polish: `http://localhost:1313/pl/`
- English: `http://localhost:1313/en/`
- Ukrainian: `http://localhost:1313/uk/`

**Recommendation:** Keep default behavior (no prefix for Polish) for cleaner URLs.

---

## Content Structure

### Language-Specific Directories

Each language has its own content directory:

```
content/
├── pl/                # Polish content
│   ├── _index.md      # Polish homepage
│   ├── services/
│   └── about/
├── en/                # English content
│   ├── _index.md      # English homepage
│   ├── services/
│   └── about/
└── uk/                # Ukrainian content
    ├── _index.md      # Ukrainian homepage
    ├── services/
    └── about/
```

### Root _index.md

**Do NOT create** `content/_index.md` at the root level.

**Why?** When you have language-specific versions (`content/pl/_index.md`, `content/en/_index.md`), the root file is unnecessary. Hugo uses `defaultContentLanguage` to redirect `/` to `/pl/`.

---

## Language Configuration

### hugo.toml Structure

```toml
baseURL = '/'
defaultContentLanguage = 'pl'

[languages]
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

  [languages.uk]
    languageCode = 'uk'
    languageName = 'Українська'
    weight = 3
    contentDir = 'content/uk'
    title = 'DeaPrint - Професійна Фотографія'
    [languages.uk.params]
      description = 'Професійні фотопослуги...'
```

### Language Configuration Fields Explained

#### Language Identifier: `[languages.pl]`

The identifier in brackets (e.g., `pl`, `en`, `uk`) controls:

**1. URL structure**:
```
/pl/           ← from [languages.pl]
/en/           ← from [languages.en]
/uk/           ← from [languages.uk]
```

**2. Content directories**:
```
content/pl/    ← matches [languages.pl]
content/en/    ← matches [languages.en]
content/uk/    ← matches [languages.uk]
```

**3. i18n files**:
```
i18n/pl.toml   ← matches [languages.pl]
i18n/en.toml   ← matches [languages.en]
i18n/uk.toml   ← matches [languages.uk]
```

**4. Template variable**:
```html
{{ .Language.Lang }}  → returns "pl", "en", or "uk"
```

**Convention**: Use ISO language codes as identifiers for consistency and shorter URLs.

---

#### `languageCode` vs `languageName`

```toml
[languages.pl]              # Identifier for Hugo (URLs, directories)
  languageCode = 'pl'       # ISO code for browsers/SEO
  languageName = 'Polski'   # Display name for users
```

| Field          | Value    | Used For                             | Example Output        |
|----------------|----------|--------------------------------------|-----------------------|
| Identifier     | `pl`     | URLs, directories, `.Language.Lang`  | `/pl/about/`          |
| `languageCode` | `pl`     | `<html lang="">`, SEO, accessibility | `<html lang="pl">`    |
| `languageName` | `Polski` | Language switcher, UI display        | Button shows "Polski" |

**Why both are needed**:
- `languageCode`: Technical (browsers, search engines understand `pl`)
- `languageName`: Human-readable (users see "Polski", not "pl")

**In templates**:
```html
<html lang="{{ .Language.LanguageCode }}">  <!-- <html lang="pl"> -->
<button>{{ .Language.LanguageName }}</button>  <!-- <button>Polski</button> -->
```

---

#### `contentDir` - CRITICAL Field

```toml
[languages.pl]
  contentDir = 'content/pl'  # Tells Hugo where Polish content is
```

**Purpose**: Explicitly tells Hugo which directory contains content for this language.

**Why it's critical**:
- Without it, Hugo may use wrong language context when rendering pages
- English pages might show Polish menus/translations
- Each language page must know its own language context

**Symptom of missing `contentDir`**:
- `/en/about/` shows `<html lang="pl">` instead of `<html lang="en">`
- English pages display Polish menu text
- `.Language.Lang` returns wrong language

**Always include `contentDir` for each language** to ensure correct language context.

---

#### Built-in vs Custom Fields

**Built-in fields** (accessed via `.Site.Title`, `.Language.LanguageCode`):
- `languageCode` - ISO language code
- `languageName` - Display name
- `weight` - Sort order (lower = first)
- `contentDir` - Content directory path (CRITICAL)
- `title` - Site title for this language
- `copyright` - Copyright notice

**Custom fields** (accessed via `.Site.Params.description`):
- `description` - Meta description
- `author` - Author name
- `keywords` - SEO keywords
- Any other custom parameters

**Example**:
```toml
[languages.pl]
  languageCode = 'pl'        # Built-in ✅
  languageName = 'Polski'    # Built-in ✅
  contentDir = 'content/pl'  # Built-in ✅ (CRITICAL)
  title = 'DeaPrint'         # Built-in ✅
  weight = 1                 # Built-in ✅
  [languages.pl.params]
    description = '...'      # Custom ❌ (must be in params)
    author = 'Kasia'         # Custom ❌
```

---

## Linking Translations with translationKey

### What is translationKey?

**`translationKey`** in frontmatter links translations of the same page across different languages.

**Example**:
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

# content/uk/about.md
---
title: "Про DeaPrint"
translationKey: about
---
```

All three files share `translationKey: about`, so Hugo knows they're the same page in different languages.

---

### When You Need It

**Don't need it** when filenames match:
```
content/pl/about.md
content/en/about.md
content/uk/about.md
```
Hugo automatically links these by filename.

**Need it** when filenames differ:
```
content/pl/o-nas.md          (translationKey: about)
content/en/about-us.md       (translationKey: about)
content/uk/pro-nas.md        (translationKey: about)
```
Hugo links them by the shared `translationKey`.

**Best practice**: Always use `translationKey` for explicit linking, even when filenames match. Prevents issues if you rename files later.

---

### How It Works

**Without translationKey**:
- Hugo matches by filename only
- `about.md` ↔ `about.md` ✅
- `o-nas.md` ↔ `about.md` ❌

**With translationKey**:
- Hugo matches by the key value
- Any filename works as long as `translationKey` matches
- More flexible and explicit

---

## Language Switching

### In Templates

```html
{{ range .Site.Languages }}
  <a href="{{ .Lang | relLangURL }}">{{ .LanguageName }}</a>
{{ end }}
```

### Current Page in Other Language

```html
{{ if .IsTranslated }}
  {{ range .Translations }}
    <a href="{{ .Permalink }}">{{ .Language.LanguageName }}</a>
  {{ end }}
{{ end }}
```

**Note**: `.IsTranslated` and `.Translations` rely on `translationKey` or matching filenames to find related pages.

---

### Language Switcher Example

Your header uses `translationKey` to preserve page context:

```html
{{ if $.File }}
  <li><a href='/{{ .Lang }}/{{ $.File.TranslationBaseName }}/'>{{ .LanguageName }}</a></li>
{{ end }}
```

**`$.File.TranslationBaseName`** returns:
- Filename base (if no `translationKey`): `about`
- Or the `translationKey` value: `about`

**Result**: User on `/pl/about/` clicking "English" goes to `/en/about/`, not homepage.

---

## Adding New Language

### Step 1: Add to hugo.toml

```toml
[languages.de]
  languageCode = 'de'
  languageName = 'Deutsch'
  weight = 4
  contentDir = 'content/de'  # Don't forget this!
  title = 'DeaPrint - Professionelle Fotografie'
  [languages.de.params]
    description = 'Professionelle Fotografie-Dienstleistungen...'
```

### Step 2: Create Content Directory

```bash
mkdir -p content/de
```

### Step 3: Create i18n File

```bash
touch i18n/de.toml
```

### Step 4: Add Translations

See `docs/dev/hugo-translation.md` for i18n details.

---

## Summary

- Default language (Polish) has no URL prefix: `/`
- Other languages have prefixes: `/en/`, `/uk/`
- Each language has its own `content/XX/` directory
- **CRITICAL**: Always set `contentDir` for each language to ensure correct language context
- Don't create `content/_index.md` at root
- Use `defaultContentLanguageInSubdir = true` to show `/pl/` prefix
- Use `translationKey` in frontmatter to link translations explicitly
