# Tailwind CSS v4 Setup Guide

This project uses Tailwind CSS v4, which has a different configuration approach than v3.

## Key Differences from v3

### No JavaScript Config File
- v3: Used `tailwind.config.js`
- v4: Uses CSS-based configuration with `@import`, `@plugin`, and `@theme`

### Configuration Location
- Configuration is in `assets/css/main.css`
- PostCSS config in `postcss.config.js`

### New Directives
- `@import` - Import CSS files (e.g., `@import "tailwindcss"`)
- `@plugin` - Load Tailwind plugins (e.g., `@plugin "daisyui"`)
- `@theme` - Define custom theme values

## Installation

```bash
npm install -D tailwindcss@^4.1.0 @tailwindcss/postcss autoprefixer postcss postcss-cli
```

**Why postcss-cli?** Hugo requires the `postcss` binary to process CSS files.

## File Structure

### `assets/css/main.css`
```css
@import "tailwindcss";

@theme {
  --color-primary: #3b82f6;
  --color-secondary: #8b5cf6;
}
```

### `postcss.config.js`
```javascript
module.exports = {
  plugins: {
    '@tailwindcss/postcss': {},
  },
}
```

## Content Detection

Tailwind v4 automatically scans:
- `**/*.html`
- `**/*.jsx`
- `**/*.tsx`
- `**/*.vue`
- `**/*.svelte`

For Hugo, it will scan `layouts/**/*.html` automatically.

## Custom Theme Configuration

Add custom values in `@theme` block:

```css
@theme {
  /* Colors */
  --color-brand: #ff6b6b;
  
  /* Fonts */
  --font-display: "Playfair Display", serif;
  
  /* Spacing */
  --spacing-huge: 10rem;
}
```

Use in HTML:
```html
<div class="text-brand font-display p-huge">
  Custom styled content
</div>
```

## @import vs @plugin

### @import

**Purpose**: Import CSS files or modules

```css
@import "tailwindcss";           /* Tailwind core CSS */
@import "./custom-styles.css";   /* Your CSS file */
```

**Use for**: CSS files, stylesheets

### @plugin

**Purpose**: Load Tailwind plugins (npm packages)

```css
@plugin "daisyui";              /* UI component library */
@plugin "@tailwindcss/forms";   /* Form styling plugin */
```

**Use for**: Tailwind plugins that extend functionality

### Key Difference

| Directive | Purpose      | Example                 |
|-----------|--------------|-------------------------|
| `@import` | Import CSS   | `@import "tailwindcss"` |
| `@plugin` | Load plugins | `@plugin "daisyui"`     |

**Why it matters**: DaisyUI is a plugin, not a CSS file, so it requires `@plugin` not `@import`.

---

## DaisyUI with v4

**Status**: DaisyUI v5.5.5 is compatible with Tailwind v4 using the `@plugin` directive.

### Setup

```bash
npm install -D daisyui@^5.5.5
```

### Configuration

Add to `assets/css/main.css`:
```css
@import "tailwindcss";
@plugin "daisyui";

@theme {
  --color-primary: #3b82f6;
  --color-secondary: #8b5cf6;
}
```

**Important**: 
- Use `@plugin "daisyui"` not `@import "daisyui"`
- Order matters: `@import` first, then `@plugin`, then `@theme`

### Using DaisyUI Components

```html
<button class="btn btn-primary">Button</button>
<div class="card bg-base-100 shadow-xl">
  <div class="card-body">
    <h2 class="card-title">Card Title</h2>
    <p>Card content</p>
  </div>
</div>
```

## Migration from v3

If you have v3 config:

**Old (v3):**
```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: '#3b82f6',
      }
    }
  }
}
```

**New (v4):**
```css
/* assets/css/main.css */
@theme {
  --color-primary: #3b82f6;
}
```

## Build Process

Hugo automatically processes CSS through PostCSS when:
1. CSS is in `assets/css/`
2. `postcss.config.js` exists
3. PostCSS packages are installed

## Troubleshooting

### Classes not applying
- Check `assets/css/main.css` has `@import "tailwindcss"`
- Verify `postcss.config.js` exists
- Restart Hugo server

### PostCSS errors
- Ensure `@tailwindcss/postcss` is installed
- Check `postcss.config.js` syntax

### DaisyUI not working
- Ensure using `@plugin "daisyui"` not `@import "daisyui"`
- Verify DaisyUI v5.5.5+ is installed
- Check order: `@import` before `@plugin`
- Restart Hugo server after adding plugin

### Error: Can't resolve 'daisyui'
- You're using `@import "daisyui"` instead of `@plugin "daisyui"`
- DaisyUI is a plugin, not a CSS file
- Change to `@plugin "daisyui"`

### Hugo template errors
- Ensure templates use `css.PostCSS` not `resources.PostCSS` (deprecated in Hugo v0.128.0)
- Example: `{{ $css := resources.Get "css/main.css" | css.PostCSS }}`
