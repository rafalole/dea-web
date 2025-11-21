# DeaPrint Website - Implementation Prompts

This folder contains all prompts and specifications for generating the DeaPrint photography website implementation plan.

## 📋 Quick Start

**To generate the complete implementation plan, use:**

```
@master-implementation-plan.md Generate the complete implementation plan
```

This is your **entry point** - it references all other files and contains all decisions made.

---

## 📁 File Structure

### Entry Point
- **`master-implementation-plan.md`** ⭐ **START HERE**
  - Complete project request with all decisions
  - References all specification files
  - Ready to generate the full implementation plan

### Meta Instructions
- **`prompts.md`**
  - Defines HOW to generate implementation plans
  - Output format requirements
  - Response guidelines and structure
  - Reusable for future projects

### Business Specifications
- **`deaprint-description.md`**
  - Company information
  - Contact details and business hours
  - Services overview
  - Languages supported

- **`about-kasia.md`**
  - 5 versions of photographer bio
  - Choose one or combine elements

- **`deaprint-services-prices.md`**
  - Complete service catalog
  - Pricing structure
  - Product descriptions

### Technical Requirements
- **`deaprint-requirements.md`**
  - Complete technical specifications
  - Site structure and features
  - Multilingual requirements
  - Git workflow and deployment
  - Testing and documentation strategy
  - Regulatory compliance (GDPR, cookies)

### Content Guidelines
- **`content-suggestions.md`**
  - Visual content recommendations for each page
  - Photo requirements (Kasia with camera, studio shots, etc.)
  - Copy guidelines and tone
  - Asset requirements summary

---

## 🎯 Project Overview

**Project**: DeaPrint Photography Website  
**Owner**: Kasia (photographer)  
**Domain**: deaprint.pl  
**Location**: Warmińska 20/47, 54-110 Wrocław, Poland

**Main Services**:
- Document photography (passports, IDs, visas)
- Photo prints and express printing
- Photobooks and calendars
- Photo sessions and portfolio work

---

## ✅ Decisions Made

### Technology Stack
- **Static Site Generator**: Hugo Extended
- **CSS Framework**: Tailwind CSS
- **Component Library**: Daisy UI
- **Hosting**: Cloudflare Pages (free tier)
- **CMS**: Sveltia CMS
- **Translation**: OpenRouter + LLM (model configurable)

### Implementation Approach
- **Strategy**: MVP first, then iterate
- **MVP**: Home, About, Services, Contact pages
- **Documentation**: Hybrid (basic as we go, detailed at end)

### Feature Priority (After MVP)
1. Admin Panel (Sveltia CMS)
2. Info Messages System
3. Google Reviews Integration
4. Multilingual Support (Polish, English, Ukrainian)
5. Blog/Articles
6. Portfolio/Gallery
7. Advanced Features (AI content, digital delivery)

---

## 🚀 How to Use These Prompts

### Generate Full Implementation Plan
```
@master-implementation-plan.md Generate the complete implementation plan
```

### Generate Specific Phase
```
@master-implementation-plan.md Generate detailed plan for Phase 2: Admin Panel
```

### Ask Questions About Requirements
```
@deaprint-requirements.md What are the multilingual requirements?
```

### Get Content Suggestions
```
@content-suggestions.md What photos do we need for the About page?
```

---

## 📝 Notes

- All files have been reviewed and corrected for grammar and clarity
- Prices use European notation (3,50 PLN)
- Address confirmed: Warmińska 20/47, 54-110 Wrocław
- Translation LLM model will be configured later in OpenRouter
- Content suggestions include specific photo requirements (e.g., Kasia with camera)

---

## 🔄 Workflow

1. **Review** `master-implementation-plan.md` to understand the project
2. **Generate** the implementation plan using the entry point
3. **Follow** the generated step-by-step plan
4. **Reference** specific files as needed during implementation
5. **Update** documentation as you build (hybrid approach)

---

## 📞 Project Contact

- **Phone**: 882947647
- **Email**: deaprint.kasia@gmail.com
- **Languages**: Polish, Ukrainian, English, Italian

---

**Last Updated**: 2024
**Status**: Ready to generate implementation plan
