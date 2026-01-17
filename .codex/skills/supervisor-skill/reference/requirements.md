# Requirements Gathering Phase

## Goal
Understand what needs to be built and why

## Process Steps

### 1. Initial Discovery
- Gather user's rough idea or feature request
- Ask clarifying questions about purpose, scope, and users
- Identify stakeholders and their needs

### 2. User Stories Creation
- Write user stories in format: "As a [role], I want to [action], so that [benefit]"
- Prioritize stories by business value
- Validate stories with user

### 3. Acceptance Criteria Definition
- Define testable conditions for each user story
- Use clear, measurable criteria (numbered format: 1.1, 1.2, etc.)
- Include both functional and non-functional requirements

### 4. Requirements Review
- Present draft requirements document to user
- Iterate based on feedback
- Get user approval before proceeding

## Output
`requirements.md` - Approved requirements document in `.doc/{feature_name}/requirements.md`

## Property-Based Testing Considerations

When defining acceptance criteria, consider which requirements can be validated through:
- **Property-based tests**: Universal properties that must hold for all inputs
- **Example-based tests**: Specific test cases demonstrating correct behavior
- **Edge cases**: Boundary conditions and error scenarios

Each acceptance criterion should be analyzed for testability during the design phase.

---

## Document Template

```markdown
# Feature Name

## Overview
- Brief description
- Business value
- Scope

## User Stories
- Story 1: As a..., I want..., so that...
- Story 2: ...

## Acceptance Criteria
### 1. [Story/Feature Name]
- 1.1 Criterion description
- 1.2 Criterion description

## Non-Functional Requirements
- Performance
- Security
- Scalability

## Constraints & Assumptions
- Technical constraints
- Business assumptions

## Dependencies
- External systems
- Prerequisites
```

---

## Handling Incomplete Information

When information is missing or unclear:

### 1. Ask Structured Questions
- **Purpose:** What problem does this solve?
- **Scope:** What's included/excluded?
- **Users:** Who will use this?
- **Constraints:** Any technical/business limitations?

### 2. Make Reasonable Assumptions
- Document assumptions clearly in specs
- Mark with `[ASSUMPTION]` tag
- Request user validation

### 3. Use Placeholders
- Mark unclear sections with `[TBD]` or `[NEEDS CLARIFICATION]`
- Add inline questions for user
- Create draft and iterate

### 4. Iterative Refinement
- Start with what's known
- Fill gaps through conversation
- Update specs incrementally

---

## Example

**User Request:** "I need an API to manage user authentication"

**Questions to Ask:**
- What auth methods? (email/password, OAuth, etc.)
- What security requirements? (2FA, password rules, etc.)
- Who are the users? (end users, admins, etc.)
- What platforms? (web, mobile, both?)

**Sample User Story:**
```
As a user, I want to login with email and password, 
so that I can securely access my account.
```

**Sample Acceptance Criteria:**
```
1. User Login
   1.1 System must validate email format
   1.2 System must hash passwords before storage
   1.3 System must return JWT token on successful login
   1.4 System must lock account after 5 failed attempts
```

---

## Best Practices

- ✅ Start with high-level user stories, then add details
- ✅ Use concrete, measurable acceptance criteria
- ✅ Prioritize requirements (must-have vs nice-to-have)
- ✅ Include non-functional requirements early
- ✅ Get user approval before moving to design phase
- ✅ Keep requirements focused on "what", not "how"
- ✅ Consider testability of each requirement (property-based vs example-based)
- ✅ Use numbered format for acceptance criteria (1.1, 1.2, etc.) for easy reference

## Common Pitfalls

- ❌ Writing too detailed requirements upfront
- ❌ Mixing requirements with implementation details
- ❌ Not validating with user before proceeding
- ❌ Ignoring non-functional requirements
- ❌ Using vague, untestable criteria
