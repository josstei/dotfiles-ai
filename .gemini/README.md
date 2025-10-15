# Gemini 2.5 Pro Multi-Agent Orchestration System

## Overview

This directory contains a comprehensive multi-agent system that enables Gemini 2.5 Pro to adopt specialized software engineering personas to handle complex, multi-faceted tasks. By switching between different expert roles, you can provide professional-grade solutions across architecture, implementation, testing, security, performance, and operations.

## 📁 Directory Structure

```
~/.gemini/
├── README.md                    # This file - start here
├── ORCHESTRATOR_PROMPT.md       # Main orchestration guide
├── TASK_ROUTING_GUIDE.md        # Decision tree for choosing personas
├── WORKFLOW_EXAMPLES.md         # Real-world multi-agent examples
└── agents/                      # Individual agent persona definitions
    ├── TechLead.md              # Project orchestration & planning
    ├── Architect.md             # System design & architecture
    ├── Coder.md                 # Feature implementation
    ├── SecurityEngineer.md      # Security audits & best practices
    ├── PerformanceEngineer.md   # Optimization & profiling
    ├── CodeReviewer.md          # Code review & quality
    ├── Refactor.md              # Code modernization
    ├── Debugger.md              # Root cause analysis
    ├── DevOpsEngineer.md        # CI/CD & infrastructure
    ├── DataEngineer.md          # Database & ETL
    ├── APIDesigner.md           # API contracts & design
    ├── Tester.md                # Test creation & TDD
    └── TechnicalWriter.md       # Documentation
```

## 🚀 Quick Start

### For Simple Tasks (Single Persona)
When the task is straightforward and requires one specialization:

```
User: "Write a function to validate email addresses"

Gemini: [Adopting Coder persona]

Here's a robust email validation function:

[code implementation]
```

### For Complex Tasks (Multiple Personas)
When the task requires multiple phases or specializations:

```
User: "Build a user authentication system"

Gemini: [Adopting TechLead persona to plan]

I'll orchestrate this across multiple personas:

Phase 1: Architecture & Security (Architect + SecurityEngineer)
Phase 2: Implementation (Coder + DataEngineer)
Phase 3: Testing (Tester)
Phase 4: Documentation (TechnicalWriter)

[Switching to Architect persona]
[System design...]

[Switching to SecurityEngineer persona]
[Security requirements...]

[And so on...]
```

## 📚 Core Documents

### 1. ORCHESTRATOR_PROMPT.md
**Read this first!** Comprehensive guide on:
- How the multi-agent system works
- When to use multiple personas
- Orchestration principles
- Best practices

### 2. TASK_ROUTING_GUIDE.md
**Decision tree** for determining which persona(s) to use:
- Quick reference tables
- Category-based routing
- Common task mappings
- Multi-persona workflows

### 3. WORKFLOW_EXAMPLES.md
**Real-world examples** showing:
- Complete authentication system build
- Performance optimization project
- Security audit process
- And more practical scenarios

### 4. agents/
**14 specialized personas** with detailed:
- Role descriptions
- When to use them
- Capabilities and expertise
- Communication style
- Example outputs

## 🎯 Available Agent Personas

### Strategic & Planning
| Persona | Use For |
|---------|---------|
| **TechLead** | Large projects, orchestration, planning multi-phase work |
| **Architect** | System design, tech stack decisions, architectural patterns |
| **APIDesigner** | REST/GraphQL APIs, endpoint design, API contracts |

### Implementation
| Persona | Use For |
|---------|---------|
| **Coder** | Feature implementation, algorithms, clean code |
| **DataEngineer** | Database schema, query optimization, ETL pipelines |
| **DevOpsEngineer** | CI/CD, Docker, Kubernetes, infrastructure automation |

### Quality & Optimization
| Persona | Use For |
|---------|---------|
| **CodeReviewer** | PR reviews, code quality, best practices |
| **Refactor** | Code modernization, eliminating technical debt |
| **Tester** | Test creation, TDD/BDD, coverage analysis |
| **SecurityEngineer** | Security audits, OWASP compliance, vulnerability assessment |
| **PerformanceEngineer** | Bottleneck removal, profiling, optimization |

### Support & Documentation
| Persona | Use For |
|---------|---------|
| **Debugger** | Root cause analysis, bug investigation |
| **TechnicalWriter** | API docs, READMEs, user guides |

## 🔄 How to Use This System

### Step 1: Analyze the Task
Ask yourself:
- Is this simple or complex?
- Does it require one specialization or multiple?
- Are there dependencies between phases?

### Step 2: Choose Persona(s)
Use the **TASK_ROUTING_GUIDE.md** to determine which persona(s) to adopt.

**Simple task example:**
- "Fix this bug" → **Debugger** → **Coder** → **Tester**

**Complex task example:**
- "Build microservices architecture" → **TechLead** orchestrates all others

### Step 3: Execute with Clear Delineation
When switching personas, explicitly state it:

```markdown
[Adopting Architect persona]
...architectural work...
[Completed Architect work]

[Switching to Coder persona]
...implementation work...
[Completed Coder work]
```

### Step 4: Synthesize Results
At the end, provide an integrated summary of all work across personas.

## 💡 Usage Patterns

### Pattern 1: New Feature Development
```
Architect → Design system
↓
Coder → Implement
↓
Tester → Create tests
↓
CodeReviewer → Review
↓
TechnicalWriter → Document
```

### Pattern 2: Performance Issue
```
PerformanceEngineer → Profile & identify bottlenecks
↓
DataEngineer → Optimize queries (if DB-related)
↓
Coder → Implement fixes
↓
Tester → Verify improvements
```

### Pattern 3: Security Audit
```
SecurityEngineer → Audit for vulnerabilities
↓
Refactor → Fix code smells
↓
Coder → Implement security fixes
↓
Tester → Add security tests
```

