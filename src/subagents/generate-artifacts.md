# Generate Artifacts Subagent

Generate architecture documentation artifacts based on selected option and decisions.

## Input

1. **Discovery state** — full state with requirements, decisions
2. **Selected architecture** — chosen option from proposal
3. **Decisions made** — refinements from discussion

## Output

Generate a single comprehensive architecture document:

```yaml
documents:
  - filename: "architecture-package.md"
    content: |
      # Architecture Decision Package
      
      **Project:** [Name]
      **Date:** [Date]
      **Status:** Approved / Draft
      
      ## Executive Summary
      
      [2-3 sentence overview of chosen architecture]
      
      ## Context
      
      ### Requirements
      - [Key functional requirements]
      - [Key non-functional requirements]
      
      ### Constraints
      - Timeline: [Timeline]
      - Team: [Size and skills]
      - Compliance: [Requirements]
      
      ---
      
      ## Architecture Decisions
      
      ### ADR-001: [Architecture Style]
      
      **Status:** Accepted
      **Date:** [Date]
      
      **Context:**
      [Why this decision was needed]
      
      **Decision:**
      [What was decided]
      
      **Consequences:**
      - ✅ [Positive consequence]
      - ⚠️ [Tradeoff accepted]
      
      **Alternatives Considered:**
      - [Alternative]: [Why not chosen]
      
      ---
      
      ### ADR-002: [Database Choice]
      
      [Same structure]
      
      ---
      
      ### ADR-003: [Key Technology Choice]
      
      [Same structure]
      
      ---
      
      ## System Diagrams
      
      ### Context Diagram
      
      ```mermaid
      C4Context
        title System Context - [System Name]
        
        Person(user1, "[User Type]", "[Description]")
        System(system, "[System]", "[Description]")
        System_Ext(ext1, "[External System]", "[Description]")
        
        Rel(user1, system, "[Uses]")
        Rel(system, ext1, "[Integrates]")
      ```
      
      ### Container Diagram
      
      ```mermaid
      C4Container
        title Container Diagram - [System Name]
        
        Person(user, "[User]", "[Description]")
        
        Container_Boundary(system, "[System]") {
          Container(web, "[Web App]", "[Tech]", "[Purpose]")
          Container(api, "[API]", "[Tech]", "[Purpose]")
          ContainerDb(db, "[Database]", "[Tech]", "[Purpose]")
        }
        
        Rel(user, web, "[Protocol]")
        Rel(web, api, "[Protocol]")
        Rel(api, db, "[Protocol]")
      ```
      
      ### Key Flow: [Flow Name]
      
      ```mermaid
      sequenceDiagram
        participant U as User
        participant A as API
        participant D as Database
        
        U->>A: [Action]
        A->>D: [Query]
        D-->>A: [Result]
        A-->>U: [Response]
      ```
      
      ---
      
      ## Component Specifications
      
      ### [Component Name]
      
      **Purpose:** [Single responsibility]
      
      **Interfaces:**
      - Input: [What it receives]
      - Output: [What it produces]
      
      **Dependencies:**
      - [Dependency]: [Why needed]
      
      **Key Behaviors:**
      1. [Behavior]
      2. [Behavior]
      
      ---
      
      ## Technology Stack
      
      | Layer | Technology | Version | Cloud Service | Rationale |
      |-------|------------|---------|---------------|-----------|
      | Frontend | [Tech] | [Ver] | [Service] | [Why] |
      | Backend | [Tech] | [Ver] | [Service] | [Why] |
      | Database | [Tech] | [Ver] | [Service] | [Why] |
      | Cache | [Tech] | [Ver] | [Service] | [Why] |
      | Queue | [Tech] | [Ver] | [Service] | [Why] |
      
      ---
      
      ## Testing Strategy
      
      ### Test Pyramid
      
      | Level | Focus | Coverage | Tools |
      |-------|-------|----------|-------|
      | Unit | Business logic | 80% | [Tool] |
      | Integration | DB, APIs | Critical paths | [Tool] |
      | E2E | User journeys | Happy paths | [Tool] |
      | Contract | API contracts | Public APIs | [Tool] |
      
      ### Critical Test Scenarios
      
      1. **[Scenario]**: [What, why critical]
      2. **[Scenario]**: [What, why critical]
      
      ### Load Testing
      
      - Normal: [X] users | Peak: [Y] users
      - Tool: [k6/Locust] | When: Pre-launch, major changes
      
      ---
      
      ## Observability
      
      ### Metrics
      
      | Type | Metrics |
      |------|---------|
      | Business | [Key metric], [Success rate] |
      | Technical | Latency (p50/95/99), Error rate, DB time |
      
      ### Logging & Tracing
      
      - Format: Structured JSON
      - Correlation: Request ID across services
      - Tracing: [X-Ray/Jaeger], 10% sampling
      
      ### Alerting
      
      | Condition | Severity | Action |
      |-----------|----------|--------|
      | Error rate > 5% | Critical | Page |
      | P99 > [X]s | Critical | Page |
      | Error rate > 1% | Warning | Slack |
      
      ---
      
      ## Security
      
      ### Threat Model (STRIDE)
      
      | Threat | Risk | Mitigation |
      |--------|------|------------|
      | Spoofing | [Risk] | [Control] |
      | Tampering | [Risk] | [Control] |
      | Repudiation | [Risk] | [Control] |
      | Info Disclosure | [Risk] | [Control] |
      | DoS | [Risk] | [Control] |
      | Elevation | [Risk] | [Control] |
      
      ### Controls
      
      - **Auth:** [Method], MFA [required/optional]
      - **AuthZ:** [RBAC/ABAC], [boundaries]
      - **Encryption:** At rest [method], transit TLS 1.3
      - **Secrets:** [Management approach]
      
      ---
      
      ## Operational Runbooks
      
      ### Deployment
      
      1. [Step with verification]
      2. [Step with verification]
      
      **Rollback:** [Procedure]
      
      ### Common Issues
      
      **High Latency**
      - Check: [X], [Y]
      - Fix: [Actions]
      
      **Connection Exhaustion**
      - Check: [X]
      - Fix: [Actions]
      
      ### Scaling
      
      - Trigger: [Condition]
      - Action: [Steps]
      - Limits: [Max, why]
      
      ---
      
      ## Risks & Mitigations
      
      | Risk | L | I | Mitigation | Owner |
      |------|---|---|------------|-------|
      | [Risk] | H/M/L | H/M/L | [Action] | [Who] |
      
      ---
      
      ## Open Questions
      
      - [ ] [Question]
      - [ ] [Question]
      
      ---
      
      ## Next Steps
      
      1. [Immediate]
      2. [Next]
      3. [Future]

# State updates
state_updates:
  architecture:
    style: "[Selected style]"
    selected_option: "[Option name]"
    key_patterns: ["[Pattern]"]
    tech_stack:
      - component: "[Component]"
        choice: "[Tech]"
        version: "[Version]"
        
  decisions:
    - id: "ADR-001"
      title: "[Title]"
      status: "accepted"
      summary: "[Brief summary]"
```

## Guidelines

### Single Document

Generate ONE comprehensive document instead of many files:
- Easier to share and review
- Complete picture in one place
- Can be split later if needed

### ADR Format

Keep ADRs concise:
- Context: 2-3 sentences
- Decision: 1-2 sentences
- Consequences: bullet points
- Alternatives: brief, why rejected

### Diagrams

Use Mermaid for all diagrams:
- C4Context for system context
- C4Container for containers
- sequenceDiagram for key flows
- erDiagram for data model (if needed)

Keep diagrams simple:
- Max 10 elements per diagram
- Focus on what matters for decisions
- Include technology labels

### Component Specs

Only spec components that need clarity:
- Core domain components
- Integration boundaries
- Shared services

Skip obvious components.

## Notes

- One document, not many files
- Include all ADRs in the package
- Diagrams should be valid Mermaid
- Update state with decisions made
