# Requirements Gathering Phase

## Objective
Generate an initial set of requirements using EARS patterns and INCOSE quality rules. Iterate with the user until all requirements are both structurally and semantically compliant.

## Process

1. **Initial Generation**: Create requirements.md based on user's feature idea
2. **User Review**: Present requirements for approval
3. **Iteration**: Refine based on feedback until approved

## Constraints
- MUST create a `.doc/specs/{feature_name}/requirements.md` file
- MUST generate an initial version WITHOUT asking additional clarifying questions
- MUST ask: "Do the requirements look good? If so, we can move on to the design."
- MUST iterate until the user explicitly approves
- MUST correct non-compliant requirements and explain corrections
- MUST suggest improvements for incomplete requirements

## EARS Patterns

Every requirement MUST follow exactly one of six EARS patterns:

1. **Ubiquitous**: THE <system> SHALL <response>
   - Use for requirements that always apply

2. **Event-driven**: WHEN <trigger>, THE <system> SHALL <response>
   - Use for requirements triggered by specific events

3. **State-driven**: WHILE <condition>, THE <system> SHALL <response>
   - Use for requirements that apply during specific states

4. **Unwanted event**: IF <condition>, THEN THE <system> SHALL <response>
   - Use for error handling and unwanted situations

5. **Optional feature**: WHERE <option>, THE <system> SHALL <response>
   - Use for optional or configurable features

6. **Complex**: [WHERE] [WHILE] [WHEN/IF] THE <system> SHALL <response>
   - Clause order MUST be: WHERE → WHILE → WHEN/IF → THE → SHALL
   - Use when multiple conditions apply

### EARS Pattern Rules
- Each requirement must follow exactly one pattern
- System names must be defined in the Glossary
- Complex patterns must maintain the specified clause order
- All technical terms must be defined before use

## INCOSE Quality Rules

Every requirement MUST comply with these quality rules:

### Clarity and Precision
- **Active voice**: Clearly state who does what
- **No vague terms**: Avoid "quickly", "adequate", "reasonable", "user-friendly"
- **No pronouns**: Don't use "it", "them", "they" - use specific names
- **Consistent terminology**: Use defined terms from the Glossary consistently

### Testability
- **Explicit conditions**: All conditions must be measurable or verifiable
- **Measurable criteria**: Use specific, quantifiable criteria where applicable
- **Realistic tolerances**: Specify realistic timing and performance bounds
- **One thought per requirement**: Each requirement should test one thing

### Completeness
- **No escape clauses**: Avoid "where possible", "if feasible", "as appropriate"
- **No absolutes**: Avoid "never", "always", "100%" unless truly absolute
- **Solution-free**: Focus on what, not how (save implementation for design)

### Positive Statements
- **No negative statements**: Use "SHALL" not "SHALL NOT" when possible
- State what the system should do, not what it shouldn't do
- Exception: Error handling requirements may use negative statements when necessary

## Common Violations to Avoid

❌ "The system shall quickly process requests" (vague term)
✅ "WHEN a request is received, THE System SHALL process it within 200ms"

❌ "It shall validate the input" (pronoun)
✅ "THE Validator SHALL validate the input"

❌ "The system shall not crash" (negative statement)
✅ "WHEN an error occurs, THE System SHALL log the error and continue operation"

❌ "The system shall handle errors where possible" (escape clause)
✅ "WHEN an error occurs, THE System SHALL return an error code"

## Common Acceptance Criteria Patterns

Common program correctness patterns:

1. **Invariants**
   - Based on invariants preserved after transformation
   - Properties that remain constant despite changes to structure or order
   - Examples: collection size after map, contents after sort, tree balance
   - Examples: `obj.start <= obj.end`, `tree.is_balanced()`

2. **Round Trip Properties**
   - Based on combining an operation with its inverse to return to original value
   - Also includes non-strict inverses like insert/contains, create/exists
   - ALWAYS test one of these for serializers or parsers
   - Examples: serialization/deserialization, addition/subtraction, write/read
   - Examples: `decode(encode(x)) == x`, `parse(format(x)) == x`

3. **"The more things change, the more they stay the same" (Idempotence)**
   - Based on operations where doing it twice = doing it once
   - Example: distinct filter on a set returns same result when applied multiple times
   - Extends to database updates and message processing
   - Tests operations that should have no additional effect when repeated
   - Mathematically: f(x) = f(f(x))

