# Master Implementation Plan Request

Generate a complete implementation plan for the DeaPrint photography website following the structure defined in [prompts.md](prompts.md).

## Project Context

**Business**: DeaPrint - Professional photography service in Wrocław, Poland
**Owner**: Kasia (photographer)
**Domain**: deaprint.pl
**Main Service**: Document photography (passports, IDs, visas)
**Additional Services**: Photo prints, photobooks, calendars, photo sessions, document scanning

## Project Goals

1. Deploy a basic working website to production ASAP
2. Learn Hugo, Tailwind CSS, and Daisy UI through hands-on implementation
3. Create a reusable template/pattern for future web projects
4. Implement features incrementally with each step being production-ready

## Technology Stack (Suggested)

- **Static Site Generator**: Hugo Extended
- **CSS Framework**: Tailwind CSS
- **Component Library**: Daisy UI
- **Hosting**: Cloudflare Pages (free tier)
- **CMS**: Sveltia CMS
- **Version Control**: GitHub with GitHub Actions
- **Translation**: OpenRouter + LLM (suggest model: GPT-4 Turbo, Claude 3.5 Sonnet, or alternatives)

**Note**: I'm open to better alternatives if they exist. Provide comparison and reasoning.

## Detailed Requirements

See the following files for complete specifications:
- [deaprint-description.md](deaprint-description.md) - Business information, contact details, hours
- [about-kasia.md](about-kasia.md) - Photographer bio (5 versions to choose from)
- [deaprint-requirements.md](deaprint-requirements.md) - Complete technical and business requirements
- [deaprint-services-prices.md](deaprint-services-prices.md) - Services and pricing structure
- [content-suggestions.md](content-suggestions.md) - Detailed content and visual suggestions for all pages

## Key Requirements Summary

### Must-Have Features
- **Multilingual**: Polish (default), English, Ukrainian
- **Responsive**: Mobile-first design
- **SEO Optimized**: Structured data, meta tags
- **Contact Integration**: Phone, email, WhatsApp, contact form
- **Content Management**: Markdown files with frontmatter
- **Automated Deployment**: CI/CD via GitHub Actions
- **Info Messages**: Configurable warnings/promotions with user dismissal
- **Regulatory Compliance**: Cookie consent, privacy policy, GDPR

### Nice-to-Have Features
- Google Reviews integration
- Facebook comments integration
- Google Business Profile integration
- Photo gallery with automatic generation
- Blog/articles section
- FAQ section
- Digital asset delivery workflow
- AI-powered content generation in admin panel

## Git Workflow

- **main**: Production deployment (auto-deploy on merge)
- **preprod**: Staging environment with full build pipeline (translations, asset optimization)
- **develop**: Local development and feature testing
- **feature branches**: Individual feature development

## Documentation Requirements

Generate documentation for:
- Developer (embedded in code + /docs/dev)
- Admin/Site Manager (/docs/admin)
- Content Creator (/docs/content)
- Analytics/SEO (/docs/analytics)
- User/End-user (/docs/user)

## Testing Approach

Analyze and recommend:
- Whether TDD is appropriate for this project
- Unit testing strategy (if applicable)
- Acceptance test scenarios for each phase
- Testing workflow and best practices

## Output Format

Follow the exact format specified in [prompts.md](prompts.md):

1. **Master Plan Overview** with total time estimate, technology rationale, phases, and critical path
2. **Example Step** showing the complete format before generating the full plan
3. **Detailed Steps** with prerequisites, substeps, learning points, verification, edge cases
4. **Technology Alternatives** (if suggesting changes from Hugo/Tailwind/Daisy UI)

## Learning Focus

For each step, explain:
- **WHY** this approach (not just HOW)
- **Reusable patterns** for future projects
- **Common pitfalls** to avoid
- **Best practices** in the context

## Time Estimation

Estimate time in hours/days of work with explanation of:
- What makes this step time-consuming
- What can be done in parallel
- Dependencies on previous steps

## Decisions Made

### 1. Technology Stack ✅
**Decision**: Hugo + Tailwind CSS + Daisy UI
**Rationale**: Perfect for static photography site, excellent multilingual support, free hosting, great performance

### 2. Translation LLM Model ✅
**Decision**: Configure later in OpenRouter (model-agnostic setup)
**Options available**: GPT-4 Turbo, Claude 3.5 Sonnet, GPT-3.5 Turbo, Mixtral 8x7B
**Implementation**: Use OpenRouter API with configurable model parameter

### 3. Feature Scope ✅
**Decision**: MVP first (basic site → production → iterate)
**MVP includes**: Home, About, Services, Contact pages
**Rationale**: Get online fast, learn incrementally, adjust based on feedback

### 4. Feature Priority After MVP ✅
**Implementation order**:
1. **Admin Panel** (Sveltia CMS) - Enable easy content management
2. **Info Messages System** - Holiday closures, promotions
3. **Google Reviews Integration** - Social proof
4. **Multilingual Support** - Polish, English, Ukrainian
5. **Blog/Articles** - SEO and customer education
6. **Portfolio/Gallery** - Showcase work
7. **Advanced Features** - AI content generation, digital asset delivery

### 5. Documentation Strategy ✅
**Decision**: Hybrid approach
- Basic documentation as we build (setup, deployment, key concepts)
- Detailed documentation at the end (comprehensive guides for all roles)
**Rationale**: Balance learning with development speed

## Success Criteria

Each step should result in:
- ✅ Production-ready, deployable code
- ✅ Working feature visible on the website
- ✅ Documentation updated
- ✅ Tests passing (if applicable)
- ✅ Learning objectives achieved

## Additional Notes

- I'm a beginner with Hugo, Tailwind, and Daisy UI
- I prefer learning by doing with clear explanations
- I appreciate direct, honest, humorous feedback
- Challenge my assumptions and suggest better approaches
- When uncertain, provide options with pros/cons and recommendations

---

**Ready to generate the plan?** Start with the Master Plan Overview and one complete example step.
