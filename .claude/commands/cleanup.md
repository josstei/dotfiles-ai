# Dead Code Analysis & Cleanup

Perform a comprehensive analysis of this codebase to identify dead code, unused artifacts, and legacy remnants. Validate findings with a separate review agent, then plan regression testing for safe removal.

## Phase 1: Discovery

Systematically search for:

### Unused Exports & Functions
- Exported functions/classes never imported elsewhere
- Internal functions with no callers
- Unused utility functions in shared/helpers directories

### Orphaned Files
- Files not imported by any other file
- Test files for code that no longer exists
- Stale config files, scripts, or tooling artifacts

### Dead Dependencies
- package.json dependencies not imported anywhere
- devDependencies used only by removed tooling
- Duplicate or superseded packages

### Legacy Patterns
- Code marked with TODO/FIXME/DEPRECATED comments
- Old API patterns replaced by newer implementations
- Feature flags or conditional code for shipped/removed features
- Commented-out code blocks

### Unused Assets
- Images, icons, fonts not referenced in code or styles
- Unused CSS classes/variables
- Orphaned documentation for removed features

## Phase 2: Impact Analysis

For each finding, assess:
1. **Confidence level** - How certain is this truly unused?
2. **Risk level** - What breaks if we're wrong?
3. **Dependencies** - What else might rely on this indirectly?
4. **Dynamic usage** - Could this be accessed dynamically (reflection, string interpolation, external config)?

## Phase 3: Cross-Validation (REQUIRED)

After completing your analysis, you MUST validate findings with TWO independent reviewers before presenting to user.

**Write findings to a temp file first:**

Generate a unique filename with timestamp: `FINDINGS_FILE=~/.claude/temp/dead-code-findings-$(date +%s).md`

Write your complete findings to `$FINDINGS_FILE` including:
- Project context and scope analyzed
- Each finding with file path, line numbers, type, evidence, confidence level
- Any assumptions made
- Questions about edge cases

**Run BOTH validators in parallel using Task tool:**

Launch both subagents simultaneously in a single message with two Task tool calls:

1. **Codex CLI subagent** - Use Task tool with `subagent_type: "general-purpose"`:

```
Run Codex CLI to validate dead code findings:

codex exec "Review my findings in $FINDINGS_FILE. Validate each assertion - are they accurate? Any false positives? Did I miss anything? Read the file first."

Return the complete Codex response.
```

2. **CodeReviewer subagent** - Use Task tool with `subagent_type: "CodeReviewer"`:

```
Review the dead code analysis in $FINDINGS_FILE. Your job is to validate or refute each finding.

For EACH finding listed:
1. Verify it's truly unused by checking:
   - Dynamic imports (import(), require() with variables)
   - String-based references (IPC channels, event names, config keys)
   - External entry points (main process, preload, CLI)
   - Reflection patterns
   - Build tool references (vite.config, electron-builder, etc.)
2. Search the codebase yourself to confirm the evidence
3. Flag any false positives with explanation
4. Rate your confidence: HIGH/MEDIUM/LOW

Also identify anything the analysis missed.

Return a structured validation report with:
- Confirmed findings (truly dead code)
- Refuted findings (false positives) with reasoning
- Additional findings you discovered
- Overall assessment quality rating
```

**Compare results from both validators:**

After receiving responses from both subagents:

1. **Identify consensus:** Findings both validators agree are dead code (high confidence)
2. **Identify disagreements:** Findings where validators differ - investigate these further
3. **Merge additional discoveries:** Include any extra findings either validator found
4. **Resolve conflicts:** For disagreements, re-examine the code yourself and make a judgment call, documenting your reasoning

**Revise based on combined validation:**
- Only keep findings confirmed by at least one validator (with your agreement)
- Downgrade confidence for findings with validator disagreement
- Add issues either reviewer found that you missed
- Document any unresolved disagreements for user review

## Phase 4: Final Report

Present validated findings as a structured report:

### High Confidence (Safe to Remove)
| Item | Type | Location | Rationale | Validated |
|------|------|----------|-----------|-----------|

### Medium Confidence (Needs Verification)
| Item | Type | Location | Concern | Reviewer Notes |
|------|------|----------|---------|----------------|

### Low Confidence (Investigate Further)
| Item | Type | Location | Why Uncertain |
|------|------|----------|---------------|

### Recommended Removal Order
Ordered by risk (lowest first):
1. [item] - reason for ordering
2. ...

## Phase 5: Regression Test Plan

For approved removals:

1. **Baseline:** Run full test suite, document current state
2. **Removal checklist:** Order by risk (lowest first)
3. **For each removal:**
   - Remove the code
   - Run: `npm run test:run`
   - Run: `npm run lint`
   - Run: `npm run build`
   - Check for runtime errors in dev mode
   - Document any failures
4. **Commit strategy:** One commit per logical removal group

### Test Commands
```bash
# Full regression suite
npm run test:run

# With coverage to catch untested paths
npm run test:coverage

# Lint check
npm run lint

# Build verification
npm run build
```

## Phase 6: Cleanup

After presenting final report to user:

```bash
rm $FINDINGS_FILE
```

## Constraints

- Do NOT remove anything without explicit user approval
- Flag any code that might be used via dynamic imports or reflection
- Consider external integrations (IPC, plugins, CLI args, preload scripts)
- Check git history - recently added code may appear unused but be WIP
- Electron apps have multiple processes - check main/renderer/preload cross-references
- The validation step is MANDATORY - do not skip it
