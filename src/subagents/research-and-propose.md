# Research and Propose Subagent

Combined subagent that researches the current technology landscape and proposes architecture options. Uses parallel research batching for efficiency.

## Input

1. **Discovery state** — requirements, constraints, domains, domain_context
2. **Domain seed index** — for architecture style reference
3. **Architecture answers** — scale, availability, integration needs

## Conditional Execution

```yaml
# Check before running
if state.execution.skip_research:
  # Skip web searches, use domain_context patterns directly
  research: null
  proceed_to: proposal_only
else:
  proceed_to: full_research_and_proposal
```

## Process

### Step 1: Plan Research (if not skipped)

Identify what needs researching based on domain_context and constraints:

```yaml
research_plan:
  # Batch by category for parallel execution
  batches:
    database:
      technologies: ["PostgreSQL", "alternatives"]
      queries:
        - "[db] latest version features 2024"
        - "[db] [cloud] managed service"
        - "[db] multi-tenant performance"
      
    backend:
      technologies: ["FastAPI", "Django", "alternatives"]
      queries:
        - "[framework] production scale"
        - "[framework] async performance"
        - "[framework] [db] integration"
        
    infrastructure:
      technologies: ["cloud services", "managed options"]
      queries:
        - "[cloud] HIPAA eligible services"
        - "[cloud] managed [service] pricing"
        
    specialized:
      technologies: ["video platforms", "domain-specific"]
      queries:
        - "video platform HIPAA BAA comparison"
        - "[domain] managed services"
        
    patterns:
      topics: ["architecture patterns for context"]
      queries:
        - "[pattern] best practices 2024"
        - "[pattern] at scale case study"
```

### Step 2: Execute Research (Parallel Batches)

Run searches in parallel batches:

```
PARALLEL EXECUTION:
├── Batch: database
│   └── Search: PostgreSQL 16, Aurora, RLS performance
├── Batch: backend  
│   └── Search: FastAPI production, async patterns
├── Batch: infrastructure
│   └── Search: AWS HIPAA services, managed options
├── Batch: specialized
│   └── Search: Twilio vs Daily.co HIPAA
└── Batch: patterns
    └── Search: multi-tenancy patterns 2024
```

### Step 3: Synthesize Research

Consolidate findings:

```yaml
research:
  technologies:
    - name: "PostgreSQL"
      version: "16.x"
      cloud_options:
        - provider: "AWS"
          service: "Aurora PostgreSQL"
          serverless: "v2 available"
          hipaa: true
      production_signals:
        - "[Who uses at scale]"
      ecosystem:
        - "psycopg3, SQLAlchemy 2.0, asyncpg"
      fit_assessment: "high"
      key_findings:
        - "RLS overhead ~10-15% with proper indexing"
        
  compatibility:
    - stack: ["FastAPI", "PostgreSQL", "Redis"]
      status: "verified"
      libraries: ["asyncpg", "redis-py"]
      notes: "Well-supported combination"
      
  cloud_services:
    - component: "Database"
      service: "Aurora PostgreSQL Serverless v2"
      hipaa: true
      cost_model: "ACU-based"
      
  patterns:
    - name: "Row-Level Security"
      current_best_practices:
        - "Index on tenant_id column"
        - "Set tenant context at connection level"
      production_examples:
        - "[Company] at [scale]"
```

### Step 4: Generate Proposal

Based on research + domain_context, propose options:

```yaml
proposal:
  options:
    - name: "[Option A - typically simpler]"
      style: "[architecture style]"
      summary: "[1-2 sentences]"
      
      fits_because:
        - "[Alignment with constraint/requirement]"
        
      risks:
        - risk: "[Main concern]"
          mitigation: "[How to address]"
          
      tech_stack:
        - component: "Backend"
          choice: "[Technology]"
          version: "[From research]"
          cloud_service: "[Managed option]"
          rationale: "[Why, citing research]"
          
      patterns:
        - name: "[Pattern from domain_context]"
          implementation: "[How in this option]"
          
      effort: "lower"
      time_to_mvp: "[Estimate]"

    - name: "[Option B - balanced]"
      # Same structure
      
    - name: "[Option C - more sophisticated]"
      # Same structure

  comparison:
    | Criterion | Option A | Option B | Option C |
    |-----------|----------|----------|----------|
    | Complexity | Low | Medium | High |
    | Time to MVP | X months | Y months | Z months |
    | Scale ceiling | [Assessment] | | |
    | Team fit | [Assessment] | | |
    | Risk level | Low | Medium | High |

  recommendation:
    preferred: "[Option name]"
    rationale: "[Why, referencing research and constraints]"
    caveat: "[When to choose differently]"

  clarifying_questions:
    - "[Question that would change recommendation]"
```

## Output

