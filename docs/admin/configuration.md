# Configuration Parameters

This document explains all configuration parameters in `hugo.toml` and how to modify them.

## File Location

`/home/devlin/src/github/dea-web/hugo.toml`

---

## Base Configuration

### `baseURL`
```toml
baseURL = 'https://deaprint.pl/'
```

**Purpose**: The root URL of your website

**When to change**: 
- When deploying to a different domain
- When using a subdomain or subdirectory

**Examples**:
```toml
baseURL = 'https://www.deaprint.pl/'      # www subdomain
baseURL = 'https://deaprint.pl/photos/'   # subdirectory
baseURL = 'http://localhost:1313/'        # local development (auto-set)
```

**Important**: Must end with `/`

---

### `languageCode`
```toml
languageCode = 'pl'
```

**Purpose**: Primary language of the site (ISO 639-1 code)

**When to change**: When changing the main site language

**Options**:
- `'pl'` - Polish
- `'en'` - English
- `'uk'` - Ukrainian

**Impact**: Affects HTML `lang` attribute, SEO, and browser behavior

---

### `title`
```toml
title = 'DeaPrint - Professional Photography'
```

**Purpose**: Site title (appears in browser tab, search results)

**When to change**: When rebranding or changing site focus

**Usage in templates**: `{{ .Site.Title }}`

**SEO Impact**: High - used in search results and social media shares

---

### `theme`
```toml
theme = ''
```

**Purpose**: Name of Hugo theme to use

**Current value**: Empty (custom theme in `layouts/`)

**When to change**: If switching to a third-party theme

**Example**:
```toml
theme = 'ananke'  # Would use themes/ananke/
```

---

## Custom Parameters (`[params]`)

Custom parameters accessible in templates via `{{ .Site.Params.paramName }}`

### `company`
```toml
[params]
company = 'DeaPrint'
```

**Purpose**: Company/business name

**Usage**: Footer, contact forms, legal pages

**Template access**: `{{ .Site.Params.company }}`

---

### `owner`
```toml
owner = 'Kasia'
```

**Purpose**: Business owner name

**Usage**: About page, contact information

**Template access**: `{{ .Site.Params.owner }}`

---

### `address`
```toml
address = 'Warmińska 20/47, 54-110 Wrocław'
```

**Purpose**: Physical business address

**Usage**: 
- Contact page
- Footer
- Schema.org structured data
- Google Maps integration

**Template access**: `{{ .Site.Params.address }}`

**Format**: Street, postal code, city

---

### `phone`
```toml
phone = '882947647'
```

**Purpose**: Contact phone number

**Usage**: 
- Contact page
- Click-to-call links
- Schema.org structured data

**Template access**: `{{ .Site.Params.phone }}`

**Format**: Numbers only (no spaces or dashes)

**In templates**:
```html
<a href="tel:+48{{ .Site.Params.phone }}">{{ .Site.Params.phone }}</a>
```

---

### `email`
```toml
email = 'deaprint.kasia@gmail.com'
```

**Purpose**: Contact email address

**Usage**:
- Contact page
- Mailto links
- Schema.org structured data

**Template access**: `{{ .Site.Params.email }}`

**In templates**:
```html
<a href="mailto:{{ .Site.Params.email }}">{{ .Site.Params.email }}</a>
```

---

### `description`
```toml
description = 'Professional photography services in Wrocław. Document photos, prints, photobooks, and calendars.'
```

**Purpose**: Site description for SEO and social media

**Usage**:
- Meta description tag
- Open Graph description
- Search engine results

**Template access**: `{{ .Site.Params.description }}`

**Best practices**:
- 150-160 characters
- Include keywords
- Describe what you offer
- Include location

---

## Markup Configuration (`[markup]`)

### `unsafe`
```toml
[markup]
  [markup.goldmark]
    [markup.goldmark.renderer]
      unsafe = true
```

**Purpose**: Allow raw HTML in Markdown files

**Default**: `false` (HTML is escaped)

**When to use `true`**:
- Need custom HTML in content
- Embedding forms, videos, widgets
- Advanced styling in content

**Security note**: Only enable if you control all content. Don't enable if users can submit content.

**Example usage in Markdown**:
```markdown
# My Page

<div class="custom-box">
  This HTML will render
</div>
```

---

## Adding New Parameters

### How to add custom parameters

1. Add to `[params]` section:
```toml
[params]
  facebook = 'https://facebook.com/deaprint'
  instagram = 'https://instagram.com/deaprint'
  openingHours = 'Mon-Fri: 9:00-17:00'
```

2. Access in templates:
```html
<a href="{{ .Site.Params.facebook }}">Facebook</a>
<a href="{{ .Site.Params.instagram }}">Instagram</a>
<p>{{ .Site.Params.openingHours }}</p>
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

### Multiple config files (optional)

Create environment-specific configs:

```
hugo.toml                    # Base config
config/production/hugo.toml  # Production overrides
config/development/hugo.toml # Development overrides
```

---

## Common Configuration Tasks

### Change site URL
```toml
baseURL = 'https://new-domain.com/'
```

### Add social media links
```toml
[params]
  facebook = 'https://facebook.com/deaprint'
  instagram = 'https://instagram.com/deaprint'
  twitter = 'https://twitter.com/deaprint'
```

### Update contact information
```toml
[params]
  phone = '123456789'
  email = 'new@email.com'
  address = 'New Address, City'
```

### Enable/disable features
```toml
[params]
  showGallery = true
  enableBooking = false
  showPricing = true
```

---

## Configuration Validation

### Check configuration
```bash
hugo config
```

### Test configuration changes
```bash
# Start server and check for errors
hugo server -D

# Build and check for errors
hugo
```

---

## Best Practices

1. **Comments**: Add comments to explain non-obvious settings
```toml
# Allow HTML in markdown for embedded forms
unsafe = true
```

2. **Quotes**: Use single quotes for strings
```toml
title = 'DeaPrint'  # Good
title = "DeaPrint"  # Also works
```

3. **Backup**: Commit `hugo.toml` to Git before major changes

4. **Testing**: Test configuration changes locally before deploying

5. **Documentation**: Document custom parameters in this file

---

## Troubleshooting

### Site not loading correctly
- Check `baseURL` ends with `/`
- Verify `baseURL` matches deployment URL

### Templates not finding parameters
- Check parameter name spelling
- Verify parameter is in `[params]` section
- Use `{{ .Site.Params.paramName }}` (case-sensitive)

### HTML not rendering in Markdown
- Set `unsafe = true` in `[markup.goldmark.renderer]`
- Restart Hugo server after config changes

### Changes not appearing
- Restart Hugo server: `Ctrl+C` then `hugo server -D`
- Clear browser cache
- Check for syntax errors: `hugo config`

---

## Related Documentation

- [Hugo Configuration Docs](https://gohugo.io/getting-started/configuration/)
- [Hugo Parameters](https://gohugo.io/variables/site/#site-params)
- [Markup Configuration](https://gohugo.io/getting-started/configuration-markup/)
