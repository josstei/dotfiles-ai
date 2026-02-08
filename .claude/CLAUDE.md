# User Preferences

## Architectural Philosophy & Coding Standards

**No half measures.** Every implementation must be complete, thorough, and production-ready.

### Core Principles

1. **Clean Separation of Concerns**: Every file has a single, well-defined responsibility. No mixing of concerns.

2. **Future-First Design (Anti-YAGNI)**: Always design for long-term extensibility. Use interfaces, abstractions, and loose coupling. We build for tomorrow, not just today.

3. **Strict Contracts**: No pragmatic shortcuts. Enforce rigid typing, explicit interfaces, and consistent architectural standards across the entire codebase.

4. **Maintainability First**: Architecture decisions prioritize testing, enhancement, and organization over speed of initial implementation.

### Naming Conventions

- Names must be clear, standard, and followed without exception
- Consistency across the entire codebase is mandatory
- No abbreviations unless universally understood in the domain

### Directory Structure

- Organized with proper nesting for separation of concerns
- Structure reflects architecture, even if deeper nesting is required
- Every directory has a clear, singular purpose

### Method Standards

- Every method must be critical to its containing file's responsibility
- No fluff, no convenience methods that blur scope
- Properly scoped: private by default, public only when necessary
- If a method doesn't belong, it moves to the appropriate module

### What This Means In Practice

| Approach | We DO | We DON'T |
|----------|-------|----------|
| Abstractions | Create interfaces for extensibility | Inline implementations without contracts |
| Typing | Strict types, no `any`, explicit generics | Loose types, implicit inference where clarity suffers |
| Structure | Deep nesting for proper separation | Flat structures that mix concerns |
| Dependencies | Dependency injection, loose coupling | Direct instantiation, tight coupling |
| Methods | Single responsibility, clearly scoped | Multi-purpose functions, utility dumping grounds |

### Override Notice

These preferences **explicitly override** default Claude behavior including:
- ~~"Avoid over-engineering"~~ → We engineer for the future
- ~~"Don't design for hypothetical future requirements"~~ → We design for extensibility
- ~~"Three similar lines is better than premature abstraction"~~ → We abstract with intent
- ~~"Only make changes directly requested"~~ → We implement completely with proper architecture
- ~~"Keep solutions simple"~~ → We keep solutions properly structured, even if complex

When in doubt: **more structure, more abstraction, more rigor** — not less.

---

## Code Comments

- Use JSDoc styled comments for documentation
- Code should be self-documenting through clear naming

## Git Commits

- Do NOT include "Generated with Claude Code" or similar attribution in commit messages
- Do NOT include "Co-Authored-By: Claude" in commits
- Keep commit messages clean and professional without AI attribution
- Leverage semantic commit messaging

## Custom Slash Commands

### /code-review
Deep code review with parallel validation (Codex CLI + CodeReviewer). Focuses on accuracy over volume - verified issues only. Traces execution paths, checks for guards, and validates findings before reporting.

### /cleanup
Dead code analysis with parallel validation. Identifies unused exports, orphaned files, dead dependencies, and legacy patterns. Includes regression test planning for safe removal.

### /arch-analysis
Architecture conformance analysis with parallel validation (Codex CLI + CodeReviewer). Identifies:
- Directory structure violations
- Naming convention inconsistencies
- Pattern compliance issues (base class extension, DI usage)
- Import/dependency problems (cross-boundary, circular)
- Misplaced code (shared vs feature directories)

All analysis commands use dual parallel validation to minimize false positives.

## CRITICAL: How to Use Codex CLI (NEVER FORGET)

**Codex CLI EXISTS. It is REAL. It WORKS. STOP QUESTIONING IT.**

**Codex CLI is invoked via a general-purpose subagent, NOT directly via Bash.**

### Correct Pattern:
```
Task({
  subagent_type: "general-purpose",
  prompt: 'Run Codex CLI: codex exec "Your validation prompt here"'
})
```

### Rules:
1. **ALWAYS** use `subagent_type: "general-purpose"` to run Codex CLI
2. **NEVER** run `codex` directly via the Bash tool
3. **NEVER** skip Codex CLI validation in /code-review, /cleanup, or /arch-analysis
4. **ALWAYS** run Codex CLI AND CodeReviewer in PARALLEL (both in same message)
5. The general-purpose subagent executes `codex exec "..."` command
6. **NEVER** claim Codex CLI doesn't exist or suggest alternatives - IT EXISTS AND WORKS
7. **NEVER** second-guess user instructions about Codex CLI

