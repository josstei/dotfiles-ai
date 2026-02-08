---
description: Conduct a rigorous, accurate deep code review with verified findings
---

# Deep Code Review Protocol

You are conducting a rigorous code review. Your goal is **accuracy over volume**. Four real issues are infinitely more valuable than 200 false positives.

## Core Principles

1. **Verify before claiming** - Every issue must have traceable evidence
2. **Trace execution paths** - Follow code flow, don't pattern-match
3. **Respect existing design** - Understand before criticizing
4. **Quality over quantity** - Report only verified issues
5. **Assume competence** - The original author likely considered obvious cases

---

## Phase 1: Establish Ground Truth

### 1.1 Run the Test Suite
```bash
# Run tests FIRST - note pass/fail count
npm test / pytest / go test / etc.
```
- Record: `X/Y tests passing`
- If tests pass, claimed "bugs" require strong evidence
- A well-tested codebase means the code works as designed

### 1.2 Understand the Architecture
Before reviewing individual files:
- Read README, CLAUDE.md, CONTRIBUTING.md
- Identify the architectural patterns (DI, event-driven, state machines, etc.)
- Note the testing strategy and coverage
- Understand the runtime environment (browser, Node, Electron, etc.)

### 1.3 Identify Review Scope
- Which files/modules are being reviewed?
- What is the criticality level? (core logic vs utilities)
- Are there existing abstractions that handle common concerns?

---

## Phase 2: File-Level Analysis

For each file under review:

### 2.1 Read the FULL File First
- Never claim issues at line N without reading lines 1 to EOF
- Understand the class/module's purpose and design
- Identify state management patterns
- Note error handling strategies

### 2.2 Map the Control Flow
- Entry points (public methods, exports, event handlers)
- Internal flow between methods
- Exit points (returns, throws, callbacks, events)
- State transitions (if state machine)

### 2.3 Identify Dependencies
- What does this code depend on?
- What depends on this code?
- Are dependencies injected or hardcoded?

---

## Phase 3: Issue Verification (CRITICAL)

**For EVERY potential issue you identify, complete this verification:**

### 3.1 Trace the Execution Path
```
REQUIRED: Show the call chain that leads to the issue

Caller A.method()
  → Calls B.method(params)
    → Reaches problematic code at line X
    → [Explain why guards don't prevent it]
```

### 3.2 Verify Guards Don't Exist
Check for:
- Validation at function entry
- State checks before operations
- Try-catch wrapping the call site
- Type checking (TypeScript, PropTypes, runtime checks)
- Upstream filtering of invalid inputs

### 3.3 Check for State Machine Patterns
If the code has states:
- List ALL valid state transitions
- Verify the "stuck" state truly has no exit
- Check if error recovery exists in OTHER methods
- Look for reset/cleanup methods

### 3.4 Confirm with a Mental Test
Ask: "Can I write a test that fails due to this bug?"
- If yes: describe the test
- If no: the issue is likely not real

---

## Phase 4: Issue-Specific Verification Checklists

### Race Conditions
- [ ] Identified TWO concurrent call sites (not sequential)
- [ ] Showed shared mutable state between them
- [ ] Verified NO serialization exists (promises, locks, queues)
- [ ] Verified NO idempotency makes it safe
- [ ] Can describe a specific interleaving that causes failure

### Memory Leaks
- [ ] Identified the RETAINED reference (not function-local)
- [ ] Showed the object escapes its scope
- [ ] Verified no cleanup mechanism exists
- [ ] For event listeners: showed they're never removed
- [ ] For caches: showed unbounded growth without eviction

### Null/Undefined Errors
- [ ] Traced where null can originate
- [ ] Showed the path to where it's dereferenced unsafely
- [ ] Verified no upstream null checks exist
- [ ] Considered optional chaining / nullish coalescing

