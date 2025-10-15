# Gemini CLI Multi-Agent System

Specialized agent personas for Gemini CLI to handle complex software engineering tasks.

## Overview

This directory contains a multi-agent orchestration system that enables Gemini CLI to adopt 13 specialized engineering personas:

### Strategic & Planning
- **TechLead** - Large projects, orchestration, planning multi-phase work
- **Architect** - System design, tech stack decisions, architectural patterns
- **APIDesigner** - REST/GraphQL APIs, endpoint design, API contracts

### Implementation
- **Coder** - Feature implementation, algorithms, clean code
- **DataEngineer** - Database schema, query optimization, ETL pipelines
- **DevOpsEngineer** - CI/CD, Docker, Kubernetes, infrastructure automation

### Quality & Optimization
- **CodeReviewer** - PR reviews, code quality, best practices
- **Refactor** - Code modernization, eliminating technical debt
- **Tester** - Test creation, TDD/BDD, coverage analysis
- **SecurityEngineer** - Security audits, OWASP compliance, vulnerability assessment
- **PerformanceEngineer** - Bottleneck removal, profiling, optimization

### Support & Documentation
- **Debugger** - Root cause analysis, bug investigation
- **TechnicalWriter** - API docs, READMEs, user guides

## Quick Start

### 1. Install
Copy this directory to your project:
```bash
cp -r .gemini ~/your-project/
```

### 2. Initialize
In Gemini CLI, load the agent system once:
```
load agents
```

### 3. Use
All subsequent prompts will automatically use the multi-agent orchestration system:

```
"Build a user authentication system"
```

Gemini will adopt the appropriate personas (Architect → SecurityEngineer → Coder → Tester) and switch between them as needed.

## How It Works

Once initialized with `load agents`, Gemini will:

1. **Analyze your request** to determine which personas are needed
2. **Adopt personas explicitly** - You'll see `[Adopting {Persona} persona]`
3. **Execute with expertise** - Each persona brings specialized knowledge
4. **Switch when needed** - `[Switching to {Persona} persona]`
5. **Synthesize results** - Integrate work from multiple personas

## Configuration

The system is configured via:
- **GEMINI.md** - Core operating instructions and `load agents` command
- **ORCHESTRATOR_PROMPT.md** - Orchestration framework and principles
- **agents/*.md** - Individual persona definitions

## Usage Examples

### Simple Task (Single Persona)
```
"Write a function to validate email addresses"

Gemini: [Adopting Coder persona]
[implementation...]
```

### Complex Task (Multi-Persona)
```
"Optimize the database queries in this API"

Gemini: [Adopting PerformanceEngineer persona]
[profiling and analysis...]

[Switching to DataEngineer persona]
[query optimization...]

[Switching to Tester persona]
[performance tests...]
```

## Additional Resources

- **GEMINI.md** - Configuration and `load agents` command details
- **QUICK_START.md** - 5-minute tutorial
- **ORCHESTRATOR_PROMPT.md** - Deep dive on orchestration principles
- **TASK_ROUTING_GUIDE.md** - Which persona to use when
- **WORKFLOW_EXAMPLES.md** - Real-world multi-agent examples
- **INDEX.md** - Complete documentation index

## Customization

Edit the agent files in `agents/` to customize persona behavior, expertise, or communication style for your project needs.
