# Plan Validation Subagent

Validate implementation plan against normative documentation, standards, and best practices.

## Purpose

Ensure the implementation plan:
- Follows company/industry standards
- Meets compliance requirements
- Includes required checkpoints
- Aligns with security policies
- Addresses operational requirements

## Input

1. **Implementation plan** — from plan-implementation subagent
2. **Normative documents** — user-provided standards, policies, checklists
3. **Architecture package** — for context on what's being built
4. **Compliance requirements** — from discovery state

## Types of Normative Documents

```yaml
normative_types:
  company_standards:
    - "Development standards"
    - "Architecture guidelines"
    - "Security policies"
    - "Code review requirements"
    - "Testing standards"
    - "Release management process"
    
  compliance_frameworks:
    - "HIPAA requirements"
    - "PCI-DSS controls"
    - "SOC2 criteria"
    - "GDPR requirements"
    - "FedRAMP controls"
    
  industry_standards:
    - "OWASP guidelines"
    - "NIST frameworks"
    - "ISO standards"
    - "Cloud provider best practices"
    
  operational:
    - "Incident response requirements"
    - "SLA definitions"
    - "On-call requirements"
    - "Monitoring standards"
```

## Process

### Step 1: Analyze Normative Documents

Extract checkpoints and requirements:

```yaml
normative_analysis:
  documents_analyzed:
    - name: "[Document name]"
      type: "[Type]"
      requirements_extracted: [N]
      
  requirements:
    - id: "REQ-001"
      source: "[Document]"
      category: "security|compliance|process|operational"
      requirement: "[What's required]"
      applies_to: "[What parts of plan]"
      verification: "[How to verify compliance]"
      mandatory: true/false
      
  checkpoints:
    - id: "CHK-001"
      source: "[Document]"
      when: "[Phase/milestone]"
      what: "[What must happen]"
      who: "[Who approves]"
      
  gates:
    - id: "GATE-001"
      name: "[Gate name]"
      before: "[Phase/milestone]"
      criteria:
        - "[Criterion 1]"
        - "[Criterion 2]"
      approvers: ["[Role 1]"]
```

### Step 2: Map Requirements to Plan

Check coverage:

```yaml
coverage_analysis:
  covered:
    - requirement: "REQ-001"
      covered_by: "[Task/milestone]"
      evidence: "[How it's addressed]"
      
  gaps:
    - requirement: "REQ-005"
      gap: "[What's missing]"
      severity: "critical|high|medium|low"
      recommendation: "[How to address]"
      
  conflicts:
    - requirement: "REQ-010"
      plan_element: "[Task/decision]"
      conflict: "[What conflicts]"
      resolution_options:
        - "[Option 1]"
        - "[Option 2]"
```

### Step 3: Identify Missing Checkpoints

```yaml
checkpoint_gaps:
  missing_checkpoints:
    - checkpoint: "Security review before production"
      source: "[Normative doc]"
      should_be_at: "[Milestone/phase]"
      impact: "Cannot deploy without this"
      
  missing_gates:
    - gate: "Architecture review"
      source: "[Normative doc]"
      required_before: "[Phase]"
      current_plan: "Not included"
      recommendation: "Add after M1"
      
  missing_approvals:
    - approval: "[Approval type]"
      required_by: "[Document]"
      missing_from: "[Task/milestone]"
```

### Step 4: Compliance Verification

For compliance-heavy projects:

```yaml
compliance_check:
  framework: "[HIPAA/PCI/SOC2/etc]"
  
  controls:
    - control_id: "[ID]"
      control: "[Description]"
      required: true
      plan_coverage:
        addressed: true/false
        how: "[Task/milestone]"
        evidence: "[What demonstrates compliance]"
        
  gaps:
    - control_id: "[ID]"
      gap: "[What's missing]"
      risk: "[Risk of non-compliance]"
      remediation:
        task: "[What to add]"
        effort: "[Estimate]"
        milestone: "[Where to add]"
```

### Step 5: Generate Validation Report

```yaml
validation_report:
  summary:
    documents_checked: [N]
    requirements_verified: [N]
    gaps_found: [N]
    critical_gaps: [N]
    plan_compliant: true/false
    
  status: "compliant|gaps_found|critical_issues"
  
  details:
    # Full analysis as above
```

## Output