```yaml
# Combined research and proposal output

research:
  # Only present if research was performed
  technologies:
    - name: "PostgreSQL"
      version: "16.2"
      cloud_options:
        - provider: "AWS"
          service: "Aurora PostgreSQL"
          serverless: "v2 - scales 0.5-128 ACUs"
          hipaa: true
        - provider: "GCP"
          service: "Cloud SQL"
          hipaa: true
      key_findings:
        - "v16 improved parallel query performance"
        - "RLS overhead 10-15% with indexed tenant_id"
        - "Aurora Serverless v2 good for variable load"
      fit_assessment: "high"
      
    - name: "Twilio Video"
      hipaa: true
      baa: "Enterprise plan required"
      pricing: "Per-participant-minute"
      alternatives:
        - name: "Daily.co"
          hipaa: true
          pricing: "Per-minute, simpler"
        - name: "Vonage"
          hipaa: true
          pricing: "Per-minute"
      key_findings:
        - "All three offer HIPAA BAA"
        - "Daily.co simpler API, newer"
        - "Twilio most mature, higher cost"

  compatibility:
    - stack: ["FastAPI", "PostgreSQL", "Redis", "Celery"]
      status: "verified"
      notes: "Common production combination"
      
  cloud_services:
    hipaa_eligible:
      - "Aurora PostgreSQL"
      - "ElastiCache Redis"
      - "Lambda"
      - "API Gateway"
      - "S3"
      - "CloudWatch"

documents:
  - filename: "technology-research.md"
    content: |
      # Technology Research Summary
      
      **Project:** HealthTrack
      **Date:** [Date]
      
      ## Database: PostgreSQL
      
      **Current Version:** 16.2
      **Cloud Options:** Aurora (AWS), Cloud SQL (GCP), Azure Database
      
      ### Key Findings
      - RLS overhead manageable (10-15%) with proper indexing
      - Aurora Serverless v2 fits variable appointment load
      - All major clouds offer HIPAA-eligible managed service
      
      ## Video Platform Comparison
      
      | Platform | HIPAA | Pricing | Maturity | API Simplicity |
      |----------|-------|---------|----------|----------------|
      | Twilio | ✅ | $$$ | High | Medium |
      | Daily.co | ✅ | $$ | Medium | High |
      | Vonage | ✅ | $$ | High | Medium |
      
      **Recommendation:** Daily.co for simpler integration, Twilio if advanced features needed

proposal:
  options:
    - name: "Modular Monolith"
      style: "modular-monolith"
      summary: "Single deployable with clear module boundaries, event-driven internally"
      
      fits_because:
        - "Team of 5 moves faster with single codebase"
        - "4 month timeline favors simplicity"
        - "10K patients easily handled by single instance"
        
      risks:
        - risk: "Module boundaries blur under deadline pressure"
          mitigation: "Enforce via linting, code review gates"
        - risk: "Video adds complexity to monolith"
          mitigation: "Isolate as separate module, use managed service"
          
      tech_stack:
        - component: "Backend"
          choice: "FastAPI"
          version: "0.109+"
          cloud_service: "ECS Fargate or Lambda"
          rationale: "Async for video webhooks, team knows Python, OpenAPI generation"
        - component: "Database"
          choice: "PostgreSQL 16"
          cloud_service: "Aurora Serverless v2"
          rationale: "RLS for tenancy, team expertise, HIPAA on Aurora"
        - component: "Video"
          choice: "Daily.co"
          rationale: "HIPAA BAA, simpler API than Twilio, lower cost"
        - component: "Cache/Queue"
          choice: "Redis"
          cloud_service: "ElastiCache"
          rationale: "Sessions, rate limiting, Celery backend"
          
      patterns:
        - name: "Row-Level Security"
          implementation: "RLS policies on all tenant tables, context set per request"
        - name: "Background Workers"
          implementation: "Celery for reminders, notifications, EHR sync"
          
      effort: "lower"
      time_to_mvp: "3-4 months"

    - name: "Service-Oriented"
      style: "microservices"
      summary: "Separate services for scheduling, video, notifications with API gateway"
      
      fits_because:
        - "Clear separation of concerns"
        - "Video can scale independently"
        - "Easier to swap components"
        
      risks:
        - risk: "Distributed complexity for 5-person team"
          mitigation: "Limit to 3 services, shared deployment"
        - risk: "4 months tight for multiple services"
          mitigation: "May need scope cut or timeline extension"
          
      tech_stack:
        - component: "API Gateway"
          choice: "AWS API Gateway"
          rationale: "Managed, HIPAA eligible, handles auth"
        - component: "Services"
          choice: "FastAPI per service"
          rationale: "Consistent stack, team familiarity"
        - component: "Communication"
          choice: "REST + SQS for async"
          rationale: "Simple, managed queue"
          
      effort: "higher"
      time_to_mvp: "5-6 months"

    - name: "Serverless-First"
      style: "serverless"
      summary: "Lambda functions with managed services for everything possible"
      
      fits_because:
        - "Minimal ops for small team"
        - "Pay-per-use fits early stage"
        - "AWS serverless is HIPAA eligible"
        
      risks:
        - risk: "Cold starts may affect UX"
          mitigation: "Provisioned concurrency for critical paths"
        - risk: "Team learning curve"
          mitigation: "SAM framework, local testing"
          
      tech_stack:
        - component: "Compute"
          choice: "Lambda"
          rationale: "HIPAA eligible, scales automatically"
        - component: "Database"
          choice: "Aurora Serverless v2"
          rationale: "Serverless-friendly, familiar SQL"
        - component: "Orchestration"
          choice: "Step Functions"
          rationale: "Booking flow state machine"
          
      effort: "medium"
      time_to_mvp: "4-5 months"

  comparison:
    | Criterion | Modular Monolith | Service-Oriented | Serverless |
    |-----------|------------------|------------------|------------|
    | Complexity | Low | High | Medium |
    | Time to MVP | 3-4 months | 5-6 months | 4-5 months |
    | Scale ceiling | 100K users | 1M+ users | 1M+ users |
    | Team fit | Best | Stretch | Learning |
    | Ops burden | Medium | High | Low |
    | Cost at start | $$ | $$$ | $ |

  recommendation:
    preferred: "Modular Monolith"
    rationale: "Best fit for 4-month timeline, 5-person team, 10K user scale. Research confirms PostgreSQL RLS viable, Daily.co simplifies video. Clear extraction path if scale demands later."
    caveat: "If video traffic spikes independently, consider extracting video service from day 1"

  clarifying_questions:
    - "Is 4-month timeline firm, or could scope be reduced?"
    - "How variable is the appointment load (steady vs spiky)?"
```

