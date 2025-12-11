# Discovery State

Central working document. Updated by subagents throughout the session.

```yaml
# === SESSION MANAGEMENT ===

session:
  id: ""
  started_at: ""
  last_updated: ""
  phase_completed: ""  # classification, refinement, architecture, validation, implementation
  next_phase: ""
  
  version: 1
  history:
    - version: 1
      timestamp: ""
      phase: ""
      change: ""
      
  checkpoint_available: true
  resume_prompt: ""  # What to say when resuming

# === COMPLEXITY (Set in Phase 1, drives orchestration) ===

complexity:
  level: ""  # minimal, standard, complex, enterprise
  signals:
    - ""  # e.g., "Team size: 2"
    - ""  # e.g., "Timeline: 2 weeks"
    - ""  # e.g., "No compliance requirements"
  reasoning: ""  # Why this level was chosen
  
  # Derived behavior flags
  fast_path: false           # True for minimal — skip most phases
  skip_research: false       # True if team experienced or minimal
  skip_validation: false     # True if minimal or no gaps
  full_compliance: false     # True if enterprise with strict compliance
  
# === POPULATED BY classify-and-expand ===

project:
  name: ""
  type: ""  # greenfield, migration, enhancement, integration
  summary: ""

stakeholders:
  - role: ""
    concerns: []
    success_criteria: ""
    involvement: ""  # decision-maker, consulted, informed
    
  primary_users: []
  decision_makers: []
  
constraints:
  timeline: ""
  budget: ""
  team_size: ""
  team_skills: []
  compliance: []
  cloud_preference: ""

# Domains identified from classification
domains:
  primary: []      # Main domain focus (1-2)
  secondary: []    # Supporting domains (1-2)
  cross_cutting: [] # Always-relevant concerns

# Generated domain content (on-demand, not from static files)
domain_context:
  patterns: []     # Relevant patterns for this project
  questions: []    # Domain-specific questions to ask
  considerations: [] # Key things to think about
  risks: []        # Domain-specific risks

# Assumptions made when info missing
assumptions:
  - assumption: ""
    default_value: ""
    impact_if_wrong: ""
    validated: false

# === UPDATED DURING REFINEMENT ===

requirements:
  functional: []
  non_functional:
    scale: ""
    availability: ""
    latency: ""
    
# === POPULATED BY research-and-propose ===

research:
  performed: false
  skipped_reason: ""  # e.g., "team has production experience"
  technologies: []   # Current landscape findings
  compatibility: []  # Verified combinations
  cloud_services: [] # Available managed options
  
architecture:
  style: ""
  selected_option: ""
  key_patterns: []
  tech_stack: []
  
decisions:
  - id: ""
    title: ""
    status: ""  # proposed, accepted, reconsidering, rejected
    rationale: ""
    alternatives: []
    revisit_trigger: ""  # What would cause reconsideration

# === UPDATED BY validate-and-synthesize ===

gaps:
  resolved:
    - area: ""
      resolution: ""
  remaining:
    - area: ""
      status: ""  # needs_spike, deferred, blocked

risks:
  - risk: ""
    likelihood: ""
    impact: ""
    mitigation: ""
    owner: ""
    status: ""  # identified, mitigating, accepted, resolved

spikes:
  - name: ""
    question: ""
    status: ""  # recommended, in_progress, completed, cancelled
    result: ""

# === IMPLEMENTATION PLANNING ===

implementation:
  milestones: []
  current_milestone: ""
  tasks: []
  blockers: []
    
next_steps: []

# === ITERATION SUPPORT ===

iteration:
  backtrack_requests: []  # Log of "let's reconsider X"
  decisions_changed:
    - decision_id: ""
      original: ""
      changed_to: ""
      reason: ""
      cascade_effects: []
```

## Conditional Flags

```yaml
# Set by subagents to control flow
execution:
  skip_domain_expansion: false  # True if user gave detailed requirements
  skip_research: false          # True if team has production experience
  skip_validation: false        # True if no gaps remaining
  skip_implementation: false    # True if user not ready to build
  parallel_research: true       # Default: batch research by category
  
  reasoning:
    skip_research: ""           # Why skipped or not
```

## Checkpoint Format

For multi-session support:

```yaml
checkpoint:
  project: "[name]"
  saved_at: "[timestamp]"
  phase_completed: "refinement"
  next_phase: "architecture"
  
  state: { ... full state above ... }
  
  documents_generated:
    - filename: "technology-research.md"
      content: "..."
      
  resume_prompt: |
    Welcome back! We're working on [project].
    
    So far we've:
    - Identified domains: [domains]
    - Gathered requirements: [summary]
    - Key constraints: [constraints]
    
    Ready to discuss architecture options?
```

## Iteration Handling

When user requests backtrack:

```yaml
backtrack:
  trigger: "User says 'let's reconsider [X]'"
  
  process:
    1. Identify decision being reconsidered
    2. Find downstream dependencies
    3. Mark affected items as "needs_review"
    4. Re-run relevant phase
    5. Update cascade
    
  example:
    trigger: "Actually, let's reconsider the database"
    affected:
      - decision: "ADR-002: PostgreSQL"
        status: "reconsidering"
      - decision: "ADR-003: ORM choice"
        status: "needs_review (depends on DB)"
    action: "Re-run research for database options"
```
