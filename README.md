# Universal Agent System for Claude Code

A collection of specialized, universal agents for code development, quality assurance, and project management. Each agent works standalone for quick tasks or integrates seamlessly into structured iteration-based workflows for complex projects.

## Table of Contents

- [Overview](#overview)
- [What Makes These Agents Universal?](#what-makes-these-agents-universal)
- [Key Features](#key-features)
- [Available Agents](#available-agents)
  - [Core Development Agents](#core-development-agents)
  - [Quality Assurance Agents](#quality-assurance-agents)
  - [Feedback & Resolution Agents](#feedback--resolution-agents)
  - [Planning & Automation Agents](#planning--automation-agents)
- [Usage Modes](#usage-modes)
  - [Standalone Usage](#standalone-usage)
  - [Iteration Workflow](#iteration-workflow)
- [Quick Start Examples](#quick-start-examples)
- [Project Structure](#project-structure)
- [Your Role in Development](#your-role-in-development)
- [Benefits](#benefits)
- [Best Practices](#best-practices)
- [License](#license)

## Overview

This system provides specialized agents designed around **what they analyze** (artifact-centric), not **when they run** (process-centric). Each agent works independently with just git + source code, automatically adapting to your project structure, language, and available tools.

**Use them standalone** for quick tasks (code review, bug fixes, testing) or **integrate into iteration workflows** for complex multi-phase projects. Every agent discovers its context automatically, works with partial tooling gracefully, and produces self-contained results.

## What Makes These Agents Universal?

### Artifact-Centric Design
Each agent is specialized around **WHAT it analyzes**, making them reliable in any context:
- **code-quality** analyzes source code quality, style, and security
- **project-tester** analyzes test results and functionality
- **bug-fixer** analyzes bug reports and software defects
- **implementation-agent** analyzes feature requirements to implement

### Smart Environment Discovery
Every agent automatically:
- ✓ Detects project type and language (Python, JS/TS, Rust, Go, Java)
- ✓ Discovers context from git history and filesystem
- ✓ Identifies available tools (pytest, ruff, eslint, etc.)
- ✓ Determines operating mode (standalone vs workflow)
- ✓ Generates appropriate output paths

### Graceful Tool Degradation
Agents work with whatever tools are available:
- ✓ Full tooling → comprehensive analysis with automated checks
- ✓ Partial tooling → analysis with manual review + installation guidance
- ✓ No special tools → still provides value with built-in analysis
- ✓ Always documents what's missing and how to install it

### Self-Contained Reports
Each agent produces complete, standalone output:
- ✓ No dependencies on other agent outputs
- ✓ Clear status (APPROVED / NEEDS_CHANGES)
- ✓ Specific, actionable recommendations
- ✓ Can be used independently or aggregated

## Key Features

### Universal Operation
- ◆ **Works Anywhere**: Every agent operates standalone or in workflows
- ◆ **Auto-Discovery**: Agents detect project type, language, and context automatically
- ◆ **Language Support**: Python, JavaScript/TypeScript, Rust, Go, Java, and more
- ◆ **Graceful Degradation**: Works with partial tooling, provides installation guidance

### Development Power
- ◆ **Specialized Agents**: Focused tools for coding, testing, quality, documentation
- ◆ **Parallel Reviews**: Run multiple quality checks simultaneously (50-70% faster)
- ◆ **Smart Feedback**: Agents identify issues and provide specific fixes
- ◆ **Clean Git History**: Conventional commits with semantic versioning

### Flexibility
- ◆ **Quick Tasks**: Use individual agents for code review, testing, bug fixes
- ◆ **Structured Workflows**: Integrate into iteration-based development for complex projects
- ◆ **Self-Contained**: Each agent produces complete, independent reports
- ◆ **No Lock-in**: Use agents à la carte or as a complete system

## Available Agents

All agents work **standalone** (quick tasks) or **in workflow** (complex projects). Each agent automatically detects your environment and adapts.

### Core Development Agents

**`implementation-agent`** - Pure coding specialist
- **What it analyzes:** Feature requirements and source code
- **Standalone:** Direct feature implementation, bug fixes, refactoring
- **Workflow:** Implementation phase coding work
- **Output:** Working code following project conventions

**`code-quality`** - Code analysis and linting expert
- **What it analyzes:** Source code quality, style, complexity, security
- **Standalone:** Ad-hoc code review, pre-commit checks
- **Workflow:** Review phase quality assurance
- **Output:** `CODE-QUALITY-REVIEW.md` with detailed analysis
- **Tools:** ruff, mypy, bandit (Python) | eslint, prettier (JS/TS) | graceful degradation

**`project-tester`** - Testing and validation specialist
- **What it analyzes:** Test results, coverage, functionality
- **Standalone:** Quick test runs, debugging test failures
- **Workflow:** Review phase testing validation
- **Output:** `PROJECT-TESTER-REVIEW.md` with test results

**`comment-quality`** - Code documentation analyst
- **What it analyzes:** Docstrings and inline comments (NOT project docs)
- **Standalone:** Documentation audit, pre-commit doc checks
- **Workflow:** Review phase documentation validation
- **Output:** `COMMENT-QUALITY-REVIEW.md` with quality assessment

**`git-history-manager`** - Git operations specialist
- **What it manages:** Commits, branches, tags, semantic versioning
- **Standalone:** Creating commits, managing branches
- **Workflow:** Granular commits during implementation + final versioning
- **Output:** Clean git history with conventional commits

### Feedback & Resolution Agents

**`feedback-processor`** - Issue resolution specialist
- **What it analyzes:** Review feedback from any source
- **Standalone:** Fix issues from manual feedback or tool output
- **Workflow:** ADDRESSING_FEEDBACK phase - fix review issues systematically
- **Output:** `FEEDBACK-FIXES.md` with fix summary

**`bug-fixer`** - Bug diagnosis and fixing specialist
- **What it analyzes:** Bug reports and software defects
- **Standalone:** Fix any bug at any time (primary use case)
- **Workflow:** Generally NOT used in iterations (use feedback-processor instead)
- **Output:** `tmp/reports/bugs/PATCH_NOTES.md` with root cause analysis
- ⚠ **Important:** Independent of iteration workflow - use for maintenance and debugging

### Planning & Automation Agents

**`iteration-planner`** - Development planning specialist
- **What it does:** Creates structured iteration-based development plans
- **Use:** When starting complex multi-phase projects
- **Output:** Structured iteration plan with clear milestones

**`ci-cd-professional`** - DevOps automation specialist
- **What it analyzes:** Project automation needs and CI/CD requirements
- **Standalone:** Setup GitHub workflows, Makefile, automation anytime
- **Workflow:** Optional - include when CI/CD changes needed
- **Output:** `.github/workflows/`, `Makefile`, automation configs

**`documentation-manager`** - Project documentation specialist
- **What it analyzes:** Project-level docs (README, CONTRIBUTING, CHANGELOG)
- **Standalone:** Create/update project documentation anytime
- **Workflow:** Optional - include when docs need updating
- **Output:** Updated README.md, CONTRIBUTING.md, CHANGELOG.md

**`linear-manager`** - Issue tracking specialist
- **What it manages:** Linear issues, projects, milestones
- **Use:** Project management and issue tracking
- **Requires:** [linear-cli](https://github.com/AdiKsOnDev/linear-cli)

## Usage Modes

### Standalone Usage

Use individual agents for quick, focused tasks. No iteration workflow required.

**Code Review:**
```
claude: "Review the code quality in src/auth/"
```
→ code-quality agent produces `tmp/reviews/YYYYMMDD_HHMMSS/CODE-QUALITY-REVIEW.md`

**Run Tests:**
```
claude: "Run the test suite and show me the results"
```
→ project-tester agent executes tests and reports results

**Fix a Bug:**
```
claude: "There's a bug in login - users can't authenticate"
```
→ bug-fixer agent diagnoses, fixes, documents in `tmp/reports/bugs/PATCH_NOTES.md`

**Implement Feature:**
```
claude: "Add rate limiting to the API endpoints"
```
→ implementation-agent writes the code following project patterns

**Setup CI/CD:**
```
claude: "Setup GitHub workflows and Makefile for this project"
```
→ ci-cd-professional creates workflows, Makefile, configs

**Check Documentation:**
```
claude: "Review the docstrings and comments in the codebase"
```
→ comment-quality agent analyzes and provides recommendations

**Make Commits:**
```
claude: "Commit these changes with proper conventional commits"
```
→ git-history-manager creates semantic commits

### Iteration Workflow

For complex projects requiring structured development cycles.

**1. Plan Your Project**
```
claude: "Use iteration-planner to create a development plan for [your project]"
```
→ Review plan carefully, confirm before proceeding

**2. Start Development**
```
claude: "Begin iteration 1 implementation"
```
→ Agents handle coding, commits, state tracking autonomously

**3. Run Quality Reviews**
```
claude: "Run quality assurance reviews for iteration 1"
```
→ All agents run in parallel for fast feedback

**4. YOUR TURN: Review the Code** ★
- Read implementation code thoroughly
- Review quality reports in `tmp/reports/iteration_1/reviews/`
- Check git commits to understand changes
- Verify alignment with your vision
- Ask questions about anything unclear

**5. Address Feedback (if needed)**
```
claude: "Use feedback-processor to address review issues"
```
→ Repeat reviews until all quality gates pass

**6. Review Before Next Iteration**
- ✓ Understand what was built and why
- ✓ Test features yourself if possible
- ✓ Plan next steps based on learnings

## Quick Start Examples

### Scenario 1: Quick Code Review Before Commit
```
claude: "Review code quality and run tests before I commit"
```
→ Agents analyze code and tests, provide feedback
→ Fix any issues, then commit

### Scenario 2: Bug in Production
```
claude: "There's a memory leak in the background worker process"
```
→ bug-fixer agent diagnoses, fixes, documents
→ Test the fix, then deploy

### Scenario 3: New Feature Implementation
```
claude: "Implement OAuth2 authentication for the API"
```
→ implementation-agent writes the code
→ Review and test the implementation
→ Use git-history-manager to commit

### Scenario 4: Complex Multi-Phase Project
```
claude: "Use iteration-planner to build a real-time chat application"
```
→ Plan approved → Iteration 1 (backend) → Review → Iteration 2 (frontend) → Review → etc.
→ Structured progress with quality gates at each phase

### Scenario 5: Setup Project Automation
```
claude: "Setup CI/CD pipelines and project documentation"
```
→ ci-cd-professional creates GitHub workflows, Makefile
→ documentation-manager updates README, CONTRIBUTING
→ Professional project infrastructure in minutes

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
│   │   ├── comment-quality.md
│   │   ├── ci-cd-professional.md
│   │   ├── bug-fixer.md
│   │   ├── documentation-manager.md
│   │   └── linear-manager.md
│   └── settings.json                     # Claude Code settings
├── tmp/reports/                          # Iteration tracking
│   ├── iteration_X/
│   │   ├── state.md                      # Current iteration status
│   │   ├── reviews/                      # Quality review results
│   │   │   ├── CODE-QUALITY-REVIEW.md
│   │   │   ├── PROJECT-TESTER-REVIEW.md
│   │   │   ├── COMMENT-QUALITY-REVIEW.md
│   │   │   ├── CI-CD-PROFESSIONAL-REVIEW.md (if applicable)
│   │   │   └── CONSOLIDATED-REVIEW.md
│   │   └── FEEDBACK-FIXES.md             # Summary of fixes made
│   └── bugs/                             # Post-iteration bug tracking
│       └── PATCH_NOTES.md                # Bug fixes and patches
├── CLAUDE.md                             # Workflow instructions (read this!)
└── README.md                             # This file
```

## Your Role in Development

**These agents amplify your capabilities - they don't replace your judgment.**

### What Agents Do:
- ✓ Write implementation code following best practices
- ✓ Run comprehensive automated quality checks
- ✓ Identify and fix technical issues
- ✓ Maintain clean git history and documentation
- ✓ Auto-detect project context and adapt

### What YOU Do (Critical for Success):

**For Standalone Tasks:**
- ★ **Review agent output** - Agents provide analysis, you make decisions
- ★ **Verify recommendations** - Not all suggestions fit your specific context
- ★ **Guide implementation** - Provide clear requirements and feedback
- ★ **Test the results** - Automated tests don't catch everything

**For Iteration Workflows:**
- ★ **Review code between iterations** - Understand what changed and why
- ★ **Read the reports** in `tmp/reports/` - They contain important insights
- ★ **Test features yourself** when possible - Click through UIs, try edge cases
- ★ **Guide direction** - Decide what comes next based on learnings
- ★ **Ask questions** - If something isn't clear, dig deeper before proceeding
- ★ **Stay involved** - Your domain knowledge and vision are irreplaceable

### Why Your Involvement Matters:

**Without active review:**
- ✗ Project may drift from your vision
- ✗ Subtle bugs or design issues slip through
- ✗ You lose context and ownership
- ✗ Work builds on shaky foundations

**With active review:**
- ✓ Builds on solid understanding
- ✓ Catch issues early when they're cheap to fix
- ✓ Maintain full project ownership
- ✓ Code reflects your vision accurately

**Think of it this way**: Agents are expert assistants who execute your vision with high quality. You're the architect and decision-maker. Review their work, verify it matches your needs, and guide the direction.

### Iteration Workflow States

When using structured iteration workflows, each iteration progresses through these states:

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

## Benefits

### Universal Flexibility:
- ◆ **Use Anywhere**: Standalone for quick tasks or workflows for complex projects
- ◆ **No Setup Required**: Agents auto-detect your project and adapt
- ◆ **Language Agnostic**: Works with Python, JS/TS, Rust, Go, Java, and more
- ◆ **Tool Agnostic**: Works with whatever linting/testing tools you have (or none)
- ◆ **À la Carte**: Use individual agents or the complete system

### Development Speed:
- ◆ **Parallel Reviews**: Run quality checks simultaneously (50-70% faster)
- ◆ **Autonomous Implementation**: Agents handle coding while you guide direction
- ◆ **Fast Feedback**: Get immediate analysis on code quality, tests, documentation
- ◆ **Smart Automation**: CI/CD setup, documentation management in minutes

### Code Quality:
- ◆ **Automated Standards**: Linting, type checking, security scans
- ◆ **Comprehensive Testing**: Unit, integration, functional test validation
- ◆ **Documentation Quality**: Enforced docstrings and meaningful comments
- ◆ **Professional History**: Conventional commits with semantic versioning

### Project Management:
- ◆ **Clear Milestones**: Structured iterations with defined goals (when using workflows)
- ◆ **Scope Control**: Focused development prevents feature creep
- ◆ **Progress Visibility**: Detailed reports show what changed and why
- ◆ **Context Preservation**: Git history + reports maintain project knowledge

### Long-Term Success:
- ◆ **Reduced Technical Debt**: Quality gates prevent shortcuts
- ◆ **Better Decisions**: Understanding code informs better planning
- ◆ **Maintained Velocity**: Consistent standards keep development efficient
- ◆ **Knowledge Transfer**: Self-contained reports help onboarding

## Best Practices

### For Using Agents:

**Standalone Tasks:**
1. ▸ **Be specific** - "Review code quality in src/auth/" is better than "check the code"
2. ▸ **Use the right agent** - bug-fixer for bugs, feedback-processor for review issues
3. ▸ **Review agent output** - Agents provide analysis, you make final decisions
4. ▸ **Trust graceful degradation** - Agents work without perfect tooling
5. ▸ **Chain agents** - code review → fix issues → test → commit

**Iteration Workflows:**
1. ▸ **Start with iteration-planner** for complex projects - break down work systematically
2. ▸ **Run review agents in parallel** - use all agents simultaneously for fast feedback
3. ▸ **Address feedback systematically** with feedback-processor - fix all issues before proceeding
4. ▸ **Let git-history-manager handle commits** - consistent, professional git history
5. ▸ **Keep iterations small** - 3-5 days of work max, easier to review and understand

### For Your Involvement:

**Every Time You Use Agents:**
1. ★ **Provide clear context** - The more specific your request, the better the result
2. ★ **Review the output** - Agents are tools, not decision-makers
3. ★ **Verify recommendations** - Not every suggestion fits your specific needs
4. ★ **Test the results** - Automated checks don't catch everything

**For Iteration Workflows:**
1. ★ **Review code between iterations** - Read diffs, understand what changed and why
2. ★ **Read the reports** in `tmp/reports/` - They contain important insights
3. ★ **Test features yourself** - Click UIs, try edge cases, verify behavior
4. ★ **Ask clarifying questions** - Better to ask than assume
5. ★ **Guide direction** - Your vision drives what comes next
6. ★ **Stay engaged** - Don't let iterations pile up unreviewed

### For Project Health:

1. ◆ **Maintain standards** - Let agents enforce code quality consistently
2. ◆ **Document decisions** - Use reports to track why choices were made
3. ◆ **Review regularly** - Even with agents, your judgment is critical
4. ◆ **Adapt as needed** - Agents are flexible, use them how they work best for you
5. ◆ **Build knowledge** - Learn from agent analysis to improve your skills

## License

This template is provided as-is for use with Claude Code. Customize freely for your project needs.
