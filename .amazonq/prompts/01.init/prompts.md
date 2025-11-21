## Project Implementation Plan Generator

### Purpose
Generate a complete, structured implementation plan for web projects that serves as both:
1. A master roadmap to assess effort and timeline
2. A step-by-step learning guide with detailed instructions
3. A reusable template for future projects

### Context
I'm learning web development by building the DeaPrint photography website. I don't know Hugo, Tailwind, or Daisy UI yet.
I want to understand not just HOW to build, but WHY each decision matters so I can apply this knowledge to future projects.

### Core Principles
- **Incremental delivery**: Deploy a basic working site ASAP, then add features iteratively
- **Production-ready steps**: Each major step must be deployable to production
- **Learning-focused**: Explain concepts, not just commands
- **Flexibility**: Requirements can pivot completely during implementation
- **Technology agnostic**: Suggest better alternatives if they exist

---

## Output Format Requirements

### 1. Master Plan Overview
Provide upfront:
- **Total estimated time** (in hours/days of work)
- **Technology stack** with rationale for each choice
- **High-level phases** (e.g., Phase 1: Basic Site, Phase 2: Content Management)
- **Critical path** and dependencies between steps

### 2. Detailed Step Structure
For each step, use this exact format:

```markdown
## Step X: [Clear, Action-Oriented Title]

**Goal**: [One sentence describing what this achieves]
**Time Estimate**: [X hours/days] - [Why this takes this long]
**Deployment**: [Yes/No] - [What gets deployed]

### Prerequisites
| Category | Requirement | Why It's Needed |
|----------|-------------|------------------|
| Software | Hugo Extended v0.121.0+ | Supports Tailwind CSS processing |
| Account  | GitHub account | Version control and deployment |
| Knowledge| Basic terminal commands | Run build scripts |

### Substeps
- [ ] **Substep 1**: [Action] - [Expected outcome]
  ``bash
  # Exact command with explanation
  command --flag value  # What this flag does
  ``
- [ ] **Substep 2**: [Action] - [Expected outcome]
```

### Learning Points
- **Concept**: [Explanation of why this matters]
- **Pattern**: [Reusable pattern for future projects]

### Verification
- [ ] Test: [What to test]
- [ ] Expected result: [What success looks like]
- [ ] Troubleshooting: [Common issues and fixes]

### Documentation Updates
- [ ] Update: [Which file to update and what to add]

### Edge Cases
- **If [scenario]**: [How to handle it]

### 3. Examples
Before generating the plan, show one complete example step so I understand the format.

### 4. Technology Alternatives
If suggesting alternatives to Hugo/Tailwind/Daisy UI, provide:
- **Comparison table** (features, learning curve, deployment complexity)
- **Recommendation** with reasoning
- **Migration effort** if switching later

---

## Response Guidelines

### Structure & Clarity
- Use tables for comparisons and prerequisites
- Use code blocks with comments for all commands
- Use checklists for actionable items
- Number steps sequentially (Step 1, Step 2, etc.)
- Bold key terms on first use

### Detail Level
- **Commands**: Show exact syntax with version numbers
- **Accounts**: Include signup URLs and required settings
- **Configuration**: Show actual config file snippets, not descriptions
- **Knowledge**: List specific features to learn (e.g., "Hugo shortcodes for reusable components"), not general topics

### Learning Focus
- Explain WHY before HOW
- Connect concepts to future reusability
- Highlight patterns applicable to other projects
- Call out common pitfalls proactively

### Feedback Style
- Be direct, humorous, and honest
- Correct my mistakes explicitly ("That won't work because...")
- When uncertain, provide options with pros/cons and a recommendation

---

## Prompt Storage
When I ask to generate a prompt, save it in markdown format:
- **Location**: `.amazonq/prompts/[topic]/[descriptive-name].md`
- **Ask for filename** with proposition if not provided
- **Organize by topic** (e.g., deployment, features, testing)

---

## Final Notes
Everything here is a suggestion. If you see a better approach, propose it with reasoning.
I'm here to learn—challenge my assumptions and teach me better patterns.

## Specifications and data

These files provides more details - use it in prompt as well.

[deaprint](deaprint-description.md) with [kasia](about-kasia.md)
[req](deaprint-requirements.md)
[services](deaprint-services-prices.md)