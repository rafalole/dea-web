# Documentation Format Guidelines

## Documentation Triggers

When I say:
- "document [feature/component]" → Create feature documentation in `docs/content/`
- "update architecture documentation" → Update architecture docs in `docs/dev/`
- "create recipe for [task]" → Create step-by-step guide in `docs/dev/`
- "document design decision" → Create ADR-style doc in `docs/dev/design-decisions.md`
- "document admin task" → Create admin documentation in `docs/admin/`

## Documentation Goals

- Creating recipes for future reference: configuration examples, usage code, testing approaches, and key considerations
- Save the process of implementation: what was considered and what and why was picked
- Document design decisions with rationale

## Documentation Structure

- Keep documentation concise - avoid repetition
- Organize as small, separated markdown files grouped in folders by domain/feature or technology (hugo, tailwind, deployment, etc.)
- Include a content list for easy navigation
- Focus on practical examples and important details
- One topic per file - easier to maintain and find

## Project-Specific Approach

- Add detailed documentation to Hugo templates and layouts (HTML comments)
- In markdown: reference template/layout files and show only critical snippets
- Avoid duplicating full code in documentation
- Highlight key configuration or tricky parts as snippets
- Link to actual source files for complete examples
- Use relative paths from docs folder: `[header.html](../layouts/partials/header.html)`
- When showing code snippets, always indicate the source file location first
- Include line number references when possible: `[baseof.html](../layouts/_default/baseof.html#L10-L15)`

## Architecture Documentation

When I request "update architecture documentation":
- Explain design principles and rationale
- Document Hugo project structure conventions with examples
- Define naming conventions for templates, partials, content files
- Describe configuration patterns used (hugo.toml, i18n, frontmatter)
- Explain multilingual strategy and organization
- Include design decisions with reasoning (why this approach?)
- Add future considerations for growth
- Keep it practical - focus on what exists and why
- Update existing files rather than creating duplicates

## Current Documentation Organization

```
docs/
  admin/          # Admin tasks (deployment, git workflow, configuration)
  content/        # Content management documentation
  dev/            # Development documentation (Hugo, templates, setup)
```

## Code Examples Format

### Good - Reference with snippet
```markdown
See [header.html](../layouts/partials/header.html#L10-L15)

Key navigation pattern:
\`\`\`html
<nav>
  {{ range .Site.Menus.main }}
    <a href='{{ .URL }}'>{{ .Name }}</a>
  {{ end }}
</nav>
\`\`\`
```

### Bad - Full code duplication
```markdown
// Don't paste entire template files (>50 lines)
// Don't duplicate code that's already well-documented in source
```

### When to Show Full Code
- Configuration examples (hugo.toml, postcss.config.js, package.json snippets)
- Hugo template examples demonstrating patterns
- Small partial templates (<30 lines) that are instructive
- Data structure examples (JSON, TOML, frontmatter) that illustrate the format
- Hugo template expressions, conditionals, and range loops

### Documentation Depth Guidelines
- **Features**: Comprehensive with examples, template structure, integration points
- **Recipes**: Step-by-step with gotchas and troubleshooting
- **Architecture**: Design rationale, trade-offs, alternatives considered
- **Admin**: Deployment procedures, configuration management, workflows

## Recipe Format

Each recipe should follow:

1. **Goal**: What you're trying to achieve
2. **Context**: When to use this approach
3. **Data Flow**: Visual representation of the process (for complex flows)
4. **Steps**: Numbered, actionable steps with file references
5. **Example**: Minimal code snippet or reference with expected output
6. **Gotchas**: Common mistakes, edge cases, and tricky parts (numbered list)
7. **Comparison**: When applicable, compare with alternative approaches
8. **Related**: Links to related recipes, features, and source code

### Recipe Naming Convention
- Use descriptive action-based names: `hugo-multilingual.md`
- Include the technology/domain: `hugo-template-language.md`, `tailwind-v4-setup.md`
- For configuration: include config type: `hugo-toml-config.md`

## Design Decision Format

```markdown
## Decision: [Title]

**Date**: YYYY-MM-DD
**Status**: Accepted | Superseded | Deprecated

### Context
What problem are we solving?

### Considered Options
1. Option A - pros/cons
2. Option B - pros/cons

### Decision
What we chose and why

### Consequences
- Positive: ...
- Negative: ...
- Trade-offs: ...

### Implementation Details
- Key files affected
- Configuration changes
- Migration steps (if applicable)

### Future Considerations
- Technical debt introduced
- Potential improvements
- Related decisions needed
```

## Implementation Documentation Format

For feature implementation documentation:

```markdown
# [Feature Name]

## Overview

Brief summary of the feature and its purpose.

## Implementation Details

### Files Modified/Created
- `layouts/partials/component.html` - Component template
- `content/en/section/_index.md` - Content structure
- `hugo.toml` - Configuration changes

### Hugo Template Patterns Used
- Range loops for iteration
- Partial templates for reusability
- Frontmatter parameters for configuration

### Multilingual Considerations
- Translation keys in `i18n/*.toml`
- Content files per language in `content/{lang}/`

## Testing

- Local development: `hugo server`
- Build verification: `hugo --minify`
- Browser testing across languages

## Deployment

- GitHub Actions workflow updates (if applicable)
- Static asset considerations

## Summary

**Status**: ✅ Complete / 🟢 In Progress / ⏭️ Planned

**Notes**: Key learnings and next steps.
```
