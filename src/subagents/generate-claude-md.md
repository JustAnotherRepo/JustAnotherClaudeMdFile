# CLAUDE.md Generator Subagent

Generate a CLAUDE.md file that captures project context, architecture decisions, and coding conventions for AI-assisted development.

## Purpose

CLAUDE.md is the bridge between architecture discovery and implementation. It gives Claude (or any AI assistant) the context needed to:
- Understand the project's purpose and constraints
- Follow established architecture patterns
- Use the right technologies and conventions
- Avoid known pitfalls
- Write code that fits the codebase

## Scaling First

Before generating, determine project complexity:

```yaml
complexity_detection:
  minimal:
    signals:
      - team_size <= 2
      - timeline <= "2 weeks"
      - no compliance
      - "internal tool" | "prototype" | "simple"
    target_size: "50-150 lines"
    sections: ["Context (brief)", "Tech Stack", "Conventions (minimal)", "Quick Reference"]
    
  standard:
    signals:
      - team_size 3-6
      - timeline 1-3 months
      - basic compliance or none
    target_size: "200-400 lines"
    sections: ["All core sections"]
    
  complex:
    signals:
      - team_size 6-12
      - timeline 3-6 months
      - compliance requirements
    target_size: "400-700 lines"
    sections: ["All sections with detail"]
    
  enterprise:
    signals:
      - team_size > 12
      - timeline > 6 months
      - strict compliance (HIPAA, PCI, FedRAMP)
    target_size: "700-1000 lines"
    sections: ["All sections comprehensive", "Team boundaries", "Service ownership"]
```

## Input

1. **Discovery state** — full context from discovery process
2. **Architecture package** — ADRs, diagrams, tech stack
3. **Implementation plan** — structure, milestones, conventions
4. **Validation reports** — compliance requirements, security controls
5. **Company standards** — if template-adapter was used
6. **Detected complexity** — from discovery state

## Output

Generate a comprehensive CLAUDE.md file:

