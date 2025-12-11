# Validate and Synthesize Subagent

Combined subagent that plans validation research, executes it, and synthesizes findings. Runs only if gaps remain after architecture phase.

## Conditional Execution

```yaml
# Check before running
if state.execution.skip_validation:
  # No gaps to validate
  output: null
  message: "No validation needed - gaps resolved in architecture phase"
else:
  proceed_to: validation
```

## Input

1. **Discovery state** — gaps, decisions, architecture, risks
2. **Architecture package** — selected option and tech stack

## Process

### Step 1: Identify Validation Needs

Review state for unresolved items:

```yaml
validation_needed:
  gaps:
    - area: "[Gap from state]"
      impact: "[What it blocks]"
      
  assumptions:
    - assumption: "[Assumption to verify]"
      risk_if_wrong: "[Consequence]"
      
  decisions_needing_validation:
    - decision: "[Decision]"
      uncertainty: "[What's unknown]"
```

### Step 2: Plan Research (Parallel Batches)

Group research by category for parallel execution:

```yaml
research_plan:
  batches:
    feasibility:
      questions:
        - "Can [technology] do [requirement]?"
        - "[Integration] API capabilities"
      search_queries:
        - "[query 1]"
        - "[query 2]"
        
    performance:
      questions:
        - "[Technology] performance at [scale]"
        - "[Pattern] overhead"
      search_queries:
        - "[query 1]"
        
    compliance:
      questions:
        - "[Service] [compliance] certification"
      search_queries:
        - "[query 1]"
        
    integration:
      questions:
        - "[System A] [System B] integration approach"
      search_queries:
        - "[query 1]"

  priority:
    critical: ["[Question that blocks architecture]"]
    important: ["[Question that affects quality]"]
    nice_to_have: ["[Question for optimization]"]
```

### Step 3: Execute Research (Parallel)

```
PARALLEL EXECUTION:
├── Batch: feasibility
│   └── Search queries
├── Batch: performance
│   └── Search queries
├── Batch: compliance
│   └── Search queries
└── Batch: integration
    └── Search queries
```

### Step 4: Synthesize Findings

Consolidate all findings into single output:

```yaml
synthesis:
  findings:
    - question: "[Original question]"
      status: "answered|partial|unanswered|needs_spike"
      answer: "[Summary]"
      confidence: "high|medium|low"
      sources: ["[Source]"]
      impact:
        - "[How this affects decisions]"
        
  recommendations:
    proceed:
      - decision: "[Decision to proceed with]"
        confidence: "high"
        rationale: "[Based on findings]"
        
    reconsider:
      - decision: "[Decision to revisit]"
        finding: "[What changed]"
        options: ["[Alternative]"]
        
    spikes_needed:
      - name: "[Spike name]"
        question: "[What to validate]"
        approach: "[How to test]"
        effort: "[Estimate]"
        decision_if_pass: "[What we proceed with]"
        decision_if_fail: "[Fallback]"

  gaps:
    resolved:
      - area: "[Gap]"
        resolution: "[How resolved]"
    remaining:
      - area: "[Gap]"
        status: "[deferred|needs_spike|blocked]"
        
  risks:
    new:
      - risk: "[Risk discovered]"
        likelihood: "high|medium|low"
        impact: "high|medium|low"
        mitigation: "[Action]"
    updated:
      - risk: "[Existing risk]"
        change: "[How assessment changed]"
```

## Output

