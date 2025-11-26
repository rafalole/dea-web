# Tailwind CSS v4 Setup Guide

This project uses Tailwind CSS v4, which has a different configuration approach than v3.

## Key Differences from v3

### No JavaScript Config File
- v3: Used `tailwind.config.js`
- v4: Uses CSS-based configuration with `@import` and `@theme`

### Configuration Location
- Configuration is in `assets/css/main.css`
- PostCSS config in `postcss.config.js`

## Installation

```bash
npm install -D tailwindcss@^4.1.0 @tailwindcss/postcss autoprefixer postcss
```

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

## DaisyUI with v4

DaisyUI 5.5.5+ is compatible with Tailwind CSS v4.

### Setup

```bash
npm install -D daisyui@^5.5.5
```

### Configuration

Add DaisyUI plugin in `postcss.config.js`:

```javascript
module.exports = {
  plugins: {
    '@tailwindcss/postcss': {},
    'daisyui': {},
  },
}
```

### Using DaisyUI Themes

Configure themes in `assets/css/main.css`:

```css
@import "tailwindcss";
@import "daisyui";

@theme {
  --color-primary: #3b82f6;
  --color-secondary: #8b5cf6;
}

@config "./daisyui.config.js";
```

Create `daisyui.config.js`:

```javascript
module.exports = {
  themes: ["light", "dark", "cupcake"],
  darkTheme: "dark",
};
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
- Ensure DaisyUI v5.5.5+ is installed
- Add `daisyui` to PostCSS plugins
- Import DaisyUI in CSS: `@import "daisyui"`
