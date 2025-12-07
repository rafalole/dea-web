# Hugo Directory Structure

This document explains the purpose of each directory in the Hugo project structure.

## Core Directories

### `content/`
**Purpose**: Markdown files that become your website pages

**What goes here**: 
- Page content in Markdown format
- Front matter (metadata) for each page
- Organized by language, then sections [html-sections](html-sections)

**Multilingual structure**:
```
content/
├── pl/                # Polish content
│   ├── _index.md      # Polish homepage
│   ├── services/
│   └── about.md
├── en/                # English content
│   ├── _index.md      # English homepage
│   ├── services/
│   └── about.md
└── uk/                # Ukrainian content
    ├── _index.md      # Ukrainian homepage
    ├── services/
    └── about.md
```

**Key concepts**: 
- Files named `_index.md` represent section pages
- Each language has its own directory
- **Do NOT create** `content/_index.md` at root - unnecessary when you have language-specific versions
- **Do NOT create** `content/about/`, `content/services/` etc. - all content must be inside language directories
- Hugo uses `defaultContentLanguage` in `hugo.toml` to redirect `/` to `/pl/`

---

### `layouts/`
**Purpose**: HTML templates that define how pages look

**What goes here**:
- HTML templates with Go template syntax
- Partials (reusable components)
- Shortcodes (custom content blocks)

**Structure**:
```
layouts/
├── _default/
│   ├── baseof.html    # Base template (wrapper)
│   ├── single.html    # Individual page template
│   └── list.html      # Section listing template
├── partials/
│   ├── header.html    # Site header
│   ├── footer.html    # Site footer
│   └── nav.html       # Navigation
└── index.html         # Homepage template
```

**Key concept**: Templates use `{{ }}` syntax to insert dynamic content. Partials are included with `{{ partial "header.html" . }}`.

---

## Template Hierarchy Explained

### Base Template (`layouts/_default/baseof.html`)

**Purpose**: The wrapper/skeleton for ALL pages on your site.

**Contains**:
- `<!DOCTYPE html>` and `<html>` tags
- `<head>` section (meta tags, CSS, title)
- `<body>` structure
- Header and footer (via partials)
- **Blocks** that other templates fill in

**Example**:
```html
<!DOCTYPE html>
<html lang="{{ .Language.LanguageCode }}">
<head>
  <title>{{ block "title" . }}{{ .Site.Title }}{{ end }}</title>
  <meta name="description" content='{{ block "description" . }}{{ .Site.Params.description }}{{ end }}'>
</head>
<body>
  {{ partial "header.html" . }}
  <main>
    {{ block "main" . }}{{ end }}  <!-- Other templates fill this -->
  </main>
  {{ partial "footer.html" . }}
</body>
</html>
```

**Key points**:
- Used by EVERY page automatically
- Defines **blocks** using `{{ block "name" . }}...{{ end }}`
- Other templates **define** these blocks to insert their content

---

### Homepage Template (`layouts/index.html`)

**Purpose**: Defines ONLY the unique content for the homepage.

**Contains**:
- Hero section
- Services overview
- Any homepage-specific content

**Example**:
```html
{{ define "title" }}{{ .Title }} - {{ .Site.Title }}{{ end }}
{{ define "description" }}{{ .Description }}{{ end }}

{{ define "main" }}
<section class="hero">
  <h1>{{ .Site.Params.company }}</h1>
  <p>{{ .Site.Params.description }}</p>
</section>

<section class="services">
  <!-- Services cards -->
</section>
{{ end }}
```

**Key points**:
- Does NOT include `<html>`, `<head>`, or `<body>` tags
- Uses `{{ define "blockName" }}...{{ end }}` to fill blocks from baseof.html
- Automatically wrapped by baseof.html

---

### How They Work Together

**When you visit the homepage:**

1. Hugo loads `layouts/index.html`
2. Hugo sees it defines blocks ("title", "description", "main")
3. Hugo loads `layouts/_default/baseof.html` as the wrapper
4. Hugo inserts the defined blocks into baseof.html
5. Final HTML is generated

**Visual representation**:

```
baseof.html (wrapper)          index.html (content)
┌─────────────────────┐        ┌──────────────────┐
│ <html>              │        │ define "title"   │
│ <head>              │        │   Homepage       │
│   <title>           │   ←────│ end              │
│     [title block]   │        │                  │
│   </title>          │        │ define "main"    │
│ </head>             │        │   <section>      │
│ <body>              │        │     Hero         │
│   <header>          │   ←────│   </section>     │
│   <main>            │        │ end              │
│     [main block]    │        └──────────────────┘
│   </main>           │
│   <footer>          │
│ </body>             │
│ </html>             │
└─────────────────────┘
```

**Result**: Complete HTML page with header, footer, and homepage content.

---

### Other Page Templates

**Single page** (`layouts/_default/single.html`):
```html
{{ define "main" }}
<article>
  <h1>{{ .Title }}</h1>
  {{ .Content }}
</article>
{{ end }}
```