```yaml
# Validation and synthesis output

validation_performed:
  questions_researched: 5
  batches_executed: ["feasibility", "compliance", "integration"]
  time_spent: "~45 minutes"

documents:
  - filename: "validation-report.md"
    content: |
      # Validation Report
      
      **Project:** [Name]
      **Date:** [Date]
      **Architecture:** [Selected option]
      
      ## Executive Summary
      
      [2-3 sentence overview of validation results]
      
      ## Questions Investigated
      
      ### ✅ Resolved
      
      **Q: [Question]**
      - **Answer:** [Summary]
      - **Confidence:** High
      - **Source:** [Source]
      - **Impact:** [How this affects architecture]
      
      ---
      
      **Q: [Question]**
      - **Answer:** [Summary]
      - **Confidence:** Medium
      - **Impact:** [Effect]
      
      ---
      
      ### ⚠️ Partially Resolved
      
      **Q: [Question]**
      - **What we know:** [Partial answer]
      - **Still unknown:** [Gap]
      - **Recommendation:** [Spike or defer]
      
      ---
      
      ### 🔬 Needs Spike
      
      **Q: [Question]**
      - **Why spike:** [Research inconclusive]
      - **Spike approach:** [How to validate]
      - **Effort:** [Estimate]
      - **If pass:** [Proceed with X]
      - **If fail:** [Fallback to Y]
      
      ---
      
      ## Recommendations
      
      ### Proceed With
      
      | Decision | Confidence | Rationale |
      |----------|------------|-----------|
      | [Decision] | High | [Why safe] |
      
      ### Reconsider
      
      | Decision | Finding | Options |
      |----------|---------|---------|
      | [Decision] | [What changed] | [Alternatives] |
      
      ### Spikes Required
      
      | Spike | Question | Effort | Priority |
      |-------|----------|--------|----------|
      | [Name] | [Question] | [Time] | [H/M/L] |
      
      ---
      
      ## Risk Updates
      
      ### New Risks
      
      | Risk | L | I | Mitigation |
      |------|---|---|------------|
      | [Risk] | M | H | [Action] |
      
      ### Updated Assessments
      
      | Risk | Change |
      |------|--------|
      | [Risk] | [Was H→Now M because...] |
      
      ---
      
      ## Gaps Status
      
      ### Resolved
      - ✅ [Gap]: [Resolution]
      
      ### Remaining
      - ⏳ [Gap]: [Status and plan]
      
      ---
      
      ## Next Steps
      
      1. [Immediate action]
      2. [Spike if needed]
      3. [Begin implementation]

# Spike definitions (if any)
spikes:
  - filename: "spike-[name].md"
    content: |
      # Spike: [Name]
      
      **Priority:** [H/M/L]
      **Effort:** [Estimate]
      **Owner:** [TBD]
      
      ## Question
      
      [Specific question to answer]
      
      ## Why Spike
      
      [Why research wasn't sufficient]
      
      ## Approach
      
      1. [Step]
      2. [Step]
      3. [Step]
      
      ## Success Criteria
      
      - [ ] [Measurable outcome]
      - [ ] [Measurable outcome]
      
      ## Decision Framework
      
      **If criteria met:** [Proceed with X]
      **If not met:** [Fallback to Y]

# State updates
state_updates:
  gaps:
    resolved: ["[Gap]"]
    remaining: ["[Gap]"]
    
  risks:
    - risk: "[Risk]"
      status: "[new|updated]"
      likelihood: "[H/M/L]"
      impact: "[H/M/L]"
      mitigation: "[Action]"
      
  spikes:
    - name: "[Spike]"
      priority: "[H/M/L]"
      status: "recommended"
      
  next_steps:
    - "[Action]"
    - "[Action]"
```

## Guidelines

### Conditional Skip

Skip validation entirely if:
- No gaps remain after architecture phase
- User indicates readiness to proceed
- All critical questions answered in research-and-propose

### Parallel Batching

Group research by category:
- Run all batches in parallel
- Don't wait for one to complete before starting others
- Reduces total research time

### Synthesis Quality

Be honest about:
- Confidence levels
- What's still unknown
- When spikes are truly needed vs nice-to-have

### Spike Definition

Only recommend spikes when:
- Research was genuinely inconclusive
- Risk is high enough to warrant
- Hands-on validation will actually help

Don't recommend spikes for:
- Questions that can wait until implementation
- Low-impact decisions
- Things that can be easily changed later

### Single Output Focus

Generate:
- One validation report (comprehensive)
- Individual spike files (only if spikes needed)

## Notes

