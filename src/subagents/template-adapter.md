# Template Adapter Subagent

Map generated architecture artifacts to company-specific templates without losing content.

## Purpose

Companies have their own formats for:
- Architecture Decision Records
- Design documents
- Technical specs
- Security reviews
- Project proposals

This subagent takes our generated content and adapts it to their templates while preserving all information.

## Input

1. **Generated artifacts** — architecture-package.md, research docs, etc.
2. **Company template(s)** — user-provided template files or descriptions
3. **Mapping hints** — user guidance on what goes where

## Process

### Step 1: Analyze Company Template

Identify structure and required sections:

```yaml
template_analysis:
  name: "[Template name]"
  type: "[ADR, design doc, RFC, tech spec, etc.]"
  
  sections:
    - name: "[Section name]"
      required: true/false
      description: "[What goes here]"
      maps_to: "[Our content source]"
      
  fields:
    - name: "[Field name]"
      type: "[text, date, dropdown, etc.]"
      maps_to: "[Our data source]"
      
  style:
    tone: "[formal, casual, technical]"
    length: "[brief, detailed, comprehensive]"
    formatting: "[markdown, plain, specific format]"
```

### Step 2: Create Content Mapping

Map our generated content to template sections:

```yaml
content_mapping:
  # What we have → Where it goes
  
  from_architecture_package:
    - source: "Executive Summary"
      target: "[Template section]"
      transform: "[Any adaptation needed]"
      
    - source: "ADR-001: Architecture Style"
      target: "[Template ADR section]"
      transform: "Reformat to company ADR structure"
      
    - source: "System Diagrams"
      target: "[Template diagrams section]"
      transform: "None - diagrams transfer directly"
      
  from_research:
    - source: "Technology findings"
      target: "[Template evaluation section]"
      transform: "Summarize for exec audience"
      
  unmapped_content:
    - content: "[Content with no template home]"
      recommendation: "Add as appendix / Create supplementary doc"
      
  template_gaps:
    - section: "[Template section we can't fill]"
      reason: "[Why - missing info, not applicable]"
      action: "[Flag for user, skip, ask for input]"
```

### Step 3: Generate Adapted Documents

Create documents in company format:

```yaml
adapted_documents:
  - filename: "[company-template-name].md"
    template_used: "[Template name]"
    content: |
      [Full document in company format]
      
    preserved_from_original:
      - "[What content was kept]"
      
    adapted:
      - "[What was reformatted]"
      
    added:
      - "[What template required that we generated]"
      
    notes:
      - "[Any caveats or gaps]"
```

## Output

