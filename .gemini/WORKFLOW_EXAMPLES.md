# Multi-Agent Workflow Examples

This document provides real-world examples of how Gemini 2.5 Pro can orchestrate multiple agent personas to complete complex software engineering tasks.

---

## Example 1: Building User Authentication System

### Task
"Build a complete user authentication system with JWT tokens, password reset, and email verification."

### Orchestration Approach

```markdown
[Adopting TechLead persona for project planning]

## Authentication System - Project Plan

### Phase 1: Architecture & Security Design (Est: 2-3 hours)
- Architect: System design and component architecture
- SecurityEngineer: Security requirements and threat model
- APIDesigner: API endpoint contracts

### Phase 2: Implementation (Est: 6-8 hours)
- DataEngineer: User database schema
- Coder: Auth service implementation
- Coder: Email service integration

### Phase 3: Testing (Est: 3-4 hours)
- Tester: Unit and integration tests
- SecurityEngineer: Security testing

### Phase 4: Documentation & Deployment (Est: 2 hours)
- TechnicalWriter: API documentation
- DevOpsEngineer: Deployment configuration

---

[Switching to Architect persona]

## System Architecture

### Components

1. **Auth Service** (Node.js/Express)
   - Handles registration, login, logout
   - JWT token generation and validation
   - Password reset flow
   - Email verification flow

2. **Database** (PostgreSQL)
   - Users table
   - Refresh tokens table
   - Password reset tokens table
   - Email verification tokens table

3. **Email Service**
   - Integration with SendGrid/AWS SES
   - Verification and reset email templates

### Architecture Diagram

```
Client (Web/Mobile)
       │
       │ HTTPS
       ▼
┌──────────────────┐
│   Auth Service   │
│    (Express)     │
└────────┬─────────┘
         │
    ┌────┴────┐
    │         │
    ▼         ▼
┌────────┐ ┌──────────┐
│  PG DB │ │  Email   │
│        │ │ Service  │
└────────┘ └──────────┘
```

### Data Flow
1. User registers → Create user → Send verification email
2. User clicks verification link → Verify token → Activate account
3. User logs in → Validate credentials → Generate JWT + refresh token
4. User requests password reset → Generate token → Send reset email
5. User resets password → Validate token → Update password

---

[Switching to SecurityEngineer persona]

## Security Requirements

### Authentication
- **Password Storage**: bcrypt with minimum 10 rounds
- **Password Requirements**:
  - Minimum 8 characters
  - Mix of uppercase, lowercase, numbers, special chars
- **JWT**: RS256 algorithm with short-lived tokens (15 min)
- **Refresh Tokens**: Stored securely, longer-lived (7 days)

### Token Security
- Access tokens: Short-lived (15 minutes)
- Refresh tokens: Rotate on use, family detection
- Reset tokens: Single-use, expire after 1 hour
- Verification tokens: Single-use, expire after 24 hours

### Rate Limiting
- Login attempts: 5 per 15 minutes per IP
- Registration: 3 per hour per IP
- Password reset requests: 3 per hour per email

### API Security
- HTTPS only
- CORS configured
- Helmet middleware for security headers
- Input validation on all endpoints
- SQL injection prevention (parameterized queries)

---

[Switching to APIDesigner persona]

## API Design

### Endpoints

#### POST /api/auth/register
Register new user account

**Request:**
```json
{
  "email": "user@example.com",
  "password": "SecurePass123!",
  "name": "John Doe"
}
```

**Response: 201 Created**
```json
{
  "message": "Registration successful. Please check your email to verify your account.",
  "userId": "usr_123abc"
}
```

#### POST /api/auth/verify-email
Verify email address

**Request:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

**Response: 200 OK**
```json
{
  "message": "Email verified successfully",
  "user": {
    "id": "usr_123abc",
    "email": "user@example.com",
    "emailVerified": true
  }
}
```

#### POST /api/auth/login
Authenticate user

**Request:**
```json
{
  "email": "user@example.com",
  "password": "SecurePass123!"
}
```

**Response: 200 OK**
```json
{
  "accessToken": "eyJhbGciOiJSUzI1NiIs...",
  "refreshToken": "eyJhbGciOiJIUzI1NiIs...",
  "user": {
    "id": "usr_123abc",
    "email": "user@example.com",
    "name": "John Doe"
  }
}
```

#### POST /api/auth/refresh
Refresh access token

**Request:**
```json
{
  "refreshToken": "eyJhbGciOiJIUzI1NiIs..."
}
```

**Response: 200 OK**
```json
{
  "accessToken": "eyJhbGciOiJSUzI1NiIs...",
  "refreshToken": "eyJhbGciOiJIUzI1NiIs..."
}
```

#### POST /api/auth/forgot-password
Request password reset

**Request:**
```json
{
  "email": "user@example.com"
}
```

**Response: 200 OK**
```json
{
  "message": "If that email exists, a password reset link has been sent."
}
```

#### POST /api/auth/reset-password
Reset password with token

**Request:**
```json
{
  "token": "reset_token_abc123",
  "newPassword": "NewSecurePass456!"
}
```

**Response: 200 OK**
```json
{
  "message": "Password reset successful"
}
```

---

[Switching to DataEngineer persona]

## Database Schema

```sql
-- Users table
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    name VARCHAR(255) NOT NULL,
    email_verified BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_users_email ON users(email);

