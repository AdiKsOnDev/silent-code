# Iteration-Based Development Template

A structured workflow for breaking complex projects into manageable iterations with automated quality assurance through specialized Claude Code agents.

## Table of Contents

- [Overview](#overview)
- [Why This Workflow?](#why-this-workflow)
- [Key Features](#key-features)
- [Available Agents](#available-agents)
  - [Development Agents](#development-agents)
  - [Quality Assurance Agents](#quality-assurance-agents)
  - [Post-Iteration Maintenance](#post-iteration-maintenance)
  - [Utility Agents](#utility-agents)
- [Quick Start](#quick-start)
- [Project Structure](#project-structure)
- [Workflow States](#workflow-states)
- [Your Role in the Process](#your-role-in-the-process)
- [Benefits](#benefits)
- [Best Practices](#best-practices)
- [License](#license)

## Overview

This template provides a systematic approach to software development using iteration-based planning, focused implementation, and parallel quality reviews. It ensures consistent code quality and project progress through specialized agents.

## Why This Workflow?

Traditional development often suffers from:
- ✗ Scope creep and unclear milestones
- ✗ Manual quality checks that get skipped under pressure
- ✗ Inconsistent code standards across features
- ✗ Lost context between development sessions

**This workflow solves these problems** by:
- ✓ Breaking work into clear, reviewable iterations
- ✓ Automating quality assurance through parallel agents
- ✓ Enforcing standards before marking iterations complete
- ✓ Documenting progress and decisions in structured reports

**Most Importantly**: This workflow works best when YOU stay involved. The agents handle implementation and quality checks, but **you need to review the code, understand the changes, and guide the direction between iterations**. Think of this as collaborative development where agents do the heavy lifting, but you maintain ownership and vision.

## Key Features

- ◆ **Structured Iterations**: Break complex projects into manageable phases
- ◆ **Specialized Agents**: Focused tools for planning, coding, testing, and quality assurance
- ◆ **Parallel Reviews**: Run all quality checks simultaneously for faster feedback
- ◆ **Progress Tracking**: Clear visibility into development status with detailed reports
- ◆ **Quality Gates**: Automated checks before iteration completion
- ◆ **Clean Git History**: Granular commits with conventional formatting and semantic versioning

## Available Agents

### Development Agents
- **`iteration-planner`** → Creates structured development plans with clear milestones
- **`implementation-agent`** → Focused coding without workflow overhead
- **`feedback-processor`** → Rapidly addresses review feedback during iterations
- **`git-history-manager`** → Maintains clean commits with conventional formatting and semantic versioning

### Quality Assurance Agents
- **`code-quality`** → Analyzes code structure, style, complexity, and language conventions (runs ruff, mypy, bandit for Python)
- **`project-tester`** → Runs comprehensive tests and validation (unit, integration, functional)
- **`documentation-checker`** → Validates docstrings and inline comments for quality and meaningfulness
- **`ci-cd-professional`** → Creates automation workflows and deployment pipelines (GitHub Actions, Makefiles)

### Post-Iteration Maintenance
- **`bug-fixer`** → Handles bug fixes, debugging, and issue resolution **after** iterations complete
  - ⚠ **Important**: Use ONLY when user reports bugs in finished code
  - ※ NOT for iteration feedback - use `feedback-processor` instead
  - Creates patch notes in `tmp/reports/bugs/PATCH_NOTES.md`

### Utility Agents
- **`linear-manager`** → Manages Linear issues, projects, and milestones (requires [linear-cli installation](https://github.com/AdiKsOnDev/linear-cli))

## Quick Start

### 1. Plan Your Project
```bash
claude: "Use iteration-planner to create a development plan for [your project]"
```
→ Review the plan carefully and confirm before implementation begins

### 2. Start Development
```bash
claude: "Begin iteration 1 implementation"
```
→ Agents handle coding, commits, and state tracking autonomously

### 3. Run Quality Reviews
```bash
claude: "Run quality assurance reviews for iteration 1"
```
→ All agents run in parallel for fast feedback

### 4. **YOUR TURN: Review the Code**
★ **This is the most critical step**
- Read the implementation code thoroughly
- Review the quality reports in `tmp/reports/iteration_1/reviews/`
- Check the git commits to understand what changed
- Verify the changes align with your vision
- Ask questions about anything unclear

### 5. Address Feedback (if needed)
```bash
claude: "Use feedback-processor to address review issues"
```
→ Repeat reviews until all quality gates pass

### 6. Review Again Before Next Iteration
Once iteration completes:
- ✓ Understand what was built and why
- ✓ Test the features yourself if possible
- ✓ Plan what comes next based on what you learned

## Project Structure

```
├── .claude/
│   ├── agents/                           # Agent configurations
│   │   ├── iteration-planner.md
│   │   ├── implementation-agent.md
│   │   ├── feedback-processor.md
│   │   ├── git-history-manager.md
│   │   ├── code-quality.md
│   │   ├── project-tester.md
│   │   ├── documentation-checker.md
│   │   ├── ci-cd-professional.md
│   │   ├── bug-fixer.md
│   │   └── linear-manager.md
│   └── settings.json                     # Claude Code settings
├── tmp/reports/                          # Iteration tracking
│   ├── iteration_X/
│   │   ├── state.md                      # Current iteration status
│   │   ├── reviews/                      # Quality review results
│   │   │   ├── CODE-QUALITY-REVIEW.md
│   │   │   ├── PROJECT-TESTER-REVIEW.md
│   │   │   ├── DOCUMENTATION-CHECKER-REVIEW.md
│   │   │   ├── CI-CD-PROFESSIONAL-REVIEW.md (if applicable)
│   │   │   └── CONSOLIDATED-REVIEW.md
│   │   └── FEEDBACK-FIXES.md             # Summary of fixes made
│   └── bugs/                             # Post-iteration bug tracking
│       └── PATCH_NOTES.md                # Bug fixes and patches
├── CLAUDE.md                             # Workflow instructions (read this!)
└── README.md                             # This file
```

## Workflow States

Each iteration progresses through these states:

1. **PLANNING** → Creating execution plan with iteration-planner
   - ℹ You review and approve the plan before moving forward

2. **IMPLEMENTING** → Active coding with implementation-agent and git commits
   - Agents work autonomously, making granular commits

3. **WAITING_FOR_REVIEWS** → Running parallel quality checks
   - All review agents run simultaneously for speed

4. **ADDRESSING_FEEDBACK** → Fixing issues with feedback-processor
   - Loops until all quality gates pass
   - ℹ You should review consolidated reports during this phase

5. **COMPLETED** → Iteration finished with final reports and version tags
   - ★ **Your turn**: Review everything before starting next iteration

## Your Role in the Process

**This workflow is NOT "set it and forget it"** - it's designed for **active collaboration**:

### What Agents Do:
- ✓ Write implementation code
- ✓ Run automated quality checks
- ✓ Fix technical issues and bugs
- ✓ Maintain git history and documentation

### What YOU Do (Critical for Success):
- ★ **Review the code** between iterations - understand what changed and why
- ★ **Read the reports** in `tmp/reports/` - they contain important insights
- ★ **Test the features** yourself when possible - automated tests don't catch everything
- ★ **Guide the direction** - decide what comes next based on what you learned
- ★ **Ask questions** - if something isn't clear, dig deeper before proceeding
- ★ **Stay involved** - your domain knowledge and vision are irreplaceable

### Why Your Involvement Matters:

**Without active review:**
- ✗ Project may drift from your vision
- ✗ Subtle bugs or design issues slip through
- ✗ You lose context and ownership
- ✗ Iterations build on shaky foundations

**With active review:**
- ✓ Each iteration builds on solid understanding
- ✓ You catch issues early when they're cheap to fix
- ✓ You maintain full project ownership
- ✓ The codebase reflects your vision accurately

**Think of it this way**: Agents are expert contractors who execute your vision with high quality. But you're the architect and project owner. Review their work, verify it matches your needs, and guide the next steps.

## Benefits

### For Development Speed:
- ◆ **Parallel Reviews**: Run all quality checks simultaneously (50-70% faster than sequential)
- ◆ **Autonomous Implementation**: Agents handle coding, commits, and documentation while you focus on direction
- ◆ **Fast Feedback Loops**: Issues caught and fixed within the same iteration

### For Code Quality:
- ◆ **Automated Standards**: Linting, type checking, and security scans on every iteration
- ◆ **Comprehensive Testing**: Unit, integration, and functional tests before marking complete
- ◆ **Documentation Quality**: Enforced docstrings and meaningful comments

### For Project Management:
- ◆ **Clear Milestones**: Each iteration has defined goals and completion criteria
- ◆ **Scope Control**: Prevents feature creep by keeping iterations focused
- ◆ **Progress Visibility**: Detailed reports show exactly what changed and why
- ◆ **Professional History**: Clean git commits with conventional formatting and semantic versioning

### For Long-Term Success:
- ◆ **Maintained Context**: Regular reviews keep you connected to the codebase
- ◆ **Reduced Technical Debt**: Quality gates prevent shortcuts from accumulating
- ◆ **Better Decisions**: Understanding each iteration informs better planning for the next

## Best Practices

### For Agents:
1. ▸ Start complex projects with `iteration-planner` to break down work
2. ▸ Use `implementation-agent` for focused coding without workflow overhead
3. ▸ Run all review agents in parallel for fastest feedback
4. ▸ Address feedback systematically with `feedback-processor`
5. ▸ Let `git-history-manager` handle commits and versioning

### For You:
1. ★ **Review code thoroughly** after each iteration - read the diffs, understand the changes
2. ★ **Read the reports** in `tmp/reports/iteration_X/reviews/` - they highlight important issues
3. ★ **Test features yourself** when possible - click through UIs, try edge cases
4. ★ **Ask clarifying questions** if anything is unclear - better to ask than assume
5. ★ **Plan the next iteration** based on what you learned from the previous one
6. ★ **Keep iterations small** - smaller iterations = easier to review and understand
7. ★ **Stay engaged** - check in regularly, don't let iterations pile up unreviewed

## License

This template is provided as-is for use with Claude Code. Customize freely for your project needs.
