# Validate CLAUDE.md Subagent

Validate generated CLAUDE.md for completeness, consistency, and appropriate scale.

## Purpose

Ensure CLAUDE.md:
- Covers everything decided in discovery
- Is internally consistent
- Has appropriate depth for project complexity
- Will actually help Claude write good code
- Doesn't have gaps that cause confusion

## Input

1. **Generated CLAUDE.md** — from generate-claude-md
2. **Discovery state** — full context
3. **Architecture package** — decisions, tech stack
4. **Project complexity** — derived from discovery

## Complexity Detection

First, classify project scale:

```yaml
complexity_indicators:
  minimal:  # 1-2 week project, 1-2 devs
    triggers:
      - "internal tool"
      - "simple CRUD"
      - "prototype"
      - "MVP"
      - team_size <= 2
      - timeline <= "2 weeks"
      - no compliance requirements
      - single database
      - no external integrations
    claude_md_size: "~50-100 lines"
    sections_required:
      - "Project Context (brief)"
      - "Tech Stack (table only)"
      - "Code Conventions (minimal)"
      - "Quick Reference"
    sections_optional: "all others"
    
  standard:  # 1-3 month project, 3-6 devs
    triggers:
      - team_size 3-6
      - timeline 1-3 months
      - 1-2 external integrations
      - standard compliance (SOC2)
    claude_md_size: "~200-400 lines"
    sections_required:
      - "Project Context"
      - "Architecture"
      - "Tech Stack"
      - "Code Conventions"
      - "Patterns to Follow"
      - "Key Decisions"
      - "Database"
      - "Testing"
      - "Quick Reference"
    sections_optional:
      - "Security (if compliance)"
      - "API Design (if API-heavy)"
      
  complex:  # 3-6 month project, 6-12 devs
    triggers:
      - team_size 6-12
      - timeline 3-6 months
      - multiple integrations
      - compliance requirements
      - multi-tenant
    claude_md_size: "~400-600 lines"
    sections_required: "all standard + Security + Observability"
    
  enterprise:  # 6+ month, 12+ devs, strict compliance
    triggers:
      - team_size > 12
      - timeline > 6 months
      - strict compliance (HIPAA, PCI, FedRAMP)
      - multiple teams
      - complex integrations
    claude_md_size: "~600-1000 lines"
    sections_required: "all sections, comprehensive"
    additional:
      - "Team boundaries"
      - "Service ownership"
      - "Cross-team conventions"
```

## Validation Checks

### 1. Completeness Check

```yaml
completeness:
  # Every decision should be reflected
  decisions_coverage:
    - check: "Each ADR mentioned in Key Decisions"
      source: architecture_package.adrs
      target: claude_md.key_decisions
      
    - check: "Tech stack matches"
      source: architecture_package.tech_stack
      target: claude_md.tech_stack
      
    - check: "Patterns from domain_context included"
      source: discovery_state.domain_context.patterns
      target: claude_md.patterns_to_follow
      
  # Required for complexity level
  sections_present:
    - check: "All required sections for [complexity] present"
      missing: ["list of missing sections"]
      
  # No placeholder TODOs for critical sections
  no_critical_placeholders:
    - check: "No TODO in: Tech Stack, Code Conventions, Security"
      found: ["list of problematic TODOs"]
```

### 2. Consistency Check

```yaml
consistency:
  # Tech stack consistent throughout
  tech_references:
    - check: "Code examples use declared technologies"
      issue: "Example uses Flask but tech stack says FastAPI"
      
  # Patterns don't contradict
  pattern_consistency:
    - check: "Patterns don't contradict each other"
    - check: "Anti-patterns don't conflict with patterns"
    
  # Naming conventions followed in examples
  convention_adherence:
    - check: "Code examples follow stated naming conventions"
    - check: "File structure examples match conventions"
```

### 3. Scale Appropriateness Check

```yaml
scale_check:
  too_heavy:
    detected: "CLAUDE.md is 800 lines for a 2-week prototype"
    issue: "Overwhelming for simple project"
    fix: "Trim to essential sections only"
    
  too_light:
    detected: "CLAUDE.md is 100 lines for HIPAA enterprise project"
    issue: "Missing critical guidance"
    fix: "Expand Security, Compliance, Observability sections"
    
  right_sized:
    minimal: "50-150 lines"
    standard: "200-400 lines"
    complex: "400-700 lines"
    enterprise: "600-1000 lines"
```

### 4. Actionability Check

```yaml
actionability:
  # Code examples present where needed
  examples_present:
    - section: "Patterns to Follow"
      required: "code example for each pattern"
      
    - section: "Anti-Patterns"
      required: "bad code + good code for each"
      
    - section: "Logging"
      required: "example of correct log statement"
      
  # Commands are complete
  commands_runnable:
    - check: "Quick Reference commands are complete"
    - check: "No placeholder [command] without actual command"
    
  # Steps are specific
  steps_specific:
    - check: "Common Tasks have actual steps, not placeholders"
```

### 5. Safety Check (for compliance projects)

```yaml
safety_check:
  applies_when: "compliance requirements present"
  
  checks:
    - check: "Security section covers all compliance requirements"
      source: discovery_state.constraints.compliance
      
    - check: "PHI/PII handling explicitly documented"
      required_if: "HIPAA or GDPR"
      
    - check: "Audit logging mentioned"
      required_if: "any compliance"
      
    - check: "Encryption requirements documented"
      
    - check: "Anti-patterns include compliance violations"
```

