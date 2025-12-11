# Classify and Expand Subagent

Classify the project context and generate domain-specific content on-demand. Replaces static domain reference files with dynamic generation.

## Input

1. **User's initial answers** — responses to universal discovery questions
2. **Domain seed index** — compact triggers and concepts (domain-index.md)

## Process

### Step 0: Detect Project Complexity FIRST

Before anything else, determine project scale. This drives the entire workflow.

```yaml
complexity_detection:
  check_signals:
    team_size: "[from answers or infer]"      # 1-2 | 3-6 | 7-12 | >12
    timeline: "[from answers or infer]"       # ≤2wk | 1-3mo | 3-6mo | >6mo
    compliance: "[from answers or infer]"     # none | basic | moderate | strict
    scope: "[from description]"               # internal | product | platform | mission-critical
    
  inference_phrases:
    minimal: ["just need", "simple", "quick", "internal", "prototype", "basic", "weekend"]
    enterprise: ["HIPAA", "PCI", "compliance", "platform", "multi-team", "regulated", "audit"]

# OUTPUT:
complexity:
  level: "minimal|standard|complex|enterprise"
  signals:
    - "[Signal 1: e.g., Team: 2 developers]"
    - "[Signal 2: e.g., Timeline: 2 weeks]"
  reasoning: "[Why this level]"
  
  # Derived flags for orchestration
  fast_path: true|false           # Skip most phases (minimal only)
  skip_research: true|false       # Use defaults instead of web search
  skip_validation: true|false     # Skip architecture validation
  full_compliance: true|false     # Need comprehensive compliance coverage
```

**Complexity determines EVERYTHING downstream:**
- How many questions to ask
- Whether to research or use defaults
- How many architecture options to generate
- Depth of artifacts
- Size of CLAUDE.md

### Step 1: Classify Project (Scaled by Complexity)

Identify relevant domains based on triggers:

```yaml
classification:
  project:
    name: "[Inferred or stated name]"
    type: "[greenfield|migration|enhancement|integration]"
    summary: "[2-3 sentence description]"
    
  domains:
    primary:
      - domain: "[domain name]"
        confidence: "[high|medium]"
        trigger: "[what indicated this]"
    secondary:  # Skip for minimal projects
      - domain: "[domain name]"
        confidence: "[high|medium]"
        trigger: "[what indicated this]"
    cross_cutting:  # Minimal: security only. Enterprise: all relevant
      - "[concern]"
```

**Scaling:**
- **Minimal:** 1 primary domain max, skip secondary, minimal cross-cutting
- **Standard:** 1-2 primary, 0-1 secondary, relevant cross-cutting
- **Enterprise:** Full classification with all relevant domains

### Step 2: Generate Domain Content

For each identified domain, generate tailored content. **Do not use static files — generate based on domain concepts and project context.**

```yaml
domain_context:
  patterns:
    - name: "[Pattern name]"
      domain: "[Source domain]"
      description: "[What it is]"
      when_to_use: "[Conditions]"
      key_components: ["[Component 1]", "[Component 2]"]
      tradeoffs:
        pros: ["[Pro 1]"]
        cons: ["[Con 1]"]
      relevance: "[Why this matters for THIS project]"

  questions:
    high_priority:
      - question: "[Specific question]"
        domain: "[Source domain]"
        why: "[What decision this informs]"
        
    medium_priority:
      - question: "[Question]"
        domain: "[Source domain]"
        why: "[Why it matters]"

  considerations:
    - area: "[Topic]"
      domain: "[Source domain]"
      points:
        - "[Key consideration]"
        - "[Key consideration]"
      common_mistakes:
        - "[Mistake to avoid]"

  risks:
    - risk: "[Domain-specific risk]"
      domain: "[Source domain]"
      likelihood: "[high|medium|low]"
      mitigation: "[How to address]"
```

### Step 3: Set Execution Flags

Determine if phases can be skipped:

```yaml
execution:
  skip_domain_expansion: false
  skip_research: "[true if user stated production experience with stack]"
  skip_validation: false
  parallel_research: true
  
  reasoning:
    skip_research: "[Why or why not]"
```

## Output

