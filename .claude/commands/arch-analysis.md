---
description: Analyze codebase architecture for pattern violations, misplacement, and naming issues
---

# Architecture Conformance Analysis

Analyze the codebase architecture to identify deviations from documented patterns, misplaced files, naming violations, and structural inconsistencies.

## Phase 1: Establish Architecture Context

### 1.1 Discover Architecture Documentation
Search for and read architecture documentation:
- README.md, CLAUDE.md, CONTRIBUTING.md
- docs/architecture/ or similar directories
- Package.json for project structure hints
- Config files (tsconfig, vite.config, etc.) for path aliases

### 1.2 Identify Documented Patterns
Extract from documentation:
- **Directory structure conventions** - Where should different file types live?
- **Naming conventions** - File naming patterns (*.service.js, *.controller.ts, etc.)
- **Class/module patterns** - Base classes, required inheritance, interfaces
- **Dependency injection** - How are dependencies provided?
- **Communication patterns** - Events, IPC, direct calls
- **Layer boundaries** - What can import what?

### 1.3 Build Pattern Checklist
Create a checklist of expected patterns, for example:
- [ ] Services extend BaseService
- [ ] Controllers are in /controllers directory
- [ ] No circular dependencies
- [ ] Imports use path aliases (@/)
- [ ] Events follow naming convention

If no documentation exists, infer patterns from the most established parts of the codebase.

---

## Phase 2: Systematic Analysis

### 2.1 Directory Structure Audit (CRITICAL)

This is the most important audit - compare documented structure against actual implementation.

**Step 1: Extract documented directory structure**
- Look for directory tree diagrams in docs/architecture/, CLAUDE.md, or README.md
- Note every documented directory path and its stated purpose
- Create a checklist of expected directories

**Step 2: Scan actual directory structure**
```bash
find src -type d | sort
```

**Step 3: Compare documented vs actual - create a comparison table:**

| Documented Path | Actual Path | Status |
|-----------------|-------------|--------|
| `src/feature/foo/` | Exists | ✓ Match |
| `src/feature/bar/` | Does not exist | ❌ Missing |
| Not documented | `src/feature/baz/` | ⚠️ Undocumented |
| `src/old-name/` | `src/new-name/` | ⚠️ Name mismatch |

**Step 4: Check for these specific issues:**

1. **Missing documented directories** - Documented but don't exist
   - May indicate stale documentation
   - May indicate incomplete implementation

2. **Undocumented existing directories** - Exist but not documented
   - May be new features not yet documented
   - May be legacy code that should be removed

3. **Name mismatches** - Similar purpose but different names
   - `renderer/` vs `rendering/`
   - `context/` vs `acquisition/`

4. **Structural differences** - Different organization approach
   - Documented: nested subdirs (`components/canvas/`, `components/controls/`)
   - Actual: flat structure (`components/*.js`)

5. **File location violations** - Files in wrong directories
   - Service files outside `services/` directory
   - Adapters outside `adapters/` directory
   - Shared code in feature directories

**Step 5: For each discrepancy, determine:**
- Is documentation stale? (code is correct, docs need update)
- Is code misplaced? (docs are correct, code needs refactor)
- Is this intentional? (documented exception or design decision)

### 2.2 Naming Convention Audit
Check for:
- **Inconsistent suffixes**: Mix of `.service.js`, `.svc.js`, `Service.js`
- **Missing type indicators**: Files that should indicate their type but don't
- **Case inconsistencies**: Mix of kebab-case, camelCase, PascalCase
- **Abbreviation inconsistencies**: Mix of `btn`, `button`, `Btn`

### 2.3 Pattern Compliance Audit
For classes/modules check:
- Required base class extension
- Required interface implementation
- Constructor patterns (super calls, dependency declaration)
- Lifecycle method implementation
- Event subscription patterns

### 2.4 Import/Dependency Audit
Check for:
- Cross-boundary imports (renderer importing main process code)
- Circular dependencies
- Inconsistent import styles (relative vs alias)
- Direct instantiation vs dependency injection
- Hardcoded values that should be centralized

### 2.5 Code Organization Audit
Identify:
- Shared code in feature directories (should be in shared/)
- Feature code in shared directories (should be in features/)
- Utility code mixed with business logic
- Configuration scattered across files

---

## Phase 3: Cross-Validation (REQUIRED)

After completing your analysis, you MUST validate findings with TWO independent reviewers.

**Write findings to a temp file first:**

Generate a unique filename with timestamp: `FINDINGS_FILE=~/.claude/temp/arch-findings-$(date +%s).md`

Write your complete findings to `$FINDINGS_FILE` including:
- Architecture documentation found and patterns identified
- Each violation with: file path, violation type, evidence, severity
- Assumptions made about intended patterns
- Confidence level for each finding

