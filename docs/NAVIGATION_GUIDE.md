# Navigation Guide

Welcome to oddly-copilot! This guide helps you navigate the repository and find what you need.

## Quick Navigation

### 🚀 I want to get started quickly
→ Go to [Quick Start Guide](QUICK_START.md)

### 📚 I want to learn about agent design
→ Go to [Agent Design Guide](AGENT_DESIGN_GUIDE.md)

### 🔧 I need to know what tools are available
→ Go to [Tool Reference](TOOL_REFERENCE.md)

### 📋 I want to create an agent right now
→ Go to [templates/](../templates/)

### 💡 I want to see examples
→ Go to [examples/](../examples/)

### ❓ I have questions
→ Go to [FAQ](FAQ.md)

### 🤝 I want to contribute
→ Go to [CONTRIBUTING.md](../CONTRIBUTING.md)

### 📖 I need to know the agent format
→ Go to [Agent Schema](AGENT_SCHEMA.md)

## Repository Structure Overview

```
oddly-copilot/
│
├── README.md                    # Start here! Overview of the repository
├── CONTRIBUTING.md              # How to contribute to this repository
├── .gitignore                   # Files to ignore in git
│
├── .github/agents/              # Production agent definitions
│   ├── README.md                # Guide for the agents directory
│   └── [agent-name].md          # Individual agent definitions
│
├── docs/                        # Documentation and guides
│   ├── AGENT_DESIGN_GUIDE.md    # Comprehensive agent design principles
│   ├── AGENT_SCHEMA.md          # Agent definition format requirements
│   ├── FAQ.md                   # Frequently asked questions
│   ├── NAVIGATION_GUIDE.md      # This file - helps you navigate
│   ├── QUICK_START.md           # Get started in 5 minutes
│   └── TOOL_REFERENCE.md        # Available tools and usage
│
├── examples/                    # Example agent implementations
│   ├── code-review-agent.md     # Code review agent example
│   ├── documentation-agent.md   # Documentation agent example
│   └── python-testing-agent.md  # Python testing agent example
│
└── templates/                   # Templates for creating agents
    ├── basic-agent-template.md     # Template for simple agents
    └── advanced-agent-template.md  # Template for complex agents
```

## Learning Paths

### Path 1: Beginner (Never created an agent before)

```
1. README.md
   ↓ Understand what this repository is for
   
2. docs/QUICK_START.md
   ↓ Learn the basics in 5 minutes
   
3. examples/python-testing-agent.md
   ↓ See a complete example
   
4. templates/basic-agent-template.md
   ↓ Copy and start creating your own
   
5. docs/FAQ.md
   ↓ Get answers to common questions
```

### Path 2: Intermediate (Created simple agents, want to improve)

```
1. docs/AGENT_DESIGN_GUIDE.md
   ↓ Learn design principles
   
2. examples/code-review-agent.md
   ↓ Study a more complex example
   
3. docs/TOOL_REFERENCE.md
   ↓ Discover available tools
   
4. docs/AGENT_SCHEMA.md
   ↓ Understand the formal structure
   
5. templates/advanced-agent-template.md
   ↓ Use for more sophisticated agents
```

### Path 3: Advanced (Ready to contribute or create complex agents)

```
1. docs/AGENT_SCHEMA.md
   ↓ Master the formal requirements
   
2. examples/ (all files)
   ↓ Study various implementation patterns
   
3. docs/TOOL_REFERENCE.md
   ↓ Know all available tools
   
4. CONTRIBUTING.md
   ↓ Learn contribution guidelines
   
5. Create and contribute!
```

## Documentation Overview

### Core Documentation

| Document | Purpose | When to Read |
|----------|---------|--------------|
| **README.md** | Repository overview | First time visiting |
| **QUICK_START.md** | 5-minute introduction | Want to start quickly |
| **AGENT_DESIGN_GUIDE.md** | Comprehensive design guide | Learning best practices |
| **TOOL_REFERENCE.md** | Available tools catalog | Choosing tools for agent |
| **AGENT_SCHEMA.md** | Formal structure requirements | Ensuring compliance |
| **FAQ.md** | Common questions & answers | Have specific questions |
| **NAVIGATION_GUIDE.md** | This file | Finding your way around |
| **CONTRIBUTING.md** | Contribution guidelines | Ready to contribute |

### Templates

| Template | Use For | Complexity |
|----------|---------|------------|
| **basic-agent-template.md** | Simple, focused agents | ⭐ Beginner |
| **advanced-agent-template.md** | Complex, multi-step agents | ⭐⭐⭐ Advanced |

### Examples

| Example | Demonstrates | Learn About |
|---------|--------------|-------------|
| **python-testing-agent.md** | Testing agent with pytest | Test creation, fixtures, coverage |
| **code-review-agent.md** | Review agent for PRs | Code analysis, feedback generation |
| **documentation-agent.md** | Documentation creation | Technical writing, markdown |

## Common Tasks

### Task: Create My First Agent

1. Read [QUICK_START.md](QUICK_START.md) - 5 minutes
2. Copy [templates/basic-agent-template.md](../templates/basic-agent-template.md)
3. Fill in the sections following the guide
4. Test with a simple scenario
5. Iterate and improve

**Time estimate**: 10-30 minutes

### Task: Understand Available Tools

1. Open [TOOL_REFERENCE.md](TOOL_REFERENCE.md)
2. Browse tool categories
3. Read about specific tools you need
4. Check examples of tool usage

