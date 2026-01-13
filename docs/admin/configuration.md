# Configuration Parameters

See [hugo.toml](../../hugo.toml)

## Base Configuration

### `baseURL = 'https://deaprint.pl/'`
Root URL of the website. Must end with `/`. Change when deploying to different domain.

### `languageCode = 'pl'`
Primary language (ISO 639-1). Options: `pl`, `en`, `uk`. Affects HTML `lang` attribute and SEO.

### `title = 'DeaPrint - Professional Photography'`
Site title for browser tabs and search results. Access: `{{ .Site.Title }}`

### `theme = ''`
Empty = custom theme in `layouts/`. Set to theme name if using third-party theme.

## Custom Parameters

Access in templates: `{{ .Site.Params.paramName }}`

```toml
[params]
  company = 'DeaPrint'
  owner = 'Kasia'
  address = 'Warmińska 20/47, 54-110 Wrocław'
  phone = '882947647'  # Numbers only
  email = 'deaprint.kasia@gmail.com'
  description = 'Professional photography services in Wrocław...'
```

**Usage examples:**
```html
<a href='tel:+48{{ .Site.Params.phone }}'>{{ .Site.Params.phone }}</a>
<a href='mailto:{{ .Site.Params.email }}'>{{ .Site.Params.email }}</a>
```

## Markup Configuration

```toml
[markup.goldmark.renderer]
  unsafe = true  # Allow raw HTML in Markdown
```

**Security**: Only enable if you control all content.

## Adding Parameters

1. Add to `hugo.toml`:
```toml
[params]
  facebook = 'https://facebook.com/deaprint'
  openingHours = 'Mon-Fri: 9:00-17:00'
```

2. Use in templates:
```html
<a href='{{ .Site.Params.facebook }}'>Facebook</a>
```

## Common Tasks

**Validate config:**
```bash
hugo config
```

**Test changes:**
```bash
hugo server -D
```

## Troubleshooting

- `baseURL` must end with `/`
- Parameter names are case-sensitive
- Restart server after config changes
- Check syntax: `hugo config`