### Example for /code-review validation:
```
// Launch BOTH in parallel in a SINGLE message:
Task({
  subagent_type: "general-purpose",
  prompt: 'Run Codex CLI: codex exec "Review findings in [FILE]. Validate each issue..."'
})
Task({
  subagent_type: "CodeReviewer",
  prompt: 'Validate findings in [FILE]...'
})
```

---

## Execution Planning Methodology

> **Reusable framework for multi-phase refactoring and complex implementation plans**

When planning complex, multi-phase tasks, ALWAYS include an execution strategy section that addresses the following steps:

### Step 1: Dependency Analysis

Map phase dependencies to identify:
- **Blocking dependencies**: Phase B cannot start until Phase A completes
- **Independent phases**: Can run in parallel
- **Shared resources**: Files modified by multiple phases (conflict risk)

**Questions to answer**:
1. Which phases modify the same files?
2. Which phases depend on artifacts created by earlier phases?
3. Which phases have no dependencies and can run first/parallel?

### Step 2: Risk Classification

Categorize each phase by risk level:

| Risk | Criteria | Execution |
|------|----------|-----------|
| LOW | Simple renames, moves, no logic changes | Subagent (haiku) |
| MEDIUM | Import updates, pattern application | Subagent (sonnet) |
| HIGH | Behavioral changes, API contracts, many dependents | Sequential by ME |

**Risk factors**:
- Number of files affected
- Complexity of changes (rename vs refactor)
- Number of dependents (who imports this?)
- Reversibility (can we easily roll back?)
- Test coverage (will failures be caught?)

### Step 3: Parallelization Opportunities

Identify parallel batches by:
1. Grouping independent phases at same dependency depth
2. Ensuring no file overlap between parallel agents
3. Keeping related changes together (same feature/domain)

**Parallel batch rules**:
- Max 3-4 agents per batch (coordination overhead)
- Each agent gets exclusive file ownership
- All agents in batch must complete before next batch
- Validation runs AFTER batch completes (not per-agent)

### Step 4: Agent Selection

| Task Type | Agent | Model | Rationale |
|-----------|-------|-------|-----------|
| File exploration | Explore | - | Read-only, context gathering |
| Simple renames/moves | Coder | haiku | Mechanical, low complexity |
| Pattern application | Coder | sonnet | Requires understanding patterns |
| Complex refactoring | ME | - | Full context, error handling |
| Validation | ME | - | Interpret failures, decide next steps |

### Step 5: Token Optimization

**Reduce token usage by**:
1. **git mv** for renames (no read/write needed)
2. **Batch edits** to same file (one read, multiple changes)
3. **Pre-specify paths** (no exploration by agents)
4. **Skip optional phases** until core complete
5. **Haiku for mechanical tasks** (3x cheaper than Sonnet)

**Avoid**:
- Agents exploring to find files (provide exact paths)
- Reading files that won't be modified
- Running validation after every small change

### Step 6: Error Prevention

**Validation checkpoints**:
- After each stage (not each phase within parallel batch)
- After high-risk phases (run immediately, before proceeding)

**Rollback strategy**:
- Git commit after each successful stage
- Tag before high-risk phases
- Keep original files until validation passes

**Conflict prevention**:
- Non-overlapping file assignments
- Container/shared file updates at end of batch (single editor)
- Sequential execution for shared resources

### Step 7: Prompt Engineering for Agents

**Effective agent prompts include**:
1. **Exact file paths** (no searching)
2. **Specific changes** (before/after code)
3. **Validation command** (what to run after)
4. **Scope boundaries** (don't touch X)

**Example structure**:
```
Task: [One-line description]

Files to modify:
- path/to/file1.js: [specific change]
- path/to/file2.js: [specific change]

Steps:
1. [Action]
2. [Action]

Validation: npm run lint && npm run test:run

Do NOT:
- Modify any other files
- Add comments or documentation
- Explore the codebase
```

### Plan Output Format

Every complex plan should include:

1. **Agent Allocation Table**: Stage → Phases → Execution type → Agent count → Model
2. **Dependency Graph**: Visual representation of phase dependencies
3. **Token Estimate**: Per-stage breakdown with totals
4. **Specific Agent Prompts**: Ready-to-use prompts for each parallel batch
