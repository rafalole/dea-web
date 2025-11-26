# Phase 1: MVP - Basic Site

**Goal**: Deploy a working website with core pages

**Time Estimate**: 16-24 hours

**Deployment**: Yes - First production deployment

---

## Step 1.1: Create Base Layout Template

**Goal**: Build reusable base template with header, footer, and CSS integration

**Time Estimate**: 2-3 hours

**Deployment**: No

### Prerequisites

| Category | Requirement | Why It's Needed |
|----------|-------------|------------------|
| Files | Phase 0 complete | Hugo and Tailwind configured |
| Knowledge | HTML basics | Template structure |

### Substeps

- [ ] **Substep 1.1.1**: Create baseof.html template
  ```bash
  cat > layouts/_default/baseof.html << 'EOF'
  <!DOCTYPE html>
  <html lang="{{ .Site.LanguageCode }}">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ block "title" . }}{{ .Site.Title }}{{ end }}</title>
    <meta name="description" content="{{ block "description" . }}{{ .Site.Params.description }}{{ end }}">
    {{ $css := resources.Get "css/main.css" | resources.PostCSS }}
    <link rel="stylesheet" href="{{ $css.RelPermalink }}">
  </head>
  <body>
    {{ partial "header.html" . }}
    <main>
      {{ block "main" . }}{{ end }}
    </main>
    {{ partial "footer.html" . }}
  </body>
  </html>
  EOF
  ```

- [ ] **Substep 1.1.2**: Create header partial
  ```bash
  cat > layouts/partials/header.html << 'EOF'
  <header class="navbar bg-base-100 shadow-lg">
    <div class="container mx-auto">
      <div class="flex-1">
        <a href="/" class="btn btn-ghost text-xl">{{ .Site.Params.company }}</a>
      </div>
      <div class="flex-none">
        <ul class="menu menu-horizontal px-1">
          <li><a href="/">Home</a></li>
          <li><a href="/about/">About</a></li>
          <li><a href="/services/">Services</a></li>
          <li><a href="/contact/">Contact</a></li>
        </ul>
      </div>
    </div>
  </header>
  EOF
  ```

- [ ] **Substep 1.1.3**: Create footer partial
  ```bash
  cat > layouts/partials/footer.html << 'EOF'
  <footer class="footer footer-center p-10 bg-base-200 text-base-content">
    <div>
      <p class="font-bold">{{ .Site.Params.company }}</p>
      <p>{{ .Site.Params.address }}</p>
      <p>Phone: <a href="tel:+48{{ .Site.Params.phone }}">{{ .Site.Params.phone }}</a></p>
      <p>Email: <a href="mailto:{{ .Site.Params.email }}">{{ .Site.Params.email }}</a></p>
    </div>
    <div>
      <p>© {{ now.Year }} {{ .Site.Params.company }}. All rights reserved.</p>
    </div>
  </footer>
  EOF
  ```

### Learning Points

- **Concept - baseof.html**: The master template that wraps all pages. Uses `{{ block }}` to define sections that child templates can override.

- **Concept - Partials**: Reusable template fragments. Header and footer are partials because they appear on every page.

- **Concept - Hugo Pipes**: `resources.Get` and `resources.PostCSS` process CSS through Tailwind/PostCSS automatically.

### Verification

- [ ] Test: Run `hugo server -D`
- [ ] Expected: Server starts without errors
- [ ] Test: Visit http://localhost:1313
- [ ] Expected: See header with navigation and footer

### Documentation Updates

- [ ] Document template structure in `/docs/dev/templates.md`

---

## Step 1.2: Build Home Page

**Goal**: Create attractive homepage with hero section and service overview

**Time Estimate**: 3-4 hours

**Deployment**: No

### Substeps

- [ ] **Substep 1.2.1**: Update homepage content
  ```bash
  cat > content/_index.md << 'EOF'
  ---
  title: "Professional Photography Services in Wrocław"
  description: "DeaPrint offers professional photography services including document photos, photo sessions, prints, photobooks, and calendars in Wrocław."
  ---
  EOF
  ```

