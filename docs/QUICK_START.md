# Quick Start Guide

Get started with creating your first Copilot agent in minutes!

## Prerequisites

- Basic understanding of GitHub and Git
- Familiarity with the domain your agent will work in
- A clear problem you want your agent to solve

## 5-Minute Agent Creation

### Step 1: Identify Your Need (1 minute)

Ask yourself:
- **What task** do I want to automate?
- **Who** will use this agent?
- **What should** it accomplish?

Example: "I need an agent that creates React component test files using Jest"

### Step 2: Choose Your Template (30 seconds)

```bash
cd /path/to/oddly-copilot

# For simple, focused agents:
cp templates/basic-agent-template.md .github/agents/my-agent.md

# For complex, multi-step agents:
cp templates/advanced-agent-template.md .github/agents/my-agent.md
```

**When to use each:**
- **Basic Template**: Single, focused task (e.g., "format Python code")
- **Advanced Template**: Complex workflows (e.g., "migrate React components to TypeScript")

### Step 3: Fill Essential Fields (2 minutes)

Open your new agent file and fill in:

#### 1. Metadata
```yaml
name: react-jest-testing-agent
version: 1.0.0
category: testing
description: Creates Jest test files for React components
```

#### 2. Purpose
```markdown
**Problem Statement:**
React developers spend significant time writing boilerplate test code.

**When to Use:**
- Creating tests for new React components
- Adding test coverage to existing components

**Expected Outcomes:**
- Jest test file with basic structure
- Tests for component rendering
- Tests for props and state
```

#### 3. Core Instructions (The Most Important Part!)
```markdown
You are a React Testing Agent specialized in creating Jest test files.

Your main objectives are:
1. Create a Jest test file for the given React component
2. Include tests for rendering, props, and user interactions
3. Follow React Testing Library best practices

When creating tests:
- Use screen queries (getByRole, getByText, etc.)
- Test user behavior, not implementation
- Use userEvent for interactions
- Add meaningful test descriptions

You must NOT:
- Test implementation details
- Use enzyme or shallow rendering
- Skip accessibility considerations
```

### Step 4: Add One Example (1 minute)

```markdown
## Example

**Input:**
```jsx
// Button.jsx
export function Button({ label, onClick }) {
  return <button onClick={onClick}>{label}</button>;
}
```

**Expected Output:**
```jsx
// Button.test.jsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { Button } from './Button';