```yaml
documents:
  - filename: "CLAUDE.md"
    content: |
      # [Project Name]
      
      > [One-line description of what this project does]
      
      ## Project Context
      
      ### What We're Building
      [2-3 sentence description of the project, its purpose, and who it's for]
      
      ### Key Constraints
      - **Timeline:** [Timeline]
      - **Team:** [Size and composition]
      - **Compliance:** [Requirements like HIPAA, SOC2, etc.]
      - **Cloud:** [Provider/preferences]
      
      ### Success Criteria
      - [Criterion 1]
      - [Criterion 2]
      
      ---
      
      ## Architecture
      
      ### Style
      **[Architecture style]** — [Brief rationale]
      
      ### System Overview
      ```
      [ASCII diagram or description of main components]
      ```
      
      ### Key Components
      
      | Component | Purpose | Technology |
      |-----------|---------|------------|
      | [Component] | [Purpose] | [Tech] |
      
      ### Data Flow
      [Brief description of how data moves through the system]
      
      ---
      
      ## Tech Stack
      
      | Layer | Technology | Version | Notes |
      |-------|------------|---------|-------|
      | Language | [Lang] | [Ver] | [Notes] |
      | Framework | [Framework] | [Ver] | [Notes] |
      | Database | [DB] | [Ver] | [Notes] |
      | Cache | [Cache] | [Ver] | [Notes] |
      | Queue | [Queue] | [Ver] | [Notes] |
      | Cloud | [Provider] | - | [Notes] |
      
      ### Key Libraries
      - **[Library]**: [What it's used for]
      - **[Library]**: [What it's used for]
      
      ---
      
      ## Code Conventions
      
      ### File Structure
      ```
      /src
        /[module]          # [Description]
          __init__.py
          models.py        # Data models
          services.py      # Business logic
          api.py           # API endpoints
          tests/
        /shared            # Cross-cutting concerns
        /infrastructure    # DB, external services
      ```
      
      ### Naming Conventions
      - **Files:** [Convention, e.g., snake_case.py]
      - **Classes:** [Convention, e.g., PascalCase]
      - **Functions:** [Convention, e.g., snake_case]
      - **Constants:** [Convention, e.g., UPPER_SNAKE]
      - **Database tables:** [Convention]
      - **API endpoints:** [Convention, e.g., /api/v1/resource]
      
      ### Code Style
      - [Style guide reference, e.g., PEP 8]
      - [Linter configuration]
      - [Formatter, e.g., black, prettier]
      
      ### Import Order
      ```python
      # 1. Standard library
      # 2. Third-party packages  
      # 3. Local imports
      ```
      
      ---
      
      ## Patterns to Follow
      
      ### [Pattern 1: e.g., Repository Pattern]
      ```python
      # Example of correct usage
      class UserRepository:
          def get_by_id(self, id: str) -> User | None:
              ...
      ```
      **Why:** [Rationale]
      
      ### [Pattern 2: e.g., Dependency Injection]
      ```python
      # Example
      def get_user_service(
          repo: UserRepository = Depends(get_user_repo)
      ) -> UserService:
          return UserService(repo)
      ```
      **Why:** [Rationale]
      
      ### [Pattern 3: e.g., Error Handling]
      ```python
      # Always use custom exceptions
      class UserNotFoundError(DomainError):
          pass
      
      # Never catch bare Exception
      try:
          ...
      except UserNotFoundError:
          ...
      ```
      
      ---
      
      ## Anti-Patterns to Avoid
      
      ### ❌ [Anti-pattern 1]
      ```python
      # DON'T do this
      [bad code example]
      ```
      **Why it's bad:** [Explanation]
      **Instead:** [What to do]
      
      ### ❌ [Anti-pattern 2]
      [Description]
      **Instead:** [What to do]
      
      ---
      
      ## Key Decisions (ADRs)
      
      ### ADR-001: [Decision Title]
      **Decision:** [What we decided]
      **Rationale:** [Why]
      **Implications:** [What this means for code]
      
      ### ADR-002: [Decision Title]
      **Decision:** [What we decided]
      **Rationale:** [Why]
      **Implications:** [What this means for code]
      
      ---
      
      ## API Design
      
      ### Endpoint Conventions
      - Base path: `/api/v1`
      - Resource naming: plural nouns (`/users`, `/appointments`)
      - Actions: use HTTP verbs (GET, POST, PUT, DELETE)
      
      ### Request/Response Format
      ```json
      // Success response
      {
        "data": { ... },
        "meta": { "request_id": "..." }
      }
      
      // Error response
      {
        "error": {
          "code": "USER_NOT_FOUND",
          "message": "User with ID xyz not found"
        },
        "meta": { "request_id": "..." }
      }
      ```
      
      ### Authentication
      [How auth works — JWT, session, etc.]
      
      ### Rate Limiting
      [Limits and how they're applied]
      
      ---
      
      ## Database
      
      ### Schema Conventions
      - Primary keys: `id` (UUID)
      - Timestamps: `created_at`, `updated_at`
      - Soft deletes: `deleted_at` (nullable)
      - Foreign keys: `[table]_id`
      
      ### Migrations
      - Tool: [Alembic, Flyway, etc.]
      - Location: `[path]`
      - Naming: `YYYYMMDD_HHMMSS_description.py`
      
      ### Multi-tenancy
      [How tenant isolation works, if applicable]
      
      ---
      
      ## Testing
      
      ### Test Structure
      ```
      /tests
        /unit           # Fast, isolated tests
        /integration    # DB, external services
        /e2e            # Full user flows
      ```
      
      ### Running Tests
      ```bash
      # All tests
      [command]
      
      # Unit only
      [command]
      
      # With coverage
      [command]
      ```
      
      ### Test Conventions
      - Name: `test_[function]_[scenario]_[expected]`
      - Use factories for test data: `[Factory library]`
      - Mock external services, not internal code
      
      ### Coverage Expectations
      - Unit tests: 80% on business logic
      - Integration: Critical paths covered
      - E2E: Happy paths for main flows
      
      ---
      
      ## Security
      
      ### Sensitive Data
      - **PII fields:** [List fields that are PII]
      - **Encryption:** [How sensitive data is encrypted]
      - **Logging:** Never log [sensitive fields]
      
      ### Authentication
      [Details on auth implementation]
      
      ### Authorization
      [RBAC/ABAC details, permission model]
      
      ### Compliance
      [Key compliance requirements to remember]
      
      ---
      
      ## Observability
      
      ### Logging
      ```python
      # Use structured logging
      logger.info("User created", extra={"user_id": user.id, "email_domain": domain})
      
      # Never log
      logger.info(f"User {user.email} created")  # PII!
      ```
      
      ### Metrics
      Key metrics to emit:
      - [Metric 1]
      - [Metric 2]
      
      ### Tracing
      [How distributed tracing works]
      
      ---
      
      ## Development Workflow
      
      ### Local Setup
      ```bash
      # Clone and setup
      [commands]
      
      # Run locally
      [commands]
      
      # Run tests
      [commands]
      ```
      
      ### Branch Strategy
      - `main`: Production-ready
      - `develop`: Integration branch
      - `feature/[ticket]-[description]`: Feature work
      - `fix/[ticket]-[description]`: Bug fixes
      
      ### PR Requirements
      - [ ] Tests pass
      - [ ] Linting passes
      - [ ] [Other requirements]
      
      ### Deployment
      [How deployment works]
      
      ---
      
      ## Common Tasks
      
      ### Adding a New Endpoint
      1. [Step 1]
      2. [Step 2]
      3. [Step 3]
      
      ### Adding a New Model
      1. [Step 1]
      2. [Step 2]
      3. [Step 3]
      
      ### Adding a Migration
      ```bash
      [command to create migration]
      [command to run migration]
      ```
      
      ---
      
      ## External Services
      
      | Service | Purpose | Docs | Env Vars |
      |---------|---------|------|----------|
      | [Service] | [Purpose] | [Link] | `[VAR]` |
      
      ---
      
      ## Troubleshooting
      
      ### [Common Issue 1]
      **Symptom:** [What you see]
      **Cause:** [Why it happens]
      **Fix:** [How to resolve]
      
      ### [Common Issue 2]
      **Symptom:** [What you see]
      **Fix:** [How to resolve]
      
      ---
      
      ## Resources
      
      - **Architecture docs:** [Link]
      - **API docs:** [Link]
      - **Runbooks:** [Link]
      - **Team channel:** [Link]
      
      ---
      
      ## Quick Reference
      
      ```bash
      # Start dev server
      [command]
      
      # Run tests
      [command]
      
      # Create migration
      [command]
      
      # Deploy to staging
      [command]
      ```

state_updates:
  documents_generated:
    - "CLAUDE.md"
  implementation:
    claude_md_created: true
```

