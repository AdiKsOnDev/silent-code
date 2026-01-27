# Silent Code

A collection of specialized AI agents for code development, quality assurance, and project management. Each agent works independently for quick tasks or integrates into structured workflows for complex projects.

## Table of Contents

- [What This Is](#what-this-is)
- [Installation](#installation)
- [Core Agents](#core-agents)
  - [Development](#development)
  - [Quality Assurance](#quality-assurance)
  - [Test Creation](#test-creation)
  - [Problem Resolution](#problem-resolution)
  - [Automation and Documentation](#automation-and-documentation)
- [Usage](#usage)
  - [Quick Tasks](#quick-tasks)
  - [Structured Workflows](#structured-workflows)
- [Project Structure](#project-structure)
- [How Agents Work](#how-agents-work)
  - [Environment Discovery](#environment-discovery)
  - [Graceful Degradation](#graceful-degradation)
  - [Self-Contained Reports](#self-contained-reports)
- [Your Role](#your-role)
- [Best Practices](#best-practices)
- [Compatibility](#compatibility)
- [License](#license)

## What This Is

Silent Code provides artifact-centric agents that analyze specific aspects of your codebase. Instead of generic AI assistance, you get specialized tools that understand what they're looking at and provide targeted help.

The system works with any project structure, automatically detecting your language, tools, and conventions. Use agents individually for quick tasks or chain them together for comprehensive development workflows.

## Installation

### For Claude Code

Clone this repository into your project or use it as a template:

```bash
git clone git@github.com:AdiKsOnDev/silent-code.git
```

The `.claude/agents/` directory contains all agent configurations. Claude Code will automatically detect and load them.

### For OpenCode

Use the `.opencode/agent/` directory, which contains condensed versions of each agent (under 100 lines). Configure keybindings in `opencode.json`.

## Core Agents

### Development

**explorer** - Understand your codebase
- Answers questions about architecture, implementation, and testing strategies
- Provides code examples and file references
- Use when: "How does authentication work?" or "What's the testing strategy?"

**implementation-agent** - Write code
- Implements features, fixes bugs, handles refactoring
- Follows project conventions automatically
- Use when: "Add rate limiting to API endpoints" or "Implement OAuth2"

**git-history-manager** - Manage commits and versioning
- Creates conventional commits with semantic versioning
- Handles branches, tags, and git operations
- Use when: "Commit these changes" or "Create a release tag"

### Quality Assurance

**code-quality** - Analyze code quality
- Runs linters, checks style, identifies security issues
- Works with ruff, mypy, bandit (Python) or eslint, prettier (JS/TS)
- Generates: `CODE-QUALITY-REVIEW.md`

**project-tester** - Run and validate tests
- Executes test suites, reports coverage and failures
- Works with pytest, jest, or any test framework
- Generates: `PROJECT-TESTER-REVIEW.md`

**comment-quality** - Review code documentation
- Analyzes docstrings and inline comments (not project docs)
- Checks completeness and clarity
- Generates: `COMMENT-QUALITY-REVIEW.md`

### Test Creation

**unit-test-writer** - Create unit tests
- Writes isolated tests for individual functions and classes
- Includes proper mocking and test isolation
- Use when: "Write unit tests for the authentication module"

**integration-test-writer** - Create integration tests
- Tests multi-component interactions with real dependencies
- Validates data flow and component communication
- Use when: "Create integration tests for the API with database"

**e2e-test-writer** - Create end-to-end tests
- Tests complete user workflows
- Uses Playwright, Cypress, Selenium, or subprocess
- Use when: "Create E2E tests for user signup and checkout"

### Problem Resolution

**bug-fixer** - Diagnose and fix bugs
- Analyzes bug reports, identifies root causes, implements fixes
- Documents fixes in `tmp/reports/bugs/PATCH_NOTES.md`
- Use when: "There's a memory leak in the background worker"

**feedback-processor** - Address review feedback
- Systematically fixes issues from quality reviews
- Loops until all quality gates pass
- Use when: Working through iteration feedback

**debug** - Deep debugging analysis
- Analyzes stack traces, runtime errors, and complex bugs
- Provides root cause analysis and debugging strategies
- Use when: bug-fixer needs deeper investigation

**refactoring** - Orchestrate code improvements
- Systematic refactoring guided by quality metrics
- Uses review agents to validate improvements
- Use when: "Refactor the authentication system"

### Automation and Documentation

**ci-cd-professional** - Setup automation
- Creates GitHub workflows, Makefiles, automation configs
- Use when: "Setup CI/CD for this project"

**documentation-manager** - Manage project docs
- Updates README, CONTRIBUTING, CHANGELOG
- Use when: "Update project documentation"

**linear-manager** - Sync with Linear
- Creates and updates Linear issues
- Synchronizes development progress with project tracking
- Use when: "Create a Linear issue for this feature"

**project-structure** - Analyze project organization
- Audits folder structure against best practices
- Provides recommendations for maintainability
- Use when: "Review the project structure"

## Usage

### Quick Tasks

Use agents directly for focused work:

```
"Review code quality in src/auth/"
```
Runs code-quality agent, generates review report.

```
"Run the test suite"
```
Executes tests via project-tester, shows results.

```
"Fix the login bug where users can't authenticate"
```
bug-fixer diagnoses and fixes, documents in PATCH_NOTES.md.

```
"Commit these changes"
```
git-history-manager creates conventional commits.

### Structured Workflows

For complex projects, use iteration-based development:

1. **Plan** - Use planner to break down work into iterations
2. **Implement** - implementation-agent writes code with granular commits
3. **Review** - Run quality agents in parallel for fast feedback
4. **Fix** - feedback-processor addresses all issues
5. **Complete** - Review everything before next iteration

Example:

```
"Use planner to build a real-time chat application"
```

The system creates a structured plan, then guides you through each iteration with quality gates at every phase.

## Project Structure

```
.claude/
├── agents/              # Full agent configurations for Claude Code
└── settings.json

.opencode/
└── agent/              # Condensed agents for OpenCode (<100 lines)

tmp/reports/
├── iteration_X/
│   ├── state.md        # Current iteration status
│   ├── reviews/        # Quality review results
│   └── FEEDBACK-FIXES.md
└── bugs/
    └── PATCH_NOTES.md  # Bug fixes and patches

AGENTS.md               # Coding conventions
CLAUDE.md               # Workflow instructions
opencode.json           # OpenCode configuration
```

## How Agents Work

### Environment Discovery

Every agent automatically detects:
- Project type and language (Python, JS/TS, Rust, Go, Java)
- Available tools (pytest, ruff, eslint, etc.)
- Git history and project context
- Operating mode (standalone vs workflow)

### Graceful Degradation

Agents work with whatever tools are available:
- Full tooling: Comprehensive automated analysis
- Partial tooling: Analysis with manual review guidance
- No special tools: Built-in analysis still provides value

Missing tools are documented with installation instructions.

### Self-Contained Reports

Each agent produces complete, standalone output:
- Clear status (APPROVED / NEEDS_CHANGES)
- Specific, actionable recommendations
- No dependencies on other agent outputs
- Can be used independently or aggregated

## Your Role

These agents amplify your capabilities but don't replace your judgment.

**Agents handle:**
- Writing implementation code
- Running automated quality checks
- Identifying technical issues
- Maintaining git history and documentation

**You handle:**
- Reviewing agent output and making decisions
- Verifying recommendations fit your context
- Testing results beyond automated checks
- Guiding project direction and vision

For iteration workflows, review code between iterations. Read the reports in `tmp/reports/`, test features yourself, and ask questions about anything unclear. Your involvement ensures the project stays aligned with your vision.

## Best Practices

**Be specific** - "Review code quality in src/auth/" beats "check the code"

**Use the right agent** - bug-fixer for bugs, feedback-processor for review issues

**Review output** - Agents provide analysis, you make final decisions

**Chain agents** - code review → fix issues → test → commit

**Keep iterations small** - 3-5 days of work maximum for easier review

**Stay engaged** - Don't let iterations pile up unreviewed

## Compatibility

Works with:
- Claude Code (use `.claude/` directory)
- OpenCode (use `.opencode/` directory)
- Custom AI tools (adapt agent definitions)

## License

MIT - Use freely for your projects.