**Time estimate**: 15-30 minutes

### Task: Study Best Practices

1. Read [AGENT_DESIGN_GUIDE.md](AGENT_DESIGN_GUIDE.md) - Principles section
2. Review examples in [examples/](../examples/)
3. Study the instructions sections in examples
4. Apply learnings to your agent

**Time estimate**: 1-2 hours

### Task: Contribute an Agent

1. Read [CONTRIBUTING.md](../CONTRIBUTING.md)
2. Ensure agent follows [AGENT_SCHEMA.md](AGENT_SCHEMA.md)
3. Test thoroughly
4. Submit PR with clear description
5. Address review feedback

**Time estimate**: Variable, plus review time

## Key Concepts Index

Looking for specific concepts? Find them here:

### Agent Design
- **Single Responsibility**: [AGENT_DESIGN_GUIDE.md](AGENT_DESIGN_GUIDE.md#design-principles)
- **Instructions Writing**: [AGENT_DESIGN_GUIDE.md](AGENT_DESIGN_GUIDE.md#instructions-guidelines)
- **Tool Selection**: [AGENT_DESIGN_GUIDE.md](AGENT_DESIGN_GUIDE.md#tool-selection)
- **Testing**: [AGENT_DESIGN_GUIDE.md](AGENT_DESIGN_GUIDE.md#testing-and-validation)

### Tools
- **Tool Catalog**: [TOOL_REFERENCE.md](TOOL_REFERENCE.md#table-of-contents)
- **Tool Selection Guide**: [TOOL_REFERENCE.md](TOOL_REFERENCE.md#tool-selection-guidelines)
- **Code Tools**: [TOOL_REFERENCE.md](TOOL_REFERENCE.md#code-tools)
- **Testing Tools**: [TOOL_REFERENCE.md](TOOL_REFERENCE.md#testing-tools)

### Agent Structure
- **Required Sections**: [AGENT_SCHEMA.md](AGENT_SCHEMA.md#required-sections)
- **Metadata Format**: [AGENT_SCHEMA.md](AGENT_SCHEMA.md#metadata-block)
- **Validation Rules**: [AGENT_SCHEMA.md](AGENT_SCHEMA.md#validation-rules)

### Examples
- **Testing Example**: [examples/python-testing-agent.md](../examples/python-testing-agent.md)
- **Review Example**: [examples/code-review-agent.md](../examples/code-review-agent.md)
- **Documentation Example**: [examples/documentation-agent.md](../examples/documentation-agent.md)

## Tips for Navigation

### 🔍 Searching for Information

1. **Use your editor's search**: Search across all files for keywords
2. **Check the FAQ first**: Many questions are already answered
3. **Look at examples**: Often the fastest way to learn
4. **Follow links**: Documents are well cross-referenced

### 📚 Reading Order Suggestions

**If you're new to agents:**
```
README → QUICK_START → Examples → Templates → Design Guide
```

**If you're experienced:**
```
README → Tool Reference → Schema → Advanced Template → Contributing
```

**If you're contributing:**
```
Contributing → Schema → Design Guide → Examples (for quality reference)
```

### 🎯 Finding Specific Information

**"How do I...?"** → Start with FAQ.md
**"What tools...?"** → Go to TOOL_REFERENCE.md
**"What's the format...?"** → Check AGENT_SCHEMA.md
**"Show me an example..."** → Browse examples/
**"Best practices for...?"** → Read AGENT_DESIGN_GUIDE.md

## Document Dependencies

Understanding which documents reference others:

```
README.md
  └─ Links to: All documentation

QUICK_START.md
  └─ Links to: Templates, Examples, Design Guide

AGENT_DESIGN_GUIDE.md
  └─ Links to: Tool Reference, Examples, Templates

TOOL_REFERENCE.md
  └─ Links to: Agent Design Guide, Examples

AGENT_SCHEMA.md
  └─ Links to: Design Guide, Tool Reference, Contributing

FAQ.md
  └─ Links to: All documentation

CONTRIBUTING.md
  └─ Links to: Schema, Design Guide, Tool Reference

Examples (all)
  └─ Demonstrate: Concepts from all guides
```

## Getting Help

### Within the Repository

1. **FAQ.md** - Check if your question is answered
2. **Examples** - Look for similar use cases
3. **Templates** - Follow the structure
4. **Design Guide** - Understand principles

### External Help

1. **GitHub Issues** - Report bugs or ask questions
2. **GitHub Discussions** - Community discussions
3. **Pull Requests** - Contribute improvements

## Maintenance

This navigation guide is maintained alongside the repository. If you find:
- Broken links
- Outdated information
- Missing navigation paths
- Unclear directions

Please open an issue or submit a PR!

---

## Quick Reference Card

```
┌─────────────────────────────────────────────────────────┐
│  ODDLY-COPILOT QUICK REFERENCE                          │
├─────────────────────────────────────────────────────────┤
│  Get Started:        docs/QUICK_START.md                │
│  Learn Design:       docs/AGENT_DESIGN_GUIDE.md         │
│  Find Tools:         docs/TOOL_REFERENCE.md             │
│  Check Format:       docs/AGENT_SCHEMA.md               │
│  Ask Questions:      docs/FAQ.md                        │
│  See Examples:       examples/                          │
│  Use Templates:      templates/                         │
│  Contribute:         CONTRIBUTING.md                    │
└─────────────────────────────────────────────────────────┘
```

Happy agent building! 🚀