## Content Mapping

Pull content from discovery artifacts:

```yaml
content_sources:
  project_context:
    - discovery_state.project
    - discovery_state.constraints
    - discovery_state.stakeholders
    
  architecture:
    - architecture_package.executive_summary
    - architecture_package.diagrams
    - architecture_package.tech_stack
    - discovery_state.architecture
    
  decisions:
    - architecture_package.adrs
    - discovery_state.decisions
    
  patterns:
    - domain_context.patterns (relevant ones)
    - research findings
    
  anti_patterns:
    - domain_context.considerations.common_mistakes
    - validation findings
    
  testing:
    - architecture_package.testing_strategy
    
  security:
    - architecture_package.security
    - compliance requirements
    
  observability:
    - architecture_package.observability
    
  development:
    - implementation_plan (if available)
    - company standards (if adapted)
```

## Guidelines

### Keep It Actionable

Every section should help Claude write better code:
- Include code examples (correct and incorrect)
- Be specific about conventions
- Link to relevant ADRs for "why"

### Right Level of Detail

- **Include:** Things that affect every coding decision
- **Exclude:** One-time setup details (put in README)
- **Reference:** Deep details (link to full docs)

### Update Triggers

CLAUDE.md should note when to update:
```markdown
<!-- 
Update this file when:
- Adding new patterns or conventions
- Making architecture decisions
- Changing tech stack
- Adding external services
-->
```

### Project-Specific Sections

Add sections based on project type:

```yaml
conditional_sections:
  multi_tenant:
    - "Multi-tenancy" section
    - Tenant context in examples
    
  compliance_heavy:
    - Expanded "Security" section
    - "Compliance" section
    - Audit logging examples
    
  api_focused:
    - Detailed "API Design" section
    - Request/response examples
    - Versioning strategy
    
  event_driven:
    - "Events" section
    - Event naming conventions
    - Event handling patterns
    
  migration:
    - "Legacy Integration" section
    - Migration patterns in use
    - What NOT to touch
```

## Example: Healthcare Scheduling Project

