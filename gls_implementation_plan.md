# Global Labour Service (GLS) — Implementation Plan

> Interactive reference document. Convert to HTML via Claude Code for GitHub Pages deployment.

---

## Contents

1. [Overview](#overview)
2. [Architecture & Flow](#architecture--flow)
3. [Phase 1 — Central Rules](#phase-1--central-rules)
4. [Phase 2 — AI Discovery & Scoping](#phase-2--ai-discovery--scoping)
5. [Phase 3 — AI Deployment](#phase-3--ai-deployment)
6. [Capabilities by Phase](#capabilities-by-phase)
7. [Component Responsibilities](#component-responsibilities)
8. [Architecture Principles](#architecture-principles)
9. [Risks — Development Resource](#risks--development-resource)
10. [Risks — Productivity Consultancy](#risks--productivity-consultancy)
11. [Risks — Stakeholder Involvement](#risks--stakeholder-involvement)
12. [Stakeholder Map](#stakeholder-map)
13. [Evolution Timeline](#evolution-timeline)

---

## Overview

The Global Labour Service (GLS) is a phased programme to centralise labour rules, generate AI-driven insights, and ultimately automate labour optimisation across all sites. It replaces LP as the configuration source and becomes the control plane for all downstream scheduling and execution systems.

**Three phases. One control plane. Progressive automation.**

| Phase | Name | Outcome |
|-------|------|---------|
| 1 | Central Rules | GLS is single source of truth for all labour rules |
| 2 | AI Discovery | Insights, overrides analysis, scenario modelling |
| 3 | AI Deployment | Automated optimisation and execution |

---

## Architecture & Flow

```
[User (iQ360 UI)]
        │
        ▼
[Micro-Frontend (GLS UI)]
        │
        ▼
[GLS API Layer]
        │
        ├─────────────┬──────────────────┬──────────────────┐
        ▼             ▼                  ▼                  ▼
     [CUP]   [Labour Rules Store]  [SLM Service]     [CFM Service]
  (Access)     (Source of Truth)   (Modelling)        (Execution)
        │             │                  │                  │
        ▼             ▼                  ▼                  ▼
 [User Roles]  [Rules+Versions]    [Scenarios]        [Schedules]
        │             │                  │                  │
        └─────────────┴──────────┬───────┴──────────────────┘
                                 ▼
                      [Insight / Event Layer]
                                 │
                                 ▼
                      [Decision Engine (Optional)]
                                 │
                                 ▼
                         [iQ Alerts / Actions]
                                 │
                                 ▼
                        [Location Manager]
```

### Flow sequence

1. User → iQ360 UI → GLS Micro-Frontend
2. GLS API Layer routes to CUP (access), Labour Rules Store (truth), SLM (modelling), CFM (execution)
3. Outputs aggregate into Insight / Event Layer
4. Decision Engine (Phase 3) orchestrates alerts and actions
5. Location Manager executes at site level

---

## Phase 1 — Central Rules

### Goal

Establish GLS as the single source of truth for labour rules. Remove all dependency on LP for rule configuration.

### Scope

- Move labour rules from LP to GLS
- Rule lifecycle management: create, edit, assign, version, audit
- Multi-tenant + single-tenant UI via iQ360 micro-frontend
- CUP-based access control
- Integration with CFM and SLM for downstream consumption
- Rule assignment notifications and change events

### Capabilities

| Capability | Area | Systems | Status |
|-----------|------|---------|--------|
| Central rules store | Rule management | GLS, CFM, SLM | In scope |
| CUP integration | Access control | CUP, iQ360 | In scope |
| Assignment alerts | Notifications | Decision engine, iQ360 | In scope |

### Success Criteria

- All rules managed in GLS
- LP no longer used for configuration
- Rules consumed by CFM and SLM
- UI fully accessible via iQ360

### Delivery Estimate

| Item | Value |
|------|-------|
| Duration | 12–16 weeks |
| Dev cost | Medium |
| Consultancy need | Rule migration workshops, LP decommission planning |

---

## Phase 2 — AI Discovery & Scoping

### Goal

Enable insight generation and rule performance understanding through data analysis and scenario modelling.

### Scope

- Analyse rule usage and overrides
- Integrate timecards and schedules
- Scenario modelling via SLM
- "What if" comparisons (past vs future rule configurations)
- Insight display via iQ360 event layer

### Capabilities

| Capability | Area | Systems | Status |
|-----------|------|---------|--------|
| Insight generation | Analytics | SLM, LP feeds | Planned |
| What-if comparisons | Scenario modelling | SLM, iQ360 | Planned |

### Success Criteria

- Users can compare rule scenarios
- Insights available in iQ360
- Clear optimisation opportunities identified

### Delivery Estimate

| Item | Value |
|------|-------|
| Duration | 10–14 weeks |
| Dev cost | High |
| Consultancy need | Productivity analysis framework, insight interpretation training |

---

## Phase 3 — AI Deployment

### Goal

Enable automated optimisation and rule execution driven by AI recommendations.

### Scope

- AI-driven rule and schedule recommendations
- Approval-based and automated execution via CFM
- Auto Scheduling and Dynamic Labour integration
- Decision engine orchestration
- Location-level execution via Location Manager

### Capabilities

| Capability | Area | Systems | Status |
|-----------|------|---------|--------|
| AI recommendations | AI optimisation | Decision engine | Planned |
| Auto execution | Automation | CFM, SLM | Planned |
| Labour cost optimisation | Performance | iQ360, Location mgr | Planned |

### Success Criteria

- Improved labour cost %
- Increased schedule efficiency
- Reduced manual intervention

### Delivery Estimate

| Item | Value |
|------|-------|
| Duration | 14–20 weeks |
| Dev cost | Very high |
| Consultancy need | Change management, productivity ROI modelling, exec reporting |

---

## Capabilities by Phase

| Capability | Phase | Area | Key Systems | Flow Step |
|-----------|-------|------|-------------|-----------|
| Central rules store | 1 | Rule management | GLS, CFM, SLM | Rules centralised |
| CUP integration | 1 | Access control | CUP, iQ360 | Access enforced |
| Assignment alerts | 1 | Notifications | Decision engine, iQ360 | Alerts sent |
| Insight generation | 2 | Analytics | SLM, LP feeds | Insights surfaced |
| What-if comparisons | 2 | Scenario modelling | SLM, iQ360 | Scenarios compared |
| AI recommendations | 3 | AI optimisation | Decision engine | Recs generated |
| Auto execution | 3 | Automation | CFM, SLM | Executed |
| Labour cost optimisation | 3 | Performance | iQ360, Location mgr | Optimised |

---

## Component Responsibilities

| Component | Responsibility | Phase active |
|----------|----------------|-------------|
| GLS | Rule source of truth — control plane | 1, 2, 3 |
| CUP | Access control and user role management | 1, 2, 3 |
| SLM | Simulation, scenario modelling, comparison | 1, 2, 3 |
| CFM | Schedule execution and rule application | 1, 2, 3 |
| LP | Data feeds during migration period | 1 |
| iQ360 | UI layer — micro-frontend host | 1, 2, 3 |
| Decision Engine | Alert orchestration and AI recommendations | 3 |
| Location Manager | Site-level execution | 3 |

---

## Architecture Principles

- **GLS = Control plane.** All rule configuration flows through GLS. No bypasses.
- **SLM = Intelligence layer.** Simulation and comparison are SLM responsibilities, not GLS.
- **Execution decoupled from rules.** CFM executes; GLS defines. These are separate concerns.
- **Unified UI via iQ360 micro-frontends.** GLS has no standalone UI — it surfaces inside iQ360.
- **Event-driven downstream.** Insight and alert layers are async. Latency must be designed for.
- **Approval gates before automation.** Phase 3 execution requires configurable approval before auto-apply.

---

## Risks — Development Resource

### R1 — Bandwidth across concurrent phase work
**Severity: High**

Phases share core engineering teams. Without dedicated squads per phase, Phase 1 delays cascade directly into Phase 2 and 3. Timeline commitments made before resource allocation is confirmed are not credible.

**Mitigation:** Define named squads with explicit capacity per phase before programme kick-off. Ring-fence GLS resource from BAU work.

---

### R2 — LP data migration complexity
**Severity: High**

Moving rules from LP to GLS has unknown data quality risk. No clean mapping may exist between LP rule structures and the GLS schema. Discovering this late will break Phase 1 delivery.

**Mitigation:** Run a discovery spike in Week 1–2 of Phase 1 to map LP rule data before committing to a migration timeline. Budget for a data cleansing workstream.

---

### R3 — Event-driven latency between systems
**Severity: Medium**

The Insight and Event Layer introduces async complexity. CFM and SLM dependencies on near-real-time rule state may surface timing issues under load, particularly in high-volume scheduling periods.

**Mitigation:** Define SLA for event propagation in Phase 1 architecture. Load test the event layer before Phase 2 insight features go live.

---

### R4 — CUP access model scope creep
**Severity: Medium**

Multi-tenant access control across GLS involves complex role inheritance. Undefined edge cases will block UI delivery and risk delaying Phase 1 sign-off while access debates continue.

**Mitigation:** Run a dedicated CUP access model workshop in Phase 1 discovery. Document all edge cases and get sign-off before build begins.

---

## Risks — Productivity Consultancy

### R5 — Consultancy engagement too late
**Severity: High**

Productivity consultancy brought in at Phase 2 or 3 misses the chance to shape rule design in Phase 1. Insights and scenario models are only as useful as the rules they analyse. Poorly structured rules in Phase 1 means the Phase 2 intelligence layer is working with bad inputs.

**Mitigation:** Engage productivity consultancy before Phase 1 rule design begins. Their input should inform what rules are built and how they are structured for analysis.

---

### R6 — Consultancy output not actioned
**Severity: Medium**

Without a defined process for turning productivity insights into rule change requests, Phase 2 outputs sit in a report. Phase 3 never receives useful training signal and AI recommendations will be generic or incorrect.

**Mitigation:** Define the insight-to-rule-change workflow in Phase 2 design. Build an approval path from insight to rule update in the GLS UI.

---

### R7 — Scenario modelling adoption
**Severity: Low**

SLM what-if comparisons are only useful if labour planners trust and understand the model outputs. Without consultancy-led training alongside the tool launch, Phase 2 investment is wasted.

**Mitigation:** Include user enablement as a Phase 2 delivery requirement, not an afterthought. Productivity consultants should co-design the training materials.

---

## Risks — Stakeholder Involvement

### R8 — No single business owner across phases
**Severity: High**

Ownership ambiguity is an existing known risk. Without a named, empowered product owner for GLS end-to-end, phase handoffs will stall at sign-off, priorities will conflict between workstreams, and the programme will lose coherence across its 18–24 month span.

**Mitigation:** Name a single product owner before Phase 1 starts. This person must have authority to make scope and priority decisions across all three phases.

---

### R9 — Alert fatigue from notification layer
**Severity: Medium**

Without threshold governance from Phase 1, the Decision Engine in Phase 3 will generate high alert volumes. Stakeholders will disengage from iQ360 alerts and the value of Phase 3 automation will be undermined before it launches.

**Mitigation:** Define alert governance policy in Phase 1 even though the Decision Engine is Phase 3. Build threshold controls into the notification layer from the start.

---

### R10 — Ops teams bypassing GLS for urgent changes
**Severity: Low**

If rule change turnaround in GLS is slower than existing LP workarounds, operational teams will revert to legacy processes. The system will become inconsistent and trust in GLS as the source of truth will erode.

**Mitigation:** GLS rule change UX must be at least as fast as LP. Define change-request SLAs in Phase 1 acceptance criteria. Monitor bypass behaviour post-launch.

---

## Stakeholder Map

### Product Owner
**Role:** GLS programme  
**Phases:** 1, 2, 3  
**Involvement:** Owns end-to-end delivery. Signs off phase transitions. Chairs steering group. Must be named and empowered before Phase 1 build begins. Single point of accountability for scope and priority decisions across all phases.

---

### Labour Planner
**Role:** Operations / productivity  
**Phases:** 1, 2, 3  
**Involvement:** Primary end user of rules and scenario models. Defines rule requirements in Phase 1. Validates scenario model outputs in Phase 2. Reviews and approves AI recommendations in Phase 3. Critical for adoption — if planners don't trust the system, Phase 3 automation has no mandate.

---

### Productivity Consultant
**Role:** External / internal  
**Phases:** 1, 2, 3  
**Involvement:** Must be engaged from Phase 1 rule design — not after. Defines the insight and analysis framework for Phase 2. Leads ROI modelling and organisational change management in Phase 3. Late engagement is flagged as a high-severity risk.

---

### Tech Lead
**Role:** GLS engineering  
**Phases:** 1, 2, 3  
**Involvement:** Architects integration with CFM, SLM, CUP. De-risks LP migration. Owns event layer design and performance. Key escalation point for technical risk across all phases.

---

### Ops Exec Sponsor
**Role:** Senior leadership  
**Phases:** 1, 3  
**Involvement:** Approves programme funding. Provides visible sponsorship for Phase 1 launch communications. Required for Phase 3 automated execution sign-off — AI-driven rule changes require exec-level governance and audit trail.

---

### LP System Owner
**Role:** Existing platform  
**Phases:** 1  
**Involvement:** Provides data feed continuity during migration. Must agree LP decommission timeline and support rule data mapping in Phase 1. Risk: disengagement or obstruction after Phase 1 delivery scope is defined.

---

## Evolution Timeline

```
TODAY
└── Rules → Used as-is in LP
        No versioning, no central management

PHASE 1  (Months 1–4)
└── Rules → Centralised in GLS
        Single source of truth
        Full lifecycle: create, version, audit
        Accessible via iQ360

PHASE 2  (Months 4–7)
└── Rules → Analysed
        Usage patterns surfaced
        Override exceptions visible
        What-if scenario comparisons available

PHASE 3  (Months 7–12)
└── Rules → Optimised automatically
        AI recommendations generated
        Approval-based or automated execution
        Labour cost and schedule efficiency improved
```

---

## Claude Code — HTML Conversion Notes

To convert this file to a deployable HTML page for GitHub Pages:

```bash
# In Claude Code — suggested prompt:
# "Convert gls_implementation_plan.md to a single-file HTML page with:
#  - A sidebar navigation linking to each section
#  - Phase-coloured headings (Phase 1 = teal, Phase 2 = amber, Phase 3 = coral)
#  - Risk severity badges (High = red, Medium = amber, Low = green)
#  - A stakeholder grid replacing the stakeholder sections
#  - Clean, minimal styling suitable for GitHub Pages
#  - No external dependencies — inline everything"
```

### GitHub Pages deployment

1. Create repo `gls-implementation-plan` (or add to existing)
2. Drop `index.html` into `/docs` folder or root
3. Enable GitHub Pages in repo settings → Source: main branch
4. Access at `https://[org].github.io/gls-implementation-plan`

---

*Last updated: March 2026 — Global Labour Service programme*