4. **Metamorphic Properties**
   - When you know some relationship must hold between two components, without knowing the specific
   - Examples: `len(filter(x)) < len(x)`

5. **Model Based Testing**
   - Optimized implementation vs a standard, easy implementation

6. **Confluence**
   - Order of applications doesn't matter

7. **Error Conditions**
   - Generate bad inputs and ensure they properly signal errors

## Special Requirements Guidance

**Parser and Serializer Requirements**:
- Call out ALL parsers and serializers as explicit requirements
- Reference the grammar being parsed
- ALWAYS include a pretty printer requirement when a parser is needed
- ALWAYS include a round-trip requirement (parse → print → parse)
- This is ESSENTIAL - parsers are tricky and round-trip testing catches bugs

**Example Parser Requirements**:
```markdown
### Requirement N: Parse Configuration Files

**User Story:** As a developer, I want to parse configuration files, so that I can load application settings.

#### Acceptance Criteria

1. WHEN a valid configuration file is provided, THE Parser SHALL parse it into a Configuration object
2. WHEN an invalid configuration file is provided, THE Parser SHALL return a descriptive error
3. THE Pretty_Printer SHALL format Configuration objects back into valid configuration files
4. FOR ALL valid Configuration objects, parsing then printing then parsing SHALL produce an equivalent object (round-trip property)
```

## Output
`requirements.md` - Approved requirements document in `.doc/specs/{feature_name}/requirements.md`

## Requirements Document Template

```markdown
# Requirements Document

## Introduction

[Summary of the feature/system]

## Glossary

- **System/Term**: [Definition]
- **Another_Term**: [Definition]

## Requirements

### Requirement 1

**User Story:** As a [role], I want [feature], so that [benefit]

#### Acceptance Criteria

1. WHEN [event], THE [System_Name] SHALL [response]
2. WHILE [state], THE [System_Name] SHALL [response]
3. IF [undesired event], THEN THE [System_Name] SHALL [response]
4. WHERE [optional feature], THE [System_Name] SHALL [response]
5. [Complex pattern as needed]

### Requirement 2

**User Story:** As a [role], I want [feature], so that [benefit]

#### Acceptance Criteria

1. THE [System_Name] SHALL [response]
2. WHEN [event], THE [System_Name] SHALL [response]

[Continue with additional requirements...]
```

## Iteration and Feedback Rules

- MUST ask for explicit approval after every iteration of edits
- MUST make modifications if the user requests changes or does not explicitly approve
- MUST continue the feedback-revision cycle until explicit approval is received
- MUST NOT proceed to the next step until receiving clear approval
- MUST incorporate all user feedback before proceeding
- MUST offer to return to previous steps if gaps are identified

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

**Sample Acceptance Criteria (using EARS patterns):**
```
1. User Login
   1.1 WHEN a user submits valid credentials, THE Auth_System SHALL return a JWT token
   1.2 WHEN a user submits invalid credentials, THE Auth_System SHALL return an error message
   1.3 THE Auth_System SHALL hash passwords before storage
   1.4 WHEN a user fails login 5 times, THE Auth_System SHALL lock the account for 15 minutes
```

---

## Best Practices

- ✅ Use EARS patterns for all acceptance criteria (WHEN/WHILE/IF/WHERE/THE...SHALL)
- ✅ Define all system names and technical terms in Glossary
- ✅ Start with high-level user stories, then add detailed acceptance criteria
- ✅ Use concrete, measurable acceptance criteria (avoid vague terms)
- ✅ Follow INCOSE quality rules (clarity, testability, completeness)
- ✅ Prioritize requirements (must-have vs nice-to-have)
- ✅ Include non-functional requirements early
- ✅ Get user approval before moving to design phase
- ✅ Keep requirements focused on "what", not "how"
- ✅ Consider testability of each requirement (property-based vs example-based)
- ✅ Use numbered format for acceptance criteria (1.1, 1.2, etc.) for easy reference
- ✅ For parsers/serializers: always include round-trip properties

## Common Pitfalls

- ❌ Writing too detailed requirements upfront
- ❌ Mixing requirements with implementation details
- ❌ Not validating with user before proceeding
- ❌ Ignoring non-functional requirements
- ❌ Using vague, untestable criteria (e.g., "quickly", "user-friendly")
- ❌ Using pronouns instead of defined system names
- ❌ Including escape clauses ("where possible", "if feasible")
- ❌ Not following EARS patterns consistently
- ❌ Forgetting to define terms in Glossary