-- Refresh tokens table
CREATE TABLE refresh_tokens (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    token_hash VARCHAR(255) NOT NULL,
    expires_at TIMESTAMP NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    revoked BOOLEAN DEFAULT FALSE
);

CREATE INDEX idx_refresh_tokens_user_id ON refresh_tokens(user_id);
CREATE INDEX idx_refresh_tokens_expires_at ON refresh_tokens(expires_at);

-- Email verification tokens
CREATE TABLE email_verification_tokens (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    token VARCHAR(255) UNIQUE NOT NULL,
    expires_at TIMESTAMP NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    used BOOLEAN DEFAULT FALSE
);

CREATE INDEX idx_verification_tokens_token ON email_verification_tokens(token);

-- Password reset tokens
CREATE TABLE password_reset_tokens (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    token VARCHAR(255) UNIQUE NOT NULL,
    expires_at TIMESTAMP NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    used BOOLEAN DEFAULT FALSE
);

CREATE INDEX idx_reset_tokens_token ON password_reset_tokens(token);
```

---

[Switching to Coder persona]

## Implementation Highlights

### User Registration
```javascript
const bcrypt = require('bcrypt');
const crypto = require('crypto');

async function registerUser(email, password, name) {
  // Validate input
  if (!email || !password || !name) {
    throw new ValidationError('All fields are required');
  }

  if (!isValidEmail(email)) {
    throw new ValidationError('Invalid email format');
  }

  if (!isStrongPassword(password)) {
    throw new ValidationError('Password does not meet requirements');
  }

  // Check if user exists
  const existingUser = await db.query(
    'SELECT id FROM users WHERE email = $1',
    [email]
  );

  if (existingUser.rows.length > 0) {
    throw new ConflictError('User with this email already exists');
  }

  // Hash password
  const passwordHash = await bcrypt.hash(password, 10);

  // Create user
  const result = await db.query(
    `INSERT INTO users (email, password_hash, name, email_verified)
     VALUES ($1, $2, $3, false)
     RETURNING id, email, name, created_at`,
    [email, passwordHash, name]
  );

  const user = result.rows[0];

  // Generate verification token
  const verificationToken = crypto.randomBytes(32).toString('hex');
  await db.query(
    `INSERT INTO email_verification_tokens (user_id, token, expires_at)
     VALUES ($1, $2, NOW() + INTERVAL '24 hours')`,
    [user.id, verificationToken]
  );

  // Send verification email
  await emailService.sendVerificationEmail(email, verificationToken);

  return { userId: user.id };
}
```

[Complete implementation would continue for all endpoints...]

---

[Switching to Tester persona]

## Test Suite

```javascript
const request = require('supertest');
const app = require('../app');
const db = require('../db');

