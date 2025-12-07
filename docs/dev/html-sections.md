# HTML Sections Explained

This document explains the `<section>` element and how it's used in your Hugo templates.

---

## What is `<section>`?

**`<section>`** is an HTML5 semantic element that represents a thematic grouping of content.

**Purpose**: Divides your page into logical, meaningful parts.

---

## Semantic HTML

**Semantic** = HTML tags that describe their meaning/purpose.

**Non-semantic** (describes nothing):
```html
<div>Hero content</div>
<div>Services content</div>
```

**Semantic** (describes purpose):
```html
<section>Hero content</section>
<section>Services content</section>
```

**Benefits**:
- Better SEO (search engines understand structure)
- Improved accessibility (screen readers)
- Easier to maintain code

---

## Your Homepage Sections

Your `layouts/index.html` has two main sections:

### 1. Hero Section

```html
<section class="hero" style="min-height: 75vh; background-image: url(/images/hero-deaprint5.png);">
  <div class="hero-overlay bg-opacity-60"></div>
  <div class="hero-content text-center text-neutral-content">
    <div class="max-w-md">
      <h1>{{ .Site.Params.company }}</h1>
      <p>{{ .Site.Params.description }}</p>
      <a href='{{ relLangURL "contact" }}' class="btn btn-primary">{{ i18n "contactUs" }}</a>
    </div>
  </div>
</section>
```

**Purpose**: First impression, main message, call-to-action

**Contains**:
- Background image
- Company name (h1)
- Description
- Contact button

**Visual**: Full-height banner with centered text

---

### 2. Services Section

```html
<section class="py-16">
  <div class="container mx-auto px-4">
    <h2>{{ i18n "ourServices" }}</h2>
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
      <!-- Service cards here -->
    </div>
  </div>
</section>
```

**Purpose**: Display available services

**Contains**:
- Section heading (h2)
- Grid of service cards
- 6 service cards total

**Visual**: Grid layout with cards

---

## Section Structure Pattern

**Typical section structure**:

```html
<section class="[styling-classes]">
  <div class="container mx-auto px-4">  <!-- Centers content -->
    <h2>Section Title</h2>              <!-- Section heading -->
    <div class="[layout-classes]">      <!-- Content layout -->
      <!-- Section content -->
    </div>
  </div>
</section>
```

**Layers**:
1. `<section>` - Semantic wrapper
2. `<div class="container">` - Centers and constrains width
3. `<h2>` - Section title
4. Content layout - Grid, flex, etc.

---

## Common Section Types

### Hero Section
```html
<section class="hero">
  <!-- Large banner, main message -->
</section>
```

### Content Section
```html
<section class="py-16">
  <!-- Regular content with padding -->
</section>
```

### Feature Section
```html
<section class="bg-base-200">
  <!-- Highlighted features, different background -->
</section>
```

### CTA Section
```html
<section class="text-center">
  <!-- Call-to-action, centered -->
</section>
```

---

## Section vs Div

**When to use `<section>`**:
- Thematic content grouping
- Has a heading (h1-h6)
- Represents a distinct part of the page
- Would appear in a table of contents

**When to use `<div>`**:
- Pure styling/layout purposes
- No semantic meaning
- Wrapper for CSS/JavaScript

**Example**:

```html
<!-- GOOD: Semantic structure -->
<section>
  <h2>Our Services</h2>
  <div class="grid">  <!-- Layout wrapper -->
    <div class="card">Service 1</div>
    <div class="card">Service 2</div>
  </div>
</section>

<!-- BAD: All divs, no meaning -->
<div>
  <h2>Our Services</h2>
  <div class="grid">
    <div class="card">Service 1</div>
    <div class="card">Service 2</div>
  </div>
</div>
```

---

## Other Semantic Elements

**Similar to `<section>`**:

| Element     | Purpose                | Example                     |
|-------------|------------------------|-----------------------------|
| `<header>`  | Page/section header    | Site header, article header |
| `<footer>`  | Page/section footer    | Site footer, article footer |
| `<nav>`     | Navigation links       | Main menu, breadcrumbs      |
| `<article>` | Self-contained content | Blog post, news article     |
| `<aside>`   | Tangential content     | Sidebar, related links      |
| `<main>`    | Main content           | Primary page content        |

**Your site structure**:

```html
<!DOCTYPE html>
<html>
<head>...</head>
<body>
  <header>Site header with logo/nav</header>
  
  <main>
    <section>Hero</section>
    <section>Services</section>
    <section>About</section>
  </main>
  
  <footer>Site footer with contact</footer>
</body>
</html>
```

---

## Adding New Sections

**To add a new section to homepage**:

1. Edit `layouts/index.html`
2. Add new section inside `{{ define "main" }}`
3. Follow the pattern:

```html
{{ define "main" }}
<!-- Existing sections -->

<!-- New Section: Testimonials -->
<section class="py-16 bg-base-200">
  <div class="container mx-auto px-4">
    <h2 class="text-3xl font-bold text-center mb-12">{{ i18n "testimonials" }}</h2>
    <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
      <!-- Testimonial cards -->
    </div>
  </div>
</section>
{{ end }}
```

---

## Section Styling with Tailwind

**Common patterns**:

**Vertical padding**:
```html
<section class="py-16">  <!-- 4rem top/bottom padding -->
```

**Background color**:
```html
<section class="bg-base-200">  <!-- Light background -->
```

**Full height**:
```html
<section class="min-h-screen">  <!-- At least viewport height -->
```

**Centered content**:
```html
<section class="text-center">  <!-- Center text -->
```

**Combined**:
```html
<section class="py-16 bg-base-200 text-center">
```

---

## Summary

**`<section>`**:
- Semantic HTML5 element
- Groups related content
- Should have a heading
- Better than generic `<div>` for meaningful content areas

**Your homepage has**:
1. Hero section (banner)
2. Services section (grid of cards)

**Pattern**:
```html
<section class="[styling]">
  <div class="container mx-auto px-4">
    <h2>Title</h2>
    <div class="[layout]">Content</div>
  </div>
</section>
```
