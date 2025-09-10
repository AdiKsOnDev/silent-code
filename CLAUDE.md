# Optimized Iteration-Based Development Orchestration

This document outlines the high-performance orchestration process for managing iteration-based development using specialized agents and parallel execution for maximum efficiency.

## Overview

The development process uses a decomposed agent architecture where specialized agents handle specific responsibilities, enabling parallel execution and faster iteration cycles while maintaining comprehensive quality assurance.

## Optimized Agent Architecture

### Core Development Agents
- **iteration-planner**: Creates structured, iteration-based development plans for complex multi-component projects
- **implementation-agent**: Pure coding specialist optimized for feature development
  
- **feedback-processor**: Rapid issue resolution specialist for addressing review feedback

**Note**: Claude Code itself acts as the iteration orchestrator, managing workflow state and delegating to specialized agents.

### Review & Quality Agents
- **code-quality**: Analyzes code structure, readability, complexity, and language conventions with automated linting
- **project-tester**: Runs comprehensive unit tests, integration tests, and functional validation
- **documentation-checker**: Validates docstrings, inline comments, and ensures meaningful documentation
- **ci-cd-professional**: Creates GitHub workflows, automation scripts, and deployment pipelines

### Project Management Agents
- **linear-manager**: Manages Linear issues, projects, and workflows ONLY when explicitly requested by user

### Performance Benefits
- **Parallel Reviews**: All QA agents run simultaneously instead of sequentially
- **Specialized Context**: Each agent optimized for specific tasks with minimal overhead
- **Faster Cycles**: Reduced context switching and improved focus per agent
- **Better Scaling**: Independent agents can be optimized and cached separately

## Agent Planning Phase

### 0. Planning an Iteration
If `tmp/project_plan.md` does not exist, ask user clarifying questions and use the iteration-planner to create `tmp/project_plan.md`

**iteration-planner Capabilities:**
- Analyzes complex project requirements and breaks them into manageable iterations
- Creates structured development plans with clear deliverables and dependencies
- Sequences iterations logically to minimize rework and risk
- Defines success metrics and acceptance criteria for each iteration
- Provides estimation and timeline guidance
- Identifies potential bottlenecks and mitigation strategies

## Orchestration Workflow

### 1. Starting an Iteration
```
User: "Start iteration 1 from the project plan"
→ Claude Code analyzes tmp/project_plan.md  
→ Creates tmp/reports/iteration_1/state.md with PLANNING status
→ Delegates to implementation-agent using Task tool for feature development
→ Monitors implementation progress through state management
```

### 2. Parallel Review Orchestration Process
When implementation is complete:

```
Claude Code: "Implementation complete. Running parallel reviews."
→ Launches ALL review agents in parallel using Task tool:
  ├── code-quality agent (async)
  ├── project-tester agent (async)  
  ├── documentation-checker agent (async)
  └── ci-cd-professional agent (async)
→ Aggregates results into consolidated-review.md
→ Returns APPROVED/NEEDS_CHANGES status to Claude Code
```

### 3. Feedback Processing (if needed)
If reviews show NEEDS_CHANGES:

```
Claude Code: "Issues found. Delegating to feedback-processor."
→ Uses Task tool to launch feedback-processor
→ feedback-processor reads consolidated review feedback
→ Systematically addresses all NEEDS_CHANGES items  
→ Creates feedback-fixes.md with change summary
→ Claude Code triggers re-review cycle with parallel agents
→ Repeats until all reviews show APPROVED
```

### 4. Iteration Completion
When all reviews are APPROVED:

```
Claude Code: "All reviews approved. Completing iteration."
→ Updates state to COMPLETED
→ Generates comprehensive final-report.md
→ Prepares context for next iteration
```

## State Management