describe('Auth API', () => {
  beforeEach(async () => {
    await db.clearTables();
  });

  describe('POST /api/auth/register', () => {
    test('successfully registers new user', async () => {
      const userData = {
        email: 'test@example.com',
        password: 'SecurePass123!',
        name: 'Test User'
      };

      const response = await request(app)
        .post('/api/auth/register')
        .send(userData)
        .expect(201);

      expect(response.body).toHaveProperty('userId');
      expect(response.body.message).toContain('check your email');

      // Verify user in database
      const user = await db.getUserByEmail(userData.email);
      expect(user).toBeDefined();
      expect(user.emailVerified).toBe(false);
    });

    test('rejects registration with weak password', async () => {
      const userData = {
        email: 'test@example.com',
        password: 'weak',
        name: 'Test User'
      };

      await request(app)
        .post('/api/auth/register')
        .send(userData)
        .expect(400);
    });

    test('rejects duplicate email', async () => {
      const userData = {
        email: 'test@example.com',
        password: 'SecurePass123!',
        name: 'Test User'
      };

      // Register first time
      await request(app)
        .post('/api/auth/register')
        .send(userData)
        .expect(201);

      // Try to register again
      await request(app)
        .post('/api/auth/register')
        .send(userData)
        .expect(409);
    });
  });

  describe('POST /api/auth/login', () => {
    beforeEach(async () => {
      // Create verified user
      await db.createUser({
        email: 'verified@example.com',
        password: 'SecurePass123!',
        emailVerified: true
      });
    });

    test('successfully logs in verified user', async () => {
      const response = await request(app)
        .post('/api/auth/login')
        .send({
          email: 'verified@example.com',
          password: 'SecurePass123!'
        })
        .expect(200);

      expect(response.body).toHaveProperty('accessToken');
      expect(response.body).toHaveProperty('refreshToken');
      expect(response.body.user.email).toBe('verified@example.com');
    });

    test('rejects login with wrong password', async () => {
      await request(app)
        .post('/api/auth/login')
        .send({
          email: 'verified@example.com',
          password: 'WrongPassword'
        })
        .expect(401);
    });
  });
});
```

---

[Switching to TechnicalWriter persona]

## API Documentation

### Authentication API

Complete API documentation with examples, error codes, and integration guides.

[Full API documentation would be generated...]

---

[Switching to DevOpsEngineer persona]

## Deployment Configuration

### Docker Compose
```yaml
version: '3.8'

services:
  auth-service:
    build: .
    ports:
      - "3000:3000"
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/authdb
      - JWT_SECRET=${JWT_SECRET}
      - JWT_PRIVATE_KEY=${JWT_PRIVATE_KEY}
      - JWT_PUBLIC_KEY=${JWT_PUBLIC_KEY}
      - EMAIL_API_KEY=${EMAIL_API_KEY}
    depends_on:
      - db

  db:
    image: postgres:14
    environment:
      - POSTGRES_DB=authdb
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=pass
    volumes:
      - postgres-data:/var/lib/postgresql/data

volumes:
  postgres-data:
```

---

[Final Summary by TechLead]

## Project Completion Summary

### Deliverables
✅ System architecture designed
✅ Security requirements defined
✅ API endpoints designed and documented
✅ Database schema created
✅ Complete implementation
✅ Comprehensive test suite (90%+ coverage)
✅ API documentation
✅ Deployment configuration

### Key Features Implemented
- User registration with email verification
- Secure login with JWT tokens
- Password reset flow
- Token refresh mechanism
- Rate limiting on all endpoints
- Comprehensive security measures

### Next Steps
1. Deploy to staging environment
2. Conduct security penetration testing
3. Load testing for scalability
4. Monitor error rates and performance
5. Gather user feedback
```

---

## Example 2: Performance Optimization

### Task
"Our API response times are slow. Users are complaining. Investigate and fix."

### Orchestration Approach

```markdown
[Adopting PerformanceEngineer persona]

## Performance Investigation

### Current Metrics
- Average response time: 2.5s (target: <200ms)
- p95 latency: 5s
- p99 latency: 8s
- Database CPU: 85%
- API server CPU: 35%

### Profiling Results

#### Database Analysis
```sql
-- Top slow queries
SELECT query, calls, total_time, mean_time
FROM pg_stat_statements
ORDER BY mean_time DESC
LIMIT 5;
```

**Findings:**
1. **Query #1**: User dashboard fetch - 1.2s average
   - Missing index on `orders.user_id`
   - N+1 query problem loading related data

2. **Query #2**: Product search - 800ms average
   - Full table scan on products
   - No full-text search index

3. **Query #3**: Order history - 600ms average
   - Fetching unnecessary columns
   - No pagination

### Bottlenecks Identified
1. 🔴 **Critical**: Missing database indexes
2. 🔴 **Critical**: N+1 query problems
3. 🟡 **High**: No caching layer
4. 🟡 **High**: Inefficient data fetching
5. 🔵 **Medium**: No query result pagination

### Optimization Plan
1. Add database indexes (DataEngineer)
2. Fix N+1 queries (Coder)
3. Implement Redis caching (Coder)
4. Add pagination (Coder)
5. Verify improvements (Tester + PerformanceEngineer)

---

[Switching to DataEngineer persona]

## Database Optimizations

### Index Creation
```sql
-- Add missing indexes
CREATE INDEX idx_orders_user_id ON orders(user_id);
CREATE INDEX idx_orders_created_at ON orders(created_at DESC);
CREATE INDEX idx_order_items_order_id ON order_items(order_id);

