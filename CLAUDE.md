# Iteration-Based Development Workflow

## Core Workflow Overview

This workflow is for structured iteration-based development. Use when:
- User requests iteration-related work ("start iteration X", "begin next iteration")
- User asks to make a project plan (ask clarifying questions first)
- Working on multi-phase projects requiring systematic development

## Pre-Workflow Setup

### 1. Determine Iteration Number
```bash
# Check for existing iterations
ls tmp/reports/
# If iterations exist, increment highest number
# If none exist, start with iteration_1
```

### 2. Create Directory Structure
```
tmp/reports/iteration_X/
├── state.md
├── reviews/
│   ├── CODE-QUALITY-REVIEW.md
│   ├── PROJECT-TESTER-REVIEW.md  
│   ├── DOCUMENTATION-CHECKER-REVIEW.md
│   ├── CI-CD-PROFESSIONAL-REVIEW.md (if applicable)
│   └── CONSOLIDATED-REVIEW.md
└── FEEDBACK-FIXES.md
```

## Main Iteration Workflow

### Phase 1: Planning
1. **Use iteration-planner agent** (user typically requests this explicitly)
2. **Wait for user verification** of the plan before proceeding
3. **Create state.md** with status: PLANNING → IMPLEMENTING

### Phase 2: Implementation  
1. **Use implementation-agent** with clear requirements from the plan
2. **Use git-history-manager** to make granular commits during implementation
3. **Update state.md** with progress and files modified
4. **Set status to IMPLEMENTING → WAITING_FOR_REVIEWS**

### Phase 3: Review (Parallel Execution)
1. **Run ALL review agents simultaneously using Task tool:**
   - `Task(subagent_type="code-quality")` → saves to `CODE-QUALITY-REVIEW.md`
   - `Task(subagent_type="project-tester")` → saves to `PROJECT-TESTER-REVIEW.md`
   - `Task(subagent_type="documentation-checker")` → saves to `DOCUMENTATION-CHECKER-REVIEW.md`
   - `Task(subagent_type="ci-cd-professional")` → saves to `CI-CD-PROFESSIONAL-REVIEW.md` (see Agent Usage Rules)

2. **Aggregate results** into `CONSOLIDATED-REVIEW.md` with overall APPROVED/NEEDS_CHANGES status
3. **Update state.md** status to WAITING_FOR_REVIEWS → ADDRESSING_FEEDBACK (if needed)

### Phase 4: Feedback Loop
1. **If any review shows NEEDS_CHANGES:**
   - Use `Task(subagent_type="feedback-processor")` to address all issues
   - Save changes summary to `FEEDBACK-FIXES.md`
   - Return to Phase 3 (re-run reviews)

2. **If all reviews show APPROVED:**
   - Use git-history-manager to create final commits and version tags
   - Update state.md status to COMPLETED
   - Generate final iteration report

## File Naming Standards

**Required Files (create/update for each iteration):**
- `tmp/reports/iteration_X/state.md` - Current iteration state and progress
- `tmp/reports/iteration_X/reviews/CODE-QUALITY-REVIEW.md` - Code quality analysis
- `tmp/reports/iteration_X/reviews/PROJECT-TESTER-REVIEW.md` - Testing results  
- `tmp/reports/iteration_X/reviews/DOCUMENTATION-CHECKER-REVIEW.md` - Documentation review
- `tmp/reports/iteration_X/reviews/CONSOLIDATED-REVIEW.md` - Aggregated review status
- `tmp/reports/iteration_X/FEEDBACK-FIXES.md` - Summary of fixes made

**Conditional Files:**
- `tmp/reports/iteration_X/reviews/CI-CD-PROFESSIONAL-REVIEW.md` - Only when CI/CD is relevant

## Agent Usage Rules

### Always Use These Agents:
- `iteration-planner` - For creating structured development plans
- `implementation-agent` - For all coding work  
- `feedback-processor` - For addressing review feedback
- `code-quality` - For code analysis and linting
- `project-tester` - For running tests and validation
- `documentation-checker` - For documentation quality
- `git-history-manager` - For maintaining clean git history with proper commits and versioning

### Conditionally Use:
- `ci-cd-professional` - Include when:
  - Project already has CI/CD pipelines that need updates
  - User specifically requests pipeline/automation setup
  - Project is reaching deployment phase

### Development Dependencies Management:
**CRITICAL: Install Required Tools Before Agent Execution**

Before running review agents, ensure all necessary development tools are installed:

**For Python Projects:**
```bash
pip install ruff mypy bandit
```

**For Node.js/TypeScript Projects:**
```bash
npm install -D eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin prettier
```

**For Other Languages:**
- **Rust:** `cargo install clippy rustfmt`
- **Go:** `go install golang.org/x/tools/cmd/goimports@latest`
- **Java:** Ensure Maven/Gradle and checkstyle are available

**Installation Strategy:**
1. **Check project type** (package.json, requirements.txt, Cargo.toml, etc.)
2. **Install language-specific linting tools** before running code-quality agent
3. **Install testing frameworks** if missing before running project-tester agent
4. **Handle installation failures gracefully** - document missing tools in review reports

**Note:** The code-quality agent specifically requires ruff, mypy, and bandit for Python projects. Install these tools at the beginning of any iteration to ensure successful agent execution.

## Decision Points

### Continue Feedback Loop When:
- Any review agent returns NEEDS_CHANGES status
- Tests are failing
- Code quality issues exist
- Documentation is incomplete

### Complete Iteration When:
- All review agents return APPROVED status
- All tests pass
- No blocking issues remain

### Planning Clarification:
When user asks to make a project plan, ask clarifying questions about:
- Project scope and requirements
- Technology preferences  
- Timeline constraints
- Success criteria

## State Tracking

Update `state.md` at each phase transition:
```markdown
# Iteration X State

**Status:** PLANNING | IMPLEMENTING | WAITING_FOR_REVIEWS | ADDRESSING_FEEDBACK | COMPLETED
**Last Updated:** [timestamp]

## Current Phase
[Description of current work]

## Files Modified
- [list of changed files]

## Next Steps
[what happens next]
```

## Execution Flow - When to Stop vs Continue

### ONLY Stop Execution For:
1. **Before starting an iteration** - Wait for user confirmation to begin
2. **Before running iteration-planner** - Ask user clarifying questions about project scope, requirements, technology preferences, timeline, and success criteria

### NEVER Stop During These Steps - Execute Continuously:
- Pre-workflow setup (determining iteration number, creating directories)
- Implementation phase (run implementation-agent, git-history-manager, and update state.md)
- Review phase (run all review agents in parallel and aggregate results)
- Feedback loop (use feedback-processor and re-run reviews until all approve)
- Git history management (granular commits, versioning, tagging)
- State updates and file management
- Final iteration report generation

### Key Principle: **AUTONOMOUS EXECUTION**
Once you start an iteration (after user confirmation), execute all phases continuously without stopping for user input. The workflow is designed to be self-contained and autonomous.

## Important Notes

- **Never handle tasks directly** - always delegate to appropriate agents
- **Always run review agents in parallel** using Task tool for efficiency
- **Keep iteration numbers sequential** across sessions
- **Execute phases continuously** - only stop at the two specific points mentioned above
- **Continue feedback loop** until all reviews approve