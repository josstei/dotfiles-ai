---
description: Orchestrate full engineering team review and improvement effort
---

As TechLead, coordinate a comprehensive engineering team review of the codebase. Delegate work to all relevant specialized agents on your team:

## Team Coordination Instructions

Launch specialized agents IN PARALLEL (single message with multiple Task tool calls) for the following responsibilities:

**Architecture & Design:**
- **Architect**: Review overall system architecture, layer separation, module boundaries, and design patterns. Identify architectural improvements.

**Code Quality:**
- **CodeReviewer**: Perform thorough code review for best practices, code style, naming conventions, and general code quality issues.
- **Refactor**: Identify code duplication, tight coupling, and refactoring opportunities. Focus on DRY violations and code smells.

**Security & Performance:**
- **SecurityEngineer**: Conduct security audit for vulnerabilities (command injection, path traversal, etc.) and OWASP compliance.
- **PerformanceEngineer**: Profile performance bottlenecks, analyze algorithmic complexity, review caching strategies, and identify optimization opportunities.

**Testing & Quality Assurance:**
- **Tester**: Assess test coverage, identify missing tests, and recommend testing strategy improvements.
- **Debugger**: Review error handling patterns, logging adequacy, and debugging capabilities.

**Domain-Specific:**
- **APIDesigner**: Review public API design, function signatures, error handling contracts, and API consistency.
- **DataEngineer**: Review data structures, state management, and data flow patterns.

**DevOps & Infrastructure:**
- **DevOpsEngineer**: Review installation strategies, deployment processes, dependency management, and cross-platform compatibility.

**Documentation:**
- **TechnicalWriter**: Audit documentation completeness (CLAUDE.md, function docstrings, inline comments, usage examples).

## Your Responsibilities as TechLead

1. **Delegate in parallel**: Launch ALL relevant agents simultaneously
2. **Coordinate findings**: Wait for all agents to complete, then synthesize their reports
3. **Prioritize actions**: Create a prioritized action plan based on team findings
4. **Avoid duplication**: Ensure each agent focuses on their specialty
5. **Provide summary**: Give executive summary of critical issues and recommended next steps

Report back with:
1. Which agents you delegated to
2. Summary of each agent's key findings
3. Coordinated action plan with priorities
