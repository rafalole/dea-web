# Hugo Template Coding Rules

## HTML Attribute Quoting with Hugo Templates

When using Hugo template syntax `{{ }}` inside HTML attributes, use single quotes for the attribute value to avoid quote conflicts.

### Rule

**Use single quotes for HTML attributes that contain Hugo template syntax with double quotes.**

### Examples

**Wrong** (nested double quotes):
```html
<meta name="description" content="{{ block "description" . }}{{ .Site.Params.description }}{{ end }}">
<a href="{{ relLangURL "about" }}" title="{{ i18n "about" }}">Link</a>
```

**Correct** (single quotes for attribute):
```html
<meta name="description" content='{{ block "description" . }}{{ .Site.Params.description }}{{ end }}'>
<a href='{{ relLangURL "about" }}' title='{{ i18n "about" }}'>Link</a>
```

### When to Apply

Apply this rule when:
- HTML attribute value contains `{{ }}` Hugo template syntax
- Hugo template syntax uses double quotes (e.g., `"title"`, `"description"`)
- Attribute is `content`, `href`, `title`, `alt`, `src`, or any other HTML attribute

### Exceptions

If Hugo template doesn't use quotes inside, either style works:
```html
<div class="{{ .Params.class }}">  <!-- OK -->
<div class='{{ .Params.class }}'>  <!-- Also OK -->
```

### Why This Matters

- Prevents HTML parsing errors
- Avoids IntelliJ/IDE warnings
- Ensures proper template rendering
- Makes code more maintainable