-- Full-text search index for products
CREATE INDEX idx_products_search ON products
USING GIN(to_tsvector('english', name || ' ' || description));

-- Composite index for common query pattern
CREATE INDEX idx_orders_user_status ON orders(user_id, status, created_at DESC);
```

### Query Optimization
```sql
-- Before: N+1 queries (slow)
SELECT * FROM users WHERE id = $1;
-- Then for each order:
SELECT * FROM orders WHERE user_id = $1;

-- After: Single query with JOIN (fast)
SELECT
  u.*,
  json_agg(
    json_build_object(
      'id', o.id,
      'total', o.total,
      'status', o.status,
      'created_at', o.created_at
    )
  ) as orders
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE u.id = $1
GROUP BY u.id;
```

---

[Switching to Coder persona]

## Implementation

### Add Redis Caching
```javascript
const redis = require('redis');
const client = redis.createClient();

async function getUserDashboard(userId) {
  const cacheKey = `dashboard:${userId}`;

  // Try cache first
  const cached = await client.get(cacheKey);
  if (cached) {
    return JSON.parse(cached);
  }

  // Fetch from database with optimized query
  const dashboard = await db.query(`
    SELECT
      u.id,
      u.name,
      u.email,
      json_agg(
        json_build_object(
          'id', o.id,
          'total', o.total,
          'status', o.status
        ) ORDER BY o.created_at DESC
      ) FILTER (WHERE o.id IS NOT NULL) as recent_orders
    FROM users u
    LEFT JOIN LATERAL (
      SELECT * FROM orders
      WHERE user_id = u.id
      ORDER BY created_at DESC
      LIMIT 10
    ) o ON true
    WHERE u.id = $1
    GROUP BY u.id, u.name, u.email
  `, [userId]);

  // Cache for 5 minutes
  await client.setEx(cacheKey, 300, JSON.stringify(dashboard));

  return dashboard;
}
```

### Add Pagination
```javascript
async function getOrders(userId, page = 1, limit = 20) {
  const offset = (page - 1) * limit;

  const result = await db.query(`
    SELECT
      id,
      total,
      status,
      created_at
    FROM orders
    WHERE user_id = $1
    ORDER BY created_at DESC
    LIMIT $2 OFFSET $3
  `, [userId, limit, offset]);

  const countResult = await db.query(
    'SELECT COUNT(*) FROM orders WHERE user_id = $1',
    [userId]
  );

  return {
    orders: result.rows,
    pagination: {
      total: parseInt(countResult.rows[0].count),
      page,
      limit,
      pages: Math.ceil(countResult.rows[0].count / limit)
    }
  };
}
```

---

[Switching to Tester persona]

## Performance Tests

```javascript
const autocannon = require('autocannon');

describe('Performance Tests', () => {
  test('dashboard endpoint under 200ms', async () => {
    const result = await autocannon({
      url: 'http://localhost:3000/api/dashboard',
      connections: 10,
      duration: 30,
      headers: {
        'Authorization': 'Bearer test-token'
      }
    });

    expect(result.latency.mean).toBeLessThan(200);
  });

  test('handles 100 concurrent requests', async () => {
    const result = await autocannon({
      url: 'http://localhost:3000/api/orders',
      connections: 100,
      duration: 10
    });

    expect(result.errors).toBe(0);
    expect(result.timeouts).toBe(0);
  });
});
```

---

[Switching back to PerformanceEngineer persona]

## Results

### Before Optimization
- Average response time: 2.5s
- p95 latency: 5s
- p99 latency: 8s
- Throughput: 40 req/s

### After Optimization
- Average response time: 120ms ✅ (95% improvement)
- p95 latency: 250ms ✅ (95% improvement)
- p99 latency: 400ms ✅ (95% improvement)
- Throughput: 500 req/s ✅ (12.5x improvement)

### Optimizations Applied
1. ✅ Added 5 database indexes
2. ✅ Fixed N+1 queries
3. ✅ Implemented Redis caching (5-min TTL)
4. ✅ Added pagination to all list endpoints
5. ✅ Optimized SELECT queries (fetch only needed columns)

### Performance Achieved
- Target: <200ms average response time ✅ ACHIEVED
- All endpoints now under 200ms
- System can handle 10x more traffic
```