**List page** (`layouts/_default/list.html`):
```html
{{ define "main" }}
<h1>{{ .Title }}</h1>
{{ range .Pages }}
  <div>{{ .Title }}</div>
{{ end }}
{{ end }}
```

All use the same baseof.html wrapper, just define different "main" block content.

---

## Shortcodes Explained

### What are Shortcodes?

**Shortcodes** are reusable snippets you can insert into **markdown content files** (not templates).

**Think of them as**: Custom markdown commands that generate HTML.

---

### Shortcodes vs Partials

| Feature | Shortcodes | Partials |
|---------|------------|----------|
| **Used in** | Markdown files (`content/`) | Templates (`layouts/`) |
| **Syntax** | `{{< shortcode >}}` | `{{ partial "name.html" . }}` |
| **Purpose** | Add complex HTML to content | Reuse template code |
| **Location** | `layouts/shortcodes/` | `layouts/partials/` |

---

### Built-in Shortcodes

Hugo includes several shortcodes:

**YouTube video**:
```markdown
{{< youtube dQw4w9WgXcQ >}}
```

**Figure with caption**:
```markdown
{{< figure src="/images/photo.jpg" title="My Photo" >}}
```

**Highlight code**:
```markdown
{{< highlight python >}}
print("Hello")
{{< /highlight >}}
```

---

### Custom Shortcodes

**Create**: `layouts/shortcodes/button.html`
```html
<a href="{{ .Get "url" }}" class="btn btn-primary">
  {{ .Inner }}
</a>
```

**Use in markdown**: `content/pl/about.md`
```markdown
---
title: "About"
---

Welcome to our site!

{{< button url="/contact" >}}Contact Us{{< /button >}}

More content here...
```

**Output HTML**:
```html
<p>Welcome to our site!</p>
<a href="/contact" class="btn btn-primary">Contact Us</a>
<p>More content here...</p>
```

---

### Shortcode Parameters

**Named parameters**:
```html
<!-- layouts/shortcodes/alert.html -->
<div class="alert alert-{{ .Get "type" }}">
  {{ .Inner }}
</div>
```

**Usage**:
```markdown
{{< alert type="warning" >}}
This is a warning message!
{{< /alert >}}
```

**Positional parameters**:
```html
<!-- layouts/shortcodes/image.html -->
<img src="{{ .Get 0 }}" alt="{{ .Get 1 }}">
```

**Usage**:
```markdown
{{< image "/images/photo.jpg" "My Photo" >}}
```

---

### Self-closing vs Paired

**Self-closing** (no content inside):
```markdown
{{< youtube dQw4w9WgXcQ >}}
```

**Paired** (has content inside):
```markdown
{{< alert type="info" >}}
This is the content inside.
{{< /alert >}}
```

Access inner content with `{{ .Inner }}`.

---

### When to Use Shortcodes

**Use shortcodes when**:
- Content editors need to add complex HTML
- You want consistent styling for repeated elements
- Markdown alone isn't enough (videos, galleries, alerts)

**Examples**:
- Image galleries
- Call-to-action buttons
- Alert boxes
- Embedded videos
- Pricing tables
- Contact forms

**Don't use shortcodes for**:
- Template-level components (use partials)
- Site-wide elements like header/footer (use partials)
- Simple markdown formatting (use regular markdown)

---

### Example: Service Card Shortcode

**Create**: `layouts/shortcodes/service.html`
```html
<div class="card bg-base-100 shadow-xl">
  <div class="card-body">
    <h3 class="card-title">{{ .Get "title" }}</h3>
    {{ with .Get "price" }}
      <p class="font-bold">{{ . }}</p>
    {{ end }}
    <p>{{ .Inner }}</p>
  </div>
</div>
```

**Use in**: `content/pl/services.md`
```markdown
---
title: "Our Services"
---

## Available Services

{{< service title="Document Photos" price="50 PLN" >}}
Professional passport and ID photos.
{{< /service >}}

{{< service title="Prints" price="From 2 PLN" >}}
High-quality photo prints in various sizes.
{{< /service >}}
```

---

### Summary

**Shortcodes**:
- Custom markdown commands
- Used in content files (`content/`)
- Stored in `layouts/shortcodes/`
- Syntax: `{{< name >}}` or `{{< name >}}content{{< /name >}}`
- Perfect for content editors to add rich elements

---

### `static/`
**Purpose**: Files copied as-is to the final site (no processing)

**What goes here**:
- Images (photos, logos, icons)
- Fonts
- Favicon
- robots.txt
- Any file that doesn't need processing

**Example**:
```
static/
├── images/
│   ├── logo.jpg
│   └── photos/
├── favicon.ico
└── robots.txt
```

**Key concept**: Files in `static/` are served from the root URL. `static/images/logo.jpg` becomes `https://deaprint.pl/images/logo.jpg`.

---