### State File Format (`tmp/reports/iteration_{number}/state.md`)
```markdown
# Iteration {number} State

**Current Phase:** WAITING_FOR_REVIEWS  
**Current Agent:** Claude Code (parallel reviews)
**Last Updated:** 2025-01-15 14:30:00

## Completed Tasks
- [x] Implemented user authentication system (via implementation-agent)
- [x] Added login/logout functionality  
- [x] Created user registration form

## Active Delegation
- **Current**: Claude Code running parallel quality assurance reviews
- **Pending**: All 4 review agents executing simultaneously:
  - [ ] code-quality - review code structure and conventions
  - [ ] project-tester - validate authentication flows  
  - [ ] documentation-checker - verify docstrings and comments
  - [ ] ci-cd-professional - validate automation (if applicable)

## Files Modified  
- src/auth/login.js
- src/auth/register.js
- src/components/LoginForm.jsx

## Next Steps
Waiting for Claude Code to aggregate parallel review results into consolidated-review.md.

## Agent Context for Resume
- **implementation-agent**: Authentication system complete with JWT tokens
- **Claude Code**: All review agents launched in parallel
- **Claude Code**: Ready to process review results and delegate next steps
```

### State Phases & Agent Delegation
- `PLANNING`: Claude Code analyzing requirements and creating execution plan
- `IMPLEMENTING`: implementation-agent actively working on features and fixes  
- `WAITING_FOR_REVIEWS`: Claude Code coordinating parallel review execution
- `ADDRESSING_FEEDBACK`: feedback-processor systematically resolving review issues
- `COMPLETED`: Claude Code generating final reports and preparing next iteration

## Optimized Review Agent Coordination

### Parallel Review Execution by Claude Code
Claude Code coordinates all review agents simultaneously for maximum efficiency:

- **code-quality**: Analyze code structure, readability, and conventions (async)
- **project-tester**: Run tests and validate functionality (async)  
- **documentation-checker**: Verify documentation quality (async)
- **ci-cd-professional**: Setup or validate CI/CD if applicable (async)

### Performance Benefits
- **4x Speed Improvement**: All reviews execute in parallel instead of sequentially  
- **Aggregated Results**: Consolidated review report with unified approval status
- **Smart Coordination**: Claude Code handles timing, failures, and result synthesis
- **Maintained Quality**: Same comprehensive coverage with faster execution

### Detailed Review Agent Capabilities

**code-quality Agent:**
- Runs comprehensive linting tools (ruff, mypy, bandit for Python; eslint, prettier for JS/TS)
- Analyzes code structure using Beautiful Code Principles (8 principles framework)
- Checks coding standards compliance and naming conventions
- Evaluates complexity and maintainability metrics
- Validates security best practices and vulnerability scanning
- Provides specific improvement recommendations with file/line references

**project-tester Agent:**
- Automatically detects testing frameworks (pytest, jest, vitest, etc.)
- Runs comprehensive unit, integration, and functional tests
- Validates application runtime and feature functionality
- Tests API endpoints and CLI functionality where applicable
- Generates coverage reports and identifies untested code paths
- Provides detailed failure analysis with reproduction steps

**documentation-checker Agent:**
- Validates docstring presence and quality for all public interfaces
- Ensures inline comments explain WHY (not WHAT) with meaningful context
- Checks adherence to documentation style guides (Google/NumPy for Python, JSDoc for JS)
- Identifies missing documentation for complex logic and business rules
- Flags inappropriate comments that restate obvious code
- Provides specific text recommendations for documentation improvements

**ci-cd-professional Agent:**
- Creates comprehensive GitHub workflows for automated testing and deployment
- Generates robust Makefiles with development, testing, and deployment targets
- Sets up multi-language linting pipelines (ruff/mypy/bandit for Python, etc.)
- Implements security monitoring and dependency vulnerability scanning
- Configures multi-environment testing matrices
- Optimizes build performance with intelligent caching strategies

### Review Result Format

**Individual Review Files** saved by each review agent:
`tmp/reports/iteration_{number}/reviews/{agent-name}-review.md`:
```markdown
# {Agent Name} Review - Iteration {number}

**Status:** APPROVED | NEEDS_CHANGES

## Summary
Brief overview of review findings

## Detailed Feedback
- Specific issues found
- Recommendations for improvement
- Code quality metrics

## Approval Criteria
What needs to be fixed for approval (if NEEDS_CHANGES)
```