---

## Example 3: Security Audit

### Task
"Conduct a security audit of our application before production launch."

```markdown
[Adopting SecurityEngineer persona]

## Security Audit Report

### Scope
- Authentication and authorization
- API endpoints
- Data protection
- Input validation
- Dependency vulnerabilities

### Findings

#### 🔴 Critical Issues

**1. SQL Injection Vulnerability**
- **Location**: `src/api/users.js:45`
- **Issue**: User input directly concatenated into SQL query
```javascript
// VULNERABLE CODE
const query = `SELECT * FROM users WHERE email = '${email}'`;
```
- **Impact**: Attacker can execute arbitrary SQL
- **Remediation**: Use parameterized queries
```javascript
// FIXED CODE
const query = 'SELECT * FROM users WHERE email = $1';
await db.query(query, [email]);
```

**2. Passwords Stored in Plain Text**
- **Location**: User registration endpoint
- **Issue**: Passwords stored without hashing
- **Impact**: Database breach exposes all passwords
- **Remediation**: Use bcrypt with minimum 10 rounds

**3. JWT Secret in Source Code**
- **Location**: `src/config/jwt.js`
- **Issue**: Hardcoded secret in version control
- **Impact**: Anyone can forge tokens
- **Remediation**: Move to environment variables

#### 🟡 High Priority Issues

**4. No Rate Limiting**
- **Impact**: Vulnerable to brute force attacks
- **Remediation**: Implement rate limiting (5 requests/15 min for login)

**5. Sensitive Data in Logs**
- **Location**: Multiple files
- **Issue**: Passwords and tokens logged
- **Remediation**: Sanitize logs, never log sensitive data

**6. No HTTPS Enforcement**
- **Impact**: Man-in-the-middle attacks possible
- **Remediation**: Enforce HTTPS, set HSTS header

[Complete audit report continues...]

---

[Switching to Refactor persona]

## Security Fixes Implementation

[Refactored secure code...]

---

[Switching to Tester persona]

## Security Test Suite

```javascript
describe('Security Tests', () => {
  test('prevents SQL injection', async () => {
    const maliciousInput = "'; DROP TABLE users; --";

    await request(app)
      .post('/api/users')
      .send({ email: maliciousInput })
      .expect(400); // Should reject, not execute SQL

    // Verify users table still exists
    const users = await db.query('SELECT COUNT(*) FROM users');
    expect(users).toBeDefined();
  });

  test('rate limits login attempts', async () => {
    // Make 6 login attempts rapidly
    for (let i = 0; i < 6; i++) {
      const response = await request(app)
        .post('/api/auth/login')
        .send({ email: 'test@example.com', password: 'wrong' });

      if (i < 5) {
        expect(response.status).toBe(401);
      } else {
        expect(response.status).toBe(429); // Too many requests
      }
    }
  });
});
```

---

[Final summary by SecurityEngineer]

## Audit Summary

### Issues Found
- 3 Critical vulnerabilities
- 6 High priority issues
- 12 Medium priority issues

### All Critical Issues Resolved
✅ SQL injection fixed
✅ Password hashing implemented
✅ JWT secrets moved to environment variables

### Security Posture: APPROVED FOR PRODUCTION ✅
```

---

## More Quick Examples

### Example 4: Simple Bug Fix
```
Task: "Login button doesn't work on mobile"

[Debugger] → Investigate and find root cause
[Coder] → Implement fix
[Tester] → Add regression test
```

### Example 5: Add New API Endpoint
```
Task: "Add endpoint to export user data as CSV"

[APIDesigner] → Design endpoint contract
[Coder] → Implement endpoint
[Tester] → Write tests
[TechnicalWriter] → Document endpoint
```

### Example 6: Database Migration
```
Task: "Add user preferences table"

[DataEngineer] → Design schema and migration
[Coder] → Implement application logic
[Tester] → Test migration and rollback
```

---

## Key Takeaways

1. **Always start with planning** for complex tasks (TechLead/Architect)
2. **Security is not optional** - include SecurityEngineer for sensitive features
3. **Test everything** - Tester persona after implementation
4. **Document as you go** - TechnicalWriter for user-facing features
5. **Quality gates matter** - CodeReviewer before merging
6. **Be explicit about persona switches** - Makes orchestration clear
7. **Synthesize at the end** - Provide integrated summary of all work
