# Project Manifest (package.json)

This document explains the `package.json` file - the project's manifest that tells npm everything about the DeaPrint website project.

**File Location**: `/home/devlin/src/github/dea-web/package.json`

---

## What is package.json?

The **manifest file** that defines:
1. Project metadata (name, version, description)
2. Dependencies (external packages needed)
3. Scripts (command shortcuts)
4. Repository information

---

## Project Metadata

```json
{
  "name": "dea-web",
  "version": "1.0.0",
  "description": "DeaPrint Photography Website"
}
```

### name
- **Value**: `dea-web`
- **Purpose**: Project identifier used by npm
- **Usage**: Package name if published to npm registry

### version
- **Value**: `1.0.0`
- **Purpose**: Current project version
- **Format**: Semantic versioning (major.minor.patch)
  - Major: Breaking changes (1.0.0 → 2.0.0)
  - Minor: New features (1.0.0 → 1.1.0)
  - Patch: Bug fixes (1.0.0 → 1.0.1)

### description
- **Value**: `DeaPrint Photography Website`
- **Purpose**: Brief project description
- **Usage**: Documentation, npm registry

---

## Entry Point (Not Used)

```json
{
  "main": "index.js"
}
```

- **Purpose**: Entry point for Node.js packages
- **For DeaPrint**: Not used (Hugo static site, not Node.js package)
- **Can Remove**: Yes, not needed for Hugo projects

---

## Directories

```json
{
  "directories": {
    "doc": "docs"
  }
}
```

- **Purpose**: Tells npm where documentation lives
- **For DeaPrint**: Just metadata
- **Can Remove**: Yes, optional

---

## Scripts (Important!)

```json
{
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "hugo server -D",
    "build": "hugo --minify",
    "clean": "rm -rf public resources"
  }
}
```

Scripts are **command shortcuts** you run with `npm run <script-name>`.

### npm run dev
```bash
npm run dev
# Executes: hugo server -D
```
- Starts Hugo development server
- `-D` flag includes draft content
- Site available at http://localhost:1313
- Auto-reloads on file changes

### npm run build
```bash
npm run build
# Executes: hugo --minify
```
- Builds production-ready site
- `--minify` compresses HTML/CSS/JS
- Output to `public/` directory
- Ready for deployment

### npm run clean
```bash
npm run clean
# Executes: rm -rf public resources
```
- Deletes generated files
- `public/` - Built site
- `resources/` - Hugo cache
- Use before fresh build

### npm test
```bash
npm test
# Executes: echo "Error: no test specified" && exit 1
```
- Currently shows error (no tests configured)
- Placeholder for future testing

### Why Use Scripts?

**Benefits**:
- Shorter commands: `npm run dev` vs `hugo server -D`
- Consistent across team members
- Works on any OS (Windows, Mac, Linux)
- Can chain multiple commands
- Easy to remember

**Example of chaining**:
```json
"scripts": {
  "rebuild": "npm run clean && npm run build"
}
```

---

## Repository Information

```json
{
  "repository": {
    "type": "git",
    "url": "git+https://github.com/rafalole/dea-web.git"
  },
  "bugs": {
    "url": "https://github.com/rafalole/dea-web/issues"
  },
  "homepage": "https://github.com/rafalole/dea-web#readme"
}
```

### repository
- **type**: Version control system (git)
- **url**: GitHub repository URL
- **Usage**: npm, documentation tools, GitHub integration

### bugs
- **url**: Where to report issues
- **Links to**: GitHub Issues page

### homepage
- **url**: Project homepage
- **Links to**: GitHub README

---

## Metadata (Optional)