## Guidelines

### Parallel Research

Batch searches by category to run concurrently:
- Database batch: all DB-related queries together
- Backend batch: framework queries together
- Don't wait for one batch to complete before starting next

### Research Prioritization

Focus research on:
1. Technologies team doesn't have production experience with
2. Compliance-critical components (HIPAA, etc.)
3. Components with multiple viable options
4. Integration points

Skip research for:
- Technologies team knows well
- Commoditized choices (team preference)
- Low-impact decisions

### Proposal Quality

Options should be:
- Meaningfully different (not minor variations)
- Feasible given constraints
- Informed by research findings
- Honest about tradeoffs

### Document Generation

Generate:
- `technology-research.md` — comprehensive research findings
- Research informs proposal but document stands alone

## Notes

- Check skip_research flag before web searches
- Batch parallel searches for efficiency
- Cite research findings in tech_stack rationale
- Generate one comprehensive research doc, not many small ones
- Options should span complexity spectrum (simple → sophisticated)

---

## Error Handling

### Search Returns No Results

```yaml
fallback_strategy:
  1_broaden_query:
    original: "PostgreSQL 16 RLS multi-tenant healthcare HIPAA"
    broadened: "PostgreSQL row level security performance"
    
  2_alternative_terms:
    original: "FastAPI production scaling"
    alternatives: ["Starlette performance", "Python async web framework scale"]
    
  3_use_domain_context:
    action: "Fall back to patterns from classify-and-expand"
    message: "I couldn't find current benchmarks for [X]. Proceeding with established patterns."
    
  4_flag_for_validation:
    action: "Add to gaps for validate-and-synthesize phase"
    gap: "[Topic] needs validation - research inconclusive"
```

### Conflicting Research

```yaml
conflict_resolution:
  - type: "Different sources disagree"
    action:
      - Note both perspectives
      - Prefer: official docs > recent benchmarks > blog posts > forums
      - Flag uncertainty in recommendation
    output: "Sources disagree on [X]. [Source A] says [Y], [Source B] says [Z]. Recommending [choice] because [rationale], but flagging for validation."
    
  - type: "Outdated information"
    action:
      - Check publish dates
      - Prefer sources < 1 year old
      - Note when using older sources
    output: "Most recent data is from [date]. Technology may have changed."
```

### Technology Not Found

```yaml
unknown_tech:
  action:
    - Search for alternatives in same category
    - Check if technology is deprecated/renamed
    - Recommend established alternative
  output: "I couldn't find [X]. It may be deprecated or niche. Consider [established alternative] instead."
```

---

## Migration-Specific Proposals

When `project.type == "migration"`:

