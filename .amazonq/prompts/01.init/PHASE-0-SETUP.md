# Phase 0: Setup & Environment

**Goal**: Set up development environment and initialize project structure

**Time Estimate**: 4-6 hours

**Deployment**: No - Preparation phase

---

## Step 0.1: Install Required Software

**Goal**: Install Hugo Extended, Node.js, and Git on your system

**Time Estimate**: 1 hour - Includes download, installation, and verification

**Deployment**: No

### Prerequisites

| Category | Requirement            | Why It's Needed            |
|----------|------------------------|----------------------------|
| System   | Linux operating system | Based on your IDE context  |
| Access   | sudo/root access       | To install system packages |
| Internet | Stable connection      | Download packages          |

### Substeps

- [ ] **Substep 0.1.1**: Install Hugo Extended
  ```bash
  # Check if Hugo is already installed
  hugo version
  
  # If not installed or version < 0.121.0, install latest
  wget https://github.com/gohugoio/hugo/releases/download/v0.121.2/hugo_extended_0.121.2_linux-amd64.deb
  sudo dpkg -i hugo_extended_0.121.2_linux-amd64.deb
  
  # Verify installation
  hugo version  # Should show "hugo v0.121.2+extended"
  ```

- [ ] **Substep 0.1.2**: Install Node.js and npm
  ```bash
  # Check current version
  node --version
  npm --version
  
  # If not installed or version < 18, install via nvm (recommended)
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
  source ~/.bashrc
  nvm install 18
  nvm use 18
  
  # Verify
  node --version  # Should show v18.x.x
  npm --version   # Should show 9.x.x or higher
  ```

- [ ] **Substep 0.1.3**: Verify Git installation
  ```bash
  # Git should already be installed on your system
  git --version  # Should show git version 2.x.x
  
  # Configure Git if not already done
  git config --global user.name "Your Name"
  git config --global user.email "your.email@example.com"
  ```

### Learning Points

- **Concept - Hugo Extended**: The "Extended" version includes Sass/SCSS support and is required for Tailwind CSS processing. Regular Hugo won't work with our stack.

- **Concept - Node Version Manager (nvm)**: Using nvm instead of system Node.js allows you to switch between Node versions per project. This is a best practice for JavaScript development.

- **Pattern - Version Verification**: Always verify installed versions match requirements. Mismatched versions are the #1 cause of "it works on my machine" problems.

### Verification

- [x] Test: Run `hugo version` `hugo v0.152.2-6abdacad3f3fe944ea42177844469139e81feda6+extended linux/amd64 BuildDate=2025-10-24T15:31:49Z VendorInfo=snap:0.152.2`
- [x] Expected: Output contains "extended" keyword
- [x] Test: Run `node --version && npm --version`
- [x] Expected: Node v18+ and npm v9+
- [x] Test: Run `git --version`
- [x] Expected: Git v2.x.x

### Documentation Updates

- [x] Create `/docs/dev/setup.md` with installation commands
- [x] Document versions used for reproducibility

### Edge Cases

- **If Hugo Extended not available for your architecture**: Build from source or use Docker
- **If nvm installation fails**: Use system package manager (apt, yum) but may get older versions
- **If permission denied errors**: Use sudo for system-wide installation or nvm for user-level

---

## Step 0.2: Create GitHub Repository

**Goal**: Set up version control and remote repository

**Time Estimate**: 30 minutes

**Deployment**: No

### Prerequisites

| Category  | Requirement        | Why It's Needed                |
|-----------|--------------------|--------------------------------|
| Account   | GitHub account     | Version control and deployment |
| Software  | Git installed      | From Step 0.1.3                |
| Knowledge | Basic Git commands | commit, push, pull             |

### Substeps

- [x] **Substep 0.2.1**: Create repository on GitHub
  ```
  1. Go to https://github.com/new
  2. Repository name: dea-web
  3. Description: "DeaPrint Photography Website"
  4. Visibility: Public (for free GitHub Pages/Actions)
  5. Initialize: Do NOT add README, .gitignore, or license (we'll create these)
  6. Click "Create repository"
  ```

