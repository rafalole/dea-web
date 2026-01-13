# Hugo Directory Structure

## Core Directories

### `content/`
Markdown files that become website pages.

```
content/
├── pl/                # Polish content
│   ├── _index.md      # Polish homepage
│   ├── services/
│   └── about.md
├── en/                # English content
└── uk/                # Ukrainian content
```

**Don't create:**
- `content/_index.md` at root
- `content/about/`, `content/services/` - all content must be inside language directories

### `layouts/`
HTML templates with Go template syntax.

```
layouts/
├── _default/
│   ├── baseof.html    # Base template (wrapper)
│   ├── single.html    # Individual page template
│   └── list.html      # Section listing template
├── partials/
│   ├── header.html
│   └── footer.html
└── index.html         # Homepage template
```

### `static/`
Files copied as-is (no processing).

- Images, fonts, favicon, robots.txt
- `static/images/logo.jpg` → `https://deaprint.pl/images/logo.jpg`

### `assets/`
Files Hugo processes (CSS, JS, images for optimization).

```
assets/
├── css/
│   └── main.css       # Tailwind CSS input
└── js/
    └── main.js
```

Access: `{{ $css := resources.Get "css/main.css" }}`

### `public/`
Generated website output. **DO NOT EDIT**. Regenerated on each build.

### `i18n/`
Translation files for UI elements.

```
i18n/
├── pl.toml
├── en.toml
└── uk.toml
```

Use: `{{ i18n "contact" }}`

## Template Hierarchy

### Base Template (`baseof.html`)

Wrapper for ALL pages.

```html
<!DOCTYPE html>
<html lang='{{ .Language.LanguageCode }}'>
<head>
  <title>{{ block "title" . }}{{ .Site.Title }}{{ end }}</title>
</head>
<body>
  {{ partial "header.html" . }}
  <main>
    {{ block "main" . }}{{ end }}
  </main>
  {{ partial "footer.html" . }}
</body>
</html>
```

### Homepage Template (`index.html`)

Defines ONLY homepage content.

```html
{{ define "title" }}{{ .Title }} - {{ .Site.Title }}{{ end }}

{{ define "main" }}
<section class='hero'>
  <h1>{{ .Site.Params.company }}</h1>
</section>
{{ end }}
```

**How they work together:**
1. Hugo loads `index.html`
2. Sees it defines blocks
3. Loads `baseof.html` as wrapper
4. Inserts blocks into baseof
5. Generates final HTML

## Shortcodes

Reusable snippets for **markdown content files**.

**Create:** `layouts/shortcodes/button.html`
```html
<a href='{{ .Get "url" }}' class='btn btn-primary'>
  {{ .Inner }}
</a>
```

**Use in markdown:**
```markdown
{{< button url="/contact" >}}Contact Us{{< /button >}}
```

### Shortcodes vs Partials

| Feature | Shortcodes | Partials |
|---------|------------|----------|
| **Used in** | Markdown files | Templates |
| **Syntax** | `{{< shortcode >}}` | `{{ partial "name.html" . }}` |
| **Location** | `layouts/shortcodes/` | `layouts/partials/` |

## Template Lookup Order

Hugo searches in order. First match wins.

**Homepage** (`/`):
1. `layouts/index.html`
2. `layouts/_default/list.html`
3. `layouts/_default/baseof.html`

**Section** (`/about/`):
1. `layouts/about/list.html`
2. `layouts/_default/list.html`

**Single page** (`/about/team/`):
1. `layouts/about/single.html`
2. `layouts/_default/single.html`

## Build Process

```
Content (content/) + Data (data/)
  ↓
Layouts (layouts/) → Apply templates
  ↓
Assets (assets/) → Process CSS/JS
  ↓
Static (static/) → Copy as-is
  ↓
Output → public/
```

## Quick Reference

| Directory | Editable | Committed | Purpose |
|-----------|----------|-----------|---------|
| `content/` | ✅ | ✅ | Page content |
| `layouts/` | ✅ | ✅ | Templates |
| `static/` | ✅ | ✅ | Static files |
| `assets/` | ✅ | ✅ | Processed files |
| `i18n/` | ✅ | ✅ | Translations |
| `public/` | ❌ | ❌ | Generated output |
| `resources/` | ❌ | ❌ | Hugo cache |

## Common Tasks

**Add page:**
```bash
hugo new content/pl/services/photography.md
```

**Add image:**
```bash
cp photo.jpg static/images/
# Reference: ![Alt](/images/photo.jpg)
```

**Create partial:**
```bash
# Create layouts/partials/mypartial.html
# Use: {{ partial "mypartial.html" . }}
```
