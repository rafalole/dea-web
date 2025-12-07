# Hugo Site Architecture Requirements

## Content Organization Principles

### Modular Content Structure

Each logical content unit must be stored in a separate markdown file:
- Individual services/products
- Owner/team member profiles
- Location/contact information
- Testimonials
- Portfolio items

### Frontmatter Requirements

Every content file must include frontmatter with:

**Required fields:**
```yaml
---
title: "Service Name"           # Main heading
weight: 10                      # Display order (lower = first)
draft: false                    # Enable/disable visibility
---
```

**Optional fields:**
```yaml
subtitle: "Short description"   # Subheading
featured: true                  # Highlight/emphasize item
icon: "camera"                  # Icon identifier
image: "/images/service.jpg"    # Associated image
price: "50 PLN"                 # Pricing info
---
```

### Content Body

Markdown content goes below frontmatter:
```markdown
---
title: "Document Photos"
weight: 1
featured: true
---

Professional passport and ID photos that meet all official requirements.
```

## Template Architecture

### Reusable Partials

Items of the same type must use the same partial template:

**Example structure:**
```
layouts/
  partials/
    service-card.html      # Renders one service
    team-member.html       # Renders one team member
    testimonial.html       # Renders one testimonial
```

**Usage in page template:**
```html
{{ range where .Site.RegularPages "Section" "services" }}
  {{ partial "service-card.html" . }}
{{ end }}
```

### Sorting and Filtering

Use frontmatter fields for control:

**Sort by weight:**
```html
{{ range (where .Site.RegularPages "Section" "services").ByWeight }}
  {{ partial "service-card.html" . }}
{{ end }}
```

**Filter out drafts:**
```html
{{ range where (where .Site.RegularPages "Section" "services") "Draft" false }}
  {{ partial "service-card.html" . }}
{{ end }}
```

**Show only featured:**
```html
{{ range where .Site.RegularPages ".Params.featured" true }}
  {{ partial "service-card.html" . }}
{{ end }}
```

## Directory Structure

```
content/
  pl/
    services/
      document-photos.md
      prints.md
      photobooks.md
    team/
      kasia.md
  en/
    services/
      document-photos.md
      prints.md
      photobooks.md
    team/
      kasia.md
```

## Benefits

- **Maintainability**: Edit one file to update one item
- **Flexibility**: Reorder by changing weight values
- **Consistency**: Same template ensures uniform appearance
- **Scalability**: Easy to add/remove items
- **Multilingual**: Each language has its own content files
- **Control**: Enable/disable items without deleting files

## Example: Service Card Partial

**layouts/partials/service-card.html:**
```html
<div class="card bg-base-100 shadow-xl{{ if .Params.featured }} ring-2 ring-primary{{ end }}">
  <div class="card-body">
    <h3 class="card-title">{{ .Title }}</h3>
    {{ with .Params.subtitle }}
      <p class="text-sm opacity-70">{{ . }}</p>
    {{ end }}
    <p>{{ .Content }}</p>
    {{ with .Params.price }}
      <p class="font-bold">{{ . }}</p>
    {{ end }}
  </div>
</div>
```

## Anti-Patterns to Avoid

❌ **Don't hardcode content in templates:**
```html
<!-- BAD -->
<div class="card">
  <h3>Document Photos</h3>
  <p>Professional passport photos</p>
</div>
```

✅ **Do use content files:**
```html
<!-- GOOD -->
{{ range .Pages }}
  {{ partial "service-card.html" . }}
{{ end }}
```

❌ **Don't duplicate templates:**
```
service-card-1.html
service-card-2.html
service-card-3.html
```

✅ **Do use one template with parameters:**
```
service-card.html  (handles all variations via frontmatter)
```
