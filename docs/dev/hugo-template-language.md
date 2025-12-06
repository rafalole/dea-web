# Hugo Template Language Guide

A practical guide to Hugo's template syntax for junior developers.

---

## Delimiters

### `{{ }}`

**What**: Executes Go template code

**Examples**:
```html
{{ .Title }}                   → Output variable
{{ $var := "value" }}          → Assign variable
{{ if .Condition }}...{{ end }} → Control flow
```

### `{{- }}`

**What**: Trim whitespace before

```html
<p>
  {{- .Title }}
</p>
```

**Output**: `<p>Title</p>` (no newline/spaces before)

### `{{ -}}`

**What**: Trim whitespace after

```html
<p>
  {{ .Title -}}
</p>
```

**Output**: `<p>Title</p>` (no newline/spaces after)

### `{{/* */}}`

**What**: Comment (not in output)

```html
{{/* This won't appear in HTML */}}
```

---

## The Dot (`.`)

### What is `.`?

**The dot** = Current context (data available to template)

### Root Context

```html
{{ . }}                        → Entire page object
{{ .Title }}                   → Page title
{{ .Content }}                 → Page content
{{ .Site.Title }}              → Site title
```

### Context Changes

**In loops**:
```html
{{ range .Pages }}
  {{ .Title }}                 → Title of current page in loop
{{ end }}
```

**Access root from inside loop**:
```html
{{ range .Pages }}
  {{ .Title }}                 → Page title (local)
  {{ $.Site.Title }}           → Site title (root, using $)
{{ end }}
```

**`$`** = Always refers to root context

### Passing Context

```html
{{ partial "header.html" . }}  → Pass current context
{{ partial "card.html" .Page }} → Pass specific data
```

---

## Variables

### Site Variables

**From `hugo.toml`**:
```html
{{ .Site.Title }}              → Site title
{{ .Site.LanguageCode }}       → Language code
{{ .Site.Params.company }}     → Custom parameter
{{ .Site.Params.phone }}       → Custom parameter
```

### Page Variables

**From content front matter**:
```html
{{ .Title }}                   → Page title
{{ .Description }}             → Page description
{{ .Content }}                 → Rendered markdown
{{ .Date }}                    → Page date
{{ .Params.author }}           → Custom front matter field
```

### Language Variables

```html
{{ .Language.Lang }}           → Current language (pl/en/uk)
{{ .Language.LanguageCode }}   → Language code
{{ .Language.LanguageName }}   → Language name (Polski/English)
```

### Custom Variables

**Assign**:
```html
{{ $name := "John" }}          → Create variable
{{ $age := 25 }}               → Number
{{ $items := slice "a" "b" }}  → Array
```

**Use**:
```html
<p>{{ $name }}</p>             → Output: John
<p>{{ $age }}</p>              → Output: 25
```

**Reassign**:
```html
{{ $name = "Jane" }}           → Change value (use = not :=)
```

---

## Blocks

### `{{ block "name" . }}`

**What**: Define overridable section

**Parts**:
- `block` - Keyword
- `"name"` - Block identifier (string)
- `.` - Context to pass
- Content between `{{ block }}` and `{{ end }}` - Default content

**In base template** (`baseof.html`):
```html
{{ block "title" . }}
  {{ .Site.Title }}
{{ end }}
```

**Override in child** (`index.html`):
```html
{{ define "title" }}
  Home - {{ .Site.Title }}
{{ end }}
```

**If child doesn't override**: Uses default content

### Multiple Blocks

```html
{{ block "title" . }}Default Title{{ end }}
{{ block "description" . }}Default Description{{ end }}
{{ block "main" . }}Default Content{{ end }}
```

Each can be overridden independently.

---

## Define

### `{{ define "name" }}`

**What**: Override block from base template

**Parts**:
- `define` - Keyword
- `"name"` - Must match block name in base
- Content between `{{ define }}` and `{{ end }}` - New content

**Example**:
```html
{{ define "main" }}
  <h1>Custom content</h1>
{{ end }}
```

**Must match** block name exactly (case-sensitive).

---

## Partials

### `{{ partial "file.html" . }}`

**What**: Include reusable template fragment

**Parts**:
- `partial` - Keyword
- `"file.html"` - Filename in `layouts/partials/`
- `.` - Context to pass

**Examples**:
```html
{{ partial "header.html" . }}          → Pass all context
{{ partial "card.html" .Page }}        → Pass specific data
{{ partial "button.html" (dict "text" "Click") }} → Pass custom data
```

### Creating Partials

**File**: `layouts/partials/card.html`
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