```markdown
# HealthTrack

> Patient scheduling and video visit platform for healthcare providers

## Project Context

### What We're Building
A B2B SaaS platform enabling healthcare providers to manage patient 
appointments and conduct video visits. Primary users are clinic staff 
(scheduling) and healthcare providers (video visits).

### Key Constraints
- **Timeline:** 4 months to MVP
- **Team:** 5 developers (Python/React background)
- **Compliance:** HIPAA required
- **Cloud:** AWS (HIPAA-eligible services only)

---

## Architecture

### Style
**Modular Monolith** — Single deployable with clear module boundaries.
Chosen for team velocity and timeline; extraction path available.

### Key Components

| Component | Purpose | Technology |
|-----------|---------|------------|
| API | REST endpoints | FastAPI |
| Database | Primary storage | PostgreSQL 16 (Aurora) |
| Video | Video visits | Daily.co |
| Queue | Background jobs | Redis + Celery |
| Auth | Authentication | Auth0 |

---

## Code Conventions

### Multi-Tenancy
Every database query MUST include tenant context:

```python
# ✅ Correct - tenant from request context
async def get_appointments(tenant_id: str, db: Session):
    return db.query(Appointment).filter(
        Appointment.tenant_id == tenant_id
    ).all()

# ❌ Wrong - no tenant filtering
async def get_appointments(db: Session):
    return db.query(Appointment).all()  # NEVER DO THIS
```

### PHI Handling
```python
# ✅ Correct - structured logging without PHI
logger.info("Appointment created", extra={
    "appointment_id": appt.id,
    "provider_id": appt.provider_id
})

# ❌ Wrong - contains PHI
logger.info(f"Appointment for {patient.name} at {patient.email}")
```

---

## Security

### Sensitive Data
- **PHI fields:** patient_name, date_of_birth, medical_record_number, 
  diagnosis, notes, contact_info
- **Never log:** Any PHI field, full patient records
- **Always encrypt:** PHI at rest (Aurora encryption) and transit (TLS)

### HIPAA Reminders
- All PHI access must be logged (audit trail)
- Minimum necessary principle for data access
- BAA required for all external services
- Session timeout: 15 minutes
```

## Error Handling

### Missing Information

If discovery didn't cover something:
```yaml
gap_handling:
  - gap: "No logging conventions discussed"
    action: "Generate reasonable defaults based on tech stack"
    note: "Add TODO comment: 'Confirm logging conventions with team'"
    
  - gap: "No external services defined yet"
    action: "Leave section as template"
    content: |
      ## External Services
      <!-- TODO: Add services as they're integrated -->
      | Service | Purpose | Docs | Env Vars |
      |---------|---------|------|----------|
```

### Incomplete Architecture

If architecture isn't fully defined:
```yaml
incomplete:
  action: "Generate what's known, mark unknowns"
  format: |
    ### [Section]
    <!-- TODO: Define after [decision/spike] -->
    [Placeholder or partial content]
```

---

## Scaled Examples

### Example: Minimal Project (Internal Tool)

**Input:** 1-2 devs, 2 week timeline, internal equipment tracker

**Output CLAUDE.md (~80 lines):**

```markdown
# Equipment Tracker

> Internal tool for tracking equipment loans to employees

## Tech Stack

| Layer | Technology |
|-------|------------|
| Backend | Django 5.0 |
| Database | SQLite (dev) / PostgreSQL (prod) |
| Frontend | Django templates + HTMX |

## Code Conventions

- Follow PEP 8
- Use Django's built-in patterns (CBVs for CRUD)
- Models in `tracker/models.py`
- Views in `tracker/views.py`

## Quick Reference

```bash
# Setup
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt
python manage.py migrate

# Run
python manage.py runserver

# Test
python manage.py test
```

## Key Decisions

**Django + SQLite:** Simple, fast to develop, team knows it well.
No need for complex architecture — this is an internal tool.

---

*Last updated: [Date]*
```

**Note:** Only 4 sections, ~80 lines. Appropriate for scope.

---

### Example: Enterprise Project (Healthcare Platform)

See full example above — 700+ lines covering:
- Comprehensive architecture
- Multi-tenancy patterns
- HIPAA compliance throughout
- Full security section
- Detailed observability
- Team boundaries
- On-call expectations

---

## Notes

- CLAUDE.md is for AI assistants AND human developers
- Keep it up-to-date as decisions evolve
- Include both "what" and "why"
- Code examples are essential
- Cross-reference to detailed docs, don't duplicate
- This is a living document — include update guidance
- **Scale to project complexity — don't over-engineer simple projects**
