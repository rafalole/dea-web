# DeaPrint Website - Complete Implementation Plan

Generated from: [master-implementation-plan.md](master-implementation-plan.md)

---

## Master Plan Overview

### Total Time Estimate

**Total Project Time**: 80-120 hours (10-15 days of focused work)

**Breakdown by Phase**:
- Phase 0: Setup & Environment (4-6 hours)
- Phase 1: MVP - Basic Site (16-24 hours)
- Phase 2: Admin Panel (8-12 hours)
- Phase 3: Info Messages System (6-8 hours)
- Phase 4: Google Reviews Integration (4-6 hours)
- Phase 5: Multilingual Support (12-16 hours)
- Phase 6: Blog/Articles (8-12 hours)
- Phase 7: Portfolio/Gallery (10-14 hours)
- Phase 8: Advanced Features (12-20 hours)

**Note**: Times assume you're learning Hugo/Tailwind/Daisy UI as you go. Experienced developers could cut this by 30-40%.

---

### Technology Stack Rationale

| Technology | Why This Choice | Alternatives Considered |
|------------|----------------|------------------------|
| **Hugo Extended** | Fast static generation, excellent i18n, free hosting, perfect for content-heavy sites | Astro (easier but less mature), Next.js (overkill) |
| **Tailwind CSS** | Utility-first, rapid development, small bundle size, industry standard | Bootstrap (less flexible), custom CSS (slower) |
| **Daisy UI** | 50+ components, Tailwind-based, beautiful defaults, saves development time | Headless UI (more work), custom components (slower) |
| **Cloudflare Pages** | Free, fast CDN, automatic SSL, GitHub integration, unlimited bandwidth | Netlify (similar), Vercel (similar), GitHub Pages (slower) |
| **Sveltia CMS** | Modern, Git-based, works with Hugo, better UX than Decap CMS | Decap CMS (older), Forestry (discontinued), custom admin (too much work) |
| **GitHub Actions** | Free for public repos, integrated with GitHub, flexible CI/CD | GitLab CI (if using GitLab), Cloudflare Pages CI (simpler but less control) |
| **OpenRouter** | Model-agnostic, access to multiple LLMs, pay-as-you-go, no vendor lock-in | Direct API (vendor lock-in), Google Translate (less quality) |

---

### High-Level Phases

```
Phase 0: Setup & Environment
    ↓
Phase 1: MVP (Home, About, Services, Contact)
    ↓ [DEPLOY TO PRODUCTION]
    ↓
Phase 2: Admin Panel (Sveltia CMS)
    ↓ [DEPLOY]
    ↓
Phase 3: Info Messages System
    ↓ [DEPLOY]
    ↓
Phase 4: Google Reviews Integration
    ↓ [DEPLOY]
    ↓
Phase 5: Multilingual Support
    ↓ [DEPLOY]
    ↓
Phase 6: Blog/Articles
    ↓ [DEPLOY]
    ↓
Phase 7: Portfolio/Gallery
    ↓ [DEPLOY]
    ↓
Phase 8: Advanced Features
    ↓ [DEPLOY]
```

---

### Critical Path & Dependencies

**Critical Path** (must be done in order):
1. Setup → MVP → Production Deployment
2. Admin Panel (enables easier content management for later phases)
3. Multilingual (affects all content created after)

**Can be done in parallel** (after MVP):
- Info Messages + Google Reviews
- Blog + Portfolio (independent features)

**Flexible order**:
- Advanced features can be added anytime after MVP

---

## Example Step Format

Before diving into the full plan, here's one complete example step showing the exact format:

---

## Step 1.3: Create Home Page Layout

**Goal**: Build a responsive home page with hero section, services overview, and contact CTA

**Time Estimate**: 3-4 hours - Includes learning Daisy UI components, creating Hugo template, styling with Tailwind, and testing responsiveness

**Deployment**: No - Part of MVP, deploys with Phase 1

### Prerequisites

| Category | Requirement | Why It's Needed |
|----------|-------------|------------------|
| Software | Hugo Extended v0.121.0+ installed | Processes Tailwind CSS and Daisy UI |
| Software | Node.js v18+ and npm | Installs Tailwind and Daisy UI packages |
| Knowledge | Basic HTML structure | Understanding of semantic markup |
| Knowledge | Daisy UI hero component | Pre-built component for hero sections |
| Files | Hugo project initialized | From Step 1.1 |
| Files | Tailwind + Daisy UI configured | From Step 1.2 |

### Substeps

