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

## Questions to Address

Before generating the plan, clarify:
1. Should we use Hugo/Tailwind/Daisy UI or suggest better alternatives?
2. Which OpenRouter LLM model for translations? (GPT-4 Turbo, Claude 3.5 Sonnet, Mixtral, etc.)
3. Should we implement all nice-to-have features or focus on MVP first?
4. What's the priority order for features after basic site deployment?
5. Should documentation be generated progressively or all at once at the end?

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
