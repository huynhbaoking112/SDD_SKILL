# Implementation Planning Phase

## Objective
Create an actionable implementation plan with a checklist of coding tasks based on requirements and design.

## Prerequisites
- Design document must be approved
- Requirements document must exist
- For design-first workflow: design properties must be mapped to requirements

## CRITICAL FIRST STEP: Programming Language Selection (If Design Used Pseudocode)

**BEFORE creating the task list**, MUST determine what programming language will be used for implementation.

**Check the design document**:
- If the design document uses a **specific programming language** (Python, TypeScript, Java, etc.) → Use that language for tasks
- If the design document uses **pseudocode** → **MUST** ask user to choose implementation language

**IF design used pseudocode, MUST ask this question:**

"The design uses pseudocode. Which programming language would you like to use for implementation?"

Provide options:
- Python (Popular for APIs, data processing, and general-purpose development)
- TypeScript (Type-safe JavaScript, great for web APIs and full-stack development)
- JavaScript (Flexible and widely-used for web development and APIs)
- Java (Enterprise-grade, strongly-typed, excellent for large-scale systems)
- Go (Fast, concurrent, ideal for microservices and cloud-native apps)
- Rust (Memory-safe, high-performance, great for systems programming)
- C# (Modern, type-safe, excellent for .NET and enterprise applications)
- Other (Specify a different language)

**STOP and WAIT for the user's response before proceeding.**

## Process

1. **Convert Design to Tasks**: Break down design into discrete coding steps
2. **Add Testing Tasks**: Include property tests and unit tests as sub-tasks
3. **Mark Optional Tasks**: Mark test-related sub-tasks as optional with "*"
4. **Add Checkpoints**: Include checkpoint tasks at reasonable breaks
5. **User Review**: Present task list and ask about optional tasks
6. **Iteration**: Refine based on feedback until approved

## Constraints

- MUST create a `.doc/specs/{feature_name}/tasks.md` file
- MUST return to design if user indicates design changes needed
- MUST return to requirements if user indicates additional requirements needed
- MUST use these specific instructions:
  ```
  Convert the feature design into a series of prompts for a code-generation LLM that will implement each step with incremental progress. Make sure that each prompt builds on the previous prompts, and ends with wiring things together. There should be no hanging or orphaned code that isn't integrated into a previous step. Focus ONLY on tasks that involve writing, modifying, or testing code.
  ```

## Output
`tasks.md` - Approved implementation task list in `.doc/specs/{feature_name}/tasks.md`

---

## Task List Format

**Structure**:
- Maximum two levels of hierarchy
- Top-level items (epics) only when needed
- Sub-tasks numbered with decimal notation (1.1, 1.2, 2.1)
- Each item must be a checkbox
- Simple structure is preferred

**Task Item Requirements**:
- Clear objective involving writing, modifying, or testing code
- Additional information as sub-bullets under the task
- Specific references to requirements (granular sub-requirements, not just user stories)

**Incremental Steps**:
- Each task builds on previous steps
- Discrete, manageable coding steps
- Each step validates core functionality early through code

**Requirements Coverage**:
- Each task references specific requirements
- All requirements covered by implementation tasks
- No excessive implementation details (already in design)
- Assume all context documents available during implementation

**Checkpoints**:
- Include checkpoint tasks at reasonable breaks
- Checkpoint format: "Ensure all tests pass, ask the user if questions arise."
- Multiple checkpoints are okay

### Checkbox Status
- `- [ ]` = Not started (space inside brackets)
- `- [x]` = Completed (x inside brackets)
- `- [-]` = In progress (dash inside brackets)
- `- [~]` = Queued (tilde inside brackets)

## Testing Task Patterns

**Property-Based Tests**:
- MUST be written for universal properties
- Unit tests and property tests are complementary
- Testing MUST NOT have stand-alone tasks
- Testing SHOULD be sub-tasks under parent tasks