```yaml
# Plan validation output

validation:
  summary:
    status: "gaps_found"
    documents_analyzed: 3
    requirements_checked: 45
    covered: 38
    gaps: 7
    critical_gaps: 2
    
  normative_sources:
    - name: "Company Security Policy v2.3"
      requirements_extracted: 15
      covered: 12
      gaps: 3
      
    - name: "HIPAA Implementation Guide"
      requirements_extracted: 20
      covered: 18
      gaps: 2
      
    - name: "Release Management Process"
      requirements_extracted: 10
      covered: 8
      gaps: 2

  coverage_report:
    covered:
      - requirement: "All code must have security review"
        source: "Security Policy"
        covered_by: "Task T025: Security review"
        status: "✅ Covered"
        
      - requirement: "PHI must be encrypted at rest"
        source: "HIPAA Guide"
        covered_by: "ADR-002: PostgreSQL with encryption"
        status: "✅ Covered"
        
    gaps:
      - requirement: "Penetration test before production"
        source: "Security Policy"
        severity: "critical"
        current_plan: "Not included"
        recommendation:
          action: "Add penetration test task"
          milestone: "Before M3 (Launch)"
          effort: "1 week + external vendor"
          cost: "$5-15K for external pen test"
          
      - requirement: "Architecture review board approval"
        source: "Company Standards"
        severity: "high"
        current_plan: "Not included"
        recommendation:
          action: "Add ARB review gate"
          when: "After M1, before major development"
          effort: "2 weeks for review cycle"
          
      - requirement: "Disaster recovery test"
        source: "HIPAA Guide"
        severity: "high"
        current_plan: "Not included"
        recommendation:
          action: "Add DR test task"
          milestone: "Before launch"
          effort: "3-5 days"

  missing_checkpoints:
    - checkpoint: "Security review gate"
      add_before: "M2: Core Features"
      criteria:
        - "SAST scan clean"
        - "Dependency scan clean"
        - "Security team sign-off"
        
    - checkpoint: "Compliance verification"
      add_before: "M3: Launch"
      criteria:
        - "HIPAA checklist complete"
        - "BAAs in place"
        - "Audit logging verified"

  compliance_matrix:
    framework: "HIPAA"
    status: "2 gaps"
    
    | Control | Requirement | Status | Gap |
    |---------|-------------|--------|-----|
    | Access Control | Unique user IDs | ✅ | - |
    | Audit | Log all PHI access | ✅ | - |
    | Encryption | Encrypt PHI at rest | ✅ | - |
    | DR | Test disaster recovery | ❌ | Add DR test |
    | Pen Test | Annual penetration test | ❌ | Add pen test |

  plan_adjustments:
    required:
      - adjustment: "Add penetration test"
        effort: "+1 week"
        cost: "+$10K"
        blocks: "Production deployment"
        
      - adjustment: "Add ARB review"
        effort: "+2 weeks"
        cost: "-"
        blocks: "Major development start"
        
    recommended:
      - adjustment: "Add security champion role"
        effort: "Ongoing"
        benefit: "Faster security reviews"
        
  timeline_impact:
    current_target: "[Date]"
    with_required_adjustments: "[Date + X weeks]"
    reason: "Security gates and compliance tasks"

documents:
  - filename: "validation-report.md"
    content: |
      # Plan Validation Report
      
      **Project:** [Name]
      **Date:** [Date]
      **Status:** ⚠️ Gaps Found (2 Critical)
      
      ## Summary
      
      Validated implementation plan against:
      - Company Security Policy v2.3
      - HIPAA Implementation Guide
      - Release Management Process
      
      **Result:** 7 gaps identified, 2 critical
      
      ## Critical Gaps
      
      ### 1. Missing Penetration Test
      
      **Requirement:** Security Policy §4.2 requires penetration testing before production deployment.
      
      **Current Plan:** Not included
      
      **Impact:** Cannot deploy to production without this.
      
      **Remediation:**
      - Add pen test task before launch milestone
      - Effort: 1 week + vendor coordination
      - Cost: $5-15K
      - Timeline impact: +1 week
      
      ### 2. Missing Architecture Review
      
      **Requirement:** Company Standards require ARB approval for new systems.
      
      **Current Plan:** Not included
      
      **Impact:** May need to rework after review.
      
      **Remediation:**
      - Add ARB review gate after M1
      - Effort: 2 week review cycle
      - Timeline impact: +2 weeks
      
      ## Other Gaps
      
      | Gap | Severity | Source | Remediation |
      |-----|----------|--------|-------------|
      | DR test missing | High | HIPAA | Add before launch |
      | [Gap] | Medium | [Source] | [Fix] |
      
      ## Compliance Status
      
      ### HIPAA
      
      | Control | Status |
      |---------|--------|
      | Access Control | ✅ |
      | Audit Logging | ✅ |
      | Encryption | ✅ |
      | Disaster Recovery | ❌ Gap |
      | Penetration Test | ❌ Gap |
      
      ## Required Plan Adjustments
      
      1. **Add penetration test** (Critical)
         - When: Before M3
         - Effort: 1 week
         - Owner: Security team
         
      2. **Add ARB review gate** (Critical)
         - When: After M1
         - Effort: 2 weeks
         
      3. **Add DR test** (High)
         - When: Before launch
         - Effort: 3-5 days
         
      ## Timeline Impact
      
      | Scenario | Target Date |
      |----------|-------------|
      | Original plan | [Date] |
      | With required gaps addressed | [Date + 3 weeks] |
      
      ## Checkpoints to Add
      
      ```mermaid
      flowchart LR
        M0[M0: Foundation] --> ARB[ARB Review]
        ARB --> M1[M1: Core]
        M1 --> SEC[Security Gate]
        SEC --> M2[M2: Features]
        M2 --> PEN[Pen Test]
        PEN --> DR[DR Test]
        DR --> COMP[Compliance Check]
        COMP --> M3[M3: Launch]
      ```
      
      ## Approvals Needed
      
      | Gate | Approver | When |
      |------|----------|------|
      | ARB Review | Architecture Board | After M1 |
      | Security Gate | Security Team | Before M2 |
      | Compliance | Compliance Officer | Before Launch |
      
      ## Next Steps
      
      1. Review gaps with team
      2. Update implementation plan
      3. Schedule ARB review
      4. Engage pen test vendor
      5. Re-validate after adjustments

state_updates:
  implementation:
    validation_status: "gaps_found"
    critical_gaps: 2
    timeline_adjusted: true
    new_target: "[Adjusted date]"
    
  risks:
    - risk: "Compliance gaps may delay launch"
      likelihood: "high"
      impact: "high"
      mitigation: "Address critical gaps immediately"
      status: "identified"
      
  next_steps:
    - "Address critical validation gaps"
    - "Re-run validation after plan update"
```

