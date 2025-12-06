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

## How It Works

```
assets/css/main.css
       ↓
   PostCSS reads file
       ↓
   Runs plugins in order
       ↓
   @tailwindcss/postcss → Processes Tailwind
       ↓
   daisyui → Adds component styles
       ↓
   public/css/main.css (final output)
```

---

## DeaPrint Configuration

```javascript
module.exports = {
  plugins: {
    '@tailwindcss/postcss': {},
    'daisyui': {},
  },
}
```

### module.exports

**Purpose**: Node.js syntax to export configuration

**Why**: PostCSS reads this file to know which plugins to use

### plugins

**Purpose**: Object listing plugins to run

**Format**: `'plugin-name': { options }`

**Order matters**: Plugins run top to bottom

---

## Plugin Breakdown

### @tailwindcss/postcss

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

**Why needed**: Tailwind v4 requires this plugin (different from v3)

**Package**: Installed via `npm install -D @tailwindcss/postcss`

---

### daisyui

```javascript
'daisyui': {}
```

**What it does**:
- Adds UI component styles on top of Tailwind
- Provides semantic classes (btn, card, navbar)
- Includes theme system

**Input** (`assets/css/main.css`):
```css
@import "daisyui";
```

**Output** (simplified):
```css
.btn {
  display: inline-flex;
  padding: 0.5rem 1rem;
  border-radius: 0.5rem;
  /* ...more styles */
}

.card {
  background: white;
  border-radius: 1rem;
  /* ...more styles */
}
```

**Why needed**: Provides pre-built components without writing custom CSS

**Package**: Installed via `npm install -D daisyui`

---

## Plugin Order

**Current order**:
```javascript
plugins: {
  '@tailwindcss/postcss': {},  // 1. Runs first
  'daisyui': {},               // 2. Runs second
}
```

**Why this order**:
- Tailwind must run first (creates base utilities)
- DaisyUI extends Tailwind (needs Tailwind classes to exist)

**Wrong order breaks the build**:
```javascript
plugins: {
  'daisyui': {},               // ❌ Runs first - no Tailwind yet
  '@tailwindcss/postcss': {},  // ❌ Runs second - overwrites DaisyUI
}
```

---

## How Hugo Uses PostCSS

### 1. You Write CSS

**File**: `assets/css/main.css`
```css
@import "tailwindcss";
@import "daisyui";

@theme {
  --color-primary: #570df8;
}
```

### 2. Hugo Detects PostCSS

Hugo automatically runs PostCSS when it sees:
- `postcss.config.js` exists
- CSS files in `assets/` directory

### 3. PostCSS Processes

Reads `postcss.config.js` → Runs plugins → Generates CSS

### 4. Output to Public

**File**: `public/css/main.css` (or similar)
```css
/* Complete CSS with all utilities and components */
.flex { display: flex; }
.btn { /* button styles */ }
/* ...thousands of lines */
```

---

## Plugin Options

### Empty Options

```javascript
'@tailwindcss/postcss': {}
```

**Means**: Use default settings

### With Options

```javascript
'@tailwindcss/postcss': {
  base: false,  // Don't include base styles
}
```

**For DeaPrint**: Empty options work fine (use defaults)

---

## Common PostCSS Plugins

### autoprefixer

**What**: Adds vendor prefixes for browser compatibility

```javascript
plugins: {
  '@tailwindcss/postcss': {},
  'daisyui': {},
  'autoprefixer': {},  // Add this
}
```

**Transforms**:
```css
/* Input */
.box {
  display: flex;
}

/* Output */
.box {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
}
```

**Install**: `npm install -D autoprefixer`

**Needed?**: Not critical - modern browsers support most CSS

---

### cssnano

**What**: Minifies CSS for production

```javascript
plugins: {
  '@tailwindcss/postcss': {},
  'daisyui': {},
  'cssnano': {},  // Add this
}
```

**Transforms**:
```css
/* Input */
.button {
  background-color: blue;
  padding: 10px;
}

/* Output */
.button{background-color:blue;padding:10px}
```

**Install**: `npm install -D cssnano`

**Needed?**: Hugo's `--minify` flag already minifies CSS

---

### postcss-import

**What**: Resolves `@import` statements

```javascript
plugins: {
  'postcss-import': {},  // Add this first
  '@tailwindcss/postcss': {},
  'daisyui': {},
}
```

**Needed?**: No - Tailwind v4 handles imports

---

## Configuration Formats

### JavaScript (Current)

```javascript
// postcss.config.js
module.exports = {
  plugins: {
    '@tailwindcss/postcss': {},
    'daisyui': {},
  },
}
```

**Pros**: Most common, easy to read

---

### JSON

```json
{
  "plugins": {
    "@tailwindcss/postcss": {},
    "daisyui": {}
  }
}
```

**File**: `postcss.config.json`

**Pros**: Simpler syntax

**Cons**: Can't use JavaScript logic

---

### YAML

```yaml
plugins:
  '@tailwindcss/postcss': {}
  daisyui: {}
```

**File**: `.postcssrc.yml`

**Pros**: Clean syntax