- [x] **Substep 0.2.2**: Initialize local Git repository
  ```bash
  # Navigate to your project directory
  cd /home/devlin/src/github/dea-web
  
  # Initialize Git (if not already done)
  git init
  
  # Create .gitignore
  cat > .gitignore << 'EOF'
  # Hugo
  /public/
  /resources/_gen/
  /assets/jsconfig.json
  hugo_stats.json
  
  # Node
  node_modules/
  package-lock.json
  
  # OS
  .DS_Store
  Thumbs.db
  
  # IDE
  .vscode/
  .idea/
  *.swp
  *.swo
  
  # Temporary
  *.log
  .hugo_build.lock
  EOF
  ```

- [x] **Substep 0.2.3**: Connect to remote repository
  ```bash
  # Add remote (replace with your GitHub username)
  git remote add origin https://github.com/YOUR_USERNAME/dea-web.git
  
  # Verify remote
  git remote -v
  ```

- [x] **Substep 0.2.4**: Create initial commit
  ```bash
  # Stage all files
  git add .
  
  # Create initial commit
  git commit -m "Initial commit: Project setup"
  
  # Push to GitHub
  git push -u origin main
  ```

### Learning Points

- **Concept - .gitignore**: Prevents committing generated files (public/, node_modules/) that can be rebuilt. Keeps repository clean and reduces size.

