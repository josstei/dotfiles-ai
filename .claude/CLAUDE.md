# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This directory contains a specialized agent system for Claude Code with 13 engineering personas that handle different software engineering tasks. The agents are automatically invoked based on task requirements or can be called directly using `@agent-{Name}` syntax.

## Directory Structure

```
.claude/
├── agents/           # 13 specialized agent persona definitions
├── commands/         # Slash commands (e.g., /orch)
└── README.md         # Agent system documentation
```

## The 13 Specialized Agents

**Strategic & Planning:**
- `TechLead` - Orchestrates multi-agent workflows, manages large refactorings, delegates to specialists (uses opus model)
- `Architect` - System design, architecture patterns, tech stack decisions (uses opus model)
- `APIDesigner` - REST/GraphQL API design, endpoint contracts (uses opus model)

**Implementation:**
- `Coder` - Feature implementation, algorithms, clean code (uses sonnet model)
- `DataEngineer` - Database schema, ETL pipelines, query optimization (uses sonnet model)
- `DevOpsEngineer` - CI/CD, containerization, infrastructure automation (uses sonnet model)

**Quality & Optimization:**
- `CodeReviewer` - PR reviews, code quality, best practices (uses sonnet model)
- `Refactor` - Code modernization, technical debt reduction (uses opus model)
- `Tester` - Test creation, TDD/BDD, coverage analysis (uses sonnet model)
- `SecurityEngineer` - Security audits, vulnerability assessment (uses opus model)
- `PerformanceEngineer` - Optimization, profiling, bottleneck removal (uses opus model)

**Support & Documentation:**
- `Debugger` - Bug investigation, root cause analysis (uses opus model)
- `TechnicalWriter` - API docs, READMEs, user guides (uses sonnet model)

## Agent File Format

Each agent is defined in `agents/{AgentName}.md` with YAML frontmatter:

```yaml
---
name: AgentName
description: **Use cases:**\n- Use case 1\n- Use case 2
model: opus|sonnet
---
```

Followed by the persona definition describing the agent's approach, philosophy, and expertise.

## Usage Patterns

### Automatic Agent Selection
Claude Code automatically loads agents based on requests:
- "Implement a feature" → Coder agent
- "Review this code" → CodeReviewer agent
- "Fix this bug" → Debugger then Coder agents
- Complex tasks → TechLead orchestrates multiple agents

### Direct Agent Invocation
Users can explicitly call agents:
```
@agent-TechLead plan the refactoring of the authentication system
@agent-SecurityEngineer audit this code for vulnerabilities
@agent-PerformanceEngineer analyze bottlenecks in this function
```

### Slash Commands

The `/orch` command demonstrates TechLead orchestration by launching multiple specialized agents in parallel to perform comprehensive codebase review and creating a coordinated action plan.

## Key Workflows

### Adding a New Agent

1. Create `agents/{AgentName}.md`
2. Add YAML frontmatter with name, description (use cases format), and model
3. Write persona definition following existing agent patterns
4. Update README.md to reference the new agent

### Modifying Existing Agents

Edit the agent file in `agents/` to adjust:
- Persona characteristics and communication style
- Technical approach and methodology
- Specialized capabilities and expertise areas
- Model selection (opus for complex reasoning, sonnet for straightforward tasks)

### Creating Slash Commands

Create `commands/{name}.md` with:
- YAML frontmatter including `description` field
- Command prompt that delegates to agents using Task tool
- For parallel delegation: "Launch ALL agents simultaneously in a single message with multiple Task tool calls"
