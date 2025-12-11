# Implementation Planning Subagent

Generate detailed implementation plan from architecture decisions.

## Input

1. **Architecture package** — selected architecture, tech stack, ADRs
2. **Constraints** — timeline, team size, skills
3. **Risks and spikes** — from validation phase
4. **Stakeholder context** — who needs what when

## Process

### Step 1: Decompose Architecture into Workstreams

```yaml
workstreams:
  - name: "Foundation"
    description: "Infrastructure, CI/CD, project setup"
    owner: "[Role/person]"
    
  - name: "Core Domain"
    description: "[Primary business logic]"
    owner: "[Role/person]"
    
  - name: "Integration"
    description: "[External system connections]"
    owner: "[Role/person]"
    
  - name: "Frontend"
    description: "[User-facing components]"
    owner: "[Role/person]"
```

### Step 2: Define Milestones

```yaml
milestones:
  - id: "M0"
    name: "Foundation"
    target_date: "[Date]"
    description: "Project infrastructure and team can deploy"
    
    deliverables:
      - name: "Repository and CI/CD"
        tasks: ["repo-setup", "ci-pipeline", "cd-pipeline"]
      - name: "Development environment"
        tasks: ["local-dev", "staging-env"]
      - name: "Database schema v1"
        tasks: ["schema-design", "migrations"]
        
    exit_criteria:
      - "Team can clone, build, and deploy to staging"
      - "Database migrations run successfully"
      - "Health check endpoint responds"
      
    dependencies: []
    risks: ["[Milestone-specific risks]"]
    
  - id: "M1"
    name: "Walking Skeleton"
    target_date: "[Date]"
    description: "Thinnest possible end-to-end slice"
    
    deliverables:
      - name: "[Core flow working]"
        tasks: ["[task-ids]"]
        
    exit_criteria:
      - "[Specific testable behavior]"
      
    dependencies: ["M0"]
```

### Step 3: Break Down Tasks

```yaml
tasks:
  - id: "task-001"
    name: "[Task name]"
    workstream: "Foundation"
    milestone: "M0"
    
    description: "[What needs to be done]"
    
    estimation:
      complexity: "simple|medium|complex"
      uncertainty: "low|medium|high"
      optimistic: "[X days]"
      likely: "[Y days]"
      pessimistic: "[Z days]"
      
    dependencies:
      tasks: ["[task-ids]"]
      external: ["[External dependencies]"]
      
    skills_required: ["[Skill 1]", "[Skill 2]"]
    assignee: "[TBD or person]"
    
    acceptance_criteria:
      - "[Criterion 1]"
      - "[Criterion 2]"
      
    risks:
      - risk: "[Task-specific risk]"
        mitigation: "[How to handle]"
```

### Step 4: Determine Build Order

```yaml
build_order:
  principles:
    - "Walking skeleton first (thin end-to-end slice)"
    - "High-risk technical validation early"
    - "Core user value before nice-to-haves"
    - "Integration points early, not at end"
    
  sequence:
    - phase: "Foundation"
      what: ["Repo", "CI/CD", "Environments", "Schema"]
      why: "Team can work and deploy"
      
    - phase: "Walking Skeleton"
      what: ["[Minimal flow end-to-end]"]
      why: "Proves architecture works, unblocks parallel work"
      
    - phase: "Risk Validation"
      what: ["[Spikes]", "[Highest risk integrations]"]
      why: "Know early if we need to pivot"
      
    - phase: "Core Features"
      what: ["[Primary features]"]
      why: "Main user value"
      parallel_tracks:
        - track: "Backend"
          tasks: ["[tasks]"]
        - track: "Frontend"
          tasks: ["[tasks]"]
          
    - phase: "Hardening"
      what: ["Testing", "Observability", "Security", "Performance"]
      why: "Production readiness"
      
    - phase: "Launch Prep"
      what: ["Documentation", "Runbooks", "Training"]
      why: "Operational readiness"
```

### Step 5: Resource Planning

```yaml
resources:
  team:
    - role: "[Role]"
      count: [N]
      allocation: "[Full-time / Partial]"
      when_needed: "[Phase/dates]"
      skills: ["[Skills]"]
      
  infrastructure:
    - resource: "[Cloud resources]"
      when_needed: "[Phase]"
      estimated_cost: "[Monthly cost]"
      
  external:
    - dependency: "[External team/vendor]"
      what_needed: "[Deliverable]"
      when_needed: "[Date]"
      status: "[Confirmed/Pending]"
```

### Step 6: Risk-Adjusted Timeline

```yaml
timeline:
  start_date: "[Date]"
  
  phases:
    - phase: "Foundation"
      start: "[Date]"
      end: "[Date]"
      buffer: "[X days]"
      confidence: "high"
      
    - phase: "Core Development"
      start: "[Date]"
      end: "[Date]"
      buffer: "[X days]"
      confidence: "medium"
      risks_factored: ["[Risk 1]", "[Risk 2]"]
      
  milestones_summary:
    | Milestone | Target | Confidence | Risk Buffer |
    |-----------|--------|------------|-------------|
    | M0 | [Date] | High | 2 days |
    | M1 | [Date] | Medium | 5 days |
    | MVP | [Date] | Medium | 10 days |
    
  critical_path:
    - "[Task/milestone that can't slip]"
    
  target_completion: "[Date]"
  worst_case: "[Date]"
```

## Output

