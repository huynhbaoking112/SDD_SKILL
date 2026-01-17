# Implementation Planning Phase

## Goal
Break down work into actionable tasks

## Process Steps

### 1. Task Breakdown
- Decompose design into implementable tasks
- Create hierarchical task structure (parent/sub-tasks)
- Link tasks to requirements and design sections

### 2. Dependency Mapping
- Identify task dependencies
- Order tasks by logical sequence
- Mark optional vs required tasks

### 3. Property-Based Test Planning
- Create PBT tasks for each correctness property
- Define test strategies and generators
- Link tests to specific properties in design

### 4. Tasks Review
- Present task breakdown to user
- Validate task scope and estimates
- Get user approval to begin implementation

## Output
`tasks.md` - Approved implementation task list in `.doc/{feature_name}/tasks.md`

---

## Document Template

```markdown
# Feature Name - Tasks

## Task List

- [ ] 1. Setup & Configuration
  - [ ] 1.1 Initialize project structure
  - [ ] 1.2 Configure dependencies

- [ ] 2. Core Implementation
  - [ ] 2.1 Implement component A
  - [ ] 2.2 Implement component B

- [ ] 3. Property-Based Testing
  - [ ] 3.1 Property: [Name] (validates Requirements X.Y)
  - [ ] 3.2 Property: [Name] (validates Requirements X.Z)

- [ ] 4. Integration & Deployment
  - [ ] 4.1 Integration testing
  - [ ] 4.2 Deployment setup

## Task Dependencies
- Task 2.1 depends on 1.1, 1.2
- Task 3.1 depends on 2.1
```

---

## Task Format

### Checkbox Status
- `- [ ]` = Not started (space inside brackets)
- `- [x]` = Completed (x inside brackets)
- `- [-]` = In progress (dash inside brackets)
- `- [~]` = Queued (tilde inside brackets)

### Required vs Optional Tasks
- **Required tasks:** No asterisk after checkbox
  ```
  - [ ] 1. Implement user authentication
  ```
- **Optional tasks:** Asterisk after checkbox
  ```
  - [ ]* Add caching layer
  ```

### Task Numbering
Use hierarchical numbering for clarity:
```
- [ ] 1. Parent Task
  - [ ] 1.1 Sub-task A
  - [ ] 1.2 Sub-task B
    - [ ] 1.2.1 Sub-sub-task
```

---

## Task Categories

### 1. Setup & Configuration
- Project initialization
- Dependency installation
- Environment configuration
- Database setup

### 2. Core Implementation
- Business logic
- Data models
- API endpoints
- Service layer

### 3. Testing
- Unit tests
- Property-based tests
- Integration tests
- End-to-end tests

### 4. Documentation
- API documentation
- Code comments
- User guides
- Deployment guides

### 5. Deployment
- CI/CD setup
- Environment configuration
- Monitoring setup
- Production deployment

---

## Property-Based Testing Tasks

For each correctness property in the design, create a corresponding test task:

```markdown
- [ ] 3.1 Property: JWT Token Expiration
  **Validates:** Requirements 1.3, Design Property 1
  **Description:** Test that all JWT tokens expire after 24 hours
  **Strategy:** Generate random token creation times and verify expiration
```

---

## Example

**Feature:** User Authentication API

```markdown
# User Authentication - Tasks

## Task List

- [ ] 1. Setup & Configuration
  - [ ] 1.1 Initialize FastAPI project structure
  - [ ] 1.2 Install dependencies (FastAPI, bcrypt, PyJWT, SQLAlchemy)
  - [ ] 1.3 Configure database connection
  - [ ] 1.4 Setup environment variables

- [ ] 2. Data Models
  - [ ] 2.1 Create User model with email, passwordHash fields
  - [ ] 2.2 Create Session model with token, userId, expiresAt
  - [ ] 2.3 Add database migrations

- [ ] 3. Core Authentication Logic
  - [ ] 3.1 Implement password hashing service
  - [ ] 3.2 Implement JWT token generation
  - [ ] 3.3 Implement JWT token validation
  - [ ] 3.4 Implement login endpoint
  - [ ] 3.5 Implement logout endpoint
  - [ ] 3.6 Implement token refresh endpoint

- [ ] 4. Property-Based Testing
  - [ ] 4.1 Property: JWT Token Expiration (validates Requirements 1.3)
  - [ ] 4.2 Property: Password Hashing (validates Requirements 1.2)
  - [ ] 4.3 Property: Email Uniqueness (validates Requirements 1.1)

- [ ] 5. Unit Testing
  - [ ] 5.1 Test password hashing service
  - [ ] 5.2 Test JWT token generation
  - [ ] 5.3 Test login endpoint
  - [ ] 5.4 Test logout endpoint

- [ ] 6. Integration Testing
  - [ ] 6.1 Test full login flow
  - [ ] 6.2 Test token refresh flow
  - [ ] 6.3 Test account lockout after failed attempts

- [ ] 7. Documentation & Deployment
  - [ ] 7.1 Write API documentation
  - [ ] 7.2 Setup CI/CD pipeline
  - [ ] 7.3* Add monitoring and logging (optional)

## Task Dependencies
- Task 2.x depends on 1.x (setup must be complete)
- Task 3.x depends on 2.x (models must exist)
- Task 4.x depends on 3.x (implementation must be complete)
- Task 5.x depends on 3.x (implementation must be complete)
- Task 6.x depends on 3.x, 5.x (implementation and unit tests)
- Task 7.x depends on 6.x (all testing complete)
```

---

## Task Breakdown Guidelines

### Granularity
- Each task should be completable in 1-4 hours
- If a task takes longer, break it into sub-tasks
- Sub-tasks should be independently testable

### Clarity
- Use clear, action-oriented language
- Start with verbs: "Implement", "Create", "Test", "Configure"
- Include enough detail to understand what needs to be done

### Traceability
- Link tasks to requirements and design sections
- Reference specific properties being validated
- Note dependencies between tasks

### Prioritization
- Order tasks by logical dependencies
- Mark optional tasks clearly
- Identify critical path tasks

---

## Best Practices

- ✅ Break down large tasks into smaller, manageable pieces
- ✅ Link each task to requirements/design
- ✅ Create test tasks for each correctness property
- ✅ Order tasks by dependencies
- ✅ Mark optional vs required tasks clearly
- ✅ Include setup and deployment tasks
- ✅ Get user approval before starting implementation

## Common Pitfalls

- ❌ Tasks too large or vague
- ❌ Missing dependencies between tasks
- ❌ No test tasks for correctness properties
- ❌ Skipping setup or deployment tasks
- ❌ Not linking tasks to requirements
- ❌ Starting implementation without approval
