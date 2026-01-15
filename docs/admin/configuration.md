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
  web3formsKey = 'YOUR_ACCESS_KEY_HERE'  # Contact form submission key
  googleMapsEmbed = '<iframe src="https://www.google.com/maps/embed?pb=..." width="600" height="450" style="border:0;" allowfullscreen="" loading="lazy"></iframe>'
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

## Contact Form Configuration

The contact form uses Web3Forms for email delivery on static hosting (Cloudflare Pages).

### Setup Web3Forms

1. Go to https://web3forms.com
2. Enter your email: `deaprint.kasia@gmail.com`
3. Get your access key from the confirmation email
4. Add to `hugo.toml`:

```toml
[params]
  web3formsKey = 'your-actual-access-key-here'
```

5. The form will automatically use this key

**Security**: The access key is public (visible in HTML) but only allows sending messages to your email. It cannot be used to read messages or access your account.

**Features**:
- Rate limiting (prevents spam)
- Email notifications
- No backend code needed
- Works on static hosting

## Google Maps Configuration

Add an embedded Google Map to the contact page.

### Setup Google Maps Embed

1. Go to https://www.google.com/maps
2. Search for: `Warmińska 20/47, 54-110 Wrocław`
3. Click "Share" button
4. Select "Embed a map" tab
5. Copy the entire `<iframe>` code
6. Add to `hugo.toml`:

```toml
[params]
  googleMapsEmbed = '<iframe src="https://www.google.com/maps/embed?pb=..." width="600" height="450" style="border:0;" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>'
```

**Note**: Paste the entire iframe code as a single line string.

**Usage in template**:
```html
{{ with .Site.Params.googleMapsEmbed }}
  <div class="mt-8">
    {{ . | safeHTML }}
  </div>
{{ end }}
```

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