**Cons**: Less common

---

## Troubleshooting

### PostCSS Not Running

**Check**:
1. `postcss.config.js` exists in project root
2. Plugins installed: `npm install`
3. Hugo server restarted

**Fix**:
```bash
npm install
hugo server -D
```

---

### Plugin Not Found Error

**Error**:
```
Error: Cannot find module '@tailwindcss/postcss'
```

**Fix**:
```bash
npm install -D @tailwindcss/postcss
```

---

### Wrong Plugin Order

**Symptom**: DaisyUI components don't work

**Fix**: Ensure Tailwind runs before DaisyUI
```javascript
plugins: {
  '@tailwindcss/postcss': {},  // First
  'daisyui': {},               // Second
}
```

---

### CSS Not Updating

**Fix**:
1. Stop Hugo server (`Ctrl+C`)
2. Clear cache: `npm run clean`
3. Restart: `npm run dev`

---

## Build Process Flow

### Development (`npm run dev`)

```
1. Hugo starts server
2. Reads assets/css/main.css
3. Detects @import statements
4. Runs PostCSS with plugins
5. Generates CSS in memory
6. Serves at http://localhost:1313
7. Watches for changes
8. Re-runs PostCSS on CSS changes
```

### Production (`npm run build`)

```
1. Hugo builds site
2. Reads assets/css/main.css
3. Runs PostCSS with plugins
4. Generates CSS
5. Minifies (if --minify flag)
6. Outputs to public/css/
7. Ready for deployment
```

---

## File Relationships

### postcss.config.js → package.json

```
package.json (defines which packages to install)
     ↓
npm install
     ↓
node_modules/@tailwindcss/postcss (installed)
node_modules/daisyui (installed)
     ↓
postcss.config.js (tells PostCSS to use them)
```

### postcss.config.js → assets/css/main.css

```
assets/css/main.css (source CSS)
     ↓
Hugo detects @import statements
     ↓
Runs PostCSS with postcss.config.js
     ↓
Plugins transform CSS
     ↓
public/css/main.css (output)
```

### postcss.config.js → Hugo

```
Hugo build/server
     ↓
Looks for postcss.config.js
     ↓
If found, runs PostCSS on CSS files
     ↓
If not found, copies CSS as-is
```

---

## Best Practices

### 1. Keep Plugins Minimal

**Good**:
```javascript
plugins: {
  '@tailwindcss/postcss': {},
  'daisyui': {},
}
```

**Avoid**:
```javascript
plugins: {
  'plugin1': {},
  'plugin2': {},
  'plugin3': {},
  // ...10 more plugins
}
```

**Why**: More plugins = slower builds

---

### 2. Order Matters

**Correct order**:
1. Import resolvers (postcss-import)
2. CSS processors (Tailwind)
3. Extensions (DaisyUI)
4. Optimizers (autoprefixer, cssnano)

---

### 3. Use Empty Options When Possible

**Good**:
```javascript
'@tailwindcss/postcss': {}
```

**Only add options if needed**:
```javascript
'@tailwindcss/postcss': {
  base: false,  // Only if you need to disable base styles
}
```

---

### 4. Comment Complex Configurations

```javascript
module.exports = {
  plugins: {
    // Tailwind v4 - must run before DaisyUI
    '@tailwindcss/postcss': {},
    
    // DaisyUI components - extends Tailwind
    'daisyui': {},
    
    // Autoprefixer - only for older browser support
    // 'autoprefixer': {},  // Uncomment if needed
  },
}
```

---

## When to Modify

### Add Plugin

**When**: Need new CSS transformation

**Example**: Add autoprefixer for older browsers
```bash
npm install -D autoprefixer
```

```javascript
plugins: {
  '@tailwindcss/postcss': {},
  'daisyui': {},
  'autoprefixer': {},  // Add here
}
```

---

### Remove Plugin

**When**: Don't need a plugin anymore

**Example**: Remove DaisyUI
```bash
npm uninstall daisyui
```

```javascript
plugins: {
  '@tailwindcss/postcss': {},
  // 'daisyui': {},  // Remove this line
}
```

---

### Change Plugin Order

**When**: Plugins conflict or don't work

**Example**: Move optimizer to end
```javascript
plugins: {
  '@tailwindcss/postcss': {},
  'daisyui': {},
  'cssnano': {},  // Must be last
}
```

---

## Summary

### PostCSS is:
- **CSS processor** that runs plugins
- **Required** for Tailwind v4
- **Configured** via postcss.config.js
- **Automatic** in Hugo (no manual setup)

### Key Points:
- Plugins run in order (top to bottom)
- Tailwind must run before DaisyUI
- Hugo automatically detects and runs PostCSS
- Output goes to `public/` folder

### For DeaPrint:
- Current config is correct
- No changes needed
- Only modify if adding new CSS features

---

## Related Documentation

- [tailwind-v4-setup.md](tailwind-v4-setup.md) - Tailwind CSS v4 configuration
- [package-json-project-manifest.md](package-json-project-manifest.md) - npm dependencies
- [hugo-toml-config.md](hugo-toml-config.md) - Hugo configuration
