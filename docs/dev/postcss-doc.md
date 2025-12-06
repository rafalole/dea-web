# PostCSS Configuration (postcss.config.js)

This document explains PostCSS - the CSS processor that transforms your stylesheets using JavaScript plugins.

**File Location**: `/home/devlin/src/github/dea-web/postcss.config.js`

---

## What is PostCSS?

**PostCSS** = A tool that transforms CSS using JavaScript plugins.

Think of it as an **assembly line for CSS**:
- Input: Your source CSS
- Process: Plugins transform the CSS
- Output: Browser-ready CSS

---

## Current Configuration

```javascript
module.exports = {
  plugins: {
    '@tailwindcss/postcss': {},
  },
}
```

**Note**: DaisyUI removed due to Tailwind v4 compatibility issues.

---

## Required Package: postcss-cli

**What**: Provides the `postcss` binary command

**Why needed**: Hugo requires the `postcss` binary to process CSS files

**Install**:
```bash
npm install -D postcss-cli
```

**Verification**:
```bash
ls node_modules/.bin/postcss
```

**Not in config**: This is a CLI tool, not a PostCSS plugin

---

## Plugin: @tailwindcss/postcss

```javascript
'@tailwindcss/postcss': {}
```

**What it does**:
- Processes Tailwind CSS v4
- Converts `@import "tailwindcss"` into utility classes
- Generates thousands of CSS classes

**Input** (`assets/css/main.css`):
```css
@import "tailwindcss";
```

**Output** (simplified):
```css
.flex { display: flex; }
.grid { display: grid; }
.text-center { text-align: center; }
/* ...thousands more */
```

**Package**: `npm install -D @tailwindcss/postcss`

---

## DaisyUI Status

**Status**: Installed but not active

**Why not used**: Tailwind v4's `@import` system cannot resolve DaisyUI package

**Error when enabled**:
```
Error: Can't resolve 'daisyui' in '/home/devlin/src/github/dea-web'
```

**Package**: `npm install -D daisyui` (installed but not imported)

---

## How Hugo Uses PostCSS

### 1. You Write CSS

**File**: `assets/css/main.css`
```css
@import "tailwindcss";

@theme {
  --color-primary: #3b82f6;
}
```

### 2. Hugo Detects PostCSS

Hugo automatically runs PostCSS when:
- `postcss.config.js` exists
- CSS files in `assets/` directory
- `postcss` binary available (from postcss-cli)

### 3. PostCSS Processes

Reads `postcss.config.js` → Runs plugins → Generates CSS

### 4. Output to Public

**File**: `public/css/main.css`
```css
/* Complete CSS with all utilities */
.flex { display: flex; }
/* ...thousands of lines */
```

---

## Common PostCSS Plugins

### autoprefixer

**Status**: Installed but not used in PostCSS config

**Why not needed**: Tailwind v4 includes autoprefixer functionality

**If needed separately**:
```javascript
plugins: {
  '@tailwindcss/postcss': {},
  'autoprefixer': {},
}
```

### cssnano

**What**: Minifies CSS for production

**Needed?**: No - Hugo's `--minify` flag already minifies CSS

---

## Troubleshooting

### PostCSS Not Running

**Check**:
1. `postcss.config.js` exists in project root
2. `postcss-cli` installed: `npm install -D postcss-cli`
3. Plugins installed: `npm install`
4. Hugo server restarted

**Fix**:
```bash
npm install
hugo server -D
```

### Plugin Not Found Error

**Error**:
```
Error: Cannot find module '@tailwindcss/postcss'
```

**Fix**:
```bash
npm install -D @tailwindcss/postcss
```

### Hugo Template Error

**Error**:
```
ERROR deprecated: resources.PostCSS was deprecated
```

**Fix**: Use `css.PostCSS` instead
```html
{{ $css := resources.Get "css/main.css" | css.PostCSS }}
```

### CSS Not Updating

**Fix**:
1. Stop Hugo server (`Ctrl+C`)
2. Clear cache: `npm run clean`
3. Restart: `npm run dev`

---

## Summary

### PostCSS is:
- **CSS processor** that runs plugins
- **Required** for Tailwind v4
- **Configured** via postcss.config.js
- **Automatic** in Hugo

### Key Points:
- Only `@tailwindcss/postcss` plugin needed
- `postcss-cli` provides binary for Hugo
- DaisyUI not compatible with Tailwind v4 yet
- Hugo automatically detects and runs PostCSS

### For DeaPrint:
- Current config is correct
- No changes needed
- Only modify if adding new CSS features

---

## Related Documentation

- [tailwind-v4-setup.md](tailwind-v4-setup.md) - Tailwind CSS v4 configuration
- [package-json-project-manifest.md](package-json-project-manifest.md) - npm dependencies
- [hugo-toml-config.md](hugo-toml-config.md) - Hugo configuration