**Optional Task Marking**:
- Test-related sub-tasks SHOULD be marked optional by postfixing with "*"
- Test-related sub-tasks include: unit tests, property tests, integration tests
- Top-level tasks MUST NOT be postfixed with "*"
- Only sub-tasks can have the "*" postfix
- Optional sub-tasks are visually distinguished in UI and can be skipped
- Core implementation tasks should never be marked optional

**Implementation Rules**:
- MUST NOT implement sub-tasks postfixed with "*"
- MUST implement sub-tasks NOT prefixed with "*"
- Example: "- [ ]* 2.2 Write integration tests" → DO NOT implement
- Example: "- [ ] 2.2 Write unit tests" → MUST implement

**Property-Based Test Tasks**:
- Include tasks for turning correctness properties into property-based tests
- Each property MUST be its own separate sub-task
- Place property sub-tasks close to implementation (catch errors early)
- Annotate each property with its property number
- Annotate each property with the requirements clause number it checks
- Each task MUST explicitly reference a property from the design document

**Example Property Test Task**:
```markdown
- [ ] 2.3* Write property-based test for Property 1: JWT Token Expiration
  - Validates Requirements 1.3, Design Property 1
  - Generate random token creation times and verify expiration logic
  - Minimum 100 test iterations
```

## Coding Tasks Only

MUST ONLY include tasks that can be performed by a coding agent.

**Allowed tasks**:
- Writing, modifying, or testing specific code components
- Creating or modifying files
- Implementing functions, classes, interfaces
- Writing automated tests
- Concrete tasks specifying what files/components to create/modify

**Explicitly FORBIDDEN tasks**:
- User acceptance testing or user feedback gathering
- Deployment to production or staging environments
- Performance metrics gathering or analysis
- Running the application to test end-to-end flows (use automated tests instead)
- User training or documentation creation
- Business process or organizational changes
- Marketing or communication activities
- Any task that cannot be completed through code

---

## Review and Approval

After updating tasks document:
- First, check if the task list contains any optional tasks (sub-tasks marked with "*" like "- [ ]* 2.2 Write tests")
- **IF optional tasks exist in the task list**:
  - Ask: "The current task list marks some tasks (e.g. tests, documentation) as optional to focus on core features first."
  - Provide options:
    1. "Keep optional tasks (faster MVP)"
    2. "Make all tasks required (comprehensive from start)"
  - **If user wants comprehensive testing**:
    - Remove "*" marker from optional test tasks
    - Make them non-optional
- **IF no optional tasks exist in the task list**:
  - Do NOT ask about optional tasks
  - Simply ask for approval of the task list

## Iteration and Feedback Rules

- MUST ask for explicit approval after every iteration of edits
- MUST make modifications if the user requests changes or does not explicitly approve
- MUST continue the feedback-revision cycle until explicit approval is received
- MUST NOT proceed to the next step until receiving clear approval
- MUST incorporate all user feedback before proceeding
- MUST offer to return to previous steps if gaps are identified

MUST stop once the task document has been approved.

## Workflow Completion

**This workflow is ONLY for creating design and planning artifacts.**

- MUST NOT attempt to implement the feature as part of this workflow
- MUST clearly communicate that this workflow is complete once artifacts are created
- MUST inform the user they can begin executing tasks by:
  "You can ask me to perform any task or a step within that task."

---

## Document Template

```markdown
# Feature Name - Tasks

- [ ] 1. [Epic/Phase Name]
  - [ ] 1.1 [Specific implementation task]
    - References Requirements X.Y
    - [Additional context or notes]
  - [ ] 1.2 [Another implementation task]
  - [ ] 1.3* [Optional test task - Property-based test for Property N]
    - Validates Requirements X.Z, Design Property N
    - [Test strategy details]
    - Minimum 100 iterations

- [ ] 2. [Next Epic/Phase]
  - [ ] 2.1 [Implementation task]
  - [ ] 2.2* [Optional unit test task]

- [ ] 3. Checkpoint
  - Ensure all tests pass, ask the user if questions arise.
```

