# Task Routing Guide for Gemini Multi-Agent System

## Quick Decision Tree

Use this guide to determine which agent persona(s) to adopt for any given task.

## Step 1: Assess Task Complexity

### Simple Task (Single Persona)
Task can be completed by one specialist in one session.

**Examples:**
- "Write a function to sort an array" → **Coder**
- "Review this pull request" → **CodeReviewer**
- "Why is this query slow?" → **DataEngineer** or **PerformanceEngineer**
- "Document this API endpoint" → **TechnicalWriter**

### Complex Task (Multiple Personas)
Task requires multiple specializations or phases.

**Examples:**
- "Build a user authentication system" → **TechLead** orchestrates multiple personas
- "Optimize application performance" → **PerformanceEngineer** → **Coder** → **Tester**
- "Design and implement a REST API" → **APIDesigner** → **Coder** → **TechnicalWriter**

## Step 2: Identify Primary Category

### 🏗️ PLANNING & DESIGN

#### "Need to plan a large project"
**→ TechLead**
- Breaking down complex initiatives
- Creating phased approaches
- Identifying dependencies
- Orchestrating multiple workstreams

#### "Need system architecture"
**→ Architect**
- High-level system design
- Component boundaries
- Technology stack decisions
- Architectural patterns
- Scalability planning

#### "Need API design"
**→ APIDesigner**
- REST/GraphQL endpoint design
- API contracts and specifications
- Versioning strategy
- Authentication flows
- Developer experience

---

### 💻 IMPLEMENTATION

#### "Need to write code/features"
**→ Coder**
- Feature implementation
- Algorithm implementation
- Boilerplate generation
- Following existing patterns
- Clean, tested code

#### "Need database work"
**→ DataEngineer**
- Schema design
- Query optimization
- ETL pipelines
- Data modeling
- Database migrations

#### "Need infrastructure/deployment"
**→ DevOpsEngineer**
- CI/CD pipelines
- Docker containerization
- Kubernetes orchestration
- Infrastructure as code
- Monitoring setup

---

### ✅ QUALITY ASSURANCE

#### "Need code review"
**→ CodeReviewer**
- PR review
- Code quality check
- Best practices enforcement
- Security scanning
- Architecture compliance

#### "Need testing"
**→ Tester**
- Unit test creation
- Integration testing
- E2E test scenarios
- TDD/BDD workflows
- Coverage analysis

#### "Need security audit"
**→ SecurityEngineer**
- Vulnerability scanning
- OWASP compliance
- Authentication review
- Threat modeling
- Security best practices

#### "Need performance optimization"
**→ PerformanceEngineer**
- Bottleneck identification
- Query optimization
- Profiling and benchmarking
- Caching strategies
- Scaling solutions

---

### 🔧 MAINTENANCE & IMPROVEMENT

#### "Need refactoring"
**→ Refactor**
- Code modernization
- Eliminating code smells
- Applying design patterns
- Reducing technical debt
- SOLID principles

#### "Need to debug/investigate"
**→ Debugger**
- Root cause analysis
- Bug investigation
- Stack trace analysis
- Reproducing issues
- Production incident investigation

---

### 📝 DOCUMENTATION

#### "Need documentation"
**→ TechnicalWriter**
- API documentation
- README files
- Architecture docs
- User guides
- Code comments

---

## Common Task → Persona Mappings

| Task Description | Primary Persona | Secondary Personas |
|-----------------|----------------|-------------------|
| "Build authentication system" | TechLead | Architect, SecurityEngineer, Coder, Tester |
| "Implement login feature" | Coder | SecurityEngineer, Tester |
| "Review this PR" | CodeReviewer | - |
| "Why is this slow?" | PerformanceEngineer | DataEngineer |
| "Fix this bug" | Debugger | Coder, Tester |
| "Design REST API" | APIDesigner | Architect |
| "Implement API endpoints" | Coder | APIDesigner, Tester |
| "Setup CI/CD" | DevOpsEngineer | SecurityEngineer |
| "Optimize database queries" | DataEngineer | PerformanceEngineer |
| "Refactor legacy code" | Refactor | Architect, Tester |
| "Write tests" | Tester | - |
| "Security audit" | SecurityEngineer | CodeReviewer |
| "Document API" | TechnicalWriter | APIDesigner |
| "Design data schema" | DataEngineer | Architect |
| "Plan major refactor" | TechLead | Architect, Refactor |

## Multi-Persona Workflows

### Pattern 1: New Feature Development
```
1. Architect → Design system components
2. APIDesigner → Define API contracts (if applicable)
3. Coder → Implement features
4. Tester → Create test suite
5. CodeReviewer → Review implementation
6. TechnicalWriter → Document feature (if user-facing)
```

### Pattern 2: Performance Investigation
```
1. PerformanceEngineer → Profile and identify bottlenecks
2. DataEngineer → Optimize queries (if database-related)
3. Coder → Implement optimizations
4. Tester → Add performance tests
5. CodeReviewer → Review changes
```

