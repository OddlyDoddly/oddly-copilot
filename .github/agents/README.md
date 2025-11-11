# Agent Definitions

This directory contains production-ready agent definitions for GitHub Copilot.

## Directory Structure

```
.github/agents/
  ├── README.md              # This file
  └── [agent-name].md        # Individual agent definitions
```

## Adding Agents

### 🚀 Easy Way: Use the Agent Builder

The easiest way to create a new agent is to use the **Agent Builder** agent in this directory!

```
@agent-builder
```

The Agent Builder will:
- Ask you questions about what you want to create
- Guide you through the requirements
- Automatically generate a complete agent definition file
- Save it in this directory with proper formatting

**No manual file editing required!**

### 📝 Manual Way: Use Templates

To add a new agent manually:

1. **Use a Template**: Start with a template from `../../templates/`
2. **Follow the Schema**: Ensure your agent follows the schema in `../../docs/AGENT_SCHEMA.md`
3. **Test Thoroughly**: Validate your agent works as expected
4. **Submit PR**: Follow the contributing guidelines in `../../CONTRIBUTING.md`

## Agent Categories

Agents in this directory are organized by category:

### Development
Agents that help write, refactor, or optimize code.

### Testing
Agents focused on test creation, execution, and analysis.

### Documentation
Agents for creating and maintaining technical documentation.

### Security
Agents for security scanning, vulnerability detection, and remediation.

### DevOps
Agents for deployment, CI/CD, and infrastructure management.

## Using Agents

Agents defined in this directory can be invoked through GitHub Copilot's agent system. Refer to each agent's documentation for specific usage instructions.

## Examples

For example implementations, see the `../../examples/` directory:

- `python-testing-agent.md` - Testing agent example
- `code-review-agent.md` - Code review agent example
- `documentation-agent.md` - Documentation agent example

## Resources

- **Templates**: `../../templates/` - Starting points for new agents
- **Design Guide**: `../../docs/AGENT_DESIGN_GUIDE.md` - Comprehensive design principles
- **Tool Reference**: `../../docs/TOOL_REFERENCE.md` - Available tools documentation
- **Schema**: `../../docs/AGENT_SCHEMA.md` - Agent definition requirements
- **Quick Start**: `../../docs/QUICK_START.md` - Get started quickly
- **Contributing**: `../../CONTRIBUTING.md` - How to contribute

## Quality Standards

All agents in this directory must:

- ✅ Follow the agent schema
- ✅ Have clear, specific instructions
- ✅ Include working examples
- ✅ List required tools
- ✅ Be thoroughly tested
- ✅ Include proper documentation

## Getting Help

If you need help creating or using agents:

1. Review the [Quick Start Guide](../../docs/QUICK_START.md)
2. Study the [examples](../../examples/)
3. Read the [Agent Design Guide](../../docs/AGENT_DESIGN_GUIDE.md)
4. Open a GitHub issue for specific questions

---

**Note**: This directory is for production agents. For testing and experimentation, work in the `examples/` directory first.
