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

| Category  | Requirement      | Why It's Needed              |
|-----------|------------------|------------------------------|
| Files     | Phase 0 complete | Hugo and Tailwind configured |
| Knowledge | HTML basics      | Template structure           |

### Substeps

- [ ] **Substep 1.1.1**: Create baseof.html template
  ```bash
  cat > layouts/_default/baseof.html << 'EOF'
  <!DOCTYPE html>
  <html lang="{{ .Language.LanguageCode }}">
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

- [ ] **Substep 1.1.2**: Create header partial with mobile menu
  ```bash
  cat > layouts/partials/header.html << 'EOF'
  <header class="navbar bg-base-100 shadow-lg">
    <div class="container mx-auto">
      <div class="flex-1">
        <a href="{{ .Site.Home.RelPermalink }}" class="btn btn-ghost text-xl">{{ .Site.Params.company }}</a>
      </div>
      
      <!-- Desktop Navigation -->
      <div class="flex-none hidden lg:flex">
        <ul class="menu menu-horizontal px-1">
          <li><a href="{{ .Site.Home.RelPermalink }}">Home</a></li>
          <li><a href="{{ relLangURL "about" }}">About</a></li>
          <li><a href="{{ relLangURL "services" }}">Services</a></li>
          <li><a href="{{ relLangURL "contact" }}">Contact</a></li>
        </ul>
        <!-- Language Switcher -->
        <div class="dropdown dropdown-end">
          <label tabindex="0" class="btn btn-ghost">
            {{ .Language.LanguageName }}
          </label>
          <ul tabindex="0" class="dropdown-content menu p-2 shadow bg-base-100 rounded-box w-32">
            {{ range .Site.Languages }}
              <li><a href="{{ .Lang }}/">{{ .LanguageName }}</a></li>
            {{ end }}
          </ul>
        </div>
      </div>

      <!-- Mobile Menu Button -->
      <div class="flex-none lg:hidden">
        <div class="dropdown dropdown-end">
          <label tabindex="0" class="btn btn-ghost btn-circle">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
            </svg>
          </label>
          <ul tabindex="0" class="dropdown-content menu p-2 shadow bg-base-100 rounded-box w-52 mt-3">
            <li><a href="{{ .Site.Home.RelPermalink }}">Home</a></li>
            <li><a href="{{ relLangURL "about" }}">About</a></li>
            <li><a href="{{ relLangURL "services" }}">Services</a></li>
            <li><a href="{{ relLangURL "contact" }}">Contact</a></li>
            <li class="menu-title">Language</li>
            {{ range .Site.Languages }}
              <li><a href="{{ .Lang }}/">{{ .LanguageName }}</a></li>
            {{ end }}
          </ul>
        </div>
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
- [ ] Test: Resize browser to mobile width (< 1024px)
- [ ] Expected: Navigation collapses to hamburger menu

### Documentation Updates

- [ ] Document template structure in `/docs/dev/templates.md`

---

## Step 1.2: Build Home Page

**Goal**: Create attractive homepage with hero section and service overview

**Time Estimate**: 3-4 hours

**Deployment**: No

### Substeps