```yaml
# Classification and expanded domain context

classification:
  project:
    name: "HealthTrack"
    type: "greenfield"
    summary: "Patient scheduling platform with video visits for healthcare providers"
    
  domains:
    primary:
      - domain: "scheduling-booking"
        confidence: "high"
        trigger: "appointment scheduling core feature"
      - domain: "healthcare"
        confidence: "high"
        trigger: "patient data, healthcare providers, HIPAA mentioned"
    secondary:
      - domain: "identity-auth"
        confidence: "high"
        trigger: "patients and providers need separate access"
      - domain: "collaboration-realtime"
        confidence: "medium"
        trigger: "video visits require real-time"
    cross_cutting:
      - "security-architecture"
      - "compliance-frameworks"

  constraints:
    timeline: "4 months MVP"
    team_size: "5 developers"
    team_skills: ["Python", "React", "PostgreSQL"]
    compliance: ["HIPAA"]
    cloud_preference: "AWS"

domain_context:
  patterns:
    - name: "Availability Windows with Buffer Time"
      domain: "scheduling-booking"
      description: "Define provider availability with configurable buffers between appointments"
      when_to_use: "Any scheduling system needing realistic time management"
      key_components: ["Availability rules", "Buffer configuration", "Conflict detection"]
      tradeoffs:
        pros: ["Prevents back-to-back burnout", "Realistic scheduling"]
        cons: ["Reduces total bookable slots", "More complex availability logic"]
      relevance: "Healthcare appointments need buffer for documentation, patient transitions"

    - name: "Row-Level Security for Multi-Tenancy"
      domain: "healthcare"
      description: "Database-level tenant isolation using PostgreSQL RLS policies"
      when_to_use: "Multi-tenant systems with strict data isolation requirements"
      key_components: ["RLS policies", "Tenant context", "Connection management"]
      tradeoffs:
        pros: ["Strong isolation", "Audit-friendly", "Single schema simplicity"]
        cons: ["Query overhead", "Policy complexity", "Debugging harder"]
      relevance: "HIPAA requires strict patient data isolation between providers"

    - name: "FHIR Resource Mapping"
      domain: "healthcare"
      description: "Map internal data models to FHIR resources for interoperability"
      when_to_use: "Systems needing EHR integration"
      key_components: ["Resource mappers", "FHIR server/facade", "Validation"]
      tradeoffs:
        pros: ["Standard interoperability", "Future-proof for integrations"]
        cons: ["Learning curve", "Verbose data model", "Version complexity"]
      relevance: "EHR integration mentioned - FHIR is standard approach"

  questions:
    high_priority:
      - question: "Which EHR systems need integration? (Epic, Cerner, Athena, etc.)"
        domain: "healthcare"
        why: "Determines integration complexity and timeline feasibility"
        
      - question: "Is the video feature MVP or fast-follow?"
        domain: "collaboration-realtime"
        why: "Video adds significant complexity - affects timeline"
        
      - question: "What's the expected appointment volume at launch?"
        domain: "scheduling-booking"
        why: "Informs database and infrastructure decisions"
        
      - question: "Patient-facing, provider-facing, or both?"
        domain: "scheduling-booking"
        why: "Affects UX complexity and permission model"

    medium_priority:
      - question: "Do providers need to manage their own availability?"
        domain: "scheduling-booking"
        why: "Self-service vs admin-managed affects feature scope"
        
      - question: "Any existing systems to integrate or replace?"
        domain: "healthcare"
        why: "Migration complexity"
        
      - question: "Mobile app needed or web-only for MVP?"
        domain: "mobile-apps"
        why: "Platform scope"

  considerations:
    - area: "HIPAA Compliance"
      domain: "healthcare"
      points:
        - "All PHI access must be logged (audit trail)"
        - "Business Associate Agreements needed for all vendors"
        - "Encryption at rest and in transit required"
        - "Access controls must follow minimum necessary principle"
      common_mistakes:
        - "Forgetting BAAs for third-party services"
        - "Logging PHI in plain text"
        - "Inadequate session timeout"

    - area: "Scheduling Conflicts"
      domain: "scheduling-booking"
      points:
        - "Race conditions when two patients book same slot"
        - "Provider availability changes during booking flow"
        - "Time zone handling for distributed systems"
      common_mistakes:
        - "Optimistic locking without retry"
        - "Not handling provider cancellations gracefully"

  risks:
    - risk: "EHR integration may exceed timeline"
      domain: "healthcare"
      likelihood: "high"
      mitigation: "Scope to single EHR for MVP, abstract integration layer"
      
    - risk: "Video infrastructure complexity"
      domain: "collaboration-realtime"
      likelihood: "medium"
      mitigation: "Use managed service (Twilio, Daily.co) vs build"
      
    - risk: "HIPAA compliance gaps discovered late"
      domain: "compliance-frameworks"
      likelihood: "medium"
      mitigation: "Security review checkpoint before launch"

execution:
  skip_domain_expansion: false
  skip_research: false
  skip_validation: false
  parallel_research: true
  
  reasoning:
    skip_research: "Team knows PostgreSQL but video platform selection needs research"
```