**Consolidated Review Report** created by Claude Code:
`tmp/reports/iteration_{number}/reviews/consolidated-review.md`:
```markdown
# Consolidated Review Report - Iteration {number}

**Overall Status:** APPROVED | NEEDS_CHANGES
**Completed by:** Claude Code

## Review Summary
- Code Quality: APPROVED/NEEDS_CHANGES
- Testing: APPROVED/NEEDS_CHANGES  
- Documentation: APPROVED/NEEDS_CHANGES
- CI/CD: APPROVED/NEEDS_CHANGES

## Critical Issues (if any)
[Aggregated issues that must be resolved for approval]

## Approval Criteria  
[Unified requirements for achieving APPROVED status across all reviews]
```

## Example Optimized Orchestration Session

```bash
# Start iteration (Claude Code orchestrates directly)
user: "Start iteration 1 from the project plan"
# → Claude Code analyzes requirements and delegates to implementation-agent
# → Monitors progress through state management

# Parallel reviews (Claude Code runs directly)
claude: Uses Task tool to run all quality assurance review agents in parallel
# → Launches all 4 review agents simultaneously
# → Aggregates results in ~1/4 the time of sequential reviews

# Feedback processing (Claude Code delegates to feedback-processor)  
claude: Uses Task tool with feedback-processor to address consolidated review issues
# → Systematically fixes all issues
# → Prepares for rapid re-review cycle

# Completion (Claude Code handles directly)
# → Updates iteration state to COMPLETED
# → Generates final reports and prepares for next iteration
```

## Optimized Best Practices

1. **Use specialized agents** - Leverage implementation-agent and feedback-processor with Claude Code orchestration for maximum efficiency
2. **Enable parallel execution** - Claude Code runs all review agents simultaneously
3. **Maintain agent focus** - Keep each agent specialized to its core responsibility
4. **Check state before resuming** - Read state.md files to understand current context
5. **Monitor workflow transitions** - Track when Claude Code delegates between agents via Task tool
6. **Optimize feedback cycles** - Use feedback-processor for rapid issue resolution

## Performance Improvements

### Speed Optimizations
- **4x faster reviews**: Parallel execution instead of sequential
- **Reduced context switching**: Specialized agents with focused responsibilities  
- **Faster feedback cycles**: Dedicated feedback-processor for issue resolution
- **Streamlined coordination**: Claude Code directly managing workflow with native tool access

### Quality Maintenance  
- **Comprehensive coverage**: All review agents still execute (just in parallel)
- **Detailed feedback**: Consolidated review reports with actionable items
- **Systematic fixes**: feedback-processor addresses all issues methodically
- **State continuity**: Full workflow state preserved across agent transitions

## Directory Structure
```
tmp/
└── reports/
    └── iteration_{number}/
        ├── state.md              # Current iteration state (managed by Claude Code)
        ├── final-report.md       # Created when COMPLETED
        ├── feedback-fixes.md     # Changes made by feedback-processor
        └── reviews/
            ├── consolidated-review.md        # Aggregated results from Claude Code
            ├── code-quality-review.md
            ├── project-tester-review.md
            ├── documentation-checker-review.md
            └── ci-cd-review.md
```

## Agent Migration Path

### From Legacy iteration-executor
If you have existing workflows using the monolithic iteration-executor:

1. **Immediate improvement**: Use Claude Code direct orchestration instead of iteration-executor
2. **Enable parallel reviews**: Claude Code runs all review agents in parallel  
3. **Optimize feedback**: Use feedback-processor for faster issue resolution
4. **Full optimization**: Use implementation-agent for pure coding tasks

### Backward Compatibility
- All state file formats remain the same
- Directory structure is preserved with additions
- Review result formats are unchanged
- Workflow can be gradually migrated agent by agent

## Complete Workflow Integration

### Complete Agent Ecosystem Summary