- [ ] **Substep 1.2.2**: Create homepage template
  ```bash
  cat > layouts/index.html << 'EOF'
  {{ define "title" }}{{ .Title }} - {{ .Site.Title }}{{ end }}
  {{ define "description" }}{{ .Description }}{{ end }}

  {{ define "main" }}
  <!-- Hero Section -->
  <section class="hero min-h-screen bg-base-200">
    <div class="hero-content text-center">
      <div class="max-w-md">
        <h1 class="text-5xl font-bold">{{ .Site.Params.company }}</h1>
        <p class="py-6">Professional photography services in Wrocław</p>
        <a href="/contact/" class="btn btn-primary">Contact Us</a>
      </div>
    </div>
  </section>

  <!-- Services Overview -->
  <section class="py-16">
    <div class="container mx-auto px-4">
      <h2 class="text-3xl font-bold text-center mb-12">Our Services</h2>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
        <div class="card bg-base-100 shadow-xl">
          <div class="card-body">
            <h3 class="card-title">Document Photos</h3>
            <p>Professional photos for passports, IDs, and visas</p>
            <div class="card-actions justify-end">
              <a href="/services/" class="btn btn-primary">Learn More</a>
            </div>
          </div>
        </div>
        <div class="card bg-base-100 shadow-xl">
          <div class="card-body">
            <h3 class="card-title">Photo Sessions</h3>
            <p>Studio photo sessions for individuals and families</p>
            <div class="card-actions justify-end">
              <a href="/services/" class="btn btn-primary">Learn More</a>
            </div>
          </div>
        </div>
        <div class="card bg-base-100 shadow-xl">
          <div class="card-body">
            <h3 class="card-title">Photobooks & Calendars</h3>
            <p>Custom photobooks and calendars with your photos</p>
            <div class="card-actions justify-end">
              <a href="/services/" class="btn btn-primary">Learn More</a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
  {{ end }}
  EOF
  ```

### Learning Points

- **Concept - Hero Section**: Large banner at top of page. DaisyUI provides `.hero` class for this pattern.

- **Pattern - Card Grid**: Responsive grid using Tailwind. `grid-cols-1 md:grid-cols-3` = 1 column on mobile, 3 on desktop.

### Verification

- [ ] Test: Visit http://localhost:1313
- [ ] Expected: See hero section and 3 service cards
- [ ] Test: Resize browser window
- [ ] Expected: Layout adapts to mobile/desktop

---

## Step 1.3: Build About Page

**Goal**: Create about page with Kasia's bio and studio information

**Time Estimate**: 2-3 hours

**Deployment**: No

### Substeps

- [ ] **Substep 1.3.1**: Create about page content
  ```bash
  cat > content/about/_index.md << 'EOF'
  ---
  title: "About DeaPrint"
  description: "Learn about Kasia and DeaPrint photography studio in Wrocław"
  ---

  ## About Kasia

  Professional photographer with years of experience in portrait and document photography.

  ## Our Studio

  Located in the heart of Wrocław at Warmińska 20/47, our studio offers a comfortable environment for all your photography needs.

  ## Why Choose Us

  - Professional equipment
  - Years of experience
  - Friendly atmosphere
  - Competitive prices
  - Quick turnaround
  EOF
  ```

- [ ] **Substep 1.3.2**: Create about page template
  ```bash
  cat > layouts/about/list.html << 'EOF'
  {{ define "title" }}{{ .Title }} - {{ .Site.Title }}{{ end }}

  {{ define "main" }}
  <div class="container mx-auto px-4 py-16">
    <h1 class="text-4xl font-bold mb-8">{{ .Title }}</h1>
    <div class="prose max-w-none">
      {{ .Content }}
    </div>
  </div>
  {{ end }}
  EOF
  ```

### Verification

- [ ] Test: Visit http://localhost:1313/about/
- [ ] Expected: See about page with content

---

## Step 1.4: Build Services Page

**Goal**: Display all services with pricing

**Time Estimate**: 3-4 hours

