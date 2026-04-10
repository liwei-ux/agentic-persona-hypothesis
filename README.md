# Personas in the Agentic Model: A Hypothesis for Discussion

Not a roadmap. A hypothesis to pressure-test together. Best read alongside the [interactive journey map](agentic-journey-map.html).

---

## The Headline

- The persona ecosystem reorganizes from **6 functional roles** into **3 types of human judgment + an agent execution layer + the customer:**
  - **Active direction** — Peter sets pricing strategy
  - **Policy guardrails** — Fred defines financial rules agents enforce continuously
  - **Conditional review** — Emily and Diana join only when novel architecture is needed
  - Oliver is the only role fully absorbed.
- The coordination seams shift from **handoff problems to specification problems.** "Can the human see across the boundary?" becomes "has the human defined the rules agents enforce across the boundary?"
- The 4-phase journey compresses into **two modes**: a human-directed design loop, and continuous agent-driven operations.

---

## How This Relates to the Strategy Doc

This doc zooms in on [Thread 2: Agents as the Operator](https://docs.google.com/document/d/1XnmU6Gpeef-c-01t155R52q82NCPhp4Y3zjh8ri1-jM/edit?tab=t.0). Thread 2 describes the direction — agents running the revenue stack, pricing changes in minutes instead of months. This doc adds specificity about the human side: which humans persist, doing what, and why.

**Where we agree:** The operator role transforms. Agents absorb execution. The platform must be machine-operable, safe to simulate, and auto-documenting.

**Where we think Thread 2 underestimates the human side:** Thread 2 compresses the user persona to "Pricing leader / S+O." We think **4 humans persist in 3 judgment types** — not because agents can't execute, but because billing creates irreversible artifacts (invoices sent, revenue recognized, contracts signed) that demand human judgment at the architectural level:

- **Peter** (pricing strategy) — someone must decide what the company wants to be. The objective function is irreducibly human.
- **Emily** (Q2C architecture) — billing connects upstream to customer infrastructure and downstream to Salesforce, ERP, RevRec. Architectural decisions create path dependencies across this entire integration surface that can't be undone by rebuilding code.
- **Diana** (data architecture) — event schemas and data pipelines determine what's measurable. Data not captured can't be recovered retroactively.
- **Fred** (financial policy) — RevRec rules, compliance thresholds, audit. SOX requires human attestation regardless of agent capability.

**On "evals, not human review":** We agree for routine operations. But novel constructs need human judgment because the eval criteria don't exist yet for something never built. Grounded in Metronome's architecture:

- **Routine (agent handles):** New product on an existing rate card. New contract using existing rate cards. Per-customer overrides. Scheduled price changes. New billable metric against existing event types. Amendments within known patterns.
- **Novel (needs human):** New event schema design (what to capture, at what granularity). New cross-system integration mapping (Metronome to Salesforce/ERP/RevRec). New commercial model pattern (pricing that doesn't map to existing contract/rate card/override compositions). New data distribution pattern (usage data to new systems or stakeholders).

Novel decisions cluster at **onboarding** and **pricing model transitions** — exactly when stakes are highest and exactly the stage Metronome's core customers are in.

---

## What Agents Absorb vs. What Humans Retain

| Persona | Agent absorbs | Human retains |
|---|---|---|
| **Emily** | Rate card creation, product setup, webhook wiring, integration scaffolding, troubleshooting | Q2C integration architecture, data model design, system tradeoffs |
| **Oliver** | Contract transcription, override entry, lifecycle transitions, edge case triage | **Role fully absorbed.** Judgment redistributes to Fred, Emily, Peter. |
| **Diana** | Report building, data translation, cross-system stitching, data quality monitoring | Data pipeline architecture, event schema design, measurement framework |
| **Fred** | Reconciliation, error investigation, root cause tracing, executive reporting | RevRec policy, compliance rules, financial thresholds |
| **Peter** | Financial modeling/simulation, feasibility iteration | Pricing strategy, packaging, competitive positioning |
| **Caroline** | (Not an operator — improves as upstream operations improve) | -- |

---

## A Pricing Change, End to End

*A hypothesis, not a description of existing capability.*

### Today: 4 Phases, Sequential Handoffs

**Strategy** (Peter, Diana, Fred) > **Implement** (Emily, Oliver, Peter, Fred) > **Test** (Emily, Peter, Oliver) > **Roll Out** (Oliver, Emily, Fred, Caroline)

Different people, different tools, sequential handoffs. The Emily > Oliver handoff is friction — invisible upstream constraints. Fred discovers errors at month-end. Kong describes "month-long testing cycles." Monte Carlo describes customer-by-customer reconciliation.

### Agentic: 2 Modes, Tight Loops

**Mode 1 — Design & Validate** (human-directed, agent-executed)

Peter describes the pricing change in natural language. A **Revenue Modeling Agent** — operating within Diana's metrics and Fred's RevRec rules — pulls usage data, runs scenario comparisons, and flags RevRec implications.

Peter evaluates and decides. Emily joins for new constructs. Diana joins for new metrics or pipelines. Fred joins if recognition rules need updating.

An **Implementation Agent** translates the approved spec into configuration, constrained by Fred's RevRec rules. Before committing, it traverses all existing contracts referencing affected rate cards and presents the plan to Emily.

A **Validation Agent** simulates invoices across a representative customer sample and compares against the revenue model. No separate "test phase" — validation is embedded in implementation.

Emily reviews discrepancies. Peter makes the go-live decision.

**The Go-Live Boundary** — the one human gate between modes.

**Mode 2 — Operate & Monitor** (agent-driven, human-supervised)

A **Contract Configuration Agent** applies new pricing to individual contracts — amendments, credit rollovers, per-customer conventions. Separate from Implementation because the blast radius is different: one customer vs. all customers.

A **Reconciliation Agent** runs continuously. Flags anomalies at generation time, not month-end. Fred reviews a monitoring dashboard and adjusts rules.

The 5 agents are organized by blast radius: Revenue Modeling (read-only), Implementation (shared infrastructure), Validation (simulation), Contract Configuration (individual customers), Reconciliation (continuous monitoring).

---

## What This Means for Design

### The coordination seams transform

| Seam | Today | Agentic |
|---|---|---|
| Emily > Oliver | Constraints invisible on Oliver's surface | Agent traverses constraint graph. Seam becomes: are constraints checkable rules? |
| Fred as last-stop detector | Errors found at month-end | Caught at generation time. Seam becomes: has Fred defined "correct"? |
| Diana as intermediary | Data translation with human latency | Agent translates directly (Finance vocabulary is domain-standard) |

The design challenge moves from "help humans see across boundaries" to "help humans articulate rules agents enforce across boundaries."

### Three implications — each about helping humans articulate rules

1. **Constraint rules (before acting).** Emily needs to express Q2C constraints as checkable rules agents verify before committing — "this rate card change affects these Salesforce mappings," "this event schema must include these fields." Today these constraints live in Emily's head. The product surface is rule definition, not configuration. In the agentic model, this evolves from "AI helps humans configure" to "humans define constraints agents check before configuring."

2. **Enforcement rules (while operating).** Fred needs to express financial policy as rules agents enforce continuously — RevRec thresholds, compliance boundaries, what "correct" looks like. The 3-month plan's insight cards are already step one: proactive monitoring replaces Fred's manual month-end variance checks (exactly what Tony at Astronomer does today). The next step is giving Fred policy controls — not just alerts, but the ability to define and adjust the rules that generate those alerts.

3. **Authority rules (at the boundary).** Which changes need human approval vs. which agents can execute within bounds. The 3-month plan says "propose, never apply" is permanent. This hypothesis suggests it could relax where blast radius is contained. Whether and when to relax it is the product strategy question that shapes the policy-definition surface. (See Discussion Q1.)

### When this matters

The [3-month AI plan](https://docs.google.com/document/d/1cCmLXhsy9xZeq3haC8Wi8y2wOmFkeDkOJgOP0VfoXPQ/edit?tab=t.dinpzmz52nbk) (insight cards > investigation > contract sketch > permission model > agent workflows) builds toward this model. This hypothesis doesn't change the sequence — it clarifies where each step leads. Use it as a lens: does what we're building now move toward constraint-definition, enforcement-monitoring, and authority-boundary surfaces?

---

## Discussion Questions

**1. Where does "propose, never apply" relax first?**
Three options by blast radius:
- **A: Contract Configuration** — standard deals within policy, one customer at a time. Lowest blast radius.
- **B: Reconciliation** — read-only monitoring and anomaly detection. Zero blast radius.
- **C: Never** — agents remain assistants only.

**2. Should we preserve multi-perspective checking deliberately?**
Today Emily, Oliver, and Fred review the same config from engineering, operational, and financial angles. If Oliver is absorbed, do we design agents with different evaluation criteria to preserve that redundancy — or was it an artifact?

**3. Oliver fully absorbed — does that feel right?**
Is there a judgment Oliver holds that doesn't trace to Fred, Emily, or Peter's domain? If we're wrong, we need an operator-facing policy surface. Either way, the next step is the same: build the permission model.

**4. Does continuous reconciliation change what we build for Fred?**
Monthly detection to continuous monitoring is a fundamentally different product surface. Does this change the insight cards we're prototyping?