### `assets/`
**Purpose**: Files that Hugo processes (CSS, JS, images for optimization)

**What goes here**:
- CSS/SCSS files (processed by Hugo Pipes)
- JavaScript files (can be minified/bundled)
- Images that need processing (resize, optimize)

**Example**:
```
assets/
├── css/
│   └── main.css       # Tailwind CSS input
└── js/
    └── main.js        # JavaScript
```

**Key concept**: Assets are processed through Hugo Pipes. Use `{{ $css := resources.Get "css/main.css" }}` to access them.

---

### `public/`
**Purpose**: Generated website output (DO NOT EDIT)

**What happens here**:
- Hugo builds your site into this directory
- Contains the final HTML, CSS, JS, images
- This is what gets deployed

**Key concept**: Never edit files in `public/`. Always edit source files. This directory is in `.gitignore` and regenerated on each build.

---

### `data/`
**Purpose**: Structured data files (YAML, JSON, TOML)

**What goes here**:
- Configuration data
- Content that's easier to manage as data
- Multi-language content
- Lists, menus, settings

**Example**:
```
data/
├── pl/
│   └── homepage.yaml  # Polish homepage data
├── en/
│   └── homepage.yaml  # English homepage data
└── ukr/
    └── homepage.yaml  # Ukrainian homepage data
```

**Key concept**: Access data in templates with `{{ .Site.Data.pl.homepage.title }}`.

---

### `archetypes/`
**Purpose**: Templates for new content files

**What goes here**:
- Default front matter for new pages
- Content scaffolding

**Example**:
```
archetypes/
└── default.md
```

**Key concept**: When you run `hugo new content/blog/post.md`, Hugo uses the archetype as a template.

---

### `themes/`
**Purpose**: Third-party themes (not used in this project)

**What goes here**:
- Downloaded Hugo themes
- Theme overrides

**Key concept**: We're building a custom theme, so this directory is empty. Our layouts are in `layouts/` instead.

---

### `i18n/`
**Purpose**: Translation files for multi-language support

**What goes here**:
- Translation strings for UI elements
- Language-specific text

**Example**:
```
i18n/
├── pl.toml
├── en.toml
└── uk.toml
```

**Key concept**: Use `{{ i18n "contact" }}` in templates to get translated strings.

---

### `resources/`
**Purpose**: Hugo's cache for processed assets (generated)

**What happens here**:
- Hugo caches processed assets
- Speeds up subsequent builds
- Automatically managed by Hugo

**Key concept**: This directory is in `.gitignore`. Hugo regenerates it as needed.

---

## Documentation Directories

### `docs/`
**Purpose**: Project documentation (not part of Hugo site)

**Structure**:
```
docs/
├── dev/              # Developer documentation
├── admin/            # Admin/deployment docs
└── content/          # Content management docs
```

**Key concept**: These docs are for the development team, not site visitors.

---

## Configuration Files

### `hugo.toml`
**Purpose**: Main Hugo configuration file

**What it controls**:
- Site URL, title, language
- Build settings
- Custom parameters
- Menu configuration

---

### `.gitignore`
**Purpose**: Tells Git which files to ignore

**Ignored directories**:
- `public/` - Generated site
- `resources/` - Hugo cache
- `node_modules/` - npm packages

---

## Build Process Flow

1. **Content** (`content/`) + **Data** (`data/`) → Hugo reads
2. **Layouts** (`layouts/`) → Hugo applies templates
3. **Assets** (`assets/`) → Hugo processes (CSS, JS)
4. **Static** (`static/`) → Hugo copies as-is
5. **Output** → Everything goes to `public/`

---

## Quick Reference

| Directory    | Editable | Committed | Purpose          |
|--------------|----------|-----------|------------------|
| `content/`   | ✅ Yes    | ✅ Yes     | Page content     |
| `layouts/`   | ✅ Yes    | ✅ Yes     | Templates        |
| `static/`    | ✅ Yes    | ✅ Yes     | Static files     |
| `assets/`    | ✅ Yes    | ✅ Yes     | Processed files  |
| `data/`      | ✅ Yes    | ✅ Yes     | Structured data  |
| `public/`    | ❌ No     | ❌ No      | Generated output |
| `resources/` | ❌ No     | ❌ No      | Hugo cache       |
| `docs/`      | ✅ Yes    | ✅ Yes     | Documentation    |

---

## Common Tasks

### Add a new page
```bash
# For multilingual site, specify language directory:
hugo new content/pl/services/photography.md
hugo new content/en/services/photography.md
hugo new content/uk/services/photography.md
```

### Add an image
```bash
# Copy to static/images/
cp photo.jpg static/images/
# Reference in markdown: ![Alt text](/images/photo.jpg)
```

### Create a partial
```bash
# Create layouts/partials/mypartial.html
# Use in template: {{ partial "mypartial.html" . }}
```

### Add CSS
```bash
# Edit assets/css/main.css
# Hugo processes it automatically
```