---

## Block vs Partial

### Block

**Purpose**: Define **overridable** sections in base template

**Where**: Base template (`baseof.html`)

**How**: Child templates can **replace** the content

**Example**:
```html
<!-- Base: baseof.html -->
{{ block "title" . }}
  Default Title
{{ end }}

<!-- Child: index.html -->
{{ define "title" }}
  Custom Title
{{ end }}
```

**Result**: "Custom Title" (child overrides base)

### Partial

**Purpose**: **Include** reusable components

**Where**: Separate file in `partials/`

**How**: Always **includes** the same content

**Example**:
```html
<!-- Template -->
{{ partial "header.html" . }}

<!-- Always includes partials/header.html -->
```

**Result**: Header content (always the same)

### Key Differences

| Feature           | Block                    | Partial          |
|-------------------|--------------------------|------------------|
| **Can override?** | Yes                      | No               |
| **Location**      | In base template         | Separate file    |
| **Purpose**       | Vary per page            | Reuse everywhere |
| **Example**       | Page title, main content | Header, footer   |

### When to Use

**Use Block** when:
- Content changes per page type
- Want default fallback
- Example: title, hero, main content

**Use Partial** when:
- Same component everywhere
- Want to avoid duplication
- Example: header, footer, card component

---

## Conditionals

### `{{ if }}`

**Basic**:
```html
{{ if .Title }}
  <h1>{{ .Title }}</h1>
{{ end }}
```

**With else**:
```html
{{ if .Params.image }}
  <img src="{{ .Params.image }}">
{{ else }}
  <div class="placeholder"></div>
{{ end }}
```

**With else if**:
```html
{{ if eq .Type "post" }}
  Post
{{ else if eq .Type "page" }}
  Page
{{ else }}
  Unknown
{{ end }}
```

### Comparison Operators

```html
{{ if eq .A .B }}              → Equal
{{ if ne .A .B }}              → Not equal
{{ if lt .A .B }}              → Less than
{{ if le .A .B }}              → Less than or equal
{{ if gt .A .B }}              → Greater than
{{ if ge .A .B }}              → Greater than or equal
```

### Logical Operators

```html
{{ if and .A .B }}             → AND
{{ if or .A .B }}              → OR
{{ if not .A }}                → NOT
```

### Check Existence

```html
{{ if .Params.author }}        → True if exists and not empty
{{ if isset .Params "author" }} → True if key exists (even if empty)
```

---

## Loops

### `{{ range }}`

**Basic loop**:
```html
{{ range .Pages }}
  <h2>{{ .Title }}</h2>
{{ end }}
```

**With index**:
```html
{{ range $index, $page := .Pages }}
  <div>{{ $index }}: {{ $page.Title }}</div>
{{ end }}
```

**With else** (if empty):
```html
{{ range .Pages }}
  <h2>{{ .Title }}</h2>
{{ else }}
  <p>No pages found</p>
{{ end }}
```

### Context in Loops

**Inside loop**, `.` refers to current item:
```html
{{ range .Pages }}
  {{ .Title }}                 → Current page title
  {{ $.Site.Title }}           → Site title (use $ for root)
{{ end }}
```

---

## Functions

### Built-in Functions

**String functions**:
```html
{{ .Title | upper }}           → UPPERCASE
{{ .Title | lower }}           → lowercase
{{ .Title | title }}           → Title Case
{{ truncate 100 .Content }}    → First 100 chars
```

**Math functions**:
```html
{{ add 1 2 }}                  → 3
{{ sub 5 2 }}                  → 3
{{ mul 3 4 }}                  → 12
{{ div 10 2 }}                 → 5
```

**Comparison**:
```html
{{ eq .A .B }}                 → Equal
{{ ne .A .B }}                 → Not equal
{{ lt 5 10 }}                  → Less than (true)
```

**Other**:
```html
{{ len .Pages }}               → Length/count
{{ now.Year }}                 → Current year
{{ printf "%s" .Title }}       → Format string
```

### Hugo-specific Functions

**i18n** (translations):
```html
{{ i18n "home" }}              → Translated text
{{ i18n "greeting" (dict "Name" "John") }} → With variables
```

**URLs**:
```html
{{ relLangURL "about" }}       → /about/ or /en/about/
{{ absLangURL "about" }}       → https://site.com/about/
{{ .RelPermalink }}            → Relative URL
{{ .Permalink }}               → Absolute URL
```

**Resources**:
```html
{{ resources.Get "css/main.css" }} → Get asset
{{ $css := resources.Get "css/main.css" | css.PostCSS }} → Process
{{ $css.RelPermalink }}        → Output URL
```