## Guidelines

### Pattern Generation

Generate patterns that are:
- Relevant to the specific project context
- Actionable (clear when to use)
- Honest about tradeoffs
- Connected to project requirements

Don't generate generic patterns — tailor to what this project actually needs.

### Question Generation

Questions should:
- Fill gaps in understanding
- Inform specific decisions
- Be answerable by the user
- Prioritize by impact on architecture

### Consideration Generation

Focus on:
- Domain-specific gotchas
- Common mistakes in this domain
- Things the team might not know to ask about

### Risk Generation

Identify risks that are:
- Specific to the domain combination
- Likely given the constraints
- Mitigatable with different choices

## Notes

- Generate content based on domain concepts, not by looking up static files
- Tailor everything to the specific project context
- Be concise — quality over quantity
- Set execution flags honestly to skip unnecessary work

---

## Error Handling

### Incomplete User Answers

If user provides minimal context:
```yaml
fallback_strategy:
  - Ask 2-3 most critical clarifying questions
  - Make reasonable assumptions, flag them explicitly
  - Proceed with broader domain coverage
  - Set skip_domain_expansion: false to ensure refinement
  
assumptions_made:
  - assumption: "[What we assumed]"
    default: "[Value assumed]"
    impact: "[What changes if wrong]"
    question_to_validate: "[How to confirm]"
```

### Unknown Domain

If project doesn't match known domains:
```yaml
fallback_strategy:
  - Identify closest analogous domains
  - Generate patterns from first principles
  - Be transparent: "This is a novel combination, I'll adapt patterns from [similar domains]"
  - Flag for extra validation research
  
novel_domain:
  closest_analogs: ["[domain1]", "[domain2]"]
  adapted_patterns: ["[pattern with modifications]"]
  uncertainty: "high"
  recommendation: "Extra validation in research phase"
```

### Conflicting Requirements

If requirements conflict:
```yaml
conflicts_detected:
  - conflict: "[Requirement A] vs [Requirement B]"
    example: "Real-time sync vs offline-first"
    resolution_options:
      - "[Option 1 with tradeoff]"
      - "[Option 2 with tradeoff]"
    question_for_user: "Which is more important: [A] or [B]?"
```

---

## Additional Examples

### Example: Simple CRUD App

**Input:** "Building an internal tool for tracking equipment loans. Small team, just need basics."

**Output:**
```yaml
classification:
  project:
    name: "Equipment Loan Tracker"
    type: "greenfield"
    summary: "Internal tool for tracking equipment loans to employees"
    
  domains:
    primary:
      - domain: "web-apps"
        confidence: "high"
        trigger: "Internal tool, web-based"
    secondary: []
    cross_cutting:
      - "identity-auth"  # Basic auth needed

domain_context:
  patterns:
    - name: "Simple CRUD with Soft Deletes"
      domain: "web-apps"
      description: "Standard create/read/update/delete with audit trail"
      relevance: "Equipment records need history"
      
  questions:
    high_priority:
      - question: "Who needs access? Just admins or all employees?"
        domain: "identity-auth"
        why: "Determines auth complexity"
        
  considerations:
    - area: "Simplicity"
      points:
        - "Internal tool - don't over-engineer"
        - "Start with monolith, probably stays monolith"
        - "Use familiar stack, skip trendy tech"

execution:
  skip_domain_expansion: true  # Simple enough
  skip_research: true          # Use standard stack
```

### Example: Enterprise Migration

**Input:** "We have a 15-year-old Java monolith handling insurance claims. Need to modernize but can't have downtime. 500K claims/day."