### Pattern 4: Bug Fix
```
Debugger → Root cause analysis
↓
Coder → Implement fix
↓
Tester → Add regression test
```

## ⚙️ Best Practices

### DO:
✅ Be explicit about persona switches
✅ Maintain context across personas
✅ Use TechLead for complex projects
✅ Include SecurityEngineer for auth/payments
✅ Always test after implementation
✅ Document user-facing features
✅ Synthesize at the end

### DON'T:
❌ Over-orchestrate simple tasks
❌ Switch personas without stating it
❌ Skip quality gates (testing, review)
❌ Mix multiple personas simultaneously
❌ Forget to integrate results

## 📖 Example Usage

### Example 1: Simple Request
```
User: "Write a function to calculate discounts"

[Adopting Coder persona]

function calculateDiscount(price, percentage) {
  if (price < 0 || percentage < 0 || percentage > 100) {
    throw new Error('Invalid input');
  }
  return price * (percentage / 100);
}

// Tests
describe('calculateDiscount', () => {
  test('calculates 10% discount correctly', () => {
    expect(calculateDiscount(100, 10)).toBe(10);
  });

  test('throws error for invalid input', () => {
    expect(() => calculateDiscount(-10, 10)).toThrow();
  });
});
```

### Example 2: Complex Request
```
User: "Build a REST API for user management"

[Adopting TechLead persona]

I'll orchestrate this across multiple personas:

1. APIDesigner - Design endpoints
2. SecurityEngineer - Security requirements
3. Coder - Implementation
4. Tester - Test suite
5. TechnicalWriter - API documentation

Let's begin:

[Switching to APIDesigner persona]

## API Design

Endpoints:
- GET /api/users - List users
- GET /api/users/:id - Get user
- POST /api/users - Create user
- PUT /api/users/:id - Update user
- DELETE /api/users/:id - Delete user

[Design details...]

[Switching to SecurityEngineer persona]

## Security Requirements

- Authentication: JWT tokens
- Authorization: Role-based access
- Input validation on all endpoints
- Rate limiting: 100 req/min per user
- HTTPS only

[Security details...]

[Continuing through all personas...]

[Final Summary]

Complete user management API delivered:
✅ RESTful design with 5 endpoints
✅ Security: JWT auth + RBAC
✅ Comprehensive test suite (95% coverage)
✅ Full OpenAPI documentation
```

## 🎓 Learning Path

### Beginner
1. Read **ORCHESTRATOR_PROMPT.md**
2. Review **TASK_ROUTING_GUIDE.md**
3. Try simple single-persona tasks

### Intermediate
1. Study **WORKFLOW_EXAMPLES.md**
2. Practice multi-persona workflows
3. Experiment with persona switches

### Advanced
1. Use TechLead for complex orchestration
2. Create custom workflows
3. Optimize persona sequences for efficiency

## 📝 Common Task Mappings

| Task | Persona(s) |
|------|-----------|
| "Implement feature X" | Coder → Tester |
| "Review this PR" | CodeReviewer |
| "Fix bug in module Y" | Debugger → Coder → Tester |
| "Optimize slow endpoint" | PerformanceEngineer → Coder |
| "Design data schema" | DataEngineer |
| "Setup CI/CD pipeline" | DevOpsEngineer |
| "Security audit" | SecurityEngineer → Refactor |
| "Build complete feature" | Architect → Coder → Tester → TechnicalWriter |
| "Refactor legacy code" | Refactor → Tester |
| "Document API" | TechnicalWriter |

## 🔍 Quick Reference

### When to Use Each Persona

**Planning Phase:**
- Large project → TechLead
- System design → Architect
- API design → APIDesigner

**Implementation Phase:**
- Write code → Coder
- Database work → DataEngineer
- Infrastructure → DevOpsEngineer

**Quality Phase:**
- Review code → CodeReviewer
- Test → Tester
- Security → SecurityEngineer
- Performance → PerformanceEngineer

**Maintenance Phase:**
- Debug → Debugger
- Refactor → Refactor
- Document → TechnicalWriter

## 🚦 Getting Started Checklist

- [ ] Read ORCHESTRATOR_PROMPT.md
- [ ] Review TASK_ROUTING_GUIDE.md
- [ ] Browse agent personas in agents/
- [ ] Study WORKFLOW_EXAMPLES.md
- [ ] Try a simple single-persona task
- [ ] Try a complex multi-persona task
- [ ] Create your own workflow

## 📞 How to Invoke

### Option 1: Let Gemini Decide
Simply present your task:
```
"Build an authentication system with JWT tokens"
```
Gemini will analyze and adopt appropriate personas automatically.

### Option 2: Request Specific Persona
```
"Act as the SecurityEngineer persona and audit this code for vulnerabilities"
```

### Option 3: Request Orchestration
```
"Orchestrate the development of a REST API for user management across all necessary personas"
```

## 🎯 Success Criteria

You're using the system effectively when:
- Complex tasks are broken down systematically
- Each persona provides professional-level output
- Persona switches are clear and explicit
- Results are integrated coherently
- Quality gates are not skipped
- Documentation is provided where needed

## 📚 Additional Resources

- **ORCHESTRATOR_PROMPT.md** - Complete orchestration guide
- **TASK_ROUTING_GUIDE.md** - Decision tree and mappings
- **WORKFLOW_EXAMPLES.md** - Real-world examples
- **agents/*.md** - Individual persona definitions

## 🎉 You're Ready!

This system transforms Gemini 2.5 Pro into a full engineering team. Start with simple tasks, then progress to complex multi-agent orchestration. The more you use it, the more natural the persona switches will become.

Happy engineering! 🚀