**Deployment**: No

### Substeps

- [ ] **Substep 1.4.1**: Create services content
  ```bash
  cat > content/services/_index.md << 'EOF'
  ---
  title: "Services & Pricing"
  description: "DeaPrint photography services and prices in Wrocław"
  ---

  ## Photos for Documents

  - Digital version: 40 PLN
  - Paper version (4 pieces): 50 PLN
  - Paper version (6 pieces): 55 PLN
  - Digital + Paper: +10 PLN

  Complimentary photo retouching included.

  ## Photo Sessions

  Studio photo sessions:
  - 3 photos: 100 PLN
  - Each additional photo: +20 PLN

  ## Prints

  All prints available on glossy or matte paper.

  ### Photo Lab Prints (7 days)
  - 9x13 cm: 3,50 PLN
  - 10x15 cm: 4,50 PLN
  - A4: 30 PLN
  - A3: 60 PLN

  ### Express Prints (same day)
  - 9x13 cm: 8 PLN
  - 10x15 cm: 10 PLN
  - A4: 40 PLN
  - A3: 80 PLN

  ## Photobooks

  Custom photobooks with your photos:
  - A4 landscape hard cover (24 pages): 80 PLN
  - A4 portrait hard cover (24 pages): 88 PLN
  - 30x30 cm square (24 pages): 90 PLN

  Additional pages: +4 PLN per 2 pages

  ## Photo Calendars

  - A3 calendar: 55 PLN
  - A4 calendar: 45 PLN
  - Express service: +15-20 PLN

  Design included with calendar order.
  EOF
  ```

- [ ] **Substep 1.4.2**: Create services template
  ```bash
  cat > layouts/services/list.html << 'EOF'
  {{ define "title" }}{{ .Title }} - {{ .Site.Title }}{{ end }}

  {{ define "main" }}
  <div class="container mx-auto px-4 py-16">
    <h1 class="text-4xl font-bold mb-8">{{ .Title }}</h1>
    <div class="prose max-w-none">
      {{ .Content }}
    </div>
    <div class="mt-12 text-center">
      <a href="/contact/" class="btn btn-primary btn-lg">Contact Us for Booking</a>
    </div>
  </div>
  {{ end }}
  EOF
  ```

### Verification

- [ ] Test: Visit http://localhost:1313/services/
- [ ] Expected: See all services with prices

---

## Step 1.5: Build Contact Page

**Goal**: Create contact page with form and location info

**Time Estimate**: 3-4 hours

**Deployment**: No

### Substeps

- [ ] **Substep 1.5.1**: Create contact content
  ```bash
  cat > content/contact/_index.md << 'EOF'
  ---
  title: "Contact Us"
  description: "Contact DeaPrint photography studio in Wrocław"
  ---
  EOF
  ```