### Security Vulnerabilities
- [ ] Identified UNTRUSTED input source (user input, external API)
- [ ] Traced it to unsafe usage (no sanitization)
- [ ] Verified no validation/escaping layer exists
- [ ] Internal-only code paths = lower severity

### API Misuse
- [ ] Verified the API is actually called in production paths
- [ ] Checked the runtime environment provides the API
- [ ] Looked for feature detection / polyfills
- [ ] Considered graceful degradation patterns

### Error Handling Gaps
- [ ] Identified operations that CAN throw
- [ ] Showed no try-catch exists at appropriate level
- [ ] Verified error propagation would cause real problems
- [ ] Checked for global error handlers

---

## Phase 5: Severity Classification

### CRITICAL (Fix immediately)
- Security vulnerabilities with exploit path
- Data loss / corruption scenarios
- Crash in core user flows with no recovery
- **Requirement**: Must have verified exploit/reproduction

### HIGH (Fix soon)
- Errors that affect user experience significantly
- Resource leaks that compound over time
- Race conditions with observable symptoms
- **Requirement**: Must have plausible trigger scenario

### MEDIUM (Fix when convenient)
- Edge cases that rarely occur
- Performance issues with measurable impact
- Code that works but is fragile
- **Requirement**: Should have some evidence of impact

### LOW (Consider fixing)
- Style/convention violations
- Minor optimization opportunities
- Documentation gaps
- **Requirement**: Should improve maintainability

---

## Phase 6: Cross-Validation (REQUIRED)

After completing your analysis, you MUST validate findings with TWO independent reviewers before presenting to user.

**Write findings to a temp file first:**

Generate a unique filename with timestamp: `FINDINGS_FILE=~/.claude/temp/review-findings-$(date +%s).md`

Write your complete findings to `$FINDINGS_FILE` including:
- Files reviewed and test suite status
- Each finding with: location, severity, evidence, execution path
- Any assumptions made
- Confidence level for each issue

**Run BOTH validators in parallel using Task tool:**

Launch both subagents simultaneously in a single message with two Task tool calls:

1. **Codex CLI subagent** - Use Task tool with `subagent_type: "general-purpose"`:

```
Run Codex CLI to validate code review findings:

codex exec "Review my code review findings in $FINDINGS_FILE. Validate each issue - is the execution path accurate? Are the guards truly missing? Any false positives? Did I miss anything? Read the file first."

Fully ensure that any subagent leverages the Codex CLI if available using codex --version.  If available leverage codex, else use another CodeReviewr subagent

Return the complete Codex response.
```

2. **CodeReviewer subagent** - Use Task tool with `subagent_type: "CodeReviewer"`:

```
Review the code review findings in $FINDINGS_FILE. Your job is to validate or refute each finding.

For EACH finding listed:
1. Verify the execution path is accurate by tracing the code yourself
2. Confirm guards/validation truly don't exist at:
   - Function entry
   - Call sites
   - Upstream callers
   - Global error handlers
3. For state machine issues: verify all transitions, check for recovery mechanisms
4. For race conditions: confirm concurrent execution is actually possible
5. For memory leaks: verify references are actually retained
6. Search the codebase yourself to confirm the evidence
7. Flag any false positives with explanation
8. Rate your confidence: HIGH/MEDIUM/LOW

Also identify anything the analysis missed.

Return a structured validation report with:
- Confirmed findings (verified issues) with evidence
- Refuted findings (false positives) with reasoning
- Additional findings you discovered
- Overall assessment quality rating
```

**Compare results from both validators:**

After receiving responses from both subagents:

1. **Identify consensus:** Findings both validators agree are real issues (high confidence)
2. **Identify disagreements:** Findings where validators differ - investigate these further
3. **Merge additional discoveries:** Include any extra findings either validator found
4. **Resolve conflicts:** For disagreements, re-examine the code yourself and make a judgment call, documenting your reasoning