## Example Task List

**Feature:** User Authentication API

```markdown
# User Authentication - Tasks

- [ ] 1. Data Models and Database Setup
  - [ ] 1.1 Create User model with id, email, passwordHash, createdAt fields
    - References Requirements 1.1, 1.2
    - Use SQLAlchemy ORM
  - [ ] 1.2 Create Session model with token, userId, expiresAt fields
    - References Requirements 1.3
  - [ ] 1.3 Add database migrations for User and Session tables

- [ ] 2. Password Hashing Service
  - [ ] 2.1 Implement password hashing function using bcrypt
    - References Requirements 1.2
    - Salt rounds >= 10
  - [ ] 2.2* Write property-based test for Property 3: Password Hash Irreversibility
    - Validates Requirements 1.2, Design Property 3
    - Generate random passwords and verify hashes are irreversible
    - Minimum 100 iterations

- [ ] 3. JWT Token Service
  - [ ] 3.1 Implement JWT token generation with 24-hour expiration
    - References Requirements 1.3
  - [ ] 3.2 Implement JWT token validation
  - [ ] 3.3* Write property-based test for Property 1: JWT Token Expiration Validity
    - Validates Requirements 1.3, Design Property 1
    - Generate random token creation times and verify expiration
    - Minimum 100 iterations

- [ ] 4. Authentication Endpoints
  - [ ] 4.1 Implement POST /auth/login endpoint
    - References Requirements 1.1
    - Accept email and password
    - Return JWT token on success
  - [ ] 4.2 Implement POST /auth/logout endpoint
    - Invalidate session token
  - [ ] 4.3 Implement POST /auth/refresh endpoint
    - Generate new token from valid existing token
  - [ ] 4.4* Write unit tests for login endpoint edge cases
    - Test invalid credentials, empty inputs, malformed requests

- [ ] 5. Account Lockout Logic
  - [ ] 5.1 Implement failed login attempt tracking
    - References Requirements 1.4
  - [ ] 5.2 Implement account lockout after 5 failed attempts
    - Lock duration: 15 minutes
  - [ ] 5.3* Write unit test for account lockout threshold
    - Verify lockout occurs after exactly 5 failures

- [ ] 6. Checkpoint
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 7. Integration Testing
  - [ ] 7.1* Write integration test for full login flow
  - [ ] 7.2* Write integration test for token refresh flow
```

---

## Best Practices

- ✅ Focus ONLY on coding tasks (no deployment, user testing, or business tasks)
- ✅ Break down design into discrete, incremental coding steps
- ✅ Each task builds on previous tasks (no orphaned code)
- ✅ Link each task to specific requirements (X.Y format)
- ✅ Create property-based test tasks for each correctness property from design
- ✅ Mark test-related sub-tasks as optional with "*" postfix
- ✅ Place property test sub-tasks close to implementation (catch errors early)
- ✅ Include checkpoint tasks at reasonable breaks
- ✅ Maximum two levels of hierarchy (keep it simple)
- ✅ Use decimal notation for sub-tasks (1.1, 1.2, 2.1)
- ✅ Reference specific design properties in test tasks
- ✅ Specify minimum 100 iterations for property-based tests
- ✅ Get user approval before starting implementation
- ✅ Ask user about optional tasks (keep vs make required)

## Common Pitfalls

- ❌ Including non-coding tasks (deployment, user testing, documentation for users)
- ❌ Tasks too large or vague (should be discrete coding steps)
- ❌ Missing references to requirements
- ❌ No test tasks for correctness properties from design
- ❌ Test tasks as top-level items (should be sub-tasks)
- ❌ Not marking test sub-tasks as optional with "*"
- ❌ More than two levels of hierarchy (too complex)
- ❌ Orphaned code that isn't integrated
- ❌ Not including checkpoints
- ❌ Starting implementation without approval
- ❌ Forgetting to ask about optional tasks
- ❌ Not linking property tests to design document properties