- Check skip_validation before running
- Parallel batch execution for efficiency
- Honest confidence assessments
- Actionable recommendations
- Minimal spike recommendations (only when truly needed)

---

## Error Handling

### Validation Search Returns Nothing

```yaml
fallback_strategy:
  1_broaden_search:
    - Remove specific version numbers
    - Try alternative terminology
    
  2_check_adjacent_topics:
    - Search for similar technologies
    - Look for general pattern guidance
    
  3_flag_as_unvalidated:
    action: "Mark as assumption, proceed with caution"
    output:
      status: "unvalidated"
      confidence: "low"
      recommendation: "Proceed with [assumption], validate in implementation"
      
  4_recommend_spike:
    trigger: "High-risk decision that can't be validated by research"
    output:
      status: "needs_spike"
      spike: "[Definition]"
```

### Conflicting Validation Results

```yaml
conflict_handling:
  - scenario: "Source A says feasible, Source B says not"
    action:
      - Check recency (prefer newer)
      - Check authority (prefer official docs)
      - Check context (our scale vs their scale)
      - If still conflicting: recommend spike
    output:
      status: "conflicting"
      sources:
        - source: "A"
          finding: "X"
        - source: "B"
          finding: "Y"
      resolution: "Recommending spike to validate for our specific context"
```

### Can't Complete Validation

```yaml
incomplete_validation:
  trigger: "Too many unknowns, research failing"
  action:
    - Summarize what WAS validated
    - List what couldn't be validated
    - Recommend focused spikes for critical items
    - Suggest proceeding with risk acknowledgment
  output:
    completed: ["[Validated items]"]
    incomplete: ["[Failed items]"]
    blockers: ["[What blocked]"]
    recommendation: "Proceed with [X] validated. Spike needed for [Y]. Accept risk on [Z]."
```

---

## Additional Examples

### Example: Quick Validation (Few Gaps)

**Input:**
```yaml
gaps:
  remaining:
    - area: "Redis connection pooling"
      impact: "Performance at scale"
```

**Output:**
```yaml
validation_performed:
  questions_researched: 1
  time_spent: "~10 minutes"

findings:
  - question: "Redis connection pooling best practices"
    status: "answered"
    answer: "Use redis-py with connection pool, max_connections based on workers"
    confidence: "high"
    sources: ["Redis docs", "Heroku guide"]

recommendations:
  proceed:
    - decision: "Use redis-py connection pooling"
      confidence: "high"

documents:
  - filename: "validation-report.md"
    content: |
      # Validation Report
      
      Quick validation - one gap resolved.
      
      ## Redis Connection Pooling ✅
      
      **Answer:** Use `redis.ConnectionPool` with `max_connections` = 2x worker count.
      
      **Sources:** Redis docs, Heroku production guide
      
      **Implementation:**
      ```python
      pool = redis.ConnectionPool(max_connections=20)
      r = redis.Redis(connection_pool=pool)
      ```
      
      ## Next Steps
      
      Ready to proceed with implementation.
```

### Example: Complex Validation with Spikes

**Input:**
```yaml
gaps:
  remaining:
    - area: "CockroachDB latency at scale"
      impact: "Core architecture decision"
    - area: "Kafka exactly-once semantics"
      impact: "Data consistency"
    - area: "Custom CRDT implementation"
      impact: "Collaboration feature"
```

