# EventDiff: Product Brief

## Executive Summary

EventDiff prevents schema breaking changes through automated PR gates with 
**team-specific policy enforcement**. Built to demonstrate platform product 
thinking for data infrastructure roles.

## Problem Space

### The Core Issue
At scale, schema changes create hidden dependencies:
- Team A removes field → Team B's dashboard breaks
- Discovery time: hours to days post-merge
- No visibility at review time

### Current Workarounds
- Manual reviewer knowledge
- Ad-hoc Slack coordination  
- Hope nothing breaks
- Fix in production after incident

### Why This Fails at Scale
- 100+ teams changing schemas
- 1000s of events in production
- Reviewer knowledge doesn't scale
- Incidents are expensive

## Solution Architecture

### Three Layers

**1. Detection Layer**
- Git-based schema diffing
- Classification: BLOCK / WARN / PASS
- Rules: type changes, required fields, enum values

**2. Operational Layer**  
- Ownership mapping (event → team)
- Responsibility routing
- Clear action items

**3. Governance Layer**
- Team-specific policy config
- Same change = different enforcement per team
- Configurable, not hardcoded

### Key Insight: Configurability

The **differentiator** is team-specific enforcement:

| Change | Checkout Team | Payments Team |
|--------|---------------|---------------|
| required → optional | WARN | BLOCK |

**Why this matters:**
- Checkout team: move fast, accept some risk
- Payments team: zero tolerance for breaking changes
- **Platform serves both** without compromise

## Success Metrics

### Primary
- **# breaking changes blocked pre-merge** (weekly/monthly)
- **MTTD for schema issues:** Pre-merge (seconds) vs post-release (hours/days)

### Secondary  
- **Policy override rate:** % teams customizing defaults
- **Noise rate:** % PRs flagged that teams override (too strict?)
- **Adoption velocity:** # teams onboarded per week

### Platform Health
- Reduction in schema-related incidents
- Reduction in emergency rollbacks
- Increased schema change velocity (safe to move fast)

## Roadmap

### V1 (MVP - Built)
- ✅ Core diff engine
- ✅ CI integration (GitHub Actions)
- ✅ Ownership mapping
- ✅ Team-specific policy config

### V2 (Operational Automation)
- Auto-request reviewers via GitHub API
- PR comment with change summary
- Suggested migration paths per rule
- Expand rule set (format, pattern, min/max)

### V3 (Intelligence Layer)
- Impact analysis: "Affects 12 dashboards, 3 pipelines"
- AI-suggested migration patterns
- Historical incident correlation
- Predictive risk scoring

## Why I Built This

### Personal Context
After 3 years building instrumented products (fintech, marketplaces):
- Shipped 40+ A/B tests requiring precise tracking
- Debugged dozens of schema-related breakages
- Observed the pattern: individual validation ≠ platform protection

### The Insight
Most tooling helps engineers send correct events.  
**Missing piece:** preventing one team from breaking another.

This required:
- Mapping the problem space (4 areas: add tracking, validate, govern, detect)
- Finding the non-obvious gap (schema evolution > event validation)
- Building the minimum viable governance (gate + ownership + policy)

### Product Taste Demonstrated
- Small surface area (git diff + exit code)
- High leverage (protects entire platform)
- Operational from day 1 (ownership routing)
- Path to enterprise scale (configurable policy)

## Technical Implementation

**Core:** Node.js CLI (eventdiff.mjs)  
**CI:** GitHub Actions workflow  
**Config:** JSON-based policy + ownership files  
**Architecture:** Git-native (no external dependencies)

## Competitive Context

| Approach | Example | Limitation |
|----------|---------|------------|
| Event validation | EventLint | Individual focus, not platform |
| Manual review | Code review | Doesn't scale, relies on memory |
| Post-merge detection | Monitoring | Too late, incident already happened |
| **EventDiff** | **Pre-merge governance** | **Platform protection at scale** |

## Open Questions (For Discussion)

1. **Override mechanism:** Should teams force-merge breaking changes?
2. **Migration support:** Auto-generate backward-compatible change patterns?
3. **Registry integration:** Pull schemas from central registry vs git?
4. **Notification:** Slack/email to owners when PR blocked?

---

Built by **Ajith Neelakantan**  
Contact: ajithsupsc@gmail.com  
LinkedIn: linkedin.com/in/ajithharinag
```