## Guidelines

### Normative Document Analysis

- Extract specific, verifiable requirements
- Note which are mandatory vs recommended
- Identify approval gates and checkpoints
- Map to plan phases where they apply

### Gap Severity

```yaml
severity_criteria:
  critical:
    - "Blocks deployment/launch"
    - "Legal/compliance violation"
    - "Security vulnerability"
    
  high:
    - "May cause significant rework"
    - "Process violation with consequences"
    - "Missing required approval"
    
  medium:
    - "Best practice not followed"
    - "May cause minor rework"
    - "Recommended but not required"
    
  low:
    - "Nice to have"
    - "Optimization opportunity"
```

### Remediation Quality

For each gap:
- Specific action to take
- Where in plan to add it
- Effort estimate
- Who is responsible
- Impact on timeline

### Re-validation

After plan adjustments:
```yaml
re_validation:
  trigger: "Plan updated to address gaps"
  action: "Re-run validation"
  focus: "Verify gaps addressed, check for new issues"
```

## Error Handling

### Unclear Normative Document

```yaml
unclear_document:
  action: "Ask user for clarification"
  questions:
    - "Is [requirement] mandatory or recommended?"
    - "Does [section] apply to this type of project?"
    - "Who approves [gate]?"
```

### Conflicting Requirements

```yaml
conflict:
  situation: "Two normative docs conflict"
  action:
    - Flag the conflict
    - Present both requirements
    - Ask user which takes precedence
    - Document resolution
```

### Excessive Gaps

```yaml
excessive_gaps:
  threshold: "> 10 critical gaps"
  action:
    - Summarize by category
    - Prioritize top issues
    - Suggest phased remediation
    - Consider if project is feasible with current constraints
```
