# DeaPrint - Professional Photography Website

A modern, responsive website for DeaPrint photography services built with Hugo, Tailwind CSS, and Daisy UI.

## 🚀 Features

- **Modern Design**: Clean, professional layout with Daisy UI components
- **Multilingual Support**: Polish, English, and Ukrainian languages
- **Responsive**: Mobile-first design that works on all devices
- **SEO Optimized**: Structured data, meta tags, and semantic HTML
- **Fast Loading**: Optimized images and minified assets
- **Contact Integration**: WhatsApp, phone, and email integration
- **GitHub Pages Deployment**: Automated deployment via GitHub Actions

## 🛠️ Technology Stack

- **Hugo**: Static site generator
- **Tailwind CSS**: Utility-first CSS framework
- **Daisy UI**: Component library for Tailwind CSS
- **GitHub Pages**: Hosting and deployment

## 📁 Project Structure

```
dea-web/
├── content/           # Markdown content files
│   ├── services/      # Services pages
│   ├── gallery/       # Gallery pages
│   ├── pricing/       # Pricing pages
│   └── contact/       # Contact pages
├── layouts/           # Hugo templates
│   ├── _default/      # Default layouts
│   ├── services/      # Services page layouts
│   ├── contact/       # Contact page layouts
│   └── pricing/       # Pricing page layouts
├── assets/css/        # CSS files
├── static/            # Static assets
├── .github/workflows/ # GitHub Actions
└── hugo.toml          # Hugo configuration
```

## 🚀 Getting Started

### Prerequisites

- Hugo Extended (v0.121.0 or later)
- Node.js (v18 or later)
- Git

### Installation

1. Clone the repository:
```bash
git clone https://github.com/rafalole/dea-web.git
cd dea-web
```

2. Install dependencies:
```bash
npm install
```

3. Start development server:
```bash
npm run dev
```

4. Open your browser and visit `http://localhost:1313`

### Available Scripts

- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm run clean` - Clean build artifacts
- `npm run preview` - Preview production build

## 🌐 Deployment

The site is automatically deployed to GitHub Pages when changes are pushed to the `main` branch.

### Manual Deployment

1. Build the site:
```bash
hugo --minify
```

2. The generated files will be in the `public/` directory

## 📝 Content Management

### Adding New Pages

1. Create a new markdown file in the appropriate content directory
2. Add front matter with title and description
3. Write your content in markdown

Example:
```markdown
---
title: "Page Title"
description: "Page description for SEO"
---

# Your Content Here
```

### Updating Contact Information

Edit the contact details in `hugo.toml`:

```toml
[params]
  company = 'DeaPrint'
  owner = 'Kasia'
  address = 'Warciańska 20/47, 54-101 Wrocław'
  phone = '882947647'
  email = 'deaprint.kasia@gmail.com'
```

## 🎨 Customization

### Colors and Themes

The site uses Daisy UI themes. You can customize colors by modifying the CSS variables in `assets/css/main.css`.

### Adding New Services

1. Update the services content in `content/services/_index.md`
2. Modify the services layout in `layouts/services/list.html`
3. Add service cards to the homepage in `layouts/index.html`

## 📱 Features

### Multilingual Support

The site supports three languages:
- Polish (default)
- English
- Ukrainian

Language files are automatically generated based on the Hugo configuration.

### Contact Features

- Click-to-call phone numbers
- WhatsApp integration
- Contact form (requires backend integration)
- Google Maps integration placeholder

### SEO Features

- Structured data for local business
- Open Graph meta tags
- Twitter Card meta tags
- Semantic HTML structure
- Optimized page titles and descriptions

## 🔧 Configuration

### Hugo Configuration

Key settings in `hugo.toml`:

- `baseURL`: Your site's URL
- `languages`: Multilingual configuration
- `params`: Site parameters (contact info, etc.)
- `menu`: Navigation menu items

### Tailwind Configuration

Customize Tailwind CSS in `tailwind.config.js`:

- Add custom colors
- Extend spacing
- Configure responsive breakpoints

## 📞 Support

For support and questions about this website:

- **Phone**: 882947647
- **Email**: deaprint.kasia@gmail.com
- **Address**: Warciańska 20/47, 54-101 Wrocław

## 📄 License

This project is created for DeaPrint photography services. All rights reserved.

---

Built with ❤️ for DeaPrint by Amazon Q Developer