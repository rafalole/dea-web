# PostCSS Configuration

See [postcss.config.js](../../postcss.config.js)

## What is PostCSS?

CSS processor that transforms stylesheets using JavaScript plugins. Think of it as an assembly line: input CSS → plugins transform → output browser-ready CSS.

## Current Configuration

```javascript
module.exports = {
  plugins: {
    '@tailwindcss/postcss': {},
  },
}
```

**Note**: DaisyUI removed due to Tailwind v4 compatibility issues.

## Required: postcss-cli

Provides the `postcss` binary command that Hugo needs. Already installed in package.json.

**Not in config**: CLI tool, not a PostCSS plugin.

## Plugin: @tailwindcss/postcss

Processes Tailwind CSS v4. Converts `@import "tailwindcss"` into utility classes.

**Input** (`assets/css/main.css`):
```css
@import "tailwindcss";
```

**Output** (simplified):
```css
.flex { display: flex; }
.grid { display: grid; }
/* ...thousands more */
```

## How Hugo Uses PostCSS

1. You write CSS in `assets/css/main.css`
2. Hugo detects `postcss.config.js` and runs PostCSS automatically
3. PostCSS processes via plugins
4. Output goes to `public/css/main.css`

**Template usage:**
```html
{{ $css := resources.Get "css/main.css" | css.PostCSS }}
```

## DaisyUI Status

Installed but not active. Tailwind v4's `@import` system cannot resolve DaisyUI package.

**Error when enabled:**
```
Error: Can't resolve 'daisyui'
```

## Troubleshooting

**PostCSS not running:**
```bash
npm install
hugo server -D
```

**Plugin not found:**
```bash
npm install -D @tailwindcss/postcss
```

**CSS not updating:**
1. Stop server (`Ctrl+C`)
2. Clear cache: `npm run clean`
3. Restart: `npm run dev`

## Summary

- Only `@tailwindcss/postcss` plugin needed
- `postcss-cli` provides binary for Hugo
- Hugo automatically detects and runs PostCSS
- Current config is correct, no changes needed
