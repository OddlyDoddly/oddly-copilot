# Agent Design Guide

This guide provides comprehensive principles and best practices for designing effective GitHub Copilot agents.

## Table of Contents

1. [Understanding Agents](#understanding-agents)
2. [Agent Anatomy](#agent-anatomy)
3. [Design Principles](#design-principles)
4. [Instructions Guidelines](#instructions-guidelines)
5. [Tool Selection](#tool-selection)
6. [Testing and Validation](#testing-and-validation)
7. [Common Patterns](#common-patterns)

## Understanding Agents

A GitHub Copilot agent is a specialized AI assistant configured to perform specific tasks within a development workflow. Agents are distinguished by:

- **Specialization**: Focused expertise in a particular domain
- **Autonomy**: Ability to make decisions within their scope
- **Tool Access**: Integration with relevant development tools
- **Context Awareness**: Understanding of the project and codebase

## Agent Anatomy

Every agent definition should include:

### 1. Metadata
```yaml
name: agent-name
version: 1.0.0
description: Brief description of what the agent does
```

### 2. Purpose Statement
A clear explanation of:
- What problem the agent solves
- When to use this agent
- What outcomes to expect

### 3. Expertise Domain
Define the agent's area of specialization:
- Programming languages
- Frameworks and libraries
- Development practices
- Domain knowledge

### 4. Instructions
Detailed behavioral guidelines:
- How to approach problems
- What to prioritize
- What to avoid
- Decision-making criteria

### 5. Tool Configuration
Specify which tools the agent needs:
- Code editing tools
- Testing frameworks
- Linters and formatters
- External APIs

## Design Principles

### 1. Single Responsibility Principle
Each agent should have one clear purpose. Avoid creating "Swiss Army knife" agents that try to do everything.

**Good**: "Python Testing Agent - Creates and maintains pytest test suites"
**Bad**: "Python Agent - Does everything Python related"

### 2. Clear Scope Boundaries
Define what is within and outside the agent's responsibility.

```markdown
**In Scope:**
- Writing unit tests
- Running test suites
- Analyzing test coverage

**Out of Scope:**
- Fixing production bugs
- Deploying code
- Database migrations
```

### 3. Explicit Instructions
Be specific about how the agent should behave. Avoid ambiguous language.

**Good**: "Always run the test suite after making changes and fix any failures before reporting completion"
**Bad**: "Make sure tests work"

### 4. Context Awareness
Instruct the agent to gather context before acting:
- Understand the project structure
- Review existing patterns
- Check for conventions
- Consider dependencies

### 5. Minimal Intervention
Agents should make the smallest possible changes to achieve their goals:
- Surgical edits over rewrites
- Preserve existing style and patterns
- Avoid unnecessary refactoring

## Instructions Guidelines

### Structure Your Instructions

1. **Opening Context**: What is this agent's role?
2. **Primary Objectives**: What should it accomplish?
3. **Approach**: How should it work?
4. **Constraints**: What limitations exist?
5. **Quality Criteria**: What defines success?

### Example Structure

```markdown
You are a specialized [DOMAIN] agent for [PURPOSE].

Your primary objectives are:
1. [Objective 1]
2. [Objective 2]
3. [Objective 3]

When performing your tasks:
- [Guideline 1]
- [Guideline 2]
- [Guideline 3]

You must NOT:
- [Constraint 1]
- [Constraint 2]

Success criteria:
- [Criterion 1]
- [Criterion 2]
```

### Language Tips

- Use imperative mood ("Do this" not "You should do this")
- Be specific with technical terms
- Provide examples when helpful
- Use bullet points for clarity
- Include decision trees for complex scenarios

## Tool Selection

### Choosing the Right Tools

Only grant access to tools the agent actually needs:

1. **Code Tools**: If the agent writes/edits code
2. **Test Tools**: If the agent runs tests
3. **Build Tools**: If the agent compiles/builds
4. **API Tools**: If the agent needs external data
5. **Git Tools**: If the agent manages version control

### Tool Configuration Example

```yaml
tools:
  - name: bash
    description: Run commands
    restrictions: Read-only access to /src
  
  - name: edit
    description: Edit code files
    patterns: ["**/*.py", "**/*.js"]
  
  - name: pytest
    description: Run Python tests
    config: "--verbose --coverage"
```

## Testing and Validation

### Test Your Agents

Before deploying an agent, validate it with:

1. **Simple Scenarios**: Basic use cases
2. **Edge Cases**: Unusual inputs or conditions
3. **Error Handling**: How it responds to failures
4. **Integration**: Works with other tools/agents
5. **Performance**: Completes tasks efficiently

### Validation Checklist

- [ ] Agent understands its scope
- [ ] Instructions are clear and unambiguous
- [ ] Agent uses appropriate tools
- [ ] Agent follows best practices
- [ ] Agent handles errors gracefully
- [ ] Agent provides useful output
- [ ] Agent completes tasks efficiently

## Common Patterns

### Pattern 1: Analyzer Agent
Examines code and provides insights without making changes.

```markdown
You are a code analysis agent. Your role is to:
1. Examine the provided code
2. Identify patterns, issues, or opportunities
3. Provide detailed reports
4. Recommend actions (but don't take them)
```

### Pattern 2: Builder Agent
Creates new code or components from specifications.

```markdown
You are a builder agent. Your role is to:
1. Understand the requirements
2. Design the solution architecture
3. Implement the code
4. Validate functionality
5. Document your work
```

### Pattern 3: Maintainer Agent
Updates existing code to fix issues or improve quality.

```markdown
You are a maintenance agent. Your role is to:
1. Analyze existing code
2. Identify specific issues
3. Make minimal, targeted fixes
4. Preserve existing patterns
5. Verify no regressions
```

### Pattern 4: Orchestrator Agent
Coordinates multiple tasks or other agents.

```markdown
You are an orchestrator agent. Your role is to:
1. Break down complex tasks
2. Determine optimal sequence
3. Delegate to specialized agents
4. Coordinate results
5. Ensure overall success
```

## Advanced Topics

### Dynamic Behavior

Agents can be instructed to adapt their behavior based on context:

```markdown
If working with legacy code:
- Prioritize stability over modernization
- Maintain existing patterns
- Add tests before refactoring

If working with new code:
- Follow modern best practices
- Use latest language features
- Optimize for maintainability
```

### Error Recovery

Provide clear guidance for handling failures:

```markdown
If a test fails:
1. Analyze the failure message
2. Check if it's related to your changes
3. If yes, fix and retry
4. If no, report and ask for guidance
5. Never ignore test failures
```

### Progress Reporting

Instruct agents on how to communicate:

```markdown
Report progress when:
- Starting a new major step
- Completing a significant milestone
- Encountering blockers
- Finishing the task

Use the format:
- What you completed
- What you're working on
- What's remaining
- Any issues or blockers
```

## Best Practices Summary

1. ✅ Define a clear, single purpose
2. ✅ Provide explicit, detailed instructions
3. ✅ Grant minimal necessary tool access
4. ✅ Include error handling guidance
5. ✅ Test with multiple scenarios
6. ✅ Document the agent's capabilities
7. ✅ Version your agent definitions
8. ✅ Review and refine based on usage
9. ✅ Keep instructions maintainable
10. ✅ Consider security implications

## Additional Resources

- [Tool Reference](TOOL_REFERENCE.md) - Available tools and their usage
- [Examples Directory](../examples/) - Real-world agent implementations
- [Templates Directory](../templates/) - Starting points for new agents

## Questions?

If you have questions about agent design, consider:
1. Reviewing existing agents in the `examples/` directory
2. Starting with a template from `templates/`
3. Testing your design with simple scenarios first
4. Iterating based on real-world usage
