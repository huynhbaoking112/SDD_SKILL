# Architecture & Design Phase

## Objective
Develop a comprehensive design document based on approved feature requirements. Conduct necessary research during the design process.

## Process

1. **Research**: Identify and research areas needed for design
2. **Design Writing**: Write design sections (stop before Correctness Properties)
4. **Properties**: Write correctness properties
5. **User Review**: Present design for approval
6. **Iteration**: Refine based on feedback until approved

## Constraints

- MUST create a `.doc/specs/{feature_name}/design.md` file
- MUST identify areas where research is needed
- MUST conduct research and build up context in the conversation
- SHOULD NOT create separate research files
- MUST summarize key findings that inform the design
- SHOULD cite sources and include relevant links
- MUST incorporate research findings into the design

## Output
`design.md` - Approved design document with correctness properties in `.doc/specs/{feature_name}/design.md`

## Testing Strategy Requirements

**Dual Testing Approach**:
- **Unit tests**: Verify specific examples, edge cases, and error conditions
- **Property tests**: Verify universal properties across all inputs
- Both are complementary and necessary for comprehensive coverage

**Unit Testing Balance**:
- Unit tests are helpful for specific examples and edge cases
- Avoid writing too many unit tests - property-based tests handle covering lots of inputs
- Unit tests should focus on:
  - Specific examples that demonstrate correct behavior
  - Integration points between components
  - Edge cases and error conditions
- Property tests should focus on:
  - Universal properties that hold for all inputs
  - Comprehensive input coverage through randomization

**Property Test Configuration**:
- Minimum 100 iterations per property test (due to randomization)
- Each property test must reference its design document property
- Tag format: **Feature: {feature_name}, Property {number}: {property_text}**

---

## Review and Approval

After updating the design document:
- Check if requirements.md exists (it should for requirements-first workflow)
- Ask: "Does the design look good? If so, we can move on to the implementation plan."

## Iteration and Feedback Rules

- MUST ask for explicit approval after every iteration of edits
- MUST make modifications if the user requests changes or does not explicitly approve
- MUST continue the feedback-revision cycle until explicit approval is received
- MUST NOT proceed to the next step until receiving clear approval
- MUST incorporate all user feedback before proceeding
- MUST offer to return to previous steps if gaps are identified
- MUST offer to return to requirements clarification if gaps are identified

---

## Document Template

