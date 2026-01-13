# HTML Sections

## What is `<section>`?

HTML5 semantic element for thematic grouping of content.

**Semantic** = Tags that describe their meaning/purpose.

**Benefits**: Better SEO, improved accessibility, easier maintenance.

## Homepage Sections

See [index.html](../../layouts/index.html)

### Hero Section
```html
<section class='hero' style='min-height: 75vh; background-image: url(/images/hero-deaprint5.png);'>
  <div class='hero-overlay bg-opacity-60'></div>
  <div class='hero-content text-center text-neutral-content'>
    <h1>{{ .Site.Params.company }}</h1>
    <p>{{ .Site.Params.description }}</p>
    <a href='{{ relLangURL "contact" }}' class='btn btn-primary'>{{ i18n "contactUs" }}</a>
  </div>
</section>
```

### Services Section
```html
<section class='py-16'>
  <div class='container mx-auto px-4'>
    <h2>{{ i18n "ourServices" }}</h2>
    <div class='grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8'>
      <!-- Service cards -->
    </div>
  </div>
</section>
```

## Section Structure Pattern

```html
<section class='[styling-classes]'>
  <div class='container mx-auto px-4'>  <!-- Centers content -->
    <h2>Section Title</h2>
    <div class='[layout-classes]'>      <!-- Content layout -->
      <!-- Section content -->
    </div>
  </div>
</section>
```

## Section vs Div

**Use `<section>` when:**
- Thematic content grouping
- Has a heading (h1-h6)
- Represents distinct page part
- Would appear in table of contents

**Use `<div>` when:**
- Pure styling/layout
- No semantic meaning
- Wrapper for CSS/JavaScript

## Other Semantic Elements

| Element     | Purpose                | Example                     |
|-------------|------------------------|-----------------------------|
| `<header>`  | Page/section header    | Site header, article header |
| `<footer>`  | Page/section footer    | Site footer, article footer |
| `<nav>`     | Navigation links       | Main menu, breadcrumbs      |
| `<article>` | Self-contained content | Blog post, news article     |
| `<aside>`   | Tangential content     | Sidebar, related links      |
| `<main>`    | Main content           | Primary page content        |

## Adding New Sections

Edit `layouts/index.html`:

```html
{{ define "main" }}
<!-- Existing sections -->

<section class='py-16 bg-base-200'>
  <div class='container mx-auto px-4'>
    <h2 class='text-3xl font-bold text-center mb-12'>{{ i18n "testimonials" }}</h2>
    <div class='grid grid-cols-1 md:grid-cols-2 gap-8'>
      <!-- Testimonial cards -->
    </div>
  </div>
</section>
{{ end }}
```

## Common Tailwind Patterns

```html
<section class='py-16'>              <!-- Vertical padding -->
<section class='bg-base-200'>        <!-- Background color -->
<section class='min-h-screen'>       <!-- Full viewport height -->
<section class='text-center'>        <!-- Center text -->
<section class='py-16 bg-base-200 text-center'>  <!-- Combined -->
```
