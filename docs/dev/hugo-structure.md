# Hugo Directory Structure

This document explains the purpose of each directory in the Hugo project structure.

## Core Directories

### `content/`
**Purpose**: Markdown files that become your website pages

**What goes here**: 
- Page content in Markdown format
- Front matter (metadata) for each page
- Organized by sections (services, about, contact, etc.)

**Example**:
```
content/
├── _index.md          # Homepage content
├── services/
│   └── _index.md      # Services page
├── about/
│   └── _index.md      # About page
└── contact/
    └── _index.md      # Contact page
```

**Key concept**: Files named `_index.md` represent section pages. Regular files become individual pages.

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

| Directory | Editable | Committed | Purpose |
|-----------|----------|-----------|---------|
| `content/` | ✅ Yes | ✅ Yes | Page content |
| `layouts/` | ✅ Yes | ✅ Yes | Templates |
| `static/` | ✅ Yes | ✅ Yes | Static files |
| `assets/` | ✅ Yes | ✅ Yes | Processed files |
| `data/` | ✅ Yes | ✅ Yes | Structured data |
| `public/` | ❌ No | ❌ No | Generated output |
| `resources/` | ❌ No | ❌ No | Hugo cache |
| `docs/` | ✅ Yes | ✅ Yes | Documentation |

---

## Common Tasks

### Add a new page
```bash
hugo new content/services/photography.md
# Edit content/services/photography.md
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