| Agent | Category | Purpose | Optimizes For | Key Benefits |
|-------|----------|---------|---------------|--------------|
| **iteration-planner** | Planning | Creates structured development plans | Project planning | Break complex projects into manageable iterations |
| **Claude Code** | Orchestration | Direct iteration lifecycle management | Workflow efficiency | Native tool access, state management, direct delegation |
| **implementation-agent** | Development | Pure feature development | Code development speed | No context switching, focused coding |
| **feedback-processor** | Resolution | Issue resolution | Fix iteration speed | Rapid, systematic problem resolution |
| **code-quality** | QA Review | Code analysis & linting | Code quality | Comprehensive quality analysis with 8 Beautiful Code principles |
| **project-tester** | QA Review | Testing & validation | Test coverage | Multi-framework testing with detailed failure analysis |
| **documentation-checker** | QA Review | Documentation quality | Documentation standards | WHY-focused comments, complete docstrings |
| **ci-cd-professional** | QA Review | Automation & deployment | DevOps efficiency | Comprehensive CI/CD with multi-language support |
| **linear-manager** | Project Management | Linear issue & project management | Project tracking | Advanced search, bulk operations, workflow integration |

### Workflow State Transitions

```
PLANNING (Claude Code orchestration)
    ↓ delegates to
IMPLEMENTING (implementation-agent via Task tool)  
    ↓ triggers  
WAITING_FOR_REVIEWS (Claude Code via Task tool → parallel QA agents)
    ↓ if NEEDS_CHANGES, delegates to
ADDRESSING_FEEDBACK (feedback-processor via Task tool)
    ↓ triggers re-review via  
WAITING_FOR_REVIEWS (Claude Code → parallel re-review)
    ↓ when all APPROVED
COMPLETED (Claude Code → final reporting)
```

### Performance Metrics

**Before Optimization (monolithic iteration-executor):**
- Sequential review execution: ~4x agent call overhead  
- Context switching between planning/coding/reviewing
- Single agent handling all responsibilities
- Review bottlenecks slow entire iteration

**After Optimization (specialized agents):**
- Parallel review execution: ~1x review cycle time
- Specialized context per agent type
- Focused responsibilities enable caching and optimization
- Parallel execution eliminates review bottlenecks

**Expected Speed Improvements:**
- **Review Phase**: 75% faster (4 agents in parallel vs sequential)
- **Implementation Phase**: 40% faster (focused coding agent)
- **Feedback Phase**: 60% faster (dedicated issue resolution)
- **Overall Iteration**: 50-70% faster completion time

## Agent Usage Guide

### When to Use Each Agent

**Planning Phase:**
- Use **iteration-planner** when starting a new complex project or when `tmp/project_plan.md` doesn't exist
- Example: `"Use iteration-planner to create a development plan for building a microservices authentication system"`

**Development Phase:**
- **Claude Code orchestrates directly** - no separate coordinator agent needed
- Use **implementation-agent** when you need focused coding without workflow overhead
- Examples: 
  - `"Start iteration 2 from the project plan"` (Claude Code handles orchestration)
  - `"Use implementation-agent to implement the user authentication API endpoints"`

**Quality Assurance Phase:**
- **Claude Code runs reviews directly** - uses Task tool to run all review agents in parallel
- Use individual review agents only when you need specific analysis
- Examples:
  - `"Run all quality assurance reviews for iteration 1"` (Claude Code handles directly)
  - `"Use code-quality agent to analyze the new authentication module"`

**Issue Resolution Phase:**
- Use **feedback-processor** when reviews identify issues that need systematic fixes
- Example: `"Use feedback-processor to address the 8 issues identified in consolidated-review.md"`

**Project Management Phase (User-Requested Only):**
- Use **linear-manager** ONLY when the user explicitly requests Linear operations
- **Important**: This agent is NOT used automatically during development workflows
- Examples:
  - `"Use linear-manager to create Linear issues for iteration 3 tasks"` (only when user asks)
  - `"Use linear-manager to update issue SEN-62 status to Done"` (only when user requests)

### Typical Workflow Sequence

1. **Project Start**: `iteration-planner` → creates `tmp/project_plan.md`
2. **Iteration Execution**: `Claude Code` → delegates to `implementation-agent` via Task tool
3. **Quality Review**: `Claude Code` → runs all QA agents in parallel via Task tool
4. **Issue Resolution**: `Claude Code` → delegates to `feedback-processor` via Task tool → fixes issues, triggers re-review
5. **Completion**: `Claude Code` → finalizes iteration, prepares next

This optimized orchestration approach provides significant performance improvements while maintaining comprehensive quality assurance and workflow continuity.