---

## Pipes

### `|` Operator

**What**: Pass output of left to input of right

**Examples**:
```html
{{ .Title | upper }}           → UPPERCASE TITLE
{{ .Title | lower | title }}   → Title Case
{{ .Content | truncate 100 | safeHTML }} → Chain multiple
```

**Read left to right** (like Unix pipes).

### Common Patterns

```html
{{ $css := resources.Get "css/main.css" | css.PostCSS }}
{{ $img := resources.Get "image.jpg" | images.Resize "800x" }}
{{ .Content | markdownify | safeHTML }}
```

---

## With

### `{{ with }}`

**What**: Set context if value exists

**Example**:
```html
{{ with .Params.author }}
  <p>By {{ . }}</p>
{{ end }}
```

**If `.Params.author` exists**: Executes block, `.` becomes author value

**If doesn't exist**: Skips block

**With else**:
```html
{{ with .Params.author }}
  <p>By {{ . }}</p>
{{ else }}
  <p>Anonymous</p>
{{ end }}
```

---

## Data Types

### String

```html
{{ $name := "John" }}
{{ $text := "Hello World" }}
```

### Number

```html
{{ $age := 25 }}
{{ $price := 19.99 }}
```

### Boolean

```html
{{ $active := true }}
{{ $hidden := false }}
```

### Array/Slice

```html
{{ $items := slice "a" "b" "c" }}
{{ index $items 0 }}           → "a"
```

### Dictionary/Map

```html
{{ $person := dict "name" "John" "age" 25 }}
{{ $person.name }}             → "John"
{{ $person.age }}              → 25
```

---

## Common Patterns

### Safe HTML

**Problem**: Hugo escapes HTML by default

```html
{{ .Content }}                 → &lt;p&gt;Text&lt;/p&gt;
```

**Solution**:
```html
{{ .Content | safeHTML }}      → <p>Text</p>
```

### Default Values

```html
{{ .Params.title | default .Title }}
{{ .Params.author | default "Anonymous" }}
```

### Conditional Classes

```html
<div class="{{ if .IsHome }}home{{ else }}page{{ end }}">
```

### Loop with Limit

```html
{{ range first 5 .Pages }}
  {{ .Title }}
{{ end }}
```

### Check Type

```html
{{ if eq .Type "post" }}
  <article>{{ .Content }}</article>
{{ end }}
```

---

## Debugging

### Print Context

```html
{{ printf "%#v" . }}           → Print entire context
{{ printf "%T" .Variable }}    → Print type
```

### Conditional Debug

```html
{{ if hugo.IsServer }}
  <pre>{{ printf "%#v" . }}</pre>
{{ end }}
```

### Check Variable

```html
{{ if .Variable }}
  Exists: {{ .Variable }}
{{ else }}
  Doesn't exist
{{ end }}
```

---

## Common Mistakes

### 1. Forgetting Context

```html
<!-- Wrong -->
{{ partial "header.html" }}

<!-- Correct -->
{{ partial "header.html" . }}
```

### 2. Wrong Quotes in Attributes

```html
<!-- Wrong (nested quotes) -->
<a href="{{ relLangURL "about" }}">

<!-- Correct -->
<a href='{{ relLangURL "about" }}'>
```

### 3. Using = Instead of :=

```html
<!-- Wrong (first assignment) -->
{{ $var = "value" }}

<!-- Correct -->
{{ $var := "value" }}

<!-- Reassignment uses = -->
{{ $var = "new value" }}
```

### 4. Forgetting $ in Loops

```html
{{ range .Pages }}
  <!-- Wrong (can't access site) -->
  {{ .Site.Title }}
  
  <!-- Correct -->
  {{ $.Site.Title }}
{{ end }}
```

---

## Summary

### Key Syntax

- `{{ }}` - Execute code
- `.` - Current context
- `$` - Root context
- `|` - Pipe (chain functions)
- `:=` - Assign variable
- `=` - Reassign variable

### Key Keywords

- `block` - Define overridable section
- `define` - Override block
- `partial` - Include file
- `if/else/end` - Conditionals
- `range/end` - Loops
- `with/end` - Context check

### Remember

- Always pass context to partials (`.`)
- Use `$` to access root in loops
- Use single quotes for attributes with Hugo syntax
- Check existence before using variables
- Use `safeHTML` for HTML content

---

## Related Documentation

- [Hugo Template Documentation](https://gohugo.io/templates/)
- [Go Template Documentation](https://pkg.go.dev/text/template)