```yaml
# Template adaptation output

template_analysis:
  name: "Company Design Doc Template"
  sections_identified: 12
  coverage: "85%"  # How much we can fill
  gaps: ["Budget section", "Legal review"]

adapted_documents:
  - filename: "design-doc-healthtrack.md"
    content: |
      # Design Document: HealthTrack
      
      <!-- Company header format -->
      **Author:** [To be filled]
      **Reviewers:** [To be filled]
      **Status:** Draft
      **Last Updated:** [Date]
      
      ## 1. Overview
      
      <!-- Mapped from: Executive Summary -->
      [Adapted content from our summary]
      
      ## 2. Goals and Non-Goals
      
      ### Goals
      <!-- Mapped from: Requirements.functional -->
      - [Goal 1]
      - [Goal 2]
      
      ### Non-Goals
      <!-- Generated based on scope discussions -->
      - [Non-goal 1]
      
      ## 3. Background
      
      <!-- Mapped from: Context section -->
      [Background content]
      
      ## 4. Proposed Solution
      
      <!-- Mapped from: Architecture selection + ADRs -->
      [Solution description]
      
      ### 4.1 System Architecture
      
      <!-- Mapped from: System Diagrams -->
      [Diagrams]
      
      ### 4.2 Key Decisions
      
      <!-- Mapped from: ADRs, reformatted -->
      
      #### Decision 1: [Title]
      
      **Context:** [From ADR]
      **Decision:** [From ADR]
      **Alternatives:** [From ADR]
      **Trade-offs:** [From ADR consequences]
      
      ## 5. Security Considerations
      
      <!-- Mapped from: Security section -->
      [Security content]
      
      ## 6. Testing Strategy
      
      <!-- Mapped from: Testing Strategy -->
      [Testing content]
      
      ## 7. Rollout Plan
      
      <!-- Mapped from: Implementation milestones -->
      [Rollout phases]
      
      ## 8. Open Questions
      
      <!-- Mapped from: Open Questions + Gaps -->
      - [Question 1]
      
      ## Appendix A: Detailed Research
      
      <!-- Content that didn't fit main template -->
      [Research details]

  - filename: "adr-001-architecture-style.md"
    template_used: "Company ADR Template"
    content: |
      # ADR-001: Architecture Style Selection
      
      <!-- Company ADR format -->
      
      | Field | Value |
      |-------|-------|
      | Status | Accepted |
      | Decision Date | [Date] |
      | Decision Makers | [To be filled] |
      | Consulted | [To be filled] |
      | Informed | [To be filled] |
      
      ## Context
      
      [Adapted from our ADR context]
      
      ## Decision
      
      [Adapted from our ADR decision]
      
      ## Consequences
      
      ### Positive
      - [From our ✅ items]
      
      ### Negative
      - [From our ⚠️ items]
      
      ### Risks
      - [From our risks]
      
      ## Alternatives Considered
      
      | Alternative | Pros | Cons | Why Not |
      |-------------|------|------|---------|
      | [Alt 1] | [Pros] | [Cons] | [Reason] |
      
      ## References
      
      - [Links to research docs]

preservation_report:
  original_content_preserved: "100%"
  reformatted_sections: 8
  new_sections_generated: 2
  gaps_flagged: 2
  
  content_locations:
    - original: "ADR-001"
      now_in: ["design-doc Section 4.2", "adr-001-architecture-style.md"]
    - original: "Technology Research"
      now_in: ["design-doc Appendix A"]
      
  user_action_needed:
    - section: "Budget"
      action: "Fill in cost estimates"
    - section: "Reviewers"
      action: "Add team members"
```

## Guidelines

### Content Preservation

Never lose information:
- If template has no place for content, add appendix
- If reformatting, keep all details
- Create supplementary docs if needed
- Track where each piece of original content went

### Template Adaptation

- Match tone and style of company template
- Use their terminology (e.g., "RFC" vs "ADR")
- Follow their section ordering
- Include their required fields (flag if can't fill)

### Gap Handling

When template needs info we don't have:
```yaml
gap_handling:
  - gap: "Budget section required"
    options:
      - "Flag as [TO BE FILLED]"
      - "Ask user for input"
      - "Generate estimate if possible"
    chosen: "[Option]"
```

### Multiple Templates

If user provides multiple templates:
```yaml
multi_template:
  - template: "Design Doc"
    use_for: "Main architecture document"
    
  - template: "ADR Template"  
    use_for: "Each decision (generate multiple)"
    
  - template: "Security Review"
    use_for: "Security section expanded"
```

## Error Handling

### Template Not Parseable

```yaml
fallback:
  action: "Ask user to describe template structure"
  questions:
    - "What are the main sections?"
    - "What's required vs optional?"
    - "Any specific formatting rules?"
```

### Major Mismatch

```yaml
mismatch:
  situation: "Template expects content we don't have"
  action:
    - Flag clearly what's missing
    - Suggest how to fill gaps
    - Offer to gather missing info
```

## Example: Adapting to Google Design Doc

**Company template sections:**
1. Overview
2. Context
3. Goals/Non-Goals
4. Proposed Design
5. Alternatives Considered
6. Cross-cutting Concerns
7. Open Questions

**Mapping:**
```yaml
mapping:
  - template: "Overview"
    source: "Executive Summary"
    
  - template: "Context"
    source: "Context + Background from ADRs"
    
  - template: "Goals/Non-Goals"
    source: "Requirements.functional → Goals"
    generate: "Non-goals from scope discussions"
    
  - template: "Proposed Design"
    source: "Architecture selection + Diagrams + Component specs"
    
  - template: "Alternatives Considered"
    source: "Other options from proposal + ADR alternatives"
    
  - template: "Cross-cutting Concerns"
    source: "Security + Observability + Testing"
    
  - template: "Open Questions"
    source: "Gaps.remaining + Open Questions"
```
