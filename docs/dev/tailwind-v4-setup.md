# Tailwind CSS v4 Setup

See [assets/css/main.css](../../assets/css/main.css) and [postcss.config.js](../../postcss.config.js)

## Key Differences from v3

- **No JavaScript config:** v3 used `tailwind.config.js`, v4 uses CSS-based configuration
- **Configuration location:** `assets/css/main.css` and `postcss.config.js`
- **New directives:** `@import`, `@plugin`, `@theme`

## Installation

```bash
npm install -D tailwindcss@^4.1.0 @tailwindcss/postcss autoprefixer postcss postcss-cli
```

**Why postcss-cli?** Hugo requires the `postcss` binary.

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

Tailwind v4 automatically scans `**/*.html`, including `layouts/**/*.html`.

## Custom Theme

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

Use: `<div class="text-brand font-display p-huge">`

## @import vs @plugin

| Directive | Purpose | Example |
|-----------|---------|---------|
| `@import` | Import CSS | `@import "tailwindcss"` |
| `@plugin` | Load plugins | `@plugin "daisyui"` |

**DaisyUI is a plugin**, not CSS, so use `@plugin`.

## DaisyUI with v4

```bash
npm install -D daisyui@^5.5.5
```

```css
@import "tailwindcss";
@plugin "daisyui";

@theme {
  --color-primary: #3b82f6;
}
```

**Order matters:** `@import` first, then `@plugin`, then `@theme`.

**Usage:**
```html
<button class='btn btn-primary'>Button</button>
<div class='card bg-base-100 shadow-xl'>
  <div class='card-body'>
    <h2 class='card-title'>Card Title</h2>
  </div>
</div>
```

## Migration from v3

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

Hugo automatically processes CSS when:
1. CSS in `assets/css/`
2. `postcss.config.js` exists
3. PostCSS packages installed

## Troubleshooting

**Classes not applying:**
- Check `@import "tailwindcss"` in `assets/css/main.css`
- Verify `postcss.config.js` exists
- Restart Hugo server

**DaisyUI not working:**
- Use `@plugin "daisyui"` not `@import "daisyui"`
- Verify DaisyUI v5.5.5+ installed
- Check order: `@import` before `@plugin`
- Restart server

**Error: Can't resolve 'daisyui':**
- Using `@import` instead of `@plugin`
- Change to `@plugin "daisyui"`

**Hugo template errors:**
- Use `css.PostCSS` not `resources.PostCSS` (deprecated)
- Example: `{{ $css := resources.Get "css/main.css" | css.PostCSS }}`
