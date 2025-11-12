# oddly-copilot

A repository for designing and managing GitHub Copilot agent scripts and behaviors.

## Overview

This repository serves as a central location for defining custom GitHub Copilot agents that can be used to automate various development tasks. Each agent is designed with specific expertise and capabilities to handle particular domains or workflows.

## Repository Structure

```
.github/agents/     # Agent definition files
examples/           # Example agent implementations
templates/          # Templates for creating new agents
docs/              # Documentation and guides
```

## What are Copilot Agents?

Copilot agents are specialized AI assistants that can be configured to handle specific tasks in your development workflow. They can be customized with:

- **Domain expertise**: Specialized knowledge in particular technologies or domains
- **Custom tools**: Access to specific tools and APIs
- **Behavioral instructions**: Guidelines for how the agent should approach problems
- **Context awareness**: Understanding of your codebase and project structure

## Getting Started

### 🚀 The Easy Way: Use the Agent Builder

**Don't want to write any config files yourself?** Use our interactive Agent Builder agent!

The Agent Builder will ask you questions about what you want to create and automatically generate a complete agent definition file for you. No manual file editing required!

**To use:**
1. Invoke the `@agent-builder` agent
2. Answer a few simple questions about your desired agent
3. Get a fully configured agent file automatically created in `.github/agents/`

**Example conversation:**
```
You: I want to create an agent that writes Python tests
Agent Builder: What should we name this agent?
You: python-test-creator
Agent Builder: [asks a few more questions...]
✅ Agent created at .github/agents/python-test-creator.md
```

See [.github/agents/agent-builder.agent.md](.github/agents/agent-builder.agent.md) for details.

### 📝 The Manual Way: Use Templates

If you prefer to create agents manually:

1. Start with a template from the `templates/` directory
2. Define your agent's:
   - **Name**: A clear, descriptive name
   - **Purpose**: What problem does this agent solve?
   - **Expertise**: What domains or technologies is it specialized in?
   - **Instructions**: How should it approach tasks?
   - **Tools**: What tools does it need access to?

3. Save your agent definition in `.github/agents/`

### Using an Agent

Agents defined in this repository can be invoked through GitHub Copilot's agent system. Reference the agent by name and provide it with the necessary context for your task.

## Examples

See the `examples/` directory for sample agent implementations that demonstrate:

- Code review agents
- Testing agents
- Documentation agents
- Deployment agents
- Security scanning agents

## Best Practices

1. **Single Responsibility**: Each agent should have a clear, focused purpose
2. **Clear Instructions**: Provide detailed, unambiguous guidance
3. **Context Awareness**: Include information about when and how to use the agent
4. **Tool Selection**: Only grant access to tools the agent actually needs
5. **Testing**: Validate agent behavior with various scenarios
6. **Documentation**: Include clear documentation for each agent

## Agent Catalog

This is a catalog of all available agents in this repository. Each agent has a unique identifier for tracking and updates.

### Agent Builder
**ID**: `agent-agent-builder-37bb1763`  
**Category**: Development  
**Version**: 2.2.0  
**Status**: Active

An interactive agent that guides you through creating new GitHub Copilot agent definitions by asking questions and generating complete configuration files automatically. This agent also maintains standardized documentation for all agents in the repository.

**Key Capabilities:**
- Interactive guided agent creation through conversation
- Generates agents with strict enforcement patterns (MANDATORY language)
- Creates unique tracking IDs for all agents
- Automatically generates and updates agent documentation in README.md
- Ensures all generated agents include Anti-Patterns, Pre-flight Checklists, and Final Reminders
- Versions every agent (1.0.0 for new) and increments version on updates

**Use When:**
- Creating a new agent definition from scratch
- Want guidance on what information to include in an agent
- Need automated documentation generation for agents
- Prefer interactive conversation over manual file editing

**Path**: `.github/agents/agent-builder.agent.md`

---

### DDD REST Backend Agent
**ID**: `agent-ddd-rest-0a7f72f9`  
**Category**: Development  
**Version**: 1.0.0  
**Status**: Active

Build REST backends using Domain-Driven Design (DDD) with Model-View-Controller (MVC) architecture. Enforces mandatory separation of layers and strict architectural patterns with zero deviation allowed.

**Key Capabilities:**
- Generate services following DDD + MVC architecture
- Enforce strict layer separation (Domain, Application, Infrastructure)
- Implement repository patterns with proper abstraction
- Apply custom standards that override language/framework conventions

**Use When:**
- Building new REST API backends
- Need strict architectural enforcement
- Working with TypeScript/Node.js, C#, or Java backend services
- Require Domain-Driven Design patterns

**Path**: `.github/agents/infrastructures/ddd/ddd-rest.agent.md`

---

## Contributing

When adding new agents:

1. Use the provided templates
2. Follow the naming conventions
3. Include comprehensive documentation
4. Test thoroughly before committing
5. Update this README if adding new categories

## Agent Categories

- **Development**: Code generation, refactoring, and optimization
- **Testing**: Test creation, execution, and analysis
- **Documentation**: Technical writing and maintenance
- **Security**: Vulnerability scanning and remediation
- **DevOps**: Deployment, CI/CD, and infrastructure

## Learn More

- See `docs/QUICK_START.md` for a quick introduction to creating agents
- See `docs/AGENT_DESIGN_GUIDE.md` for detailed agent design principles
- See `docs/TOOL_REFERENCE.md` for available tools and their usage
- See `docs/AGENT_SCHEMA.md` for agent definition requirements
- See `docs/FAQ.md` for frequently asked questions
- See `examples/` for practical implementations

## License

[Add your license here]
