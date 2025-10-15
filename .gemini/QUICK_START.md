# Quick Start Guide

## Get Started in 5 Minutes

### 1. Understand the Concept (1 minute)
Gemini 2.5 Pro can adopt 14 specialized software engineering personas:
- **TechLead**, Architect, APIDesigner (Planning)
- **Coder**, DataEngineer, DevOpsEngineer (Implementation)
- **CodeReviewer**, Tester, SecurityEngineer, PerformanceEngineer, Refactor (Quality)
- **Debugger**, TechnicalWriter (Support)

### 2. Try a Simple Task (2 minutes)
```
You: "Write a function to validate passwords"

Gemini: [Adopting Coder persona]

function validatePassword(password) {
  if (!password || password.length < 8) {
    return { valid: false, error: 'Password must be at least 8 characters' };
  }

  const hasUppercase = /[A-Z]/.test(password);
  const hasLowercase = /[a-z]/.test(password);
  const hasNumber = /\d/.test(password);
  const hasSpecial = /[!@#$%^&*]/.test(password);

  if (!hasUppercase || !hasLowercase || !hasNumber || !hasSpecial) {
    return {
      valid: false,
      error: 'Password must contain uppercase, lowercase, number, and special character'
    };
  }

  return { valid: true };
}
```

### 3. Try a Complex Task (2 minutes)
```
You: "Build a rate limiting middleware for an Express API"

Gemini: [Adopting TechLead persona to plan]

I'll coordinate across multiple personas:
1. Architect - Design rate limiting strategy
2. Coder - Implement middleware
3. SecurityEngineer - Review security implications
4. Tester - Create test suite

[Switching to Architect persona]

## Rate Limiting Design

Strategy: Token bucket algorithm with Redis storage
- Limits: 100 requests per 15 minutes per IP
- 429 response when exceeded
- Rate limit headers in response

[Switching to Coder persona]

## Implementation

[code...]

[Switching to SecurityEngineer persona]

## Security Review

[security analysis...]

[Switching to Tester persona]

## Test Suite

[tests...]

[Final Summary]
Complete rate limiting system with:
✅ Token bucket algorithm
✅ Redis-backed storage
✅ Security reviewed
✅ Comprehensive tests
```

## Next Steps

### Learn More
- Read **ORCHESTRATOR_PROMPT.md** for full system guide
- Check **TASK_ROUTING_GUIDE.md** to know which persona to use
- Study **WORKFLOW_EXAMPLES.md** for real-world examples

### Practice
1. Start with single-persona tasks
2. Progress to multi-persona workflows
3. Use TechLead for complex orchestration

### Tips
- Be specific in your requests
- Let Gemini choose personas for you
- Or request specific personas: "Act as SecurityEngineer and..."

## Common Patterns

### Single Persona
```
"Fix this bug" → Debugger → Coder → Tester
"Review this code" → CodeReviewer
"Document this API" → TechnicalWriter
```

### Multiple Personas
```
"Build feature X" → Architect → Coder → Tester → TechnicalWriter
"Optimize performance" → PerformanceEngineer → Coder → Tester
"Security audit" → SecurityEngineer → Refactor → Tester
```

## You're Ready! 🚀

Just start asking Gemini to help with your software engineering tasks. The system will automatically adopt the right personas to deliver professional-grade solutions.