```json
{
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

### keywords
- **Value**: Empty array
- **Purpose**: Search terms for npm registry
- **For DeaPrint**: Not published to npm, so empty

### author
- **Value**: Empty string
- **Purpose**: Project author/maintainer
- **Can Add**: Your name or team name

### license
- **Value**: `ISC`
- **Purpose**: Software license
- **ISC**: Permissive open source license (similar to MIT)
- **Allows**: Free use, modification, distribution

---

## Dependencies (Most Important!)

```json
{
  "devDependencies": {
    "@tailwindcss/postcss": "^4.1.17",
    "autoprefixer": "^10.4.22",
    "daisyui": "^5.5.5",
    "postcss": "^8.5.6",
    "tailwindcss": "^4.1.17"
  }
}
```

### What are devDependencies?

Packages needed **only during development/build**, not in production.

**Why devDependencies for DeaPrint?**
- Hugo generates static HTML/CSS/JS
- No Node.js runs in production
- Packages only needed to build CSS

### Package Breakdown

#### tailwindcss: ^4.1.17
- **What**: Tailwind CSS framework
- **Purpose**: Utility-first CSS framework
- **Used in**: `assets/css/main.css`
- **Provides**: CSS utility classes (flex, grid, text-center, etc.)

#### @tailwindcss/postcss: ^4.1.17
- **What**: Tailwind v4 PostCSS plugin
- **Purpose**: Processes Tailwind CSS in v4
- **Used in**: `postcss.config.js`
- **Note**: Required for Tailwind v4 (different from v3)

#### daisyui: ^5.5.5
- **What**: UI component library for Tailwind
- **Purpose**: Pre-built components (buttons, cards, navbars)
- **Used in**: `postcss.config.js`, `assets/css/main.css`
- **Provides**: Semantic classes (btn, card, navbar, etc.)

#### postcss: ^8.5.6
- **What**: CSS processor/transformer
- **Purpose**: Transforms CSS (runs plugins like Tailwind)
- **Used in**: Hugo Pipes automatically
- **Note**: Core tool that runs other plugins

#### autoprefixer: ^10.4.22
- **What**: Adds vendor prefixes to CSS
- **Purpose**: Browser compatibility
- **Used in**: `postcss.config.js` (if configured)
- **Adds**: `-webkit-`, `-moz-`, `-ms-` prefixes automatically

### Version Format Explained

**`^4.1.17`** (Caret)
- **Means**: Compatible with 4.1.17
- **Allows**: 4.1.18, 4.2.0, 4.9.99
- **Blocks**: 5.0.0 (major version = breaking changes)

**`~4.1.17`** (Tilde - not used here)
- **Means**: Approximately 4.1.17
- **Allows**: 4.1.18, 4.1.99
- **Blocks**: 4.2.0

**`4.1.17`** (Exact - not used here)
- **Means**: Exactly 4.1.17
- **Allows**: Nothing else
- **Blocks**: Any other version

---

## How Dependencies Work

### 1. Define Dependencies
```bash
npm install -D tailwindcss
```
→ Adds to `package.json` devDependencies

### 2. Install Dependencies
```bash
npm install
```
→ Reads `package.json`
→ Downloads packages to `node_modules/`
→ Creates `package-lock.json` (exact versions)

### 3. Use Dependencies
```javascript
// postcss.config.js
module.exports = {
  plugins: {
    '@tailwindcss/postcss': {},  // Uses node_modules/@tailwindcss/postcss
  }
}
```

### 4. Build Process
```
Hugo → PostCSS → Tailwind/DaisyUI → Processed CSS → public/
```

---

## Common npm Commands

```bash
# Install all dependencies from package.json
npm install

# Add new dependency
npm install -D package-name

# Remove dependency
npm uninstall package-name

# Update all dependencies
npm update

# Check for outdated packages
npm outdated

# View installed packages
npm list

# Run script
npm run dev
```

---

## What Can Be Added

### Additional Scripts

```json
"scripts": {
  "preview": "hugo server --minify",
  "translate": "node scripts/translate.js",
  "optimize-images": "node scripts/optimize-images.js",
  "deploy": "hugo --minify && wrangler pages deploy public"
}
```

### Production Dependencies

```json
"dependencies": {
  "some-runtime-package": "^1.0.0"
}
```
- Not needed for DeaPrint (static site)
- Only needed if Node.js runs in production

---

## File Relationships

### package.json → node_modules/
```
package.json (defines what to install)
     ↓
npm install
     ↓
node_modules/ (installed packages)
```

### package.json → Scripts → Hugo
```
package.json (defines "dev" script)
     ↓
npm run dev
     ↓
hugo server -D (executes)
```

### Dependencies → Build Process
```
devDependencies (Tailwind, PostCSS, DaisyUI)
     ↓
postcss.config.js (configures plugins)
     ↓
assets/css/main.css (uses plugins)
     ↓
Hugo Pipes (processes CSS)
     ↓
public/css/main.css (final output)
```

---

## Summary

### This file does 3 things:

1. **Defines project** (name, version, description)
2. **Lists dependencies** (what packages to install)
3. **Provides shortcuts** (npm run dev, npm run build)

### Most important parts:
- **scripts**: Commands you'll use daily
- **devDependencies**: Packages needed to build

### Can ignore:
- **main**, **directories**: Not used in Hugo projects
- **keywords**, **author**: Just metadata

### Key takeaway:
`package.json` is the **blueprint** for your project. It tells npm what to install and provides shortcuts for common tasks.

---

## Related Documentation

- [setup.md](setup.md) - Development environment setup
- [hugo-structure.md](hugo-structure.md) - Hugo directory structure
- [tailwind-v4-setup.md](tailwind-v4-setup.md) - Tailwind CSS v4 configuration