```markdown
# Feature Name - Design

## Overview
High-level description of the feature, its purpose, and how it fits into the system.

## Architecture
System architecture diagram and component descriptions.
Use Mermaid diagrams where appropriate.

## Components and Interfaces
Detailed design of each component:
- Component responsibilities
- Public interfaces
- Dependencies
- Interaction patterns

## Data Models
Entity definitions, schemas, and relationships:
```typescript
Entity {
  field: type
  // ...
}
```

## Acceptance Criteria Testing Prework

### X.Y [Criterion Name]
**Thoughts:** Step-by-step reasoning about testability
**Testable:** yes - property | yes - example | edge-case | no

### X.Z [Criterion Name]
**Thoughts:** [Reasoning]
**Testable:** [Classification]

### Property Reflection
- Review all properties for redundancy
- Identify properties that can be combined
- Ensure each property provides unique validation

## Correctness Properties

*A property is a characteristic or behavior that should hold true across all valid executions of a system—essentially, a formal statement about what the system should do.*

### Property 1: [Property Name]
**Validates: Requirements X.Y**

For all [inputs/conditions], [the system behavior that must hold].

### Property 2: [Property Name]
**Validates: Requirements X.Z**

For any [inputs/conditions], [the system behavior that must hold].

## Error Handling
- Error scenarios and handling strategies
- Edge cases (from prework analysis)
- Failure modes and recovery

## Testing Strategy
- Unit testing approach (for examples and edge cases from prework)
- Property-based testing approach (for properties from prework)
- Integration testing considerations
- Test configuration (minimum 100 iterations for property tests)


## Design Document Structure

MUST include these sections:
- **Overview**: High-level description of the feature and its purpose
- **Architecture**: System architecture, components, and their interactions
- **Components and Interfaces**: Detailed component design and interfaces
- **Data Models**: Entity definitions, schemas, and relationships
- **Correctness Properties**: Formal properties derived from requirements
- **Error Handling**: Error handling strategies and edge cases
- **Testing Strategy**: Approach to testing (unit tests, property-based tests)

SHOULD include:
- Diagrams or visual representations (use Mermaid for diagrams)
- Design decisions and their rationales
- Technology stack choices with justification

MUST ensure the design addresses all feature requirements

MAY ask the user for input on specific technical decisions

## Writing Order (CRITICAL)

For Requirements-First Workflow:
1. **Write sections from Overview through Data Models**
2. **STOP before writing Correctness Properties section**
3. **Write "Acceptance Criteria Testing Prework" section** in the design document to analyze all acceptance criteria
4. **Perform Property Reflection** to eliminate redundancy
5. **Continue writing the Correctness Properties section** based on prework analysis
6. **Complete remaining sections** (Error Handling, Testing Strategy)

**Important:** The prework analysis MUST be completed before writing properties. This ensures systematic thinking and high-quality properties.

---

## Correctness Properties

### What are Correctness Properties?

*A property is a characteristic or behavior that should hold true across all valid executions of a system—essentially, a formal statement about what the system should do. Properties serve as the bridge between human-readable specifications and machine-verifiable correctness guarantees.*

### Property-Based Testing Overview

Property-based testing (PBT) validates software correctness by testing universal properties across many generated inputs. Each property is a formal specification that should hold for all valid inputs.

#### Core Principles

1. **Universal Quantification**: Every property must contain an explicit "for all" statement
2. **Requirements Traceability**: Each property must reference the requirements it validates
3. **Executable Specifications**: Properties must be implementable as automated tests
4. **Comprehensive Coverage**: Properties should cover all testable acceptance criteria

### Property Creation Process

The process of creating properties from requirements follows these steps:

1. **Prework Analysis**: Use the 'prework' tool to analyze each acceptance criterion
2. **Testability Assessment**: Determine if each criterion is testable as a property, example, or edge case
3. **Property Formulation**: Convert testable criteria into universally quantified properties
4. **Requirements Mapping**: Annotate each property with the requirements it validates

### Prework Analysis Process

MUST manually create an "Acceptance Criteria Testing Prework" section in the design document BEFORE writing the Correctness Properties section.

For EVERY acceptance criteria in the requirements document, think step-by-step to determine if it is a criteria that is amenable to automated testing. If it is amenable to automated testing, decide if it's a property (a rule that applies to a collection of values) or an example. Mark edge cases specifically, so that there are no separate properties for them.

After completing the analysis, MUST perform property reflection to eliminate redundancy before writing properties.

### Prework Analysis Format

```markdown
## Acceptance Criteria Testing Prework

### X.Y [Criterion Name]
**Thoughts:** Step-by-step reasoning about whether this requirement is testable and how to test it
**Testable:** yes - property | yes - example | edge-case | no

### X.Z [Criterion Name]
**Thoughts:** [Reasoning]
**Testable:** [Classification]

[Continue for all acceptance criteria...]

### Property Reflection
After analyzing all criteria, review for redundancy:
- Identify properties that overlap or imply each other
- Combine related properties into comprehensive ones
- Mark redundant properties for removal
- Ensure each remaining property provides unique validation value
```

### Prework Example

```markdown
## Acceptance Criteria Testing Prework

### 1.1 Valid Credentials Return JWT
**Thoughts:** Can generate many valid credential pairs and verify all return valid JWT tokens. This is a universal property that should hold for all valid inputs. Can use property-based testing with random credential generation.
**Testable:** yes - property

### 1.2 Invalid Credentials Return Error
**Thoughts:** Can generate many invalid credential combinations (wrong password, non-existent user, malformed input) and verify all return appropriate errors. Universal property.
**Testable:** yes - property

### 1.3 Password Hashing
**Thoughts:** For all passwords stored in database, none should be plain text. Can verify by checking database entries and ensuring hash function is always called. Universal property about storage invariant.
**Testable:** yes - property