- [ ] **Substep 1.5.2**: Create contact template with form
  ```bash
  cat > layouts/contact/list.html << 'EOF'
  {{ define "title" }}{{ .Title }} - {{ .Site.Title }}{{ end }}

  {{ define "main" }}
  <div class="container mx-auto px-4 py-16">
    <h1 class="text-4xl font-bold mb-8">{{ .Title }}</h1>
    
    <div class="grid grid-cols-1 md:grid-cols-2 gap-12">
      <!-- Contact Form -->
      <div>
        <h2 class="text-2xl font-bold mb-4">Send us a message</h2>
        <form class="space-y-4" action="https://formspree.io/f/YOUR_FORM_ID" method="POST">
          <div class="form-control">
            <label class="label">
              <span class="label-text">Name</span>
            </label>
            <input type="text" name="name" class="input input-bordered" required>
          </div>
          <div class="form-control">
            <label class="label">
              <span class="label-text">Email</span>
            </label>
            <input type="email" name="email" class="input input-bordered" required>
          </div>
          <div class="form-control">
            <label class="label">
              <span class="label-text">Phone</span>
            </label>
            <input type="tel" name="phone" class="input input-bordered">
          </div>
          <div class="form-control">
            <label class="label">
              <span class="label-text">Message</span>
            </label>
            <textarea name="message" class="textarea textarea-bordered h-24" required></textarea>
          </div>
          <button type="submit" class="btn btn-primary">Send Message</button>
        </form>
      </div>

      <!-- Contact Info -->
      <div>
        <h2 class="text-2xl font-bold mb-4">Visit us</h2>
        <div class="space-y-4">
          <div>
            <h3 class="font-bold">Address</h3>
            <p>{{ .Site.Params.address }}</p>
          </div>
          <div>
            <h3 class="font-bold">Phone</h3>
            <p><a href="tel:+48{{ .Site.Params.phone }}" class="link">{{ .Site.Params.phone }}</a></p>
          </div>
          <div>
            <h3 class="font-bold">Email</h3>
            <p><a href="mailto:{{ .Site.Params.email }}" class="link">{{ .Site.Params.email }}</a></p>
          </div>
        </div>

        <!-- Google Maps -->
        <div class="mt-8">
          <iframe 
            src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d2505.123!2d17.0123!3d51.1234!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x0%3A0x0!2zNTHCsDA3JzI0LjIiTiAxN8KwMDAnNDQuMyJF!5e0!3m2!1sen!2spl!4v1234567890"
            width="100%" 
            height="300" 
            style="border:0;" 
            allowfullscreen="" 
            loading="lazy">
          </iframe>
        </div>
      </div>
    </div>
  </div>
  {{ end }}
  EOF
  ```

### Learning Points

- **Concept - Formspree**: Free form backend. Sign up at formspree.io and replace YOUR_FORM_ID.

- **Pattern - Two Column Layout**: Form on left, info on right. Stacks on mobile.

### Verification

- [ ] Test: Visit http://localhost:1313/contact/
- [ ] Expected: See contact form and info
- [ ] Test: Submit form (after Formspree setup)
- [ ] Expected: Receive email

### Documentation Updates

- [ ] Document Formspree setup in `/docs/admin/forms.md`

---

## Step 1.6: Add SEO Metadata

**Goal**: Improve search engine visibility

**Time Estimate**: 1-2 hours

**Deployment**: No

### Substeps

- [ ] **Substep 1.6.1**: Update baseof.html with SEO tags
  ```html
  <!-- Add to <head> in baseof.html -->
  <meta property="og:title" content="{{ block "title" . }}{{ .Site.Title }}{{ end }}">
  <meta property="og:description" content="{{ block "description" . }}{{ .Site.Params.description }}{{ end }}">
  <meta property="og:type" content="website">
  <meta property="og:url" content="{{ .Permalink }}">
  <link rel="canonical" href="{{ .Permalink }}">
  ```

### Verification

- [ ] Test: View page source
- [ ] Expected: See meta tags

---

## Step 1.7: Deploy to Cloudflare Pages

**Goal**: Make website live at deaprint.pl

**Time Estimate**: 2-3 hours

**Deployment**: Yes

### Substeps

- [ ] **Substep 1.7.1**: Create Cloudflare Pages project
  ```
  1. Go to dash.cloudflare.com
  2. Pages → Create a project
  3. Connect to GitHub repository
  4. Build settings:
     - Framework: Hugo
     - Build command: hugo --minify
     - Build output: /public
  5. Deploy
  ```

- [ ] **Substep 1.7.2**: Configure custom domain
  ```
  1. Pages → Custom domains
  2. Add deaprint.pl
  3. Follow DNS instructions
  ```

### Verification

- [ ] Test: Visit https://deaprint.pl
- [ ] Expected: Live website

### Documentation Updates

- [ ] Document deployment in `/docs/admin/deployment.md`

---

## Phase 1 Complete! ✅

**What you've accomplished**:
- ✅ Base template with header/footer
- ✅ Homepage with hero and services
- ✅ About page
- ✅ Services page with pricing
- ✅ Contact page with form
- ✅ SEO metadata
- ✅ Live website at deaprint.pl

**Next**: Phase 2 - Admin Panel (Sveltia CMS)