- [ ] **Substep 1.2.1**: Create homepage content for all languages
  ```bash
  # Polish (default)
  cat > content/pl/_index.md << 'EOF'
  ---
  title: "Profesjonalne Usługi Fotograficzne we Wrocławiu"
  description: "DeaPrint oferuje profesjonalne usługi fotograficzne: zdjęcia do dokumentów, sesje zdjęciowe, wydruki, fotoksiążki i kalendarze we Wrocławiu."
  ---
  EOF

  # English
  cat > content/en/_index.md << 'EOF'
  ---
  title: "Professional Photography Services in Wrocław"
  description: "DeaPrint offers professional photography services including document photos, photo sessions, prints, photobooks, and calendars in Wrocław."
  ---
  EOF

  # Ukrainian
  cat > content/uk/_index.md << 'EOF'
  ---
  title: "Професійні Фотопослуги у Вроцлаві"
  description: "DeaPrint пропонує професійні фотопослуги: фото на документи, фотосесії, друк, фотокниги та календарі у Вроцлаві."
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
        <p class="py-6">{{ .Site.Params.description }}</p>
        <a href='{{ relLangURL "contact" }}' class="btn btn-primary">{{ i18n "contactUs" }}</a>
      </div>
    </div>
  </section>

  <!-- Services Overview -->
  <section class="py-16">
    <div class="container mx-auto px-4">
      <h2 class="text-3xl font-bold text-center mb-12">{{ i18n "ourServices" }}</h2>
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
        <div class="card bg-base-100 shadow-xl">
          <div class="card-body">
            <h3 class="card-title">{{ i18n "documentPhotos" }}</h3>
            <p>{{ i18n "documentPhotosDesc" }}</p>
            <div class="card-actions justify-end">
              <a href='{{ relLangURL "services" }}' class="btn btn-primary">{{ i18n "learnMore" }}</a>
            </div>
          </div>
        </div>
        <div class="card bg-base-100 shadow-xl">
          <div class="card-body">
            <h3 class="card-title">{{ i18n "hqPrints" }}</h3>
            <p class="text-sm opacity-70">{{ i18n "hqPrintsSubtitle" }}</p>
            <p>{{ i18n "hqPrintsDesc" }}</p>
            <div class="card-actions justify-end">
              <a href='{{ relLangURL "services" }}' class="btn btn-primary">{{ i18n "learnMore" }}</a>
            </div>
          </div>
        </div>
        <div class="card bg-base-100 shadow-xl">
          <div class="card-body">
            <h3 class="card-title">{{ i18n "photobooks" }}</h3>
            <p>{{ i18n "photobooksDesc" }}</p>
            <div class="card-actions justify-end">
              <a href='{{ relLangURL "services" }}' class="btn btn-primary">{{ i18n "learnMore" }}</a>
            </div>
          </div>
        </div>
        <div class="card bg-base-100 shadow-xl">
          <div class="card-body">
            <h3 class="card-title">{{ i18n "photoCalendars" }}</h3>
            <p>{{ i18n "photoCalendarsDesc" }}</p>
            <div class="card-actions justify-end">
              <a href='{{ relLangURL "services" }}' class="btn btn-primary">{{ i18n "learnMore" }}</a>
            </div>
          </div>
        </div>
        <div class="card bg-base-100 shadow-xl">
          <div class="card-body">
            <h3 class="card-title">{{ i18n "photoRenovation" }}</h3>
            <p class="text-sm opacity-70">{{ i18n "photoRenovationSubtitle" }}</p>
            <p>{{ i18n "photoRenovationDesc" }}</p>
            <div class="card-actions justify-end">
              <a href='{{ relLangURL "services" }}' class="btn btn-primary">{{ i18n "learnMore" }}</a>
            </div>
          </div>
        </div>
        <div class="card bg-base-100 shadow-xl">
          <div class="card-body">
            <h3 class="card-title">{{ i18n "vhsAndSlides" }}</h3>
            <p>{{ i18n "vhsAndSlidesDesc" }}</p>
            <div class="card-actions justify-end">
              <a href='{{ relLangURL "services" }}' class="btn btn-primary">{{ i18n "learnMore" }}</a>
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
- [ ] Test: Resize browser window to mobile width
- [ ] Expected: Cards stack vertically (1 column)
- [ ] Test: Resize to desktop width
- [ ] Expected: Cards display in 3 columns

---

## Step 1.3: Build About Page

**Goal**: Create about page with Kasia's bio and studio information

**Time Estimate**: 2-3 hours

**Deployment**: No

### Substeps

- [ ] **Substep 1.3.1**: Create about page content for all languages
  ```bash
  # Polish
  cat > content/pl/about/_index.md << 'EOF'
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

- [ ] **Substep 1.4.1**: Create services content (Polish first, translate later)
  ```bash
  cat > content/pl/services/_index.md << 'EOF'
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

- [ ] **Substep 1.5.1**: Create contact content for all languages
  ```bash
  # Polish
  cat > content/pl/contact/_index.md << 'EOF'
  ---
  title: "Kontakt"
  description: "Skontaktuj się ze studiem fotograficznym DeaPrint we Wrocławiu"
  ---
  EOF

  # English
  cat > content/en/contact/_index.md << 'EOF'
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
          <!-- Google Maps for Warmińska 20/47, Wrocław -->
          <iframe 
            src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d2504.8!2d17.0385!3d51.1089!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x470fc27f60898c8d%3A0x1234567890!2sWarmi%C5%84ska%2020%2C%2054-110%20Wroc%C5%82aw!5e0!3m2!1sen!2spl!4v1234567890"
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
- [ ] Test: Resize to mobile width
- [ ] Expected: Form and info stack vertically
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