**Output:**
```yaml
classification:
  project:
    name: "Claims System Modernization"
    type: "migration"
    summary: "Modernize legacy insurance claims system with zero-downtime requirement"
    
  domains:
    primary:
      - domain: "fintech-payments"
        confidence: "high"
        trigger: "Insurance claims, financial transactions"
      - domain: "workflow-bpm"
        confidence: "high"
        trigger: "Claims processing workflow"
    secondary:
      - domain: "event-driven"
        confidence: "medium"
        trigger: "Eventual consistency may help migration"
    cross_cutting:
      - "compliance-frameworks"
      - "security-architecture"

domain_context:
  patterns:
    - name: "Strangler Fig Migration"
      domain: "migration"
      description: "Gradually route traffic from legacy to new system"
      relevance: "Zero-downtime requirement makes big-bang impossible"
      key_components: ["API gateway routing", "Feature flags", "Dual-write period"]
      
    - name: "Event Interception"
      domain: "migration"
      description: "Capture events from legacy, process in new system"
      relevance: "Decouple from legacy database gradually"
      
    - name: "Branch by Abstraction"
      domain: "migration"
      description: "Abstract interfaces, swap implementations"
      relevance: "Replace internal components safely"
      
  questions:
    high_priority:
      - question: "Which claims types are highest volume? Lowest risk?"
        domain: "migration"
        why: "Start migration with low-risk, high-volume to prove approach"
      - question: "Is the database schema documented?"
        domain: "migration"
        why: "Undocumented schema is major risk"
      - question: "What's the rollback strategy if new system fails?"
        domain: "migration"
        why: "Zero-downtime implies need for instant rollback"
        
  risks:
    - risk: "Undocumented business rules in legacy code"
      domain: "migration"
      likelihood: "high"
      mitigation: "Characterization tests before any changes"
      
    - risk: "Data inconsistency during dual-write period"
      domain: "migration"
      likelihood: "medium"
      mitigation: "Reconciliation jobs, alerting on mismatches"

execution:
  skip_domain_expansion: false
  skip_research: false  # Need current migration tooling
  parallel_research: true
```

### Example: Real-Time Collaboration

**Input:** "Building a Figma-like design tool. Multiple users editing same canvas simultaneously. Need to feel instant."

**Output:**
```yaml
classification:
  project:
    name: "Collaborative Design Tool"
    type: "greenfield"
    summary: "Real-time collaborative design canvas with multiplayer editing"
    
  domains:
    primary:
      - domain: "collaboration-realtime"
        confidence: "high"
        trigger: "Multiple users, simultaneous editing, instant feel"
    secondary:
      - domain: "web-apps"
        confidence: "high"
        trigger: "Browser-based canvas"
      - domain: "media-streaming"
        confidence: "medium"
        trigger: "May need asset streaming"
    cross_cutting:
      - "security-architecture"
      - "infrastructure"  # WebSocket scaling

domain_context:
  patterns:
    - name: "CRDTs (Conflict-free Replicated Data Types)"
      domain: "collaboration-realtime"
      description: "Data structures that merge automatically without conflicts"
      relevance: "Core to multiplayer editing - handles concurrent edits"
      key_components: ["CRDT library", "Operation log", "Peer sync"]
      tradeoffs:
        pros: ["No central coordination", "Works offline", "Eventually consistent"]
        cons: ["Complex to implement", "Memory overhead", "Some operations hard to model"]
        
    - name: "Operational Transformation (OT)"
      domain: "collaboration-realtime"
      description: "Transform operations to maintain consistency"
      relevance: "Alternative to CRDTs, used by Google Docs"
      tradeoffs:
        pros: ["Well-understood", "Good tooling"]
        cons: ["Needs central server", "Complex transformation functions"]
        
    - name: "Presence & Cursors"
      domain: "collaboration-realtime"
      description: "Show other users' selections and cursors"
      relevance: "Essential for collaborative feel"
      
  questions:
    high_priority:
      - question: "What's the maximum users per document?"
        domain: "collaboration-realtime"
        why: "Affects sync architecture dramatically"
      - question: "Is offline editing required?"
        domain: "collaboration-realtime"
        why: "CRDTs better for offline, OT needs connection"
      - question: "What granularity of undo? Per-user or global?"
        domain: "collaboration-realtime"
        why: "Affects operation history design"

  considerations:
    - area: "Latency Budget"
      points:
        - "Local-first: apply optimistically, sync later"
        - "< 100ms perceived latency for 'instant' feel"
        - "WebSocket for real-time, not polling"
      common_mistakes:
        - "Waiting for server confirmation before updating UI"
        - "Sending full document instead of operations"

execution:
  skip_research: false  # Need current CRDT/OT libraries
```
