# AI Agent Orchestration for Developers

Specialized agent personas for **Claude Code** and **Gemini CLI**.

## Overview

13 specialized engineering personas that AI assistants can adopt:

**Planning:** TechLead, Architect, APIDesigner
**Implementation:** Coder, DataEngineer, DevOpsEngineer
**Quality:** CodeReviewer, Refactor, Tester, SecurityEngineer, PerformanceEngineer
**Support:** Debugger, TechnicalWriter

## Quick Start

### Claude Code
```bash
cp -r .claude ~/your-project/
```
Claude Code automatically uses agents when working in your project.

### Gemini CLI
```bash
cp -r .gemini ~/your-project/
```

Then in Gemini CLI, initialize the multi-agent system once:
```
load agents
```

All subsequent prompts will use the agent orchestration system defined in `.gemini/GEMINI.md`.

## Documentation

- **Claude Code:** [.claude/README.md](.claude/README.md)
- **Gemini CLI:** [.gemini/GEMINI.md](.gemini/GEMINI.md) | [README](.gemini/README.md)

## License

MIT - See [LICENSE](LICENSE)
