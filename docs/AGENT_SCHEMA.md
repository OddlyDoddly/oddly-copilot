# Agent Definition Schema

This document defines the structure and requirements for agent definition files in this repository.

## File Format

- **Format**: Markdown (.md)
- **Location**: `.github/agents/` for production agents
- **Naming**: `agent-name.md` (lowercase, hyphen-separated)
- **Encoding**: UTF-8

## Required Sections

### 1. Title (H1)

The agent's display name.

```markdown
# Agent Name
```

### 2. Metadata Block

YAML-formatted metadata containing essential information.

```yaml
name: agent-name          # Required: Unique identifier
version: X.Y.Z            # Required: Semantic version
category: category-name   # Required: See categories below
description: "Brief..."   # Required: One-line description (max 100 chars)
```

**Valid Categories:**
- `development` - Code creation and modification
- `testing` - Test creation and execution
- `documentation` - Technical writing
- `security` - Security scanning and fixes
- `devops` - Deployment and infrastructure

**Version Format:**
- Follow [Semantic Versioning](https://semver.org/)
- Format: MAJOR.MINOR.PATCH
- Example: 1.0.0, 2.1.3, 1.0.0-beta

### 3. Purpose Section

Clearly articulates the agent's purpose and usage.

**Required Subsections:**

#### Problem Statement
```markdown
**Problem Statement:**
A clear description of what problem this agent solves.
```

#### When to Use
```markdown
**When to Use:**
- Scenario 1
- Scenario 2
- Scenario 3
```

#### Expected Outcomes
```markdown
**Expected Outcomes:**
- Outcome 1
- Outcome 2
- Outcome 3
```

### 4. Expertise Domain Section

Defines the agent's specialization.

**Required Content:**
- Primary Domain
- Technologies
- Skills

```markdown
## Expertise Domain

This agent specializes in:

- **Primary Domain**: [Domain]
- **Technologies**: [Tech list]
- **Skills**: [Skill list]
```

### 5. Instructions Section

The core behavioral guidelines for the agent.

**Required Subsections:**

#### Role Definition
```markdown
### Role Definition

You are a [ROLE] agent specialized in [DOMAIN].
```

#### Core Objectives
```markdown
### Core Objectives

Your main objectives are:
1. [Objective 1]
2. [Objective 2]
3. [Objective 3]
```

#### Approach
```markdown
### Approach

When performing your tasks, follow this approach:
[Detailed steps]
```

#### Guidelines
```markdown
### Guidelines

**DO:**
- ✅ [Guideline 1]
- ✅ [Guideline 2]

**DO NOT:**
- ❌ [Constraint 1]
- ❌ [Constraint 2]
```

### 6. Tool Configuration Section

Specifies what tools the agent needs.

**Required Subsections:**

#### Required Tools
```markdown
### Required Tools
- **tool-name**: [Why needed]
```

#### Optional Tools (if any)
```markdown
### Optional Tools
- **tool-name**: [When useful]
```

### 7. Examples Section

At least one complete example showing expected behavior.

**Required Format:**
```markdown
## Examples

### Example 1: [Scenario Name]

**Input:**
```
[Example input]
```

**Expected Behavior:**
```
[What agent should do]
```

**Expected Output:**
```
[Expected result]
```
```

## Optional Sections

These sections are recommended but not required:

### Testing Section

Validation approach and checklist.

```markdown
## Testing

### Test Scenarios
- Scenario 1
- Scenario 2

### Validation Checklist
- [ ] Criterion 1
- [ ] Criterion 2
```

### Maintenance Section

Version history and known limitations.

```markdown
## Maintenance

### Version History
- **1.0.0** (YYYY-MM-DD): Initial version

### Known Limitations
- Limitation 1
- Limitation 2

### Future Enhancements
- Enhancement 1
- Enhancement 2
```

### Notes Section

Additional important information.

```markdown
## Notes

- Note 1
- Note 2
```

## Validation Rules

### Metadata Validation

```yaml
name:
  - Required: true
  - Type: string
  - Pattern: ^[a-z0-9-]+$
  - Max length: 50
  - Unique: true (across all agents)

version:
  - Required: true
  - Type: string
  - Pattern: ^\d+\.\d+\.\d+(-[a-z0-9]+)?$
  - Format: Semantic versioning

category:
  - Required: true
  - Type: string
  - Allowed values: [development, testing, documentation, security, devops]

description:
  - Required: true
  - Type: string
  - Max length: 100
  - Min length: 10
```

### Content Validation

**Instructions:**
- Must be at least 200 words
- Must include specific, actionable guidelines
- Must define clear boundaries (DO/DO NOT)

**Examples:**
- At least 1 example required
- Must include input, behavior, and output
- Examples should be realistic and practical

**Tools:**
- At least 1 tool must be specified
- Tools must exist in the tool reference
- Tool usage must be justified

## Best Practices

### Naming Conventions

**File Names:**
- Use lowercase
- Separate words with hyphens
- Be descriptive but concise
- Examples: `python-testing-agent.md`, `code-review-agent.md`

**Agent Names (in metadata):**
- Same as file name (without .md)
- Should match the H1 title (converted to lowercase with hyphens)

### Writing Style

**Instructions:**
- Use imperative mood ("Do this" not "You should do this")
- Be specific and actionable
- Avoid ambiguous terms
- Use bullet points for clarity
- Include rationale for important decisions

**Examples:**
- Use realistic scenarios
- Show complete input/output
- Explain the reasoning
- Cover different complexity levels

### Version Management

**When to Increment:**
- **MAJOR**: Breaking changes to agent behavior
- **MINOR**: New features or capabilities
- **PATCH**: Bug fixes or clarifications

**Version History:**
- Document all version changes
- Include date and description
- Note any breaking changes

## Templates

Use provided templates as starting points:

- `templates/basic-agent-template.md` - For simple agents
- `templates/advanced-agent-template.md` - For complex agents

Templates include:
- All required sections
- Helpful comments and examples
- Best practice guidelines
- Validation checklists

## Validation Tools

### Manual Validation

Use this checklist when creating or reviewing agents:

```markdown
- [ ] File name follows conventions
- [ ] Metadata is complete and valid
- [ ] Purpose section clearly explains the agent
- [ ] Instructions are specific and actionable
- [ ] Tools are listed with justification
- [ ] At least one complete example
- [ ] Testing approach defined
- [ ] Version history included
- [ ] No grammatical errors
- [ ] Follows template structure
```

### Automated Validation

Future: Scripts will validate:
- Metadata format
- Required sections present
- Version format
- File naming
- Tool references
- Example completeness

## Examples of Valid Agents

See the `examples/` directory for fully compliant agent definitions:

- `examples/python-testing-agent.md` - Testing domain
- `examples/code-review-agent.md` - Development domain

## Schema Version

**Schema Version**: 1.0.0
**Last Updated**: 2025-11-11

Changes to this schema will be versioned and documented.

## References

- [Agent Design Guide](AGENT_DESIGN_GUIDE.md)
- [Tool Reference](TOOL_REFERENCE.md)
- [Contributing Guidelines](../CONTRIBUTING.md)
- [Quick Start Guide](QUICK_START.md)

## Questions?

If you have questions about the schema:

1. Review the examples in `examples/`
2. Check the templates in `templates/`
3. Read the Agent Design Guide
4. Open an issue for clarification
