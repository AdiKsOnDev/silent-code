# Iteration-Based Development Template

A structured workflow for breaking complex projects into manageable iterations with automated quality assurance through specialized Claude Code agents.

## Overview

This template provides a systematic approach to software development using iteration-based planning, focused implementation, and parallel quality reviews. It ensures consistent code quality and project progress through specialized agents.

## Key Features

- 🔄 **Structured Iterations**: Break complex projects into manageable phases
- 🤖 **Specialized Agents**: Focused tools for planning, coding, testing, and quality assurance
- ⚡ **Parallel Reviews**: Run all quality checks simultaneously for faster feedback
- 📊 **Progress Tracking**: Clear visibility into development status
- 🛡️ **Quality Gates**: Automated checks before iteration completion
- 📝 **Clean Git History**: Granular commits with conventional formatting

## Available Agents

### Development Agents
- **`iteration-planner`** - Creates structured development plans
- **`implementation-agent`** - Focused coding without workflow overhead
- **`feedback-processor`** - Rapidly addresses review feedback
- **`git-history-manager`** - Maintains clean commits and versioning

### Quality Assurance Agents
- **`code-quality`** - Analyzes structure, style, and conventions
- **`project-tester`** - Runs comprehensive tests and validation
- **`documentation-checker`** - Validates docs and comments
- **`ci-cd-professional`** - Creates automation and deployment pipelines

## Quick Start

### 1. Plan Your Project
```bash
claude: "Use iteration-planner to create a development plan for [your project]"
```

### 2. Start Development
```bash
claude: "Begin iteration 1 implementation"
```

### 3. Run Quality Reviews
```bash
claude: "Run quality assurance reviews for iteration 1"
```

### 4. Address Feedback (if needed)
```bash
claude: "Use feedback-processor to address review issues"
```

## Project Structure

```
├── .claude/agents/          # Agent configurations
├── tmp/reports/             # Iteration tracking
│   └── iteration_X/
│       ├── state.md         # Current status
│       └── reviews/         # Quality review results
├── CLAUDE.md               # Workflow instructions
└── README.md               # This file
```

## Workflow States

- **PLANNING** - Creating execution plan with iteration-planner
- **IMPLEMENTING** - Active coding with implementation-agent and git commits
- **WAITING_FOR_REVIEWS** - Running parallel quality checks
- **ADDRESSING_FEEDBACK** - Fixing issues with feedback-processor
- **COMPLETED** - Iteration finished with final reports and version tags

## Benefits

- **Faster Development**: Parallel reviews reduce iteration time by 50-70%
- **Higher Quality**: Automated checks catch issues early
- **Better Organization**: Structured approach prevents scope creep
- **Clean History**: Professional git commits and versioning
- **Predictable Progress**: Clear milestones and completion criteria

## Best Practices

1. Start complex projects with `iteration-planner`
2. Use `implementation-agent` for focused coding sessions
3. Run all review agents in parallel for fastest feedback
4. Address feedback systematically with `feedback-processor`
5. Let `git-history-manager` handle commits and versioning

## License

This template is provided as-is for use with Claude Code. Customize freely for your project needs.