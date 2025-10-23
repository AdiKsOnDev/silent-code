# Universal Agent System with Iteration Workflow

## Overview

This system provides universal agents that work standalone or as part of structured iteration-based development. Each agent is designed around **what it analyzes** (artifact-centric) rather than **when it runs** (process-centric), making them reliable and reusable in any context.

## Agent Operating Modes

**All agents support two modes:**
1. **Standalone Mode** - Agents work independently with just git + source code
2. **Iteration Workflow Mode** - Agents integrate seamlessly into structured development cycles

**How agents adapt:**
- Auto-detect environment (project type, language, existing structure)
- Discover context from git history and filesystem
- Work with partial tooling (graceful degradation)
- Generate appropriate output paths automatically
- Produce self-contained, complete reports

## Iteration-Based Development Workflow

Use iteration workflow when:
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
│   ├── COMMENT-QUALITY-REVIEW.md
│   ├── CI-CD-PROFESSIONAL-REVIEW.md (if applicable)
│   └── CONSOLIDATED-REVIEW.md
└── FEEDBACK-FIXES.md
```

## Main Iteration Workflow

### Phase 1: Planning
1. **Use project-planner agent** (user typically requests this explicitly)
2. **Wait for user verification** of the plan before proceeding
3. **Create state.md** with status: PLANNING → IMPLEMENTING

### Phase 2: Implementation  
1. **Use implementation-agent** with clear requirements from the plan
2. **Use git-history-manager** to make granular commits during implementation
3. **Update state.md** with progress and files modified
4. **Set status to IMPLEMENTING → WAITING_FOR_REVIEWS**

### Phase 3: Review (IMPORTANT: Run multiple Task invocations in a SINGLE message)
1. **Run multiple Task invocations in a SINGLE message, to ensure that the following agents run in parallel:**
   - `Task(subagent_type="code-quality")` → saves to `CODE-QUALITY-REVIEW.md`
   - `Task(subagent_type="project-tester")` → saves to `PROJECT-TESTER-REVIEW.md`
   - `Task(subagent_type="comment-quality")` → saves to `COMMENT-QUALITY-REVIEW.md`
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
- `tmp/reports/iteration_X/reviews/COMMENT-QUALITY-REVIEW.md` - Code comment and docstring quality review
- `tmp/reports/iteration_X/reviews/CONSOLIDATED-REVIEW.md` - Aggregated review status
- `tmp/reports/iteration_X/FEEDBACK-FIXES.md` - Summary of fixes made

**Conditional Files:**
- `tmp/reports/iteration_X/reviews/CI-CD-PROFESSIONAL-REVIEW.md` - Only when CI/CD is relevant

**Post-Iteration Maintenance Files:**
- `tmp/reports/bugs/PATCH_NOTES.md` - Bug fixes and patches applied after iterations (created by bug-fixer agent)

## Agent Catalog & Usage

**All agents are universal** - they work standalone or within iteration workflow. Each agent performs environment discovery to adapt to your project automatically.

### Core Development Agents

**`implementation-agent`** - Pure coding specialist
- **What it analyzes:** Feature requirements and source code to implement
- **Standalone use:** Direct feature implementation, bug fixes, refactoring
- **Iteration use:** Implementation phase coding work
- **Requirements:** Git repo + source code
- **Output:** Working code following project conventions

**`code-quality`** - Code analysis and linting expert
- **What it analyzes:** Source code quality, style, complexity, security
- **Standalone use:** Ad-hoc code review, pre-commit checks
- **Iteration use:** Review phase quality assurance
- **Requirements:** Git repo + source code (tools optional - graceful degradation)
- **Output:** `CODE-QUALITY-REVIEW.md` (auto-detects save location)

**`project-tester`** - Testing and validation specialist
- **What it analyzes:** Test results, coverage, functionality
- **Standalone use:** Quick test runs, debugging test failures
- **Iteration use:** Review phase testing validation
- **Requirements:** Git repo + source code (test framework optional)
- **Output:** `PROJECT-TESTER-REVIEW.md` (auto-detects save location)

**`comment-quality`** - Code documentation analyst
- **What it analyzes:** Docstrings and inline comments (NOT project docs)
- **Standalone use:** Documentation audit, pre-commit doc checks
- **Iteration use:** Review phase documentation validation
- **Requirements:** Git repo + source code
- **Output:** `COMMENT-QUALITY-REVIEW.md` (auto-detects save location)

**`git-history-manager`** - Git operations specialist
- **What it manages:** Commits, branches, tags, semantic versioning
- **Standalone use:** Creating commits, managing branches
- **Iteration use:** Granular commits during implementation + final versioning
- **Requirements:** Git repo + CONTRIBUTING.md (for commit scopes)
- **Output:** Clean git history with conventional commits

### Test Writing Agents

**`e2e-test-writer`** - End-to-end test creation specialist
- **What it creates:** Complete user workflow tests that simulate real user behavior
- **Standalone use:** Create E2E tests for critical user journeys anytime
- **Iteration use:** Add E2E tests during implementation or review phase
- **Requirements:** Git repo + source code + understanding of user workflows
- **Output:** E2E test files (tests/e2e/) using project's E2E framework (Playwright, Cypress, Selenium)
- **Focus:** Quality over coverage - meaningful workflows, not synthetic scenarios

**`unit-test-writer`** - Unit test creation specialist
- **What it creates:** Granular, isolated tests for individual functions/methods/classes
- **Standalone use:** Create unit tests for specific code units anytime
- **Iteration use:** Add unit tests during implementation or review phase
- **Requirements:** Git repo + source code to test
- **Output:** Unit test files with proper mocking and isolation
- **Focus:** One unit at a time, complete isolation, quality over coverage

**`integration-test-writer`** - Integration test creation specialist
- **What it creates:** Tests that validate how multiple components work together
- **Standalone use:** Create integration tests for component interactions anytime
- **Iteration use:** Add integration tests during implementation or review phase
- **Requirements:** Git repo + source code + test infrastructure (databases, etc.)
- **Output:** Integration test files with real dependencies (tests/integration/)
- **Focus:** Multi-component interactions, data flow validation, transaction integrity

### Feedback & Resolution Agents

**`feedback-processor`** - Issue resolution specialist
- **What it analyzes:** Review feedback from any source
- **Standalone use:** Fix issues from manual feedback or tool output
- **Iteration use:** ADDRESSING_FEEDBACK phase - fix review issues
- **Requirements:** Git repo + feedback (review reports, user instructions, or tool output)
- **Output:** `FEEDBACK-FIXES.md` (auto-detects save location)

**`bug-fixer`** - Bug diagnosis and fixing specialist
- **What it analyzes:** Bug reports and software defects
- **Standalone use:** Fix any bug at any time (primary use case)
- **Iteration use:** Generally NOT used in iterations (use feedback-processor instead)
- **Requirements:** Git repo + bug description
- **Output:** `tmp/reports/bugs/PATCH_NOTES.md` with root cause analysis
- **IMPORTANT:** Independent of iteration workflow - use for maintenance and debugging

**Bug-fixer vs Feedback-processor distinction:**
- Use `feedback-processor` during iterations to address review feedback (ADDRESSING_FEEDBACK phase)
- Use `bug-fixer` for standalone bug fixes when user reports issues outside iteration context
- `bug-fixer` documents in PATCH_NOTES.md, provides verification steps, does NOT commit
- `feedback-processor` works from review reports, fixes systematically, enables re-review

### Planning & Automation Agents

**`project-planner`** - Development planning specialist
- **What it analyzes:** Project requirements and user discussions to create concise plans
- **Standalone use:** Create project plans anytime user needs planning
- **Iteration use:** Phase 1 planning (when using iteration workflow)
- **Requirements:** Project description, scope, requirements
- **Output:** Concise project plan at `./tmp/project_plan.md`
- **Focus:** Keep plans SHORT - capture all requirements with minimal verbosity

**`ci-cd-professional`** - DevOps automation specialist
- **What it analyzes:** Project automation needs and CI/CD requirements
- **Standalone use:** Setup GitHub workflows, Makefile, automation anytime
- **Iteration use:** Optional - include when CI/CD changes needed
- **Requirements:** Git repo + source code
- **Output:** `.github/workflows/`, `Makefile`, `CI-CD-PROFESSIONAL-REVIEW.md`

**`documentation-manager`** - Project documentation specialist
- **What it analyzes:** Project-level documentation (README, CONTRIBUTING, CHANGELOG)
- **Standalone use:** Create/update project documentation anytime
- **Iteration use:** Optional - include when docs need updating
- **Requirements:** Git repo + codebase for analysis
- **Output:** Updated README.md, CONTRIBUTING.md, CHANGELOG.md

### Development Dependencies Management

**Agents use graceful tool degradation** - they work with partial tooling and provide installation guidance.

**Optimal tooling by language:**

**Python:**
```bash
pip install ruff mypy bandit pytest pytest-cov
```

**Node.js/TypeScript:**
```bash
npm install -D eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin prettier jest
```

**Other Languages:**
- **Rust:** `cargo install clippy rustfmt` (usually pre-installed with Rust)
- **Go:** `go install golang.org/x/tools/cmd/goimports@latest`
- **Java:** Ensure Maven/Gradle and checkstyle are available

**How agents handle missing tools:**
1. **Auto-detect** available tools in environment
2. **Work with what exists** - provide partial analysis if some tools missing
3. **Document gaps** - report which tools are missing and how to install them
4. **Provide installation commands** - include exact commands in review reports
5. **Never fail silently** - always communicate tool availability status

**Example:** code-quality agent without ruff will:
- ✓ Still analyze code structure and complexity manually
- ✓ Document that ruff is missing
- ✓ Provide `pip install ruff` command in report
- ✓ Mark review as NEEDS_CHANGES with tool installation as first step

## Standalone Agent Usage Examples

**All agents work independently outside the iteration workflow:**

### Quick Code Review
```
User: "Review the code quality in src/auth/"
→ Use code-quality agent
→ Output: tmp/reviews/YYYYMMDD_HHMMSS/CODE-QUALITY-REVIEW.md
```

### Run Tests
```
User: "Run the test suite and show me results"
→ Use project-tester agent
→ Output: tmp/reviews/YYYYMMDD_HHMMSS/PROJECT-TESTER-REVIEW.md
```

### Fix a Bug
```
User: "There's a bug in login - users can't authenticate"
→ Use bug-fixer agent (NOT feedback-processor)
→ Output: tmp/reports/bugs/PATCH_NOTES.md with fix details
```

### Check Documentation
```
User: "Review comments and docstrings in the codebase"
→ Use comment-quality agent
→ Output: tmp/reviews/YYYYMMDD_HHMMSS/COMMENT-QUALITY-REVIEW.md
```

### Setup CI/CD
```
User: "Setup GitHub workflows and Makefile"
→ Use ci-cd-professional agent
→ Output: .github/workflows/, Makefile, CI-CD-PROFESSIONAL-REVIEW.md
```

### Implement Feature
```
User: "Add rate limiting to the API endpoints"
→ Use implementation-agent
→ Output: Working code in source files
```

### Make Commits
```
User: "Commit these changes with proper conventional commits"
→ Use git-history-manager agent
→ Output: Clean git history with semantic commits
```

### Address Feedback
```
User: "Fix the 5 issues identified in the last review"
→ Use feedback-processor agent
→ Output: Fixed code + FEEDBACK-FIXES.md summary
```

### Create Tests
```
User: "Create E2E tests for the checkout flow"
→ Use e2e-test-writer agent
→ Output: E2E test files in tests/e2e/

User: "Write unit tests for the UserService class"
→ Use unit-test-writer agent
→ Output: Unit test files with mocked dependencies

User: "Create integration tests for API endpoints with database"
→ Use integration-test-writer agent
→ Output: Integration test files with real test database
```

### Create Project Plan
```
User: "Help me plan out building a task management system"
→ Use project-planner agent (asks clarifying questions first)
→ Output: Concise project plan at ./tmp/project_plan.md
```

**Key principle:** Every agent works independently with just git + source code + their specific requirements.

## Decision Points (Iteration Workflow)

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
2. **Before running project-planner** - Ask user clarifying questions about project scope, requirements, technology preferences, timeline, and success criteria

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

### Universal Agent Principles
- **Agents are artifacts, not processes** - Each agent is designed around WHAT it analyzes, not WHEN it runs
- **Always delegate to agents** - Never handle tasks directly, use specialized agents
- **Agents auto-discover context** - They detect project type, language, tools, and mode automatically
- **Graceful degradation** - Agents work with partial tooling and provide clear guidance
- **Self-contained reports** - Each agent produces complete, standalone output
- **No assumptions** - Agents don't assume iteration workflow, other agents, or specific tools exist

### Iteration Workflow Specifics
- **Run review agents in parallel** - Use multiple Task invocations in a SINGLE message for efficiency
- **Keep iteration numbers sequential** - Maintain consistent numbering across sessions
- **Execute phases continuously** - Only stop at the two specified points (before start, before planning)
- **Continue feedback loop** - Repeat until all reviews show APPROVED status

### Agent Selection
- **Bug-fixer vs Feedback-processor:**
  - Use `feedback-processor` during iterations (ADDRESSING_FEEDBACK phase) to fix review issues
  - Use `bug-fixer` for standalone bug fixes outside iteration context
  - `bug-fixer` is maintenance-focused, `feedback-processor` is review-focused

- **When to use standalone agents:**
  - User asks for specific analysis (code review, testing, documentation check)
  - User reports bugs or issues outside iteration workflow
  - Need quick feedback without full iteration cycle
  - Setting up project infrastructure (CI/CD, documentation)
