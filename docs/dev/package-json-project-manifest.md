# Project Manifest (package.json)

See [package.json](../../package.json)

npm manifest that defines project metadata, dependencies, and scripts.

## Project Metadata

```json
{
  "name": "dea-web",
  "version": "1.0.0",
  "description": "DeaPrint Photography Website"
}
```

## Scripts

Command shortcuts run with `npm run <script-name>`.

```json
{
  "scripts": {
    "dev": "hugo server -D",
    "build": "hugo --minify",
    "clean": "rm -rf public resources"
  }
}
```

### npm run dev
```bash
npm run dev
```
- Starts Hugo development server
- Includes draft content (`-D`)
- Available at http://localhost:1313
- Auto-reloads on changes

### npm run build
```bash
npm run build
```
- Builds production site
- Minifies HTML/CSS/JS
- Output to `public/`

### npm run clean
```bash
npm run clean
```
- Deletes `public/` and `resources/`
- Use before fresh build

## Dependencies

```json
{
  "devDependencies": {
    "@tailwindcss/postcss": "^4.1.17",
    "autoprefixer": "^10.4.22",
    "daisyui": "^5.5.5",
    "postcss": "^8.5.6",
    "postcss-cli": "^11.0.0",
    "tailwindcss": "^4.1.17"
  }
}
```

### Package Breakdown

**tailwindcss:** CSS framework
**@tailwindcss/postcss:** Tailwind v4 PostCSS plugin
**daisyui:** UI component library
**postcss:** CSS processor
**postcss-cli:** PostCSS command-line interface (required by Hugo)
**autoprefixer:** Adds vendor prefixes

### Version Format

**`^4.1.17`** (Caret):
- Allows: 4.1.18, 4.2.0, 4.9.99
- Blocks: 5.0.0 (major version)

## How Dependencies Work

1. **Define:** `npm install -D tailwindcss`
2. **Install:** `npm install` → Downloads to `node_modules/`
3. **Use:** Referenced in `postcss.config.js`
4. **Build:** Hugo → PostCSS → Tailwind → CSS

## Common npm Commands

```bash
npm install              # Install all dependencies
npm install -D package   # Add new dependency
npm uninstall package    # Remove dependency
npm update               # Update all
npm outdated             # Check for updates
npm run dev              # Run script
```

## Repository Information

```json
{
  "repository": {
    "type": "git",
    "url": "git+https://github.com/rafalole/dea-web.git"
  }
}
```

## Summary

**This file does 3 things:**
1. Defines project (name, version, description)
2. Lists dependencies (packages to install)
3. Provides shortcuts (npm run dev, build, clean)

**Most important:**
- **scripts:** Commands you'll use daily
- **devDependencies:** Packages needed to build
