# Hugo Templating System

This document explains how Hugo's templating system works.

---

## Core Concepts

### 1. Template Inheritance

Hugo uses **base templates** that child templates extend.

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

**Result**:
```html
<html>
<body>
  Custom content
</body>
</html>
```

**How it works**:
1. Hugo loads base template
2. Finds `{{ block "main" . }}`
3. Replaces with `{{ define "main" }}` from child
4. If no child definition, uses default

---

### 2. Partials

**Partials** = Reusable template fragments

**Define partial** (`partials/header.html`):
```html
<header>
  <h1>{{ .Site.Title }}</h1>
</header>
```

**Use partial**:
```html
{{ partial "header.html" . }}
```

**The dot (`.`)** passes current context to partial.

---

### 3. Context (The Dot)

**`.`** = Current context (data available to template)

**Root context**:
```html
{{ . }}                    → Entire page object
{{ .Title }}               → Page title
{{ .Content }}             → Page content
{{ .Site.Title }}          → Site title
```

**Passing context**:
```html
{{ partial "header.html" . }}     → Pass current context
{{ partial "header.html" .Site }} → Pass only site data
```

**Context changes in loops**:
```html
{{ range .Pages }}
  {{ .Title }}             → Title of current page in loop
{{ end }}
```

---

### 4. Variables

**Site variables** (from `hugo.toml`):
```html
{{ .Site.Title }}              → Site title
{{ .Site.Params.company }}     → Custom parameter
{{ .Site.LanguageCode }}       → Language code
```

**Page variables** (from content front matter):
```html
{{ .Title }}                   → Page title
{{ .Description }}             → Page description
{{ .Content }}                 → Rendered markdown
{{ .Params.customField }}      → Custom front matter field
```

**Custom variables**:
```html
{{ $name := "value" }}         → Define variable
{{ $name }}                    → Use variable
```

---

### 5. Template Lookup Order

Hugo searches for templates in specific order:

**For homepage** (`/`):
1. `layouts/index.html`
2. `layouts/_default/list.html`
3. `layouts/_default/baseof.html`

**For section** (`/about/`):
1. `layouts/about/list.html`
2. `layouts/_default/list.html`
3. `layouts/_default/baseof.html`

**For single page** (`/about/team/`):
1. `layouts/about/single.html`
2. `layouts/_default/single.html`
3. `layouts/_default/baseof.html`

**First match wins** - Hugo stops searching when it finds a template.

---

## Template Syntax

### Delimiters

```html
{{ }}                          → Execute code
{{- }}                         → Trim whitespace before
{{ -}}                         → Trim whitespace after
{{/* comment */}}              → Comment (not in output)
```

### Variables

```html
{{ .Variable }}                → Access variable
{{ $var := "value" }}          → Assign variable
{{ $var }}                     → Use variable
```

### Conditionals

```html
{{ if .Condition }}
  True
{{ else }}
  False
{{ end }}

{{ if eq .Lang "pl" }}         → Equal
{{ if ne .Lang "en" }}         → Not equal
{{ if and .A .B }}             → AND
{{ if or .A .B }}              → OR
```

### Loops

```html
{{ range .Pages }}
  {{ .Title }}
{{ end }}

{{ range $index, $element := .Pages }}
  {{ $index }}: {{ $element.Title }}
{{ end }}
```

### Functions

```html
{{ printf "%s" .Title }}       → Format string
{{ len .Pages }}               → Length
{{ add 1 2 }}                  → Math (1 + 2)
{{ now.Year }}                 → Current year
```

---

## Blocks vs Partials

### When to Use Blocks

**Blocks** = Overridable sections in base template

**Use when**:
- Content varies per page type
- Child templates need to customize
- Want default fallback

**Example**:
```html
<!-- Base -->
{{ block "hero" . }}
  <div>Default hero</div>
{{ end }}

<!-- Child can override or ignore -->
{{ define "hero" }}
  <div>Custom hero</div>
{{ end }}
```

### When to Use Partials

**Partials** = Reusable components

**Use when**:
- Same component on multiple pages
- Want to avoid duplication
- Component doesn't need overriding

**Example**:
```html
{{ partial "header.html" . }}  → Always same header
```

---

## Data Flow

### 1. Content → Template

```
content/about/_index.md
---
title: "About Us"
description: "Our story"
---
Content here
```

```html
<!-- Template receives -->
{{ .Title }}        → "About Us"
{{ .Description }}  → "Our story"
{{ .Content }}      → Rendered markdown
```