**Revise based on combined validation:**
- Only keep findings confirmed by at least one validator (with your agreement)
- Downgrade severity for findings with validator disagreement
- Add issues either reviewer found that you missed
- Document any unresolved disagreements for user review

**Cleanup:**
```bash
rm $FINDINGS_FILE
```

---

## Phase 7: Report Format

For each VERIFIED issue:

```markdown
### [SEVERITY] Issue Title

**Location:** `path/to/file.js:LINE`

**Problem:**
[One sentence describing the issue]

**Evidence:**
```javascript
// Quote the problematic code (with sufficient context)
```

**Execution Path:**
1. `CallerA.method()` calls `CallerB.method()`
2. `CallerB` passes [value] to problematic code
3. No guards prevent [bad outcome]

**Why This Is Real:**
- [Specific evidence that guards don't exist]
- [Specific scenario that triggers the bug]

**Suggested Fix:**
```javascript
// Minimal, targeted fix
```

**Test Case:**
```javascript
// How to verify this bug exists / is fixed
```

**Validated:** ✅ Codex CLI / CodeReviewer
```

---

## Anti-Patterns to AVOID

### Never Do This:

1. **Pattern matching without path tracing**
   - ❌ "This uses `eval()` - security risk!"
   - ✅ "User input flows to `eval()` via X→Y→Z without sanitization"

2. **Claiming issues in defensive code**
   - ❌ "No null check here!" (when caller guarantees non-null)
   - ✅ Trace where null can actually originate

3. **Ignoring the runtime environment**
   - ❌ "navigator.mediaDevices might not exist" (in Electron renderer)
   - ✅ Consider the actual deployment context

4. **Claiming memory leaks for scoped variables**
   - ❌ "Canvas element never released!" (function-local, GC'd automatically)
   - ✅ Identify actual retained references that prevent GC

5. **Inventing race conditions**
   - ❌ "Two async calls could overlap!" (when serialization exists)
   - ✅ Show the specific interleaving that causes failure

6. **Claiming state machines get stuck**
   - ❌ "State becomes ERROR and is stuck!" (without checking all transitions)
   - ✅ Map ALL transitions before claiming dead ends

7. **Quantity inflation**
   - ❌ "200+ issues found!" (mostly false positives)
   - ✅ "4 verified issues requiring attention"

8. **Suggesting fixes for working code**
   - ❌ Adding redundant validation
   - ✅ Only fix actual bugs

---

## Pre-Submission Validation

Before finalizing your review, verify each issue:

- [ ] I read the ENTIRE file, not just flagged lines
- [ ] I traced the execution path from entry to issue
- [ ] I verified no guards/validation prevent this issue
- [ ] I checked for state machine recovery mechanisms
- [ ] I can describe a specific scenario that triggers this
- [ ] This would NOT be caught by existing passing tests
- [ ] The fix I'm suggesting doesn't break existing behavior
- [ ] I'm not pattern-matching - I traced actual code flow
- [ ] **Issue has been validated by Codex CLI and CodeReviewer**

---

## Output Structure

### Executive Summary
- Files reviewed: X
- Test suite status: X/Y passing
- Issues found: X critical, Y high, Z medium, W low
- Validation: Codex CLI + CodeReviewer (dual parallel validation)
- Validator consensus: X/Y findings confirmed by both
- Overall assessment: [1-2 sentences]

### Critical/High Issues (Detailed)
[Full issue reports per format above]

### Medium/Low Issues (Summarized)
[Brief descriptions with file:line references]

### False Positives Avoided
[Issues initially flagged but removed after validation]

### Recommendations
[Prioritized action items]

---

## Remember

> "The goal of code review is not to find bugs. The goal is to find REAL bugs."

A review with 4 verified issues and 0 false positives is superior to one with 200 claimed issues where 196 are wrong. False positives waste developer time and erode trust in the review process.

When in doubt: trace the code path, verify guards don't exist, validate with both Codex CLI and CodeReviewer in parallel, and only then report.
