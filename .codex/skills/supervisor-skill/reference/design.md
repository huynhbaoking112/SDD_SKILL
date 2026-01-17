# Architecture & Design Phase

## Goal
Define how the system will be built

## Process Steps

### 1. System Architecture
- Design high-level architecture and components
- Define data models and schemas
- Identify external dependencies and integrations

### 2. Correctness Properties
- Translate acceptance criteria into formal properties
- Define invariants that must always hold
- Specify pre/post conditions for operations
- Create properties suitable for property-based testing

### 3. API & Interface Design
- Define API endpoints, methods, and contracts
- Specify request/response formats
- Document error handling strategies

### 4. Design Review
- Present design document to user
- Validate design decisions
- Iterate based on feedback
- Get user approval before implementation planning

## Output
`design.md` - Approved design document with correctness properties in `.doc/{feature_name}/design.md`

---

## Document Template

```markdown
# Feature Name - Design

## Architecture Overview
- High-level design
- Component diagram
- Technology stack

## Data Models
- Entity definitions
- Relationships
- Schemas

## Correctness Properties
### Property 1: [Name]
**Validates:** Requirements X.Y
**Description:** Formal property statement
**Test Strategy:** How to verify

## API Design
- Endpoints
- Request/Response formats
- Error handling

## Implementation Notes
- Technical decisions
- Trade-offs
- Risks
```

---

## Correctness Properties Guide

Correctness properties are formal statements that your system must satisfy. They guide testing and validation.

### Types of Properties

**1. Invariants** - Must always be true
```
Example: "User email must be unique in the system"
Example: "Account balance cannot be negative"
```

**2. Pre/Post Conditions** - Input/output relationships
```
Example: "Given valid credentials, login returns a valid JWT token"
Example: "After logout, the user's session token is invalidated"
```

**3. State Transitions** - Valid state changes
```
Example: "Order status can only transition: pending → processing → completed"
Example: "Deleted users cannot be reactivated"
```

### Writing Properties

Each property should include:
- **Name:** Clear, descriptive name
- **Validates:** Link to requirements (e.g., Requirements 1.2)
- **Description:** Formal statement of the property
- **Test Strategy:** How to verify (unit test, property-based test, etc.)

### Example

```markdown
### Property 1: JWT Token Expiration
**Validates:** Requirements 1.3
**Description:** All JWT tokens must expire after 24 hours from issuance
**Test Strategy:** Property-based test generating random token creation times 
and verifying expiration logic

### Property 2: Password Hashing
**Validates:** Requirements 1.2
**Description:** Passwords must never be stored in plain text; 
all passwords must be hashed using bcrypt with salt rounds >= 10
**Test Strategy:** Unit test verifying hash function is called; 
integration test checking database contains only hashed values
```

---

## Architecture Design Tips

### Component Design
- Break system into logical components
- Define clear interfaces between components
- Identify shared vs isolated concerns

### Data Modeling
- Define entities and their relationships
- Specify data types and constraints
- Consider data validation rules

### Technology Stack
- Choose appropriate technologies for requirements
- Consider team expertise and project constraints
- Document rationale for technology choices

---

## Example

**Feature:** User Authentication API

**Architecture Overview:**
```
┌─────────────┐      ┌──────────────┐      ┌──────────────┐
│   Client    │─────▶│  Auth API    │─────▶│   Database   │
└─────────────┘      └──────────────┘      └──────────────┘
                            │
                            ▼
                     ┌──────────────┐
                     │  JWT Service │
                     └──────────────┘
```

**Data Models:**
```typescript
User {
  id: string
  email: string (unique)
  passwordHash: string
  createdAt: timestamp
  lastLogin: timestamp
}

Session {
  token: string
  userId: string
  expiresAt: timestamp
}
```

**API Endpoints:**
```
POST /auth/login
  Request: { email, password }
  Response: { token, expiresAt }

POST /auth/logout
  Request: { token }
  Response: { success }

POST /auth/refresh
  Request: { token }
  Response: { newToken, expiresAt }
```

---

## Best Practices

- ✅ Design for testability from the start
- ✅ Define clear correctness properties
- ✅ Keep architecture simple and maintainable
- ✅ Document design decisions and trade-offs
- ✅ Consider scalability and performance early
- ✅ Link design elements back to requirements

## Common Pitfalls

- ❌ Over-engineering the solution
- ❌ Skipping correctness properties
- ❌ Not considering error handling
- ❌ Ignoring non-functional requirements
- ❌ Creating design without user validation
- ❌ Mixing design with implementation details