### 2. Config → Template

```toml
# hugo.toml
[params]
company = "DeaPrint"
```

```html
<!-- Template accesses -->
{{ .Site.Params.company }}  → "DeaPrint"
```

### 3. i18n → Template

```toml
# i18n/pl.toml
[home]
other = "Strona główna"
```

```html
<!-- Template translates -->
{{ i18n "home" }}  → "Strona główna"
```

---

## Common Patterns

### Base + Child Pattern

**Base** (`_default/baseof.html`):
```html
<!DOCTYPE html>
<html>
<head>
  <title>{{ block "title" . }}{{ .Site.Title }}{{ end }}</title>
</head>
<body>
  {{ block "main" . }}{{ end }}
</body>
</html>
```

**Child** (`index.html`):
```html
{{ define "title" }}Home - {{ .Site.Title }}{{ end }}

{{ define "main" }}
  <h1>Welcome</h1>
{{ end }}
```

### Partial with Context

**Partial** (`partials/card.html`):
```html
<div class="card">
  <h2>{{ .title }}</h2>
  <p>{{ .description }}</p>
</div>
```

**Usage**:
```html
{{ partial "card.html" (dict "title" "Hello" "description" "World") }}
```

### Conditional Rendering

```html
{{ if .Params.image }}
  <img src="{{ .Params.image }}" alt="{{ .Title }}">
{{ else }}
  <div class="placeholder"></div>
{{ end }}
```

### Loop with Index

```html
{{ range $index, $page := .Pages }}
  <div class="item-{{ $index }}">
    {{ $page.Title }}
  </div>
{{ end }}
```

---

## Pipeline

Hugo uses **pipes** to chain functions:

```html
{{ .Title | upper }}                    → UPPERCASE
{{ .Title | lower | title }}            → Title Case
{{ .Content | truncate 100 }}           → First 100 chars
{{ resources.Get "css/main.css" | css.PostCSS }}  → Process CSS
```

**How it works**:
- Output of left becomes input of right
- Read left to right
- Like Unix pipes

---

## Scoping

### Global Scope

```html
{{ .Site.Title }}              → Always accessible
{{ .Language.Lang }}           → Always accessible
```

### Local Scope

```html
{{ range .Pages }}
  {{ .Title }}                 → Page title in loop
  {{ $.Site.Title }}           → Site title ($ = root)
{{ end }}
```

**`$`** = Root context (escape local scope)

---

## Best Practices

### 1. Always Pass Context

```html
<!-- Good -->
{{ partial "header.html" . }}

<!-- Bad (no context) -->
{{ partial "header.html" }}
```

### 2. Use Descriptive Block Names

```html
<!-- Good -->
{{ block "pageTitle" . }}
{{ block "heroSection" . }}

<!-- Bad -->
{{ block "content1" . }}
{{ block "stuff" . }}
```

### 3. Check Before Using

```html
{{ if .Params.image }}
  <img src="{{ .Params.image }}">
{{ end }}
```

### 4. Use with for Cleaner Code

```html
{{ with .Params.author }}
  <p>By {{ . }}</p>
{{ end }}
```

### 5. Trim Whitespace

```html
{{- if .Condition -}}
  Content
{{- end -}}
```

---

## Debugging

### Print Context

```html
{{ printf "%#v" . }}           → Print entire context
```

### Check Variable Type

```html
{{ printf "%T" .Variable }}    → Print type
```

### Conditional Debug

```html
{{ if hugo.IsServer }}
  <div>Debug: {{ . }}</div>
{{ end }}
```

---

## Summary

### Key Concepts

- **Blocks** - Overridable sections
- **Partials** - Reusable components
- **Context (.)** - Current data
- **Lookup order** - How Hugo finds templates
- **Pipeline** - Chain functions with `|`

### Template Types

- **baseof.html** - Base wrapper
- **list.html** - Section pages
- **single.html** - Individual pages
- **index.html** - Homepage
- **partials/** - Reusable components

### Data Sources

- **Content** - Markdown files
- **Config** - hugo.toml
- **i18n** - Translation files
- **Data** - JSON/YAML/TOML files

---

## Related Documentation

- [hugo-toml-config.md](hugo-toml-config.md) - Site configuration
- [hugo-translation.md](hugo-translation.md) - i18n system
- [Hugo Templates Documentation](https://gohugo.io/templates/)