**Run BOTH validators in parallel using Task tool:**

Launch both subagents simultaneously in a single message with two Task tool calls:

1. **Codex CLI subagent** - Use Task tool with `subagent_type: "general-purpose"`:

```
Run Codex CLI to validate architecture analysis findings:

codex exec "Review my architecture analysis in $FINDINGS_FILE. Validate each finding - is the pattern violation real? Am I correctly interpreting the architecture? Any false positives? Did I miss obvious violations? Read the file first."

Return the complete Codex response.
```

2. **CodeReviewer subagent** - Use Task tool with `subagent_type: "CodeReviewer"`:

```
Review the architecture analysis in $FINDINGS_FILE. Your job is to validate or refute each finding.

For EACH finding listed:
1. Verify the documented pattern actually exists and applies
2. Confirm the file/code actually violates the pattern by:
   - Reading the file in question
   - Checking if exceptions are documented
   - Looking for alternative valid patterns
3. Check if the "correct" location/name would cause other issues
4. Verify the severity is appropriate
5. Search the codebase yourself to confirm evidence
6. Flag any false positives with explanation
7. Rate your confidence: HIGH/MEDIUM/LOW

Also identify pattern violations the analysis missed.

Return a structured validation report with:
- Confirmed findings (real violations) with evidence
- Refuted findings (false positives) with reasoning
- Additional findings you discovered
- Assessment of pattern documentation quality
```

**Compare results from both validators:**

After receiving responses from both subagents:

1. **Identify consensus:** Findings both validators agree are real violations
2. **Identify disagreements:** Findings where validators differ - investigate further
3. **Merge additional discoveries:** Include violations either validator found
4. **Resolve conflicts:** For disagreements, re-examine yourself and document reasoning

**Revise based on combined validation:**
- Only keep findings confirmed by at least one validator (with your agreement)
- Downgrade severity for findings with validator disagreement
- Add violations either reviewer found that you missed
- Document unresolved disagreements for user review

---

## Phase 4: Final Report

### Architecture Health Summary
- **Documentation quality:** [Good/Fair/Poor/Missing]
- **Directory structure accuracy:** [X]% of documented dirs exist, [Y] undocumented dirs found
- **Pattern consistency:** [X]% of files follow documented patterns
- **Violations found:** X critical, Y warning, Z info
- **Validation:** Codex CLI + CodeReviewer (dual parallel validation)
- **Validator consensus:** X/Y findings confirmed by both

### Directory Structure Audit Results

#### Documentation vs Reality Comparison

| Category | Count | Examples |
|----------|-------|----------|
| Matching directories | X | `src/app/main/`, `src/features/` |
| Missing documented dirs | X | dirs in docs but not in code |
| Undocumented existing dirs | X | dirs in code but not in docs |
| Name mismatches | X | `renderer/` vs `rendering/` |

#### Directory Structure Issues (if any)

| Documented | Actual | Issue | Recommendation |
|------------|--------|-------|----------------|
| `path/documented/` | Does not exist | Missing | Update docs or implement |
| Not documented | `path/actual/` | Undocumented | Add to docs or remove |

### Violation Categories

#### Critical (Breaks Architecture)
Violations that undermine the architecture's integrity:
- Cross-layer boundary violations
- Circular dependencies
- Security boundary breaches

| File | Violation | Expected | Actual | Validated |
|------|-----------|----------|--------|-----------|

#### Warning (Should Fix)
Inconsistencies that reduce maintainability:
- Misplaced files
- Naming convention violations
- Pattern deviations

| File | Violation | Expected | Actual | Validated |
|------|-----------|----------|--------|-----------|

#### Info (Consider Fixing)
Minor deviations and suggestions:
- Import style inconsistencies
- Documentation gaps
- Optimization opportunities

| File | Issue | Suggestion |
|------|-------|------------|

### False Positives Avoided
[Issues initially flagged but removed after validation]

### Pattern Gaps Identified
Patterns that should be documented but aren't:
- [Pattern observed but not documented]
- [Inconsistent patterns needing standardization]

### Recommended Actions
Prioritized list of fixes:
1. [Highest impact fixes first]
2. [Group related changes]
3. [Note any dependencies between fixes]

---

## Phase 5: Cleanup

After presenting final report to user:

```bash
rm $FINDINGS_FILE
```

---

## Constraints

- This analysis is READ-ONLY - do not modify any files
- Respect the existing architecture even if you'd design it differently
- Flag ambiguities in documentation rather than assuming
- Consider that some "violations" may be intentional exceptions
- Multi-process apps (Electron, microservices) have legitimate cross-boundary needs
- The validation step is MANDATORY - do not skip it