```yaml
migration_options:
  - name: "Strangler Fig (Incremental)"
    style: "strangler-fig"
    summary: "Gradually route traffic from legacy to new, feature by feature"
    
    approach:
      - "API gateway in front of both systems"
      - "Route by feature/endpoint to old or new"
      - "Migrate one bounded context at a time"
      - "Keep legacy running until fully migrated"
      
    fits_because:
      - "Zero downtime requirement"
      - "Can validate each piece before proceeding"
      - "Rollback is just routing change"
      
    risks:
      - risk: "Extended period running two systems"
        mitigation: "Set hard deadline, prioritize ruthlessly"
      - risk: "Data sync between old and new"
        mitigation: "Event-driven sync, reconciliation jobs"
        
    timeline: "6-18 months depending on scope"
    team_impact: "Team split between maintenance and new development"

  - name: "Parallel Run (Risk-Averse)"
    style: "parallel-run"
    summary: "Run both systems simultaneously, compare outputs, switch when confident"
    
    approach:
      - "New system receives same inputs as legacy"
      - "Compare outputs, log discrepancies"
      - "Fix discrepancies until match rate acceptable"
      - "Switch traffic when confident"
      
    fits_because:
      - "High-risk domain (financial, healthcare)"
      - "Business rules not well documented"
      - "Need proof new system is correct"
      
    risks:
      - risk: "2x infrastructure cost during parallel period"
        mitigation: "Time-box parallel run, scale new system minimally"
      - risk: "Discrepancies hard to debug"
        mitigation: "Detailed logging, automated comparison tools"
        
    timeline: "3-6 months parallel run after new system built"

  - name: "Big Bang (Fast but Risky)"
    style: "big-bang"
    summary: "Build new system completely, switch over in single cutover"
    
    fits_because:
      - "Legacy truly unsalvageable"
      - "Downtime acceptable (maintenance window)"
      - "Clean break preferred to prolonged migration"
      
    risks:
      - risk: "All-or-nothing cutover"
        mitigation: "Extensive testing, rehearsed rollback"
      - risk: "Missed requirements discovered post-cutover"
        mitigation: "Parallel run period even if short"
        
    timeline: "Fastest to build, highest risk at cutover"
```

---

## Additional Examples

### Example: Simple Project (Skip Research)

**Input:**
```yaml
execution:
  skip_research: true
  reason: "Team has 5 years production experience with proposed stack"
```

**Output:**
```yaml
research: null  # Skipped

proposal:
  options:
    - name: "Team's Standard Stack"
      style: "modular-monolith"
      summary: "Use team's proven stack - no experiments for this project"
      
      tech_stack:
        - component: "Backend"
          choice: "Django"  # Team's expertise
          rationale: "Team knows this cold, no ramp-up time"
          # No version/cloud_service - team decides
          
      note: "Research skipped - team expertise. Validate any new patterns in implementation."
```

### Example: Complex Enterprise

**Input:**
```yaml
project:
  type: "greenfield"
  summary: "Multi-region trading platform"
constraints:
  compliance: ["SOC2", "PCI-DSS"]
  availability: "99.99%"
requirements:
  scale: "10K transactions/second"
  latency: "p99 < 50ms"
```

**Output:**
```yaml
research:
  technologies:
    - name: "PostgreSQL"
      verdict: "Not recommended"
      reason: "p99 < 50ms at 10K TPS challenging with global distribution"
      
    - name: "CockroachDB"
      version: "23.x"
      cloud_options:
        - provider: "Self-managed"
          reason: "Cockroach Cloud doesn't have all regions needed"
      key_findings:
        - "Geo-partitioning for data locality"
        - "Survives region failure"
        - "Serializable by default (good for financial)"
      fit_assessment: "high"
      
    - name: "Kafka"
      version: "3.x"
      cloud_options:
        - provider: "Confluent Cloud"
          hipaa: true
          pci: true
      key_findings:
        - "Proven at higher scale than our needs"
        - "Exactly-once semantics available"
      fit_assessment: "high"

proposal:
  options:
    - name: "Event-Sourced with Global Distribution"
      style: "event-driven"
      summary: "Event-sourced trading core with geo-distributed database"
      
      tech_stack:
        - component: "Database"
          choice: "CockroachDB"
          version: "23.2"
          rationale: "Research confirms: geo-distribution, serializable, financial-grade"
        - component: "Event Bus"
          choice: "Kafka"
          cloud_service: "Confluent Cloud"
          rationale: "Research confirms: PCI compliant, exactly-once"
        - component: "Compute"
          choice: "Kubernetes"
          rationale: "Multi-region orchestration"
          
      patterns:
        - name: "Event Sourcing"
          implementation: "All trades as events, derived views for queries"
        - name: "CQRS"
          implementation: "Separate write (trading) and read (reporting) models"
          
      complexity: "very high"
      team_requirements: "Senior distributed systems engineers"
      
  recommendation:
    preferred: "Event-Sourced with Global Distribution"
    rationale: "Only option that meets latency + availability requirements"
    caveat: "Requires experienced team - budget for hiring or training"
```