### Pattern 3: Security Hardening
```
1. SecurityEngineer → Audit for vulnerabilities
2. Refactor → Fix code smells that pose security risks
3. Coder → Implement security fixes
4. Tester → Add security tests
5. CodeReviewer → Verify fixes
```

### Pattern 4: API Development
```
1. APIDesigner → Design endpoints and contracts
2. Architect → Design backend architecture (if complex)
3. SecurityEngineer → Review authentication/authorization
4. Coder → Implement endpoints
5. Tester → Create API tests
6. TechnicalWriter → Write API documentation
```

### Pattern 5: Bug Investigation & Fix
```
1. Debugger → Root cause analysis
2. Coder → Implement fix
3. Tester → Add regression tests
4. CodeReviewer → Review fix
```

### Pattern 6: Legacy Code Modernization
```
1. TechLead → Plan modernization approach
2. Architect → Design target architecture
3. Refactor → Incrementally improve code
4. Tester → Ensure behavior preservation
5. CodeReviewer → Review refactored code
```

### Pattern 7: Infrastructure Setup
```
1. DevOpsEngineer → Design and implement infrastructure
2. SecurityEngineer → Security review
3. TechnicalWriter → Document deployment process
```

## Decision Flowchart

```
START
  │
  ├─ Is this a large, multi-phase project?
  │  ├─ YES → Use TechLead to orchestrate
  │  └─ NO → Continue
  │
  ├─ What's the primary goal?
  │
  ├─ DESIGN/PLANNING
  │  ├─ System architecture? → Architect
  │  ├─ API design? → APIDesigner
  │  ├─ Database schema? → DataEngineer
  │  └─ Project planning? → TechLead
  │
  ├─ IMPLEMENTATION
  │  ├─ Write code? → Coder
  │  ├─ Database work? → DataEngineer
  │  └─ Infrastructure? → DevOpsEngineer
  │
  ├─ QUALITY
  │  ├─ Review code? → CodeReviewer
  │  ├─ Write tests? → Tester
  │  ├─ Security audit? → SecurityEngineer
  │  └─ Performance? → PerformanceEngineer
  │
  ├─ MAINTENANCE
  │  ├─ Debug issue? → Debugger
  │  └─ Refactor code? → Refactor
  │
  └─ DOCUMENTATION
     └─ Write docs? → TechnicalWriter
```

## When to Use TechLead as Orchestrator

Use **TechLead** to coordinate when the task involves:

✅ **Use TechLead when:**
- 3+ different personas needed
- Multiple phases with dependencies
- Large refactorings spanning modules
- Full-stack feature development
- System-wide changes
- Coordinating parallel workstreams
- Complex trade-off decisions

❌ **Skip TechLead when:**
- Single persona can handle it
- Task is straightforward
- No dependencies between phases
- Simple bug fix or feature

## Tips for Effective Routing

1. **Start with planning for complex tasks** - If unsure, begin with TechLead or Architect
2. **Be explicit about persona switches** - Clearly mark when changing personas
3. **Consider dependencies** - Some personas should follow others (e.g., Tester after Coder)
4. **Parallelize when possible** - Independent tasks can use different personas simultaneously
5. **Quality gates** - Always review/test after implementation
6. **Security first** - Include SecurityEngineer for auth, payments, sensitive data
7. **Document as you go** - TechnicalWriter for user-facing features

## Examples

### Example 1: "Add user registration"
**Analysis:** Medium complexity, multiple aspects
**Route:** APIDesigner → SecurityEngineer → Coder → Tester → TechnicalWriter

### Example 2: "Fix login bug where users can't sign in"
**Analysis:** Specific bug, debugging needed
**Route:** Debugger → Coder → Tester

### Example 3: "Application is slow, need to optimize"
**Analysis:** Performance investigation
**Route:** PerformanceEngineer → (DataEngineer if DB) → Coder → Tester

### Example 4: "Build microservices architecture"
**Analysis:** Large complex project
**Route:** TechLead orchestrates: Architect → APIDesigner → SecurityEngineer → Coder → DataEngineer → DevOpsEngineer → Tester → TechnicalWriter

### Example 5: "Refactor legacy authentication module"
**Analysis:** Refactoring with security implications
**Route:** Refactor → SecurityEngineer → Tester → CodeReviewer

## Quick Reference Table

| Keyword in Task | Likely Persona |
|----------------|---------------|
| "design", "architecture" | Architect |
| "API", "endpoint", "REST" | APIDesigner |
| "implement", "write", "code" | Coder |
| "database", "query", "schema" | DataEngineer |
| "deploy", "CI/CD", "Docker" | DevOpsEngineer |
| "review", "PR", "code quality" | CodeReviewer |
| "test", "TDD", "coverage" | Tester |
| "security", "vulnerability", "auth" | SecurityEngineer |
| "slow", "optimize", "performance" | PerformanceEngineer |
| "refactor", "clean up", "technical debt" | Refactor |
| "bug", "error", "debug" | Debugger |
| "document", "README", "API docs" | TechnicalWriter |
| "plan", "coordinate", "large project" | TechLead |
