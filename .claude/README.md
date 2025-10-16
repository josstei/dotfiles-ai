# Claude Code Agent System

Specialized agent personas for Claude Code to handle different software engineering tasks.

## Overview

This directory contains 13 specialized agent configurations that Claude Code can invoke:

### Strategic & Planning
- **TechLead** - Large refactorings, technical planning, multi-agent coordination
- **Architect** - System design, architecture patterns, tech stack decisions
- **APIDesigner** - REST/GraphQL API design, endpoint contracts

### Implementation
- **Coder** - Feature implementation, algorithms, clean code
- **DataEngineer** - Database schema, ETL pipelines, query optimization
- **DevOpsEngineer** - CI/CD, containerization, infrastructure automation

### Quality & Optimization
- **CodeReviewer** - PR reviews, code quality, best practices
- **Refactor** - Code modernization, technical debt reduction
- **Tester** - Test creation, TDD/BDD, coverage analysis
- **SecurityEngineer** - Security audits, vulnerability assessment
- **PerformanceEngineer** - Optimization, profiling, bottleneck removal

### Support & Documentation
- **Debugger** - Bug investigation, root cause analysis
- **TechnicalWriter** - API docs, READMEs, user guides

## How It Works

### Automatic Agent Selection
Claude Code automatically loads and uses these agents based on your requests:

- Ask to implement a feature → **Coder** agent is used
- Ask to review code → **CodeReviewer** agent is used
- Ask to fix a bug → **Debugger** then **Coder** agents are used
- Complex tasks → **TechLead** orchestrates multiple agents

### Direct Agent Invocation
You can call a specific agent directly using `@agent-` syntax:

```
@agent-TechLead plan the refactoring of the authentication system

@agent-SecurityEngineer audit this code for vulnerabilities

@agent-PerformanceEngineer analyze the bottlenecks in this function

@agent-Architect design a microservices architecture for this feature
```

### Slash Commands

The `/orch` command launches a comprehensive engineering team review with TechLead orchestrating multiple specialized agents in parallel:

```
/orch review the authentication system for security issues and performance bottlenecks

/orch analyze the API layer for design improvements

/orch comprehensive review of the data processing pipeline
```

This command delegates to all relevant agents (Architect, CodeReviewer, SecurityEngineer, PerformanceEngineer, Tester, etc.) simultaneously and synthesizes their findings into a coordinated action plan.

## Installation

Copy this directory to your project:
```bash
cp -r .claude ~/your-project/
```

Claude Code will automatically detect and use these agent configurations.

## Agent Files

Each agent is defined in `agents/*.md` with:
- Agent name and description
- Use cases
- Specialized capabilities
- Orchestration approach (for TechLead)

## Customization

Feel free to edit the agent files in `agents/` to customize their behavior, expertise, or communication style for your project needs.