- [ ] **Substep 1.3.1**: Create home page content file
  ```bash
  # Create content file for home page
  touch content/_index.md
  ```
  Expected outcome: Empty markdown file for home page content

- [ ] **Substep 1.3.2**: Add frontmatter to home page
  ```yaml
  ---
  title: "DeaPrint - Professional Photography in Wrocław"
  description: "Document photos, photo prints, photobooks, and calendars. Expert photography services with 5.0 star rating."
  ---
  ```
  Expected outcome: SEO-optimized metadata for home page

- [ ] **Substep 1.3.3**: Create home page template
  ```bash
  # Create layouts directory and home template
  mkdir -p layouts/_default
  touch layouts/index.html
  ```
  Expected outcome: Template file for home page layout

- [ ] **Substep 1.3.4**: Build hero section with Daisy UI
  ```html
  <!-- layouts/index.html -->
  {{ define "main" }}
  <div class="hero min-h-screen bg-base-200">
    <div class="hero-content text-center">
      <div class="max-w-md">
        <h1 class="text-5xl font-bold">{{ .Title }}</h1>
        <p class="py-6">{{ .Description }}</p>
        <a href="tel:882947647" class="btn btn-primary">Call Now</a>
        <a href="/contact" class="btn btn-outline">Contact Us</a>
      </div>
    </div>
  </div>
  {{ end }}
  ```
  Expected outcome: Responsive hero section with CTA buttons

- [ ] **Substep 1.3.5**: Add services overview section
  ```html
  <!-- Add after hero section -->
  <div class="container mx-auto px-4 py-16">
    <h2 class="text-3xl font-bold text-center mb-12">Our Services</h2>
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
      <!-- Service cards will go here -->
    </div>
  </div>
  ```
  Expected outcome: Grid layout for service cards

- [ ] **Substep 1.3.6**: Test responsive design
  ```bash
  # Start Hugo dev server
  hugo server -D
  ```
  Expected outcome: Site running at http://localhost:1313, test on mobile/tablet/desktop

### Learning Points

- **Concept - Hugo Templates**: Hugo uses Go templating. `{{ define "main" }}` creates a content block that gets inserted into your base layout. This separation of concerns (layout vs content) is reusable across all static site generators.

- **Concept - Daisy UI Components**: Daisy UI provides pre-styled components using class names like `hero`, `btn`, `btn-primary`. These are built on Tailwind utilities, so you get consistency without writing custom CSS.

- **Pattern - Mobile-First Design**: Tailwind's responsive classes (`md:grid-cols-2`, `lg:grid-cols-4`) apply styles at breakpoints. Start with mobile (default), then add larger screen styles. This pattern applies to any CSS framework.

- **Pattern - Semantic HTML**: Using `<h1>` for main title, `<h2>` for section titles improves SEO and accessibility. Search engines and screen readers understand your content structure.

### Verification

- [ ] **Test**: Open http://localhost:1313 in browser
- [ ] **Expected result**: Home page displays with hero section, title, description, and two buttons
- [ ] **Test**: Resize browser window from mobile (375px) to desktop (1920px)
- [ ] **Expected result**: Layout adapts smoothly, no horizontal scrolling, buttons stack on mobile
- [ ] **Test**: Click "Call Now" button on mobile device
- [ ] **Expected result**: Phone dialer opens with number 882947647
- [ ] **Test**: Click "Contact Us" button
- [ ] **Expected result**: Navigates to /contact (will be 404 until Step 1.6)
- [ ] **Troubleshooting**: If Tailwind classes don't work, check that Hugo server is running with `-D` flag and Tailwind is configured in Step 1.2

### Documentation Updates

- [ ] **Update**: Create `/docs/dev/templates.md`
  - Add: "Home page uses `layouts/index.html` template"
  - Add: "Hero section uses Daisy UI `hero` component"
  - Add: "Responsive grid uses Tailwind breakpoints: mobile (default), md (768px+), lg (1024px+)"

- [ ] **Update**: Create `/docs/content/home-page.md`
  - Add: "To update home page title/description, edit `content/_index.md` frontmatter"
  - Add: "Phone number in Call Now button is hardcoded in template (will be moved to config in Phase 2)"

### Edge Cases

- **If hero section is too tall on mobile**: Reduce `min-h-screen` to `min-h-[60vh]` for better mobile UX
- **If buttons overlap on small screens**: Add `flex flex-col gap-2` to button container for vertical stacking
- **If content doesn't update**: Hugo caches templates; restart server with `hugo server -D --disableFastRender`
- **If Daisy UI classes don't apply**: Check `tailwind.config.js` has `require("daisyui")` in plugins array

---