### 1.4 Account Lockout After 5 Failures
**Thoughts:** This is a specific threshold (exactly 5 attempts). Better tested with specific example rather than property-based test. Not a universal rule but a specific business rule with concrete number.
**Testable:** yes - example

### 1.5 Empty Password Handling
**Thoughts:** Edge case - what happens with empty password string, null values, whitespace-only passwords? Boundary condition testing.
**Testable:** edge-case

### Property Reflection
- Properties 1.1 and 1.2 cover the main authentication flow (valid/invalid paths)
- Property 1.3 is independent storage concern - keep separate
- Criterion 1.4 should be unit test, not property (specific threshold)
- Criterion 1.5 should be edge case tests
- No redundancy detected - all properties provide unique validation
```

### Property Reflection

After completing the initial prework analysis, MUST perform a property reflection to eliminate redundancy:

**Property Reflection Steps:**
1. Review ALL properties identified as testable in the prework
2. Identify logically redundant properties where one property implies another
3. Identify properties that can be combined into a single, more comprehensive property
4. Mark redundant properties for removal or consolidation
5. Ensure each remaining property provides unique validation value

**Examples of Redundancy:**
- If Property 1 tests "adding a task increases list length by 1" and Property 2 tests "task list contains the added task", Property 1 may be redundant if Property 2 already validates the addition
- If Property 3 tests "muting prevents messages" and Property 4 tests "muted rooms reject non-moderator messages", these can likely be combined into one comprehensive property
- If Property 5 tests "parsing then printing preserves structure" and Property 6 tests "round-trip parsing is identity", Property 6 subsumes Property 5

MUST complete this reflection before proceeding to write the Correctness Properties section.

### Converting EARS to Properties

In this section, turn EARS acceptance criteria into testable properties. Refer back to the prework section in order to complete this. Must explain reasoning step-by-step.

### Property Annotation Format

Each property must be annotated with:
- **Property Number**: Sequential numbering within the document
- **Property Title**: Descriptive name for the property
- **Property Body**: Universal quantification statement starting with "For all" or "For any"
- **Requirements Reference**: Format: **Validates: Requirements X.Y, X.Z**

### Example Properties

```markdown
### Property 1: JWT Token Expiration Validity
**Validates: Requirements 1.3**

For all JWT tokens generated by the Auth_System, the token expiration time must be exactly 24 hours from the issuance timestamp.

### Property 2: Password Hash Irreversibility
**Validates: Requirements 1.2**

For all passwords stored in the database, there must not exist a function that can recover the original password from the stored hash without brute force.

### Property 3: Round-Trip Configuration Parsing
**Validates: Requirements 2.1, 2.3**

For all valid Configuration objects, parsing the configuration file, then printing it, then parsing again must produce a Configuration object equivalent to the original.
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

- ✅ Use prework tool to analyze acceptance criteria before writing properties
- ✅ Perform property reflection to eliminate redundancy
- ✅ Write properties with universal quantification ("For all", "For any")
- ✅ Link each property to specific requirements (Validates: Requirements X.Y)
- ✅ Design for testability from the start
- ✅ Define clear correctness properties
- ✅ Keep architecture simple and maintainable
- ✅ Document design decisions and trade-offs
- ✅ Consider scalability and performance early
- ✅ Link design elements back to requirements
- ✅ Use Mermaid diagrams for visual representations
- ✅ Conduct research and cite sources when needed
- ✅ Balance unit tests and property-based tests (avoid too many unit tests)

## Common Pitfalls

- ❌ Over-engineering the solution
- ❌ Skipping correctness properties
- ❌ Writing properties without prework analysis
- ❌ Not performing property reflection (redundant properties)
- ❌ Properties without universal quantification
- ❌ Not linking properties to requirements
- ❌ Not considering error handling
- ❌ Ignoring non-functional requirements
- ❌ Creating design without user validation
- ❌ Mixing design with implementation details
- ❌ Writing too many unit tests instead of property-based tests
- ❌ Proceeding to next phase without explicit user approval
