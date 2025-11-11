# Tool Reference Guide

This document provides a comprehensive reference for tools that can be used by GitHub Copilot agents.

## Table of Contents

1. [Core Tools](#core-tools)
2. [Code Tools](#code-tools)
3. [Testing Tools](#testing-tools)
4. [Build Tools](#build-tools)
5. [Git Tools](#git-tools)
6. [GitHub Tools](#github-tools)
7. [Browser Tools](#browser-tools)
8. [Custom Tools](#custom-tools)

## Core Tools

### bash
Execute shell commands in the environment.

**Usage:**
```yaml
tool: bash
capabilities:
  - Run shell commands
  - Install packages
  - Execute scripts
  - Manage files and directories
```

**Parameters:**
- `command`: The command to execute
- `mode`: sync, async, or detached
- `sessionId`: Session identifier for interactive shells
- `timeout`: Maximum execution time (seconds)

**Best Practices:**
- Use appropriate timeouts for long-running commands
- Consider async mode for interactive tools
- Chain commands when possible for efficiency
- Disable pagers (e.g., `git --no-pager`) to avoid hangs

**Examples:**
```bash
# Build a project
npm run build

# Run tests with timeout
pytest --verbose

# Chain commands
git status && git diff
```

### view
Read files and directory contents.

**Usage:**
```yaml
tool: view
capabilities:
  - Read file contents with line numbers
  - List directory contents
  - Navigate project structure
```

**Parameters:**
- `path`: Absolute path to file or directory
- `view_range`: Optional line range [start, end]

**Best Practices:**
- Use absolute paths
- View specific line ranges for large files
- Use to understand code before editing

**Examples:**
```bash
# View entire file
path: /home/runner/project/src/main.py

# View specific lines
path: /home/runner/project/src/main.py
view_range: [10, 50]

# View directory
path: /home/runner/project/src/
```

### create
Create new files with specified content.

**Usage:**
```yaml
tool: create
capabilities:
  - Create new files
  - Set initial content
```

**Parameters:**
- `path`: Absolute path for new file
- `file_text`: Content of the file

**Best Practices:**
- Ensure parent directories exist
- Cannot overwrite existing files (use edit instead)
- Use absolute paths

**Examples:**
```bash
# Create a new Python file
path: /home/runner/project/src/utils.py
file_text: |
  def helper_function():
      pass
```

### edit
Make precise string replacements in files.

**Usage:**
```yaml
tool: edit
capabilities:
  - Replace exact text matches
  - Make surgical edits
  - Batch edits to same file
```

**Parameters:**
- `path`: Absolute path to file
- `old_str`: Exact text to replace (with whitespace)
- `new_str`: Replacement text

**Best Practices:**
- Include enough context for unique matches
- Preserve existing whitespace
- Make multiple edits in sequence
- Use for minimal, targeted changes

**Examples:**
```bash
# Rename a variable
path: /home/runner/project/src/app.py
old_str: "user_id = get_id()"
new_str: "userId = get_id()"
```

## Code Tools

### Language-Specific Tools

#### Python
- **pytest**: Run Python tests
- **pylint**: Lint Python code
- **black**: Format Python code
- **mypy**: Type checking
- **pip**: Package management

#### JavaScript/TypeScript
- **npm**: Package management and scripts
- **jest**: Testing framework
- **eslint**: Linting
- **prettier**: Code formatting
- **tsc**: TypeScript compiler

#### Go
- **go build**: Compile Go code
- **go test**: Run Go tests
- **go fmt**: Format Go code
- **go vet**: Examine Go code
- **golint**: Lint Go code

#### Java
- **maven**: Build and dependency management
- **gradle**: Build automation
- **junit**: Testing framework

#### Ruby
- **bundler**: Dependency management
- **rspec**: Testing framework
- **rubocop**: Linting

## Testing Tools

### write_bash
Send input to interactive commands.

**Usage:**
```yaml
tool: write_bash
capabilities:
  - Interact with running commands
  - Send keyboard input
  - Automate interactive tools
```

**Parameters:**
- `sessionId`: Session identifier
- `input`: Text or key sequences
- `delay`: Wait time before reading output

**Key Sequences:**
- `{enter}`: Press Enter
- `{up}`, `{down}`, `{left}`, `{right}`: Arrow keys
- `{backspace}`: Backspace

**Examples:**
```bash
# Confirm a prompt
sessionId: "install"
input: "y{enter}"
delay: 10
```

### read_bash
Read output from async commands.

**Usage:**
```yaml
tool: read_bash
capabilities:
  - Read command output
  - Check command progress
  - Monitor async processes
```

**Parameters:**
- `sessionId`: Session identifier
- `delay`: Wait time before reading

## Build Tools

### Code Compilation
Tools for building and compiling code:

- **make**: Build automation
- **cmake**: Cross-platform build system
- **gradle**: Java/Kotlin builds
- **cargo**: Rust package manager and builder
- **dotnet**: .NET CLI

### Package Management
- **npm/yarn**: JavaScript
- **pip/poetry**: Python
- **gem/bundler**: Ruby
- **cargo**: Rust
- **go mod**: Go

## Git Tools

### Basic Git Operations

**Available through bash:**
```bash
# Status and diff
git --no-pager status
git --no-pager diff

# Commit information
git --no-pager log
git --no-pager show <commit>

# Branch operations
git branch
git checkout <branch>
```

**Limitations:**
- Cannot push (use report_progress tool)
- Cannot create PRs (use GitHub API)
- Cannot force push
- Cannot rebase with force push

## GitHub Tools

### Repository Operations

Tools available through the GitHub MCP server:

- **get_file_contents**: Read files from GitHub
- **list_issues**: Query issues
- **list_pull_requests**: Query PRs
- **get_commit**: View commit details
- **search_code**: Search across repositories
- **search_issues**: Search issues/PRs
- **search_repositories**: Find repositories

### Workflow Tools

- **list_workflows**: List GitHub Actions workflows
- **list_workflow_runs**: Get workflow run history
- **get_workflow_run**: Get run details
- **list_workflow_jobs**: List jobs in a run
- **get_job_logs**: Get job logs
- **summarize_job_log_failures**: Analyze failures

### Security Tools

- **list_code_scanning_alerts**: CodeQL alerts
- **list_secret_scanning_alerts**: Secret detection
- **gh-advisory-database**: Check for vulnerabilities

## Browser Tools

### playwright-browser Tools

For web automation and testing:

- **browser_navigate**: Navigate to URLs
- **browser_snapshot**: Capture page state
- **browser_click**: Click elements
- **browser_type**: Type text
- **browser_fill_form**: Fill form fields
- **browser_take_screenshot**: Capture screenshots
- **browser_evaluate**: Run JavaScript

**Use Cases:**
- Testing web applications
- Validating UI changes
- Automated form filling
- Visual verification

## Custom Tools

### report_progress
Commit and push changes, update PR description.

**Usage:**
```yaml
tool: report_progress
capabilities:
  - Commit changes
  - Push to remote
  - Update PR description
  - Track progress
```

**Parameters:**
- `commitMessage`: Commit message
- `prDescription`: PR description with checklist

**Best Practices:**
- Use frequently to save progress
- Include markdown checklists
- Report meaningful milestones
- Review committed files

### code_review
Request automated code review.

**Usage:**
```yaml
tool: code_review
capabilities:
  - Analyze code changes
  - Provide feedback
  - Suggest improvements
```

**Parameters:**
- `prTitle`: PR title
- `prDescription`: Change description

**When to Use:**
- Before finalizing changes
- After significant modifications
- Before security scanning

### codeql_checker
Scan code for security vulnerabilities.

**Usage:**
```yaml
tool: codeql_checker
capabilities:
  - Detect security issues
  - Identify vulnerabilities
  - Suggest fixes
```

**Best Practices:**
- Run after code_review
- Fix critical issues
- Document remaining issues
- Re-run after fixes

### gh-advisory-database
Check dependencies for known vulnerabilities.

**Usage:**
```yaml
tool: gh-advisory-database
capabilities:
  - Check npm packages
  - Check pip packages
  - Check other ecosystems
  - Identify CVEs
```

**Supported Ecosystems:**
- actions, composer, erlang, go
- maven, npm, nuget, pip
- pub, rubygems, rust, swift

**Parameters:**
```yaml
dependencies:
  - name: "express"
    version: "4.17.1"
    ecosystem: "npm"
```

## Tool Selection Guidelines

### By Agent Type

**Code Review Agent:**
- view, bash (for linting)
- Code analysis tools
- git diff tools

**Testing Agent:**
- bash, write_bash, read_bash
- Language-specific test runners
- Coverage tools

**Documentation Agent:**
- view, create, edit
- Markdown tools
- Link checkers

**Security Agent:**
- codeql_checker
- gh-advisory-database
- Static analysis tools

**Build Agent:**
- bash (for build commands)
- Language-specific builders
- Artifact tools

### By Task Complexity

**Simple Tasks:**
Minimum tools needed for the specific task.

**Complex Tasks:**
May need orchestration of multiple tools.

**Interactive Tasks:**
bash (async), write_bash, read_bash for REPL-style work.

## Security Considerations

### Tool Restrictions

When configuring tools for agents:

1. **Least Privilege**: Grant minimal necessary access
2. **Read-Only When Possible**: Limit write access
3. **Scope Limits**: Restrict to relevant directories
4. **Audit Trail**: Log tool usage
5. **Validation**: Verify tool outputs

### Sensitive Operations

Be cautious with:
- File system modifications
- Network requests
- External API calls
- Secret handling
- Credential access

## Performance Tips

### Optimization Strategies

1. **Parallel Operations**: Call independent tools simultaneously
2. **Command Chaining**: Combine related bash commands
3. **Selective Viewing**: Use view_range for large files
4. **Async for Long Operations**: Use async bash mode
5. **Caching**: Reuse results when possible

### Common Patterns

```bash
# Good: Parallel file reads
view(file1) + view(file2) + view(file3)

# Good: Command chaining
bash("cmd1 && cmd2 && cmd3")

# Good: Targeted viewing
view(path, view_range=[100, 200])

# Bad: Sequential when parallel possible
view(file1) then view(file2) then view(file3)
```

## Troubleshooting

### Common Issues

**Tool Not Found:**
- Verify tool is installed
- Check PATH configuration
- Use absolute paths

**Permission Denied:**
- Check file permissions
- Verify directory access
- Use appropriate tool restrictions

**Timeout Errors:**
- Increase timeout value
- Use async mode
- Break into smaller operations

**Interactive Tool Hangs:**
- Disable pagers
- Use async mode with write_bash
- Set appropriate delays

## Examples

See the `examples/` directory for complete agent implementations demonstrating proper tool usage in real scenarios.

## Additional Resources

- [Agent Design Guide](AGENT_DESIGN_GUIDE.md)
- [GitHub API Documentation](https://docs.github.com/en/rest)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