describe('Button', () => {
  it('renders with the correct label', () => {
    render(<Button label="Click me" onClick={() => {}} />);
    expect(screen.getByRole('button', { name: 'Click me' })).toBeInTheDocument();
  });

  it('calls onClick when clicked', async () => {
    const handleClick = jest.fn();
    render(<Button label="Click me" onClick={handleClick} />);
    
    await userEvent.click(screen.getByRole('button'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });
});
```
```

### Step 5: Test It! (30 seconds)

Before using your agent, do a quick sanity check:

- [ ] Instructions are clear and specific
- [ ] Example makes sense
- [ ] Tool requirements are listed
- [ ] Purpose is clearly stated

## Your Agent is Ready!

You now have a functional agent definition. As you use it, you can refine and improve it.

## Next Steps

### Immediate Improvements

1. **Add More Examples**: Cover edge cases and complex scenarios
2. **Refine Instructions**: Based on actual usage, make instructions clearer
3. **Document Limitations**: Note what the agent cannot do

### Long-term Enhancements

1. **Test Thoroughly**: Use in various real-world scenarios
2. **Gather Feedback**: Ask teammates to try it
3. **Iterate**: Update based on learnings
4. **Share**: Contribute back to the repository

## Common Pitfalls to Avoid

### ❌ Being Too Vague

**Bad:**
```
You are a testing agent. Write good tests.
```

**Good:**
```
You are a pytest testing agent. For each Python function:
1. Create test_<function_name>.py in the tests/ directory
2. Write tests for normal cases, edge cases, and errors
3. Use pytest fixtures for common setup
4. Achieve >80% code coverage
```

### ❌ Granting Too Many Tools

**Bad:**
```
Required tools: bash, view, create, edit, git, github-api, browser, playwright, all_tools
```

**Good:**
```
Required tools:
- view: Read source code
- create: Create new test files
- bash: Run pytest
```

### ❌ Skipping Examples

Don't skip examples! They help both you and the agent understand what's expected.

### ❌ Making It Too Broad

**Bad:** "General purpose Python agent"
**Good:** "Python testing agent for pytest"

## Templates Quick Reference

### Basic Template Sections

```
1. Metadata (name, version, category, description)
2. Purpose (problem, when to use, expected outcomes)
3. Expertise Domain (technologies, skills)
4. Instructions (role, objectives, approach, guidelines)
5. Tool Configuration (required, optional tools)
6. Examples (at least 2-3)
7. Testing (validation checklist)
```

### Minimum Viable Agent

At minimum, your agent needs:

1. **Clear Purpose**: What problem does it solve?
2. **Specific Instructions**: What should it do?
3. **One Good Example**: Show expected behavior
4. **Tool List**: What tools does it need?

Everything else can be refined later!

## Example: 10-Minute Agent

Let's create a simple agent from scratch:

**Goal:** Agent that formats Python files with Black

```markdown
# Python Black Formatter Agent

## Agent Metadata
```yaml
name: python-black-formatter
version: 1.0.0
category: development
description: Formats Python code using Black formatter
```

## Purpose

**Problem Statement:**
Python code needs consistent formatting across the project.

**When to Use:**
- Before committing Python code
- Reformatting existing Python files
- Ensuring code style consistency

**Expected Outcomes:**
- Python files formatted according to Black's style
- Consistent line length and formatting
- PEP 8 compliant code

## Instructions

You are a Python Formatting Agent. Format Python code using Black.

Your objectives:
1. Run Black formatter on specified Python files
2. Report formatting changes
3. Ensure no syntax errors after formatting

When formatting:
- Use Black's default configuration
- Format entire files, not partial
- Verify code still works after formatting
- Report what changed

## Tool Configuration

Required tools:
- bash: Run Black formatter
- view: Read Python files
- edit: Apply formatting changes if needed

## Examples

### Example 1

**Input:**
```python
def hello(  name  ):
    print(  "Hello, "  +name  )
```

**Action:**
```bash
black hello.py
```

**Output:**
```python
def hello(name):
    print("Hello, " + name)
```
```

That's it! A complete, functional agent in about 10 minutes.

## Quick Testing

Test your agent quickly:

### 1. Self-Test

Read through your agent definition and ask:
- Can I understand what it should do?
- Are the instructions clear enough?
- Would someone else understand this?

### 2. Mental Simulation

Pick a scenario and mentally walk through:
- What input would I give?
- What should the agent do?
- What output would I expect?

If you can answer these clearly, your agent is ready to try!

### 3. Real Test

Use your agent with a simple, real scenario:
- Start with the easiest use case
- Verify it does what you expect
- Refine based on what happened

## Getting Help

Stuck? Here's where to look:

1. **Examples Directory**: See `examples/` for complete agents
2. **Agent Design Guide**: See `docs/AGENT_DESIGN_GUIDE.md` for detailed guidance
3. **Tool Reference**: See `docs/TOOL_REFERENCE.md` for available tools
4. **Templates**: The templates have helpful comments

## Cheat Sheet

### Agent Creation Checklist

```
[ ] Choose template (basic or advanced)
[ ] Fill metadata (name, version, category, description)
[ ] Define purpose (problem, when to use, outcomes)
[ ] Write clear instructions (role, objectives, guidelines)
[ ] List required tools
[ ] Add at least one example
[ ] Do quick self-test
[ ] Save and try it!
```

### Good Instructions Formula

```
You are a [ROLE] agent specialized in [DOMAIN].

Your objectives are:
1. [Specific objective 1]
2. [Specific objective 2]
3. [Specific objective 3]

When [DOING TASK]:
- [Specific guideline 1]
- [Specific guideline 2]
- [Specific guideline 3]

You must NOT:
- [Constraint 1]
- [Constraint 2]
```

## What's Next?

Now that you have an agent:

1. **Use It**: Try it on real tasks
2. **Refine It**: Update based on experience
3. **Share It**: Contribute to the repository
4. **Create More**: Build agents for other tasks

## Resources

- **Templates**: `templates/`
- **Examples**: `examples/`
- **Design Guide**: `docs/AGENT_DESIGN_GUIDE.md`
- **Tool Reference**: `docs/TOOL_REFERENCE.md`
- **Contributing**: `CONTRIBUTING.md`

---

**Remember**: Perfect is the enemy of good. Start simple, iterate quickly, and improve based on real usage!

Happy agent building! 🚀