## Output

```yaml
validation_result:
  status: "pass|warnings|fail"
  complexity_detected: "standard"
  claude_md_lines: 320
  expected_range: "200-400 lines"
  scale_appropriate: true
  
  completeness:
    score: "95%"
    missing:
      - section: "Troubleshooting"
        severity: "low"
        recommendation: "Add common issues section"
        
  consistency:
    score: "100%"
    issues: []
    
  actionability:
    score: "90%"
    issues:
      - section: "Common Tasks: Adding a Migration"
        issue: "Placeholder command"
        fix: "Replace [command] with actual alembic command"
        
  safety:
    score: "100%"
    issues: []
    
  recommendations:
    required:
      - "Add actual migration command to Common Tasks"
    suggested:
      - "Consider adding Troubleshooting section"
      
  final_verdict: "CLAUDE.md is ready for use with minor fixes"

documents:
  - filename: "claude-md-validation.md"
    content: |
      # CLAUDE.md Validation Report
      
      **Status:** ✅ Pass (with suggestions)
      **Complexity:** Standard
      **Size:** 320 lines (expected: 200-400) ✓
      
      ## Completeness: 95%
      
      ✅ All ADRs covered
      ✅ Tech stack complete
      ✅ All required sections present
      ⚠️ Missing: Troubleshooting (optional for standard)
      
      ## Consistency: 100%
      
      ✅ Code examples match tech stack
      ✅ Patterns are internally consistent
      ✅ Naming conventions followed in examples
      
      ## Actionability: 90%
      
      ✅ Pattern examples present
      ✅ Anti-pattern examples present
      ⚠️ Common Tasks: Migration command is placeholder
      
      ## Safety: 100%
      
      ✅ Security section covers SOC2 requirements
      ✅ Logging guidance includes what not to log
      
      ## Required Fixes
      
      1. Replace placeholder migration command:
         ```bash
         # Change from:
         [command to create migration]
         
         # To:
         alembic revision --autogenerate -m "description"
         ```
      
      ## Suggested Improvements
      
      1. Add Troubleshooting section with common issues
      2. Add example error response to API Design
      
      ## Verdict
      
      CLAUDE.md is **ready for use** after addressing the 
      migration command placeholder.
```

## Scaling Recommendations

When validation detects scale mismatch:

### Scaling DOWN (too heavy for project)

```yaml
scale_down:
  detected: "Enterprise-level CLAUDE.md for minimal project"
  
  recommendations:
    - action: "Remove sections"
      sections:
        - "Multi-Stakeholder" 
        - "Detailed Observability"
        - "Comprehensive Security"
        - "External Services (if none)"
        
    - action: "Simplify sections"
      changes:
        - "Architecture: Remove container diagram, keep overview only"
        - "Testing: Remove load testing, keep unit/integration"
        - "Code Conventions: Keep to single page"
        
    - action: "Merge sections"
      merge:
        - "Combine Database + API into single Backend section"
        
  target: "Aim for 50-150 lines total"
```

### Scaling UP (too light for project)

```yaml
scale_up:
  detected: "Minimal CLAUDE.md for enterprise project"
  
  recommendations:
    - action: "Add missing sections"
      required:
        - "Security (comprehensive)"
        - "Observability (detailed)"
        - "Multi-tenancy patterns"
        - "Compliance reminders"
        
    - action: "Expand existing sections"
      changes:
        - "Architecture: Add all C4 diagrams"
        - "Testing: Add load testing, security testing"
        - "API Design: Add versioning, rate limiting"
        - "Database: Add migration strategy, backup"
        
    - action: "Add enterprise sections"
      sections:
        - "Team Boundaries"
        - "Service Ownership"
        - "Incident Response"
        - "On-Call Expectations"
        
  target: "Aim for 600-1000 lines"
```

## Auto-Fix Capability

For common issues, offer to fix automatically:

```yaml
auto_fixable:
  - issue: "Placeholder commands"
    fix: "Replace with actual commands based on tech stack"
    
  - issue: "Missing code examples"
    fix: "Generate examples from patterns + tech stack"
    
  - issue: "Scale mismatch"
    fix: "Add/remove sections based on complexity"
    
  - issue: "Inconsistent tech references"
    fix: "Update all examples to match declared stack"

prompt_user: |
  Found [N] auto-fixable issues:
  1. [Issue]: [Fix description]
  2. [Issue]: [Fix description]
  
  Would you like me to apply these fixes?
```

## Error Handling

### CLAUDE.md Not Generated

```yaml
not_generated:
  action: "Cannot validate - CLAUDE.md missing"
  recommendation: "Run generate-claude-md first"
```

### Missing Source Documents

```yaml
missing_sources:
  action: "Cannot fully validate without source docs"
  partial_validation: true
  note: "Some consistency checks skipped"
```

## Notes

- Validation should be fast (no web searches needed)
- Offer auto-fix for common issues
- Be specific about what's wrong and how to fix
- Scale-appropriateness is as important as completeness
- A minimal project shouldn't have enterprise CLAUDE.md
