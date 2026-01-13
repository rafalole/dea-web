# Hugo Template Language

Complete guide to Hugo's template syntax and system.

## Template System

### Template Inheritance

**Base template** (`baseof.html`):
```html
<html>
<body>
  {{ block "main" . }}Default content{{ end }}
</body>
</html>
```

**Child template** (`index.html`):
```html
{{ define "main" }}
  Custom content
{{ end }}
```

Child overrides base block.

### Template Lookup Order

Hugo searches in order. First match wins.

**Homepage** (`/`):
1. `layouts/index.html`
2. `layouts/_default/list.html`

**Section** (`/about/`):
1. `layouts/about/list.html`
2. `layouts/_default/list.html`

**Single page** (`/about/team/`):
1. `layouts/about/single.html`
2. `layouts/_default/single.html`

## Syntax

### Delimiters

```html
{{ }}                          → Execute code
{{- }}                         → Trim whitespace before
{{ -}}                         → Trim whitespace after
{{/* comment */}}              → Comment (not in output)
```

### The Dot (`.`)

Current context (data available to template).

```html
{{ .Title }}                   → Page title
{{ .Site.Title }}              → Site title
{{ $.Site.Title }}             → Root context (use $ in loops)
```

**Context changes in loops:**
```html
{{ range .Pages }}
  {{ .Title }}                 → Current page in loop
  {{ $.Site.Title }}           → Site title (root)
{{ end }}
```

### Variables

**Site** (from `hugo.toml`):
```html
{{ .Site.Title }}
{{ .Site.Params.company }}
```

**Page** (from frontmatter):
```html
{{ .Title }}
{{ .Content }}
{{ .Params.author }}
```

**Language:**
```html
{{ .Language.Lang }}           → pl/en/uk
{{ .Language.LanguageName }}   → Polski/English
```

**Custom:**
```html
{{ $name := "John" }}          → Create (:=)
{{ $name = "Jane" }}           → Reassign (=)
{{ $name }}                    → Use
```

## Blocks & Partials

### Blocks & Define

**Base template** (`baseof.html`):
```html
{{ block "title" . }}
  {{ .Site.Title }}
{{ end }}
```

**Child template** (`index.html`):
```html
{{ define "title" }}
  Home - {{ .Site.Title }}
{{ end }}
```

**Block** = Define overridable section in base
**Define** = Override block from base

### Partials

```html
{{ partial "header.html" . }}          → Pass all context
{{ partial "card.html" .Page }}        → Pass specific data
{{ partial "button.html" (dict "text" "Click") }} → Pass custom data
```

**Always pass context** (the `.` parameter).

### Block vs Partial

| Feature | Block | Partial |
|---------|-------|----------|
| **Can override?** | Yes | No |
| **Location** | In base template | Separate file |
| **Purpose** | Vary per page | Reuse everywhere |
| **Example** | Page title, main content | Header, footer |

## Control Flow

### Conditionals

```html
{{ if .Title }}
  <h1>{{ .Title }}</h1>
{{ else }}
  <h1>Untitled</h1>
{{ end }}

{{ if eq .Type "post" }}       → Equal
{{ if ne .Type "page" }}       → Not equal
{{ if and .A .B }}             → AND
{{ if or .A .B }}              → OR
{{ if not .A }}                → NOT
```

### Loops

```html
{{ range .Pages }}
  <h2>{{ .Title }}</h2>
{{ else }}
  <p>No pages found</p>
{{ end }}

{{ range $index, $page := .Pages }}
  <div>{{ $index }}: {{ $page.Title }}</div>
{{ end }}
```

### With

```html
{{ with .Params.author }}
  <p>By {{ . }}</p>
{{ else }}
  <p>Anonymous</p>
{{ end }}
```

Executes block if value exists, `.` becomes the value.

## Functions

**String:**
```html
{{ .Title | upper }}           → UPPERCASE
{{ .Title | lower }}           → lowercase
{{ truncate 100 .Content }}    → First 100 chars
```

**Math:**
```html
{{ add 1 2 }}                  → 3
{{ sub 5 2 }}                  → 3
{{ mul 3 4 }}                  → 12
```

**Hugo-specific:**
```html
{{ i18n "home" }}              → Translated text
{{ relLangURL "about" }}       → /about/ or /en/about/
{{ resources.Get "css/main.css" }} → Get asset
```

## Pipes

```html
{{ .Title | upper }}           → UPPERCASE TITLE
{{ .Title | lower | title }}   → Title Case
{{ $css := resources.Get "css/main.css" | css.PostCSS }}
```

Read left to right (like Unix pipes).

## Data Flow

**Content → Template:**
```markdown
---
title: "About Us"
description: "Our story"
---
Content here
```

```html
{{ .Title }}        → "About Us"
{{ .Description }}  → "Our story"
{{ .Content }}      → Rendered markdown
```

**Config → Template:**
```toml
[params]
company = "DeaPrint"
```

```html
{{ .Site.Params.company }}  → "DeaPrint"
```

**i18n → Template:**
```toml
[home]
other = "Strona główna"
```

```html
{{ i18n "home" }}  → "Strona główna"
```

## Common Patterns

**Base + Child:**
```html
<!-- baseof.html -->
<!DOCTYPE html>
<html>
<head>
  <title>{{ block "title" . }}{{ .Site.Title }}{{ end }}</title>
</head>
<body>
  {{ block "main" . }}{{ end }}
</body>
</html>

<!-- index.html -->
{{ define "title" }}Home - {{ .Site.Title }}{{ end }}
{{ define "main" }}
  <h1>Welcome</h1>
{{ end }}
```

**Partial with context:**
```html
<!-- partials/card.html -->
<div class='card'>
  <h2>{{ .title }}</h2>
  <p>{{ .description }}</p>
</div>

<!-- Usage -->
{{ partial "card.html" (dict "title" "Hello" "description" "World") }}
```

**Safe HTML:**
```html
{{ .Content | safeHTML }}
```

**Default values:**
```html
{{ .Params.title | default .Title }}
```

**Conditional classes:**
```html
<div class='{{ if .IsHome }}home{{ else }}page{{ end }}'>
```

**Loop with limit:**
```html
{{ range first 5 .Pages }}
  {{ .Title }}
{{ end }}
```

## Common Mistakes

**1. Forgetting context:**
```html
<!-- Wrong -->
{{ partial "header.html" }}

<!-- Correct -->
{{ partial "header.html" . }}
```

**2. Wrong quotes in attributes:**
```html
<!-- Wrong (nested quotes) -->
<a href="{{ relLangURL "about" }}">

<!-- Correct -->
<a href='{{ relLangURL "about" }}'>
```

**3. Using = instead of :=:**
```html
{{ $var := "value" }}          → First assignment
{{ $var = "new" }}             → Reassignment
```

**4. Forgetting $ in loops:**
```html
{{ range .Pages }}
  {{ $.Site.Title }}           → Use $ for root
{{ end }}
```

## Debugging

```html
{{ printf "%#v" . }}           → Print context
{{ printf "%T" .Variable }}    → Print type

{{ if hugo.IsServer }}
  <pre>{{ printf "%#v" . }}</pre>
{{ end }}
```

## Best Practices

1. **Always pass context:** `{{ partial "header.html" . }}`
2. **Use descriptive block names:** `{{ block "pageTitle" . }}`
3. **Check before using:** `{{ if .Params.image }}`
4. **Use with for cleaner code:** `{{ with .Params.author }}`
