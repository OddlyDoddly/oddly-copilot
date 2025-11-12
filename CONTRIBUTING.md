# Contributing to oddly-copilot

Thank you for your interest in contributing to oddly-copilot! This guide will help you create high-quality agent definitions that benefit the entire community.

## Table of Contents

1. [Getting Started](#getting-started)
2. [Agent Development Process](#agent-development-process)
3. [Quality Standards](#quality-standards)
4. [Submission Guidelines](#submission-guidelines)
5. [Review Process](#review-process)
6. [Best Practices](#best-practices)

## Getting Started

### Prerequisites

Before creating an agent:

1. **Understand the Need**: Clearly identify the problem your agent will solve
2. **Review Existing Agents**: Check if a similar agent already exists
3. **Read Documentation**: Familiarize yourself with:
   - [Agent Design Guide](docs/AGENT_DESIGN_GUIDE.md)
   - [Tool Reference](docs/TOOL_REFERENCE.md)
   - [Example Agents](examples/)

### Repository Structure

```
.github/agents/     # Production agent definitions
examples/           # Example implementations and learning resources
templates/          # Templates for new agents
docs/              # Documentation and guides
```

## Agent Development Process

### Step 1: Plan Your Agent

Before writing, answer these questions:

- **What problem does this agent solve?**
- **Who will use this agent?**
- **What domain expertise does it need?**
- **What tools will it require?**
- **How will success be measured?**

Document your answers - they'll form the basis of your agent definition.

### Step 2: Choose a Template

Select the appropriate template:

- `templates/basic-agent-template.agent.md` - For straightforward, focused agents
- `templates/advanced-agent-template.agent.md` - For complex agents with sophisticated logic

Copy the template to start your agent:

```bash
cp templates/basic-agent-template.agent.md .github/agents/my-new-agent.agent.md
```

### Step 3: Define Your Agent

Fill in all sections of the template:

#### Essential Sections

1. **Metadata**: Name, version, category, description
2. **Purpose**: Problem statement, use cases, expected outcomes
3. **Expertise Domain**: Technologies, skills, methodologies
4. **Instructions**: Clear, detailed behavioral guidelines
5. **Tool Configuration**: Required and optional tools
6. **Examples**: Real-world usage scenarios

#### Writing Instructions

Your instructions should be:

- **Specific**: Avoid vague language
- **Actionable**: Clear steps the agent should take
- **Prioritized**: Most important behaviors first
- **Constrained**: Explicit boundaries and limitations
- **Complete**: Cover normal cases, edge cases, and errors

**Example - Vague:**
```
You should write good code.
```

**Example - Specific:**
```
When writing code:
1. Follow the existing code style in the project
2. Add error handling for all external calls
3. Write unit tests with >80% coverage
4. Include docstrings for public functions
5. Run the linter and fix all issues before completion
```

### Step 4: Create Examples

Include at least 2-3 examples showing:

1. **Basic Use Case**: Simple, common scenario
2. **Complex Use Case**: More challenging scenario
3. **Edge Case**: How the agent handles unusual situations

Each example should include:
- Context/setup
- Input
- Expected behavior (step-by-step)
- Expected output
- Rationale for decisions

### Step 5: Test Your Agent

Before submitting, thoroughly test your agent:

1. **Functionality Test**: Does it work for the intended use cases?
2. **Edge Case Test**: How does it handle unusual inputs?
3. **Error Test**: Does it fail gracefully?
4. **Consistency Test**: Does it follow its own instructions?
5. **Quality Test**: Is the output high quality?

Testing checklist:
- [ ] Agent completes intended tasks
- [ ] Agent follows all guidelines
- [ ] Agent uses tools appropriately
- [ ] Agent handles errors gracefully
- [ ] Agent provides clear output
- [ ] Agent stops at appropriate boundaries

### Step 6: Document

Ensure your agent has:

- [ ] Clear purpose statement
- [ ] Complete instructions
- [ ] Tool requirements
- [ ] Usage examples
- [ ] Known limitations
- [ ] Testing validation

## Quality Standards

All agents must meet these standards:

### 1. Single Responsibility

Each agent should do one thing well.

**Good:** "Python Testing Agent - Creates pytest test suites"
**Bad:** "Python Agent - Does everything with Python"

### 2. Clear Instructions

Instructions must be unambiguous and actionable.

**Good:** "Run `pytest --cov=src --cov-report=html` after creating tests"
**Bad:** "Make sure tests work"

### 3. Appropriate Tool Access

Only grant access to tools the agent actually needs.

**Good:** Testing agent with bash, view, create, edit, pytest
**Bad:** Testing agent with full GitHub API access

### 4. Comprehensive Examples

Examples should be realistic and cover multiple scenarios.

### 5. Well-Documented

All sections of the template should be thoughtfully completed.

### 6. Tested

Agent must be validated with real usage scenarios.

## Submission Guidelines

### For New Agents

1. **Create a Branch**: `git checkout -b agent/your-agent-name`
2. **Add Your Agent**: Place in `.github/agents/your-agent-name.md`
3. **Update README**: Add your agent to the appropriate category
4. **Create PR**: Submit with descriptive title and description

### PR Title Format

```
Add [Agent Name] for [Purpose]
```

Examples:
- "Add Rust Testing Agent for creating cargo test suites"
- "Add Documentation Agent for maintaining README files"

### PR Description Template

```markdown
## Agent Overview

**Name:** [Agent Name]
**Category:** [Category]
**Purpose:** [One-line description]

## Problem Solved

[Describe the problem this agent addresses]

## Testing Performed

- [ ] Tested with basic use case
- [ ] Tested with complex scenario
- [ ] Tested edge cases
- [ ] Tested error handling
- [ ] Validated tool usage
- [ ] Verified documentation completeness

## Checklist

- [ ] Agent follows template structure
- [ ] All sections completed
- [ ] Examples included
- [ ] Tested thoroughly
- [ ] Documentation updated
- [ ] Follows quality standards
```

### For Agent Updates

When updating an existing agent:

1. **Increment Version**: Update version in metadata
2. **Document Changes**: Update version history section
3. **Test Thoroughly**: Ensure changes don't break existing behavior
4. **Update Examples**: If behavior changed significantly

## Review Process

### What Reviewers Look For

Reviewers will check:

1. **Completeness**: All template sections filled appropriately
2. **Clarity**: Instructions are clear and unambiguous
3. **Quality**: Follows best practices and standards
4. **Testing**: Evidence of thorough testing
5. **Documentation**: Comprehensive and accurate
6. **Uniqueness**: Doesn't duplicate existing agents unnecessarily

### Review Timeline

- Initial review: Within 3-5 business days
- Follow-up reviews: Within 2 business days
- Merge: After approval and CI checks pass

### Addressing Review Comments

When you receive review comments:

1. **Read Carefully**: Understand the feedback
2. **Ask Questions**: If anything is unclear
3. **Make Changes**: Address all comments
4. **Respond**: Explain your changes
5. **Request Re-review**: When ready

## Best Practices

### DO

✅ **Start Simple**: Begin with a basic agent and iterate
✅ **Test Thoroughly**: Use your agent extensively before submitting
✅ **Seek Feedback**: Ask others to review before submitting
✅ **Document Well**: Over-document rather than under-document
✅ **Follow Templates**: They exist for good reasons
✅ **Learn from Examples**: Study existing agents
✅ **Be Specific**: Precise instructions lead to better behavior
✅ **Consider Security**: Think about potential misuse
✅ **Version Properly**: Use semantic versioning

### DON'T

❌ **Skip Testing**: Never submit untested agents
❌ **Be Vague**: Avoid ambiguous instructions
❌ **Over-Scope**: Keep agents focused on one responsibility
❌ **Grant Excessive Permissions**: Limit tool access to what's needed
❌ **Ignore Templates**: They ensure consistency
❌ **Copy Without Understanding**: Understand what you're copying
❌ **Rush**: Take time to create quality agents
❌ **Ignore Feedback**: Review comments help improve quality

## Agent Categories

When creating agents, consider these categories:

### Development
Agents that help write, refactor, or optimize code
- Code generators
- Refactoring assistants
- Code optimization

### Testing
Agents focused on test creation and execution
- Test generators
- Test runners
- Coverage analyzers

### Documentation
Agents for technical writing and maintenance
- README generators
- API documentation
- Code comment writers

### Security
Agents for security scanning and remediation
- Vulnerability scanners
- Security auditors
- Dependency checkers

### DevOps
Agents for deployment and infrastructure
- CI/CD helpers
- Deployment assistants
- Infrastructure validators

## Getting Help

If you need assistance:

1. **Check Documentation**: Review guides in `docs/`
2. **Study Examples**: Look at `examples/` directory
3. **Open Discussion**: Use GitHub Discussions for questions
4. **Ask in PR**: Request guidance in your pull request
5. **Report Issues**: Use GitHub Issues for bugs

## Code of Conduct

- Be respectful and constructive
- Focus on the work, not the person
- Welcome newcomers and help them learn
- Give credit where credit is due
- Assume positive intent

## Recognition

Contributors will be recognized through:

- GitHub contributor list
- Release notes acknowledgments
- Community highlights

## License

By contributing, you agree that your contributions will be licensed under the same license as the project.

---

## Quick Start Checklist

Ready to contribute? Follow this checklist:

- [ ] Read the Agent Design Guide
- [ ] Review existing examples
- [ ] Choose appropriate template
- [ ] Plan your agent thoroughly
- [ ] Write clear, specific instructions
- [ ] Create comprehensive examples
- [ ] Test extensively
- [ ] Complete all documentation
- [ ] Submit PR with good description
- [ ] Address review feedback promptly

Thank you for contributing to oddly-copilot! Your efforts help make agent development more accessible and effective for everyone.