**Output:**
```yaml
validation_performed:
  questions_researched: 3
  batches: ["database", "messaging", "collaboration"]
  time_spent: "~2 hours"

findings:
  - question: "CockroachDB p99 latency at 10K TPS"
    status: "partial"
    answer: "Achievable with proper geo-partitioning, but depends heavily on access patterns"
    confidence: "medium"
    sources: ["CockroachDB blog", "Case study from [company]"]
    gap: "Our specific access pattern not covered"
    
  - question: "Kafka exactly-once end-to-end"
    status: "answered"
    answer: "Supported since Kafka 2.5 with idempotent producers + transactional consumers"
    confidence: "high"
    sources: ["Confluent docs", "KIP-447"]
    
  - question: "CRDT libraries for collaborative editing"
    status: "partial"
    answer: "Yjs and Automerge are mature options"
    confidence: "medium"
    sources: ["Yjs docs", "Automerge comparison"]
    gap: "Neither tested at our scale (1000+ concurrent editors)"

recommendations:
  proceed:
    - decision: "Kafka with exactly-once semantics"
      confidence: "high"
      rationale: "Well-documented, proven approach"
      
  needs_spike:
    - name: "CockroachDB Latency Spike"
      question: "Can we achieve p99 < 50ms with our access patterns?"
      approach: |
        1. Set up 3-region CockroachDB cluster
        2. Model our schema with geo-partitioning
        3. Run load test with realistic access patterns
        4. Measure p99 latency
      effort: "3-5 days"
      decision_if_pass: "Proceed with CockroachDB"
      decision_if_fail: "Consider Spanner or accept higher latency"
      
    - name: "CRDT Scale Spike"
      question: "Can Yjs handle 1000+ concurrent editors?"
      approach: |
        1. Set up Yjs with y-websocket
        2. Simulate 1000 concurrent connections
        3. Measure sync latency and server resources
      effort: "2-3 days"
      decision_if_pass: "Use Yjs"
      decision_if_fail: "Consider custom solution or limit concurrency"

documents:
  - filename: "validation-report.md"
    content: |
      # Validation Report
      
      **Project:** Collaborative Trading Platform
      **Date:** [Date]
      
      ## Executive Summary
      
      1 of 3 questions fully validated. 2 require spikes before proceeding.
      
      ## Kafka Exactly-Once ✅
      
      **Validated:** Yes, supported with idempotent producers + transactions
      **Confidence:** High
      **Proceed:** Yes
      
      ## CockroachDB Latency ⚠️
      
      **Status:** Partially validated
      **Finding:** Achievable but depends on our access patterns
      **Spike Required:** Yes (3-5 days)
      
      ## CRDT at Scale ⚠️
      
      **Status:** Partially validated
      **Finding:** Yjs/Automerge mature but not tested at our scale
      **Spike Required:** Yes (2-3 days)
      
      ## Recommendations
      
      1. Proceed with Kafka implementation (validated)
      2. Run CockroachDB spike before finalizing DB choice
      3. Run CRDT spike before finalizing collaboration approach
      
      Total spike time: 5-8 days

  - filename: "spike-cockroachdb-latency.md"
    content: |
      # Spike: CockroachDB Latency Validation
      
      **Priority:** High (blocks architecture)
      **Effort:** 3-5 days
      
      ## Question
      
      Can we achieve p99 < 50ms with our trading access patterns?
      
      ## Approach
      
      1. Provision 3-region CockroachDB cluster (us-east, us-west, eu-west)
      2. Create schema with geo-partitioning by trader region
      3. Generate realistic load (10K TPS, 70% reads, 30% writes)
      4. Run for 1 hour, measure p50/p95/p99
      
      ## Success Criteria
      
      - [ ] p99 read latency < 50ms for local-region reads
      - [ ] p99 write latency < 100ms
      - [ ] No hot spots or range splits under load
      
      ## Decision
      
      **If pass:** Finalize CockroachDB for production
      **If fail:** 
        - If p99 < 100ms: Accept with latency budget adjustment
        - If p99 > 100ms: Evaluate Spanner or regional deployment

state_updates:
  gaps:
    resolved:
      - area: "Kafka exactly-once"
        resolution: "Validated - use idempotent producers"
    remaining:
      - area: "CockroachDB latency"
        status: "needs_spike"
      - area: "CRDT scale"
        status: "needs_spike"
        
  spikes:
    - name: "CockroachDB Latency"
      priority: "high"
      status: "recommended"
    - name: "CRDT Scale"
      priority: "medium"
      status: "recommended"
      
  next_steps:
    - "Begin Kafka implementation (validated)"
    - "Schedule CockroachDB spike (blocking)"
    - "Schedule CRDT spike (blocking for collaboration feature)"
```