- **Pattern - Public vs Private Repos**: Public repos get free GitHub Actions minutes and Pages hosting. Private repos have limits. For open-source projects or business sites, public is fine (code is visible, but that's okay for static sites).

- **Pattern - Branch Strategy**: We'll use main/preprod/develop branches. Main = production, preprod = staging, develop = active development. This is a simplified Git Flow.

### Verification

- [x] Test: Visit https://github.com/YOUR_USERNAME/dea-web
- [x] Expected: Repository exists with .gitignore file
- [x] Test: Run `git remote -v`
- [x] Expected: Shows origin pointing to your GitHub repo
- [x] Test: Run `git log`
- [x] Expected: Shows initial commit

### Documentation Updates

- [x] Create `/docs/admin/git-workflow.md` with branch strategy
- [xg] Document repository URL and access

### Edge Cases

- **If push fails with authentication error**: Set up SSH keys or use Personal Access Token (PAT) instead of password
- **If branch is named "master" instead of "main"**: Rename with `git branch -M main`
- **If repository already exists**: Clone it first, then add files

---

## Step 0.3: Initialize Hugo Project

**Goal**: Create Hugo project structure with basic configuration

**Time Estimate**: 1 hour

**Deployment**: No

### Prerequisites

| Category | Requirement                | Why It's Needed |
|----------|----------------------------|-----------------|
| Software | Hugo Extended installed    | From Step 0.1.1 |
| Files    | Git repository initialized | From Step 0.2   |

### Substeps

- [x] **Substep 0.3.1**: Create Hugo site
  ```bash
  # Navigate to project root
  cd /home/devlin/src/github/dea-web
  
  # Initialize Hugo site (in current directory)
  hugo new site . --force
  
  # This creates: config.toml, archetypes/, content/, layouts/, static/, themes/
  ```

- [x] _No needed_ **Substep 0.3.2**: Convert config.toml to hugo.toml
  ```bash
  # Hugo now prefers hugo.toml over config.toml
  mv config.toml hugo.toml
  ```

- [ ] **Substep 0.3.3**: Configure basic Hugo settings
  ```toml
  # Edit hugo.toml
  baseURL = 'https://deaprint.pl/'
  languageCode = 'pl'
  title = 'DeaPrint - Professional Photography'
  theme = ''  # We'll build custom theme
  
  [params]
    company = 'DeaPrint'
    owner = 'Kasia'
    address = 'Warmińska 20/47, 54-110 Wrocław'
    phone = '882947647'
    email = 'deaprint.kasia@gmail.com'
    description = 'Professional photography services in Wrocław. Document photos, prints, photobooks, and calendars.'
  
  [markup]
    [markup.goldmark]
      [markup.goldmark.renderer]
        unsafe = true  # Allow HTML in markdown
  ```

- [ ] **Substep 0.3.4**: Create directory structure
  ```bash
  # Create additional directories
  mkdir -p layouts/_default
  mkdir -p layouts/partials
  mkdir -p assets/css
  mkdir -p static/images
  mkdir -p content/services
  mkdir -p content/about
  mkdir -p content/contact
  mkdir -p docs/dev
  mkdir -p docs/admin
  mkdir -p docs/content
  ```

- [ ] **Substep 0.3.5**: Test Hugo server
  ```bash
  # Start development server
  hugo server -D
  
  # Should see: "Web Server is available at http://localhost:1313/"
  # Open browser to verify (will show blank page - that's expected)
  ```

### Learning Points

- **Concept - Hugo Directory Structure**: 
  - `content/` = Markdown files (your pages)
  - `layouts/` = HTML templates (how pages look)
  - `static/` = Files copied as-is (images, fonts)
  - `assets/` = Files processed by Hugo (CSS, JS)
  - `public/` = Generated site (don't commit this)

- **Concept - Hugo Configuration**: `hugo.toml` is the brain of your site. It controls URLs, languages, menus, and custom parameters. Changes here affect the entire site.

- **Pattern - Params Section**: Custom parameters in `[params]` are accessible in templates via `{{ .Site.Params.phone }}`. This centralizes configuration instead of hardcoding values.

### Verification

- [ ] Test: Run `hugo version` in project directory
- [ ] Expected: No errors, shows Hugo version
- [ ] Test: Run `hugo server -D`
- [ ] Expected: Server starts on port 1313
- [ ] Test: Open http://localhost:1313
- [ ] Expected: Blank page (no content yet) but no errors
- [ ] Test: Check directory structure
- [ ] Expected: All folders created

### Documentation Updates

- [ ] Create `/docs/dev/hugo-structure.md` explaining directory purposes
- [ ] Document configuration parameters in `/docs/admin/configuration.md`

### Edge Cases

- **If port 1313 is in use**: Hugo will try 1314, 1315, etc. Or specify port with `hugo server -p 8080`
- **If "unsafe = true" concerns you**: It's needed for embedding HTML in markdown (like contact forms). Remove if you never use HTML in markdown.
- **If baseURL is wrong**: Update it before deployment. Wrong baseURL breaks links and assets.

---

## Step 0.4: Install Tailwind CSS and Daisy UI

**Goal**: Set up Tailwind CSS and Daisy UI for styling

**Time Estimate**: 1.5 hours - Includes npm setup, configuration, and testing

**Deployment**: No

### Prerequisites

| Category | Requirement              | Why It's Needed |
|----------|--------------------------|-----------------|
| Software | Node.js and npm          | From Step 0.1.2 |
| Files    | Hugo project initialized | From Step 0.3   |

### Substeps

- [x] **Substep 0.4.1**: Initialize npm project
  ```bash
  # Create package.json
  npm init -y
  
  # Update package.json with project info
  npm pkg set name="dea-web"
  npm pkg set description="DeaPrint Photography Website"
  npm pkg set version="1.0.0"
  ```

- [x] **Substep 0.4.2**: Install Tailwind CSS and dependencies
  ```bash
  # Install Tailwind, PostCSS, and Autoprefixer
  npm install -D tailwindcss@latest postcss@latest autoprefixer@latest
  
  # Install Daisy UI
  npm install -D daisyui@latest
  
  # Verify installation
  npm list tailwindcss daisyui
  ```

- [x] **Substep 0.4.3**: Create PostCSS configuration (Tailwind v4)
  ```bash
  # Install Tailwind v4 PostCSS plugin
  npm install -D @tailwindcss/postcss
  
  # Create postcss.config.js
  cat > postcss.config.js << 'EOF'
  module.exports = {
    plugins: {
      '@tailwindcss/postcss': {},
      'daisyui': {},
    },
  }
  EOF
  ```

- [x] **Substep 0.4.4**: Create main CSS file (Tailwind v4 uses CSS-based config)
  ```bash
  # Create assets/css/main.css
  cat > assets/css/main.css << 'EOF'
  @import "tailwindcss";
  @import "daisyui";
  
  @theme {
    --color-primary: #3b82f6;
    --color-secondary: #8b5cf6;
  }
  
  /* Custom styles */
  @layer components {
    .btn-phone {
      @apply btn btn-primary gap-2;
    }
  }
  EOF
  ```

- [x] **Substep 0.4.6**: Add npm scripts
  ```bash
  # Add build scripts to package.json
  npm pkg set scripts.dev="hugo server -D"
  npm pkg set scripts.build="hugo --minify"
  npm pkg set scripts.clean="rm -rf public resources"
  ```

### Learning Points

- **Concept - Tailwind v4 CSS Config**: Tailwind v4 uses CSS-based configuration with `@import` and `@theme` instead of JavaScript config files. Configuration is now in your CSS file.

- **Concept - Automatic Content Detection**: Tailwind v4 automatically scans `**/*.html` files. No need to specify content paths like in v3.

- **Concept - Daisy UI v5**: DaisyUI 5.5.5+ is compatible with Tailwind v4. Configure themes using CSS or separate config file.

- **Concept - PostCSS**: A tool that processes CSS. Both Tailwind and DaisyUI are PostCSS plugins.

- **Pattern - @theme**: Define custom design tokens in CSS using `@theme { --color-primary: #3b82f6; }`. Use in HTML as `class="text-primary"`.

- **Pattern - @layer components**: Create reusable component classes. `btn-phone` combines multiple utility classes into one semantic class.

- **Pattern - npm scripts**: Shortcuts for common commands. `npm run dev` is easier to remember than `hugo server -D`.

### Verification

- [ ] Test: Run `npm run dev`
- [ ] Expected: Hugo server starts without errors
- [ ] Test: Check `node_modules/` exists
- [ ] Expected: Contains tailwindcss and daisyui folders
- [ ] Test: Check `postcss.config.js` exists
- [ ] Expected: File contains @tailwindcss/postcss and daisyui plugins
- [ ] Test: Check `assets/css/main.css` exists
- [ ] Expected: File contains @import "tailwindcss" and @import "daisyui"

### Documentation Updates

- [ ] Create `/docs/dev/styling.md` explaining Tailwind and Daisy UI setup
- [ ] Document available Daisy UI themes and how to switch

### Edge Cases

- **If npm install fails**: Check Node version (must be 18+), try clearing npm cache with `npm cache clean --force`
- **If Tailwind classes don't work**: Verify `@import "tailwindcss"` is in main.css and postcss.config.js has @tailwindcss/postcss plugin
- **If CSS file is huge**: Tailwind v4 automatically optimizes; ensure you're using production build with `hugo --minify`
- **If Daisy UI components look wrong**: Ensure `@import "daisyui"` is in main.css and 'daisyui' is in postcss.config.js plugins

---

## Phase 0 Complete! ✅

**What you've accomplished**:
- ✅ Development environment set up
- ✅ Hugo Extended installed and verified
- ✅ GitHub repository created and connected
- ✅ Hugo project initialized with proper structure
- ✅ Tailwind CSS and Daisy UI installed and configured
- ✅ npm scripts ready for development

**Next**: Phase 1 - Build MVP (Home, About, Services, Contact pages)

**Time to celebrate**: You've laid the foundation! The hard setup work is done. Now comes the fun part - building the actual website! 🎉