```yaml
# Implementation plan output

implementation_plan:
  summary:
    total_estimated_effort: "[X person-weeks]"
    timeline: "[Start] to [End]"
    team_size: "[N people]"
    milestone_count: [N]
    task_count: [N]
    
  workstreams:
    - name: "Foundation"
      tasks: 8
      effort: "2 weeks"
      owner: "DevOps"
      
    - name: "Core Domain"
      tasks: 15
      effort: "6 weeks"
      owner: "Backend team"
      
    # ... etc

  milestones:
    # Full milestone definitions as above
    
  tasks:
    # Full task breakdown as above
    
  build_order:
    # Sequencing as above
    
  resources:
    # Resource plan as above
    
  timeline:
    # Timeline as above

documents:
  - filename: "implementation-plan.md"
    content: |
      # Implementation Plan: [Project]
      
      **Created:** [Date]
      **Architecture:** [Selected option]
      **Timeline:** [Start] - [Target]
      **Team:** [Size]
      
      ## Executive Summary
      
      [Brief overview of approach and timeline]
      
      ## Milestones
      
      ### M0: Foundation ([Target date])
      
      **Goal:** [Description]
      
      **Deliverables:**
      - [ ] [Deliverable 1]
      - [ ] [Deliverable 2]
      
      **Exit Criteria:**
      - [Criterion 1]
      - [Criterion 2]
      
      **Risks:** [Milestone risks]
      
      ---
      
      ### M1: Walking Skeleton ([Target date])
      
      [Same structure]
      
      ---
      
      ## Task Breakdown
      
      ### Foundation
      
      | ID | Task | Est | Owner | Deps | Status |
      |----|------|-----|-------|------|--------|
      | T001 | [Task] | [Days] | [Who] | - | Not Started |
      | T002 | [Task] | [Days] | [Who] | T001 | Not Started |
      
      ### Core Domain
      
      | ID | Task | Est | Owner | Deps | Status |
      |----|------|-----|-------|------|--------|
      | T010 | [Task] | [Days] | [Who] | M0 | Not Started |
      
      ---
      
      ## Build Order
      
      ```mermaid
      gantt
        title Implementation Timeline
        dateFormat YYYY-MM-DD
        
        section Foundation
        Repo & CI/CD     :a1, [start], 5d
        Environments     :a2, after a1, 3d
        Schema           :a3, after a1, 4d
        
        section Core
        Walking Skeleton :b1, after a2 a3, 5d
        Feature 1        :b2, after b1, 10d
        Feature 2        :b3, after b1, 8d
      ```
      
      ### Parallel Tracks
      
      After walking skeleton, these can proceed in parallel:
      
      - **Backend track:** [Tasks]
      - **Frontend track:** [Tasks]
      - **Integration track:** [Tasks]
      
      ---
      
      ## Resource Requirements
      
      | Role | Count | When | Notes |
      |------|-------|------|-------|
      | [Role] | [N] | [Phase] | [Notes] |
      
      ### Infrastructure Costs
      
      | Resource | Monthly Est | When Needed |
      |----------|-------------|-------------|
      | [Resource] | $[X] | [Phase] |
      
      ---
      
      ## Timeline
      
      | Phase | Start | End | Confidence |
      |-------|-------|-----|------------|
      | Foundation | [Date] | [Date] | High |
      | Core Dev | [Date] | [Date] | Medium |
      | Hardening | [Date] | [Date] | Medium |
      | Launch | [Date] | [Date] | Medium |
      
      **Critical Path:** [Key dependencies]
      
      **Target Launch:** [Date]
      **With Risk Buffer:** [Date]
      
      ---
      
      ## Risks to Plan
      
      | Risk | Impact on Plan | Mitigation |
      |------|----------------|------------|
      | [Risk] | [Effect] | [Action] |
      
      ---
      
      ## Open Questions
      
      - [ ] [Question affecting plan]
      
      ---
      
      ## Success Criteria
      
      MVP is complete when:
      - [ ] [Criterion]
      - [ ] [Criterion]

state_updates:
  implementation:
    milestones: ["M0", "M1", "M2"]
    current_milestone: "M0"
    total_tasks: [N]
    estimated_completion: "[Date]"
    
  next_steps:
    - "Review plan with team"
    - "Assign task owners"
    - "Set up project tracking"
```

## Guidelines

### Estimation

- Use ranges, not points
- Factor in uncertainty
- Include buffer for unknowns
- Be honest about confidence

### Dependencies

- Identify blocking dependencies early
- Look for opportunities to parallelize
- External dependencies are high risk — flag them

### Build Order Principles

1. **Walking skeleton first** — proves architecture, unblocks team
2. **High risk early** — spikes, hard integrations
3. **Value early** — something usable as soon as possible
4. **Integration early** — not at the end

### Milestone Design

- Each milestone is demonstrable
- Clear exit criteria (testable)
- 2-4 week cadence typically
- Buffer between milestones

## Error Handling

### Unrealistic Timeline

```yaml
timeline_issue:
  detected: "Tasks don't fit in timeline"
  options:
    - "Reduce scope (flag what to cut)"
    - "Add resources (if available)"
    - "Extend timeline (with impact)"
    - "Increase parallelization (if possible)"
  present_to_user: true
```

### Missing Skills

```yaml
skill_gap:
  detected: "Tasks require skills team doesn't have"
  options:
    - "Training time (add to plan)"
    - "Hire/contract (timeline impact)"
    - "Simplify approach (different tech)"
  flag_for_user: true
```
