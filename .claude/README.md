# Claude Code Agent Orchestration

A complete engineering operations kit for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) — specialized agents, custom slash commands, and strict architectural standards that transform Claude into a disciplined, multi-agent development team.

## Repository Structure

```
.claude/
├── CLAUDE.md              # Project instructions, coding standards, execution methodology
├── agents/                # 13 specialized engineering agent personas
│   ├── APIDesigner.md
│   ├── Architect.md
│   ├── CodeReviewer.md
│   ├── Coder.md
│   ├── DataEngineer.md
│   ├── Debugger.md
│   ├── DevOpsEngineer.md
│   ├── PerformanceEngineer.md
│   ├── Refactor.md
│   ├── SecurityEngineer.md
│   ├── TechLead.md
│   ├── TechnicalWriter.md
│   └── Tester.md
└── commands/              # Custom slash commands with dual-validation workflows
    ├── arch-analysis.md
    ├── cleanup.md
    ├── code-review.md
    └── codex-review.md
```

## Agents

Thirteen specialized agent personas organized by engineering discipline:

| Category | Agent | Purpose |
|----------|-------|---------|
| **Planning** | TechLead | Project orchestration, task delegation, multi-agent coordination |
| | Architect | System design, microservices, DDD, tech stack decisions |
| | APIDesigner | REST/GraphQL API design, OpenAPI specs, versioning strategies |
| **Implementation** | Coder | Feature implementation, clean tested code, idiomatic patterns |
| | DataEngineer | Schema design, query optimization, ETL pipelines |
| | DevOpsEngineer | CI/CD pipelines, containerization, Kubernetes, IaC |
| **Quality** | CodeReviewer | PR reviews, code quality, security checks, best practices |
| | Refactor | Code modernization, design patterns, SOLID principles, debt reduction |
| | Tester | Unit/integration testing, coverage analysis, TDD/BDD |
| | SecurityEngineer | Security audits, OWASP compliance, vulnerability assessment |
| | PerformanceEngineer | Profiling, bottleneck removal, scaling improvements |
| **Support** | Debugger | Root cause analysis, systematic debugging, stack trace analysis |
| | TechnicalWriter | API documentation, architecture diagrams, technical guides |

Claude Code automatically selects the appropriate agent based on the task context. The **TechLead** agent serves as the orchestrator, breaking complex work into subtasks and delegating to specialist agents.

## Custom Slash Commands

### `/code-review`

Deep code review with a 7-phase protocol emphasizing **accuracy over volume**. Establishes ground truth via the test suite, traces execution paths, validates every finding, and runs dual parallel validation (Codex CLI + CodeReviewer) before reporting. Only verified issues make the final report.

### `/arch-analysis`

Architecture conformance analysis across 5 phases. Identifies directory structure violations, naming inconsistencies, pattern compliance failures, circular dependencies, and misplaced code. Dual-validated to minimize false positives.

### `/cleanup`

Dead code analysis and safe removal planning across 6 phases. Discovers unused exports, orphaned files, dead dependencies, and legacy patterns. Includes confidence-scored findings and regression test planning before any removal.

### `/codex-review`

Lightweight validation workflow using Codex CLI for quick finding verification.

## CLAUDE.md Standards

The `CLAUDE.md` file enforces project-wide engineering standards:

- **Future-First Design** — interfaces, abstractions, and loose coupling by default
- **Strict Contracts** — rigid typing, explicit interfaces, no shortcuts
- **Clean Separation** — single responsibility at every level (file, method, module)
- **Execution Planning Methodology** — a 7-step framework for complex multi-phase tasks covering dependency analysis, risk classification, parallelization, agent selection, and validation checkpoints

## Installation

Copy the `.claude/` directory into any project:

```bash
cp -r .claude/ ~/your-project/.claude/
```

Claude Code automatically detects and applies the configuration when working inside the project directory.

## Key Design Patterns

### Dual Validation

Critical commands run two independent validators in parallel — Codex CLI and the CodeReviewer agent — then cross-reference results. Only findings confirmed by both validators are reported, dramatically reducing false positives.

### Multi-Phase Analysis

Complex workflows decompose into discrete phases: **discovery** → **analysis** → **validation** → **reporting** → **cleanup**. Each phase has explicit entry/exit criteria and produces auditable intermediate artifacts.

### Agent Specialization

Each agent definition includes specific use cases, domain expertise, methodologies, and behavioral constraints. This prevents role confusion and ensures consistent, high-quality output regardless of task complexity.

## License

MIT — See [LICENSE](../LICENSE)
