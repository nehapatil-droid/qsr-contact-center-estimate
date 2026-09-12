# QSR Voice Channel — Agentforce Contact Center
## Delivery Estimate: Traditional vs. Augmented vs. AI-Native

**Program:** Walmart QSR — Subway Pilot (2 CT stores) → Domino's / Taco Bell / Dunkin / McDonald's
**Scope:** Embedding Agentforce Contact Center with native telephony (AFCC — Service Cloud Voice native + Omni-Channel + Service Console) into existing Salesforce org
**Telephony platform:** Salesforce AFCC native telephony — not Amazon Connect OEM, not BYOT
**Existing foundation:** Service Cloud (Case Management, Service Console), Experience Cloud site, Email-to-Case
**PRD Version:** v0.3, Sept 8, 2026 | **Estimate Date:** Sept 11, 2026
**Target:** FY27 Q1–Q2

> **Pricing deferred** — timeline, effort, and resourcing are complete below. Add indicative pricing by supplying bill rates via `commercials`.

> **Benchmark Disclaimer:** Duration ranges are derived from a top-down shape classification benchmarked against model training data (Salesforce implementation actuals). These are decision-support ranges, not commitments. The Solution Lead owns the final number and signs off before presenting to the customer. All figures are wall-clock elapsed (inclusive of client-side review, UAT, and compliance sign-offs) unless labeled as PS delivery envelope.

---

## Scope Summary — 12 Epics

| Epic | Title | Size | Confidence | Priority |
|------|-------|------|------------|----------|
| E01 | Service Cloud Voice – Channel Setup & IVR Flow | M | Assumed | P0 |
| E02 | Omni-Channel QSR Skill & Routing Config | M | Assumed | P0 |
| E03 | Voice Call → Case Linkage & Service Console Layout | L | Assumed | P0 |
| E04 | Order Lookup Lightning Component + OMS Integration | **L** | **Unknown** | P0 |
| E05 | Driver Lightning Component + LMD Un-batch API | **L** | **Unknown** | P0 |
| E06 | Complaint Intake Lightning Form + Allergen P0 Fast-Track | L | Assumed | P0 |
| E07 | Item Signal Lightning Form + Catalog/Adapter Routing | M | Assumed | P1 |
| E08 | Call Recording, Shield Event Monitoring & Compliance | S | Assumed | P0 |
| E09 | Callback / Voicemail Fallback + 4-hr SLA Flow | S | Confirmed | P1 |
| E10 | Einstein Conversation Insights + Post-Call Auto-Summary | M | Assumed | P1 (optional) |
| E11 | Reporting: Voice + Case Metrics + LMD Dashboard Data Pipe | M | Assumed | P1 |
| E12 | UAT, Agent Training & Go-Live Readiness | M | Assumed | P0 |

**Size distribution:** 4 × L · 5 × M · 2 × S · 1 × XL=0
**P0 epics:** 8 (must complete for go-live) · **P1 epics:** 4 (can phase to post-pilot)
**⚠ Range-wideners:** E04 and E05 are Unknown confidence — API contracts not finalized. These are the critical path and the primary source of the high-end range.

---

## Dual-Track Duration Comparison

| Lane | Duration (wall-clock) | PS Delivery Envelope | Basis |
|------|-----------------------|----------------------|-------|
| **Traditional** | **14–23 weeks** | ~12–17 weeks | Benchmark-derived, top-down shape |
| **Augmented** *(AI tooling, same operating model)* | **11–20 weeks** | ~10–14 weeks | Traditional baseline × Mid readiness realized band (~15–20%) |
| **AI-Native** *(Agentforce operating model — conditional)* | **9–15 weeks** | ~7–11 weeks | Traditional baseline × native band (~35–40%, provisional, gated) |

**Anchor lane: Traditional** (the defensible benchmark number; AI-native is conditional on operating model commitment).

### Duration derivation

**Benchmark anchor:** Salesforce KB — *"Contact Center Phase 1 (Service Cloud + Agentforce + telephony) ~12–14 weeks"*

**Adjusted up to 14–20 week baseline** for this engagement's shape:
- 12 epics with 4 × L (above a standard Phase 1)
- 4 custom Lightning components (Order Lookup, Driver, Complaint, Item Signal)
- 4 external API integrations across 2 separate systems (OMS, Subway adapter, LMD un-batch, Spark driver)
- Regulatory/compliance overlay (allergen P0 path, Shield event monitoring)
- Native AFCC telephony provisioning (Salesforce-owned infrastructure — no third-party telephony setup overhead)

**Risk adders applied:**
- **+15% high end** — Unknown API contracts on E04 (OMS + Subway adapter) and E05 (LMD un-batch + Spark driver). Two L epics with unresolved auth, field mapping, and error-handling design. *(Tighten: finalize API contracts with Michelle Fultz and Edwin Ortiz in Phase 1.)*
- **+10% high end** — Regulatory compliance overlay: allergen food-safety P0 path (15-min SLA Flow, Compliance queue creation, ack gating), Shield event monitoring activation.
- **−5% offset** — Brownfield advantage: existing Service Cloud org, Case Management, Service Console, and Experience Cloud already live reduces discovery and setup overhead.
- **−5% offset** — Native AFCC telephony: Salesforce-owned infrastructure means no third-party telephony provisioning, no BYOT integration work, and no Amazon Connect OEM lead-time risk. E01 is purely a Voice Flow + number provisioning task on Salesforce infrastructure.

**Net: +15% high-end adjustment → 14–23 weeks** (wall-clock elapsed, inclusive of client-side UAT + compliance sign-offs). Native AFCC telephony removes the BYOT/OEM provisioning risk adder and lead-time uncertainty that would have applied to a non-native stack.

**High end driven by:**
1. **E04** — OMS + Subway API adapter contracts unresolved. Tighten: *confirm REST/JSON, OAuth, field mapping, latency SLA with Michelle Fultz before Phase 2.*
2. **E05** — LMD un-batch API + Spark driver ETA API contracts unresolved. Tighten: *confirm idempotency model, rollback on failure, driver ETA polling interval with Edwin Ortiz.*
3. **E06** — Compliance on-call queue doesn't exist; Compliance/Legal owner TBD. Tighten: *assign Compliance queue owner (Open Q7) and identify Legal contact in Phase 1.*

---

## Phasing Plan (Traditional Lane)

| Phase | Name | Duration | Key Deliverables |
|-------|------|----------|-----------------|
| 1 | Discovery & Architecture | 2–3 weeks | Finalize all API contracts, resolve Open Qs (coverage hours, queue owners, dedup key, complaint policy), AFCC toll-free number provisioning (Salesforce native), Solution Architecture doc, IVR wireframe, dev environment |
| 2 | Build – P0 Core | 6–10 weeks | E01 (IVR Flow), E02 (Omni-Channel skill + queues), E03 (Voice Call→Case + Console layout), E04 (Order Lookup LWC + OMS integration), E05 (Driver LWC + LMD un-batch), E06 (Complaint form + Allergen Flow), E08 (recording + Shield) |
| 3 | Build – P1 + UAT | 4–7 weeks | E07 (Item Signal), E09 (Callback), E10 (Einstein, optional), E11 (Reporting + data pipe), E14 (warm transfers). End-to-end UAT across all 4 inquiry categories + IVR paths. Defect fix. |
| 4 | Hardening, Training & Go-Live | 2–4 weeks | Agent training (LMD codes, order/driver/complaint/item flows), pilot store testing (2 CT Subway stores), phone number publishing, go/no-go gate, production cutover, hypercare |

---

## Resource Mix

### Traditional Lane — ~3.0 FTE Program-Average

> **Roster honesty note:** This roster is a model-derived starting point requiring Solution Lead validation — the human owns the final team. Key judgment calls: (1) whether the senior Developer must be onshore (dependent on API contract responsiveness from Walmart SMEs); (2) whether the telephony specialist can be absorbed into the SA role or needs to be separate. The FTE headline is program-average across all phases; it peaks ~4.0 FTE during Phase 2 build. Resourcing is a capability being refined — treat this as decision-support, not a staffing commitment.

**Salesforce PS Team:**

| Role | Seniority | Location | Count | Active Phases | Allocation | Justification |
|------|-----------|----------|-------|---------------|------------|---------------|
| Solution Architect | Senior | Onshore | 1 | 1–4 | Full | Design authority: SCV architecture, Console layout, compliance overlay. Onshore required — close iteration with Walmart API owners. |
| Developer | Senior | Onshore | 1 | 2–3 | Full | Critical-path integrations (E04 OMS + Subway adapter, E05 LMD un-batch + Spark driver). Unknown-confidence L epics; senior + onshore for API owner collaboration. |
| Developer | Regular | Offshore | 1 | 2–3 | Full | Volume build: Complaint LWC, Item Signal form, allergen/callback Flows, Console sub-tab config. M-complexity items suitable for offshore. |
| Quality Assurance | Regular | Offshore | 1 | 2–4 | Full | 4 IVR paths × 4 inquiry categories × 4 LWC components. Allergen P0 path requires independent QA. Offshore appropriate for scope. |
| Project / Program Manager | Regular | Onshore | 1 | 1–4 | Half | Coordination across 9+ Walmart touchpoints. Onshore for relationship management. |
| Solution Architect *(Voice specialist)* | Regular | Onshore | 1 | 1–2 | Quarter | AFCC native telephony fractional: IVR Flow build, AFCC toll-free number provisioning (Salesforce-owned infrastructure), Omni-Channel routing config. Consumed Phases 1–2 only. No third-party telephony integration. |
| Change & Adoption | Regular | Onshore | 1 | 3–4 | Quarter | Agent training support, go-live readiness. Fractional, Phases 3–4 only. |

**PS FTE headline:** ~3.0 FTE program-average (4.0 FTE peak in Phase 2) · 2 onshore + 2 offshore core + 2 fractional onshore

**Client-Side Requirements (Walmart must staff):**

| Role | Count | Active Phases | Allocation | Notes |
|------|-------|---------------|------------|-------|
| Product Owner / Decision Maker (Surojit) | 1 | 1–4 | Half | Must unblock 12 open questions. Unstaffed = delivery risk. |
| Integration SMEs (Michelle Fultz + Edwin Ortiz) | 2 | 2–3 | Quarter | API contracts for E04/E05. Their responsiveness IS the critical path. |
| UAT / QA + pilot store associates | 2 | 3–4 | Half | In-store voice testing — PS cannot substitute. |
| Compliance / Legal (TBD) | 1 | 1–4 | Quarter | Disclosure script, retention period, allergen queue owner. Unstaffed = go-live blocker. |

---

### AI-Native Lane — ~2.2 FTE Program-Average *(Conditional)*

> **AI-native qualification status: CONDITIONAL.** This lane requires: (1) Surojit Mukhopadhyay available daily (full-time) to unblock decisions within 24hrs, (2) Michelle Fultz + Edwin Ortiz at half-time with <48hr API contract turnaround, (3) an AI-first mandate from Troy Avidano / Jeff Stanley. Without these commitments, the 9–16 week range is not achievable — use the Traditional or Augmented lane.

**How the compression works:** An Agent Orchestrator (senior Technical Architect) directs a Claude Code / Agentforce agent fleet for LWC scaffolding, Flow automation, Apex callout generation, and test case creation. An Intent Architect owns all design decisions and coaches the Walmart product owner. Hard-problem build (the two Unknown-confidence API integrations E04, E05) remains human-owned — agents absorb volume below the senior core, not the critical-path integrations.

**Salesforce PS Team (AI-Native):**

| Role (QL Accountability) | Seniority | Location | Count | Active Phases | Allocation | Justification |
|--------------------------|-----------|----------|-------|---------------|------------|---------------|
| Project / Program Manager *(Program Lead)* | Senior | Onshore | 1 | 1–4 | Half | Customer relationship, gate decisions, the plan. |
| Solution Architect *(Intent Architect)* | Senior | Onshore | 1 | 1–4 | Full | Owns what/why, solution intent sign-off, coaches Walmart PO, directs agent fleet for architecture decisions. |
| Technical Architect *(Agent Orchestrator)* | Senior | Onshore | 1 | 1–4 | Full | Directs agent fleet for all build volume (LWC, Flows, Apex). Owns critical-path API integrations (E04, E05) directly — the hard-problem human builder. Onshore required. |
| Quality Assurance | Senior | Onshore | 1 | 2–4 | Half | Agent-amplified (AI generates test cases) but never agent-replaced. Senior required for allergen P0 path (regulatory). Surges to full in Phase 3 hardening. |
| Solution Architect *(Voice specialist)* | Regular | Onshore | 1 | 1–2 | Quarter | AFCC native telephony fractional — same as traditional. No BYOT/OEM complexity. |

**PS FTE headline:** ~2.2 FTE program-average · Senior-weighted, agent-amplified core

**Client-Side Requirements (AI-Native — more demanding):**

| Role | Count | Active Phases | Allocation | Notes |
|------|-------|---------------|------------|-------|
| Product Owner / Decision Maker (Surojit) | 1 | 1–4 | **Full** | Daily availability required. This is the qualification gate — half-time is insufficient for AI-native. |
| Integration SMEs (Michelle Fultz + Edwin Ortiz) | 2 | 2–3 | **Half** | <48hr API contract turnaround needed. Bottleneck here erases AI-native compression. |
| UAT / QA + pilot store associates | 2 | 3–4 | Half | Same as traditional. |
| Compliance / Legal (TBD) | 1 | 1–4 | Quarter | Same as traditional. |

---

## Side-by-Side Comparison

| Dimension | Traditional | Augmented | AI-Native *(conditional)* |
|-----------|-------------|-----------|--------------------------|
| **Duration (wall-clock)** | **14–24 weeks** | **11–20 weeks** | **9–16 weeks** |
| PS Delivery Envelope | ~12–18 weeks | ~10–15 weeks | ~8–12 weeks |
| **PS FTE (program-avg)** | **~3.0 FTE** | **~3.0 FTE** | **~2.2 FTE** |
| FTE peak (Phase 2) | ~4.0 FTE | ~4.0 FTE | ~3.0 FTE |
| PS headcount (named roles) | 7 rows | 7 rows | 5 rows |
| Onshore / Offshore split | 2 onshore + 2 offshore + 2 fractional | Same | 4 onshore senior + 1 fractional |
| Senior/regular ratio | 1 senior + 3 regular | Same | 4 senior + 1 regular |
| Compression basis | — | Mid realized band ~15–20% | Native band ~35–40% (provisional) |
| Operating model change | None | None — AI tooling only | Yes — daily PO, AI-first mandate |
| Qualification gate | None | None | **Conditional** on PO availability + AI mandate |
| Indicative price | *(pricing deferred)* | *(pricing deferred)* | *(pricing deferred)* |

### Delta Decomposition (AI-Native vs. Traditional)

The AI-native lane's compression derives from three distinct sources — not a blanket discount:

1. **Roster shape:** A senior-weighted core (Intent Architect + Agent Orchestrator) with agents absorbing build/test volume vs. a scope-scaling Developer pod. Fewer total bodies; higher average seniority.
2. **Volume-to-agents:** LWC scaffolding (4 components), Flow automation (7+ Flows), test case generation, and config documentation are absorbed by the agent fleet under senior direction — these are the M-L items (E03, E06, E07, E09) where AI-native compression is real.
3. **Sequential hard-problem ownership:** The two Unknown-confidence L epics (E04, E05) are taken one-at-a-time by the Agent Orchestrator directing agents — senior covers more scope than in traditional because agents handle the surrounding volume.

**What AI-native does NOT compress:** The API contract finalization (E04, E05) and compliance inputs (E06, E08) are the critical path in both lanes. If Walmart API owners are slow, neither lane hits the low end.

---

## Key Assumptions

1. Existing org is Service Cloud Enterprise/Unlimited — SCV + AFCC licensing procurement in progress (Open Q12).
2. **Telephony is Salesforce AFCC native** — Salesforce-owned infrastructure end-to-end. No Amazon Connect OEM, no BYOT, no third-party telephony integration. Toll-free number provisioned through Salesforce AFCC directly.
3. OMS and Subway API adapter expose REST/JSON with standard OAuth — SOAP or complex auth escalates E04 to XL.
4. LMD un-batch API is synchronous REST — async/event-driven adds Flow orchestration complexity to E05.
5. No data migration required.
6. Spanish bilingual coverage excluded from Phase 1 (if required, adds ~S to E02).
7. Einstein for Service (E10) is optional/Phase 2 — excluded from critical path.
8. Power BI data pipe (E11) is a scheduled export connector — CRM Analytics recipe build escalates E11 to L.
9. Salesforce Shield is already licensed on the Walmart org.
10. Pilot is 2 CT Subway stores only — 1,300-store scale staffing is a separate WFM exercise.
11. Client-side UAT team and pilot store associates are available in Phase 3.
12. Compliance/Legal resource is identified and engaged in Phase 1.

---

## Risks & Open Items

| # | Risk / Gap | Impact | Owner | Status |
|---|-----------|--------|-------|--------|
| R1 | OMS + Subway API adapter contracts unresolved | E04 could escalate to XL; +2–4 weeks | Michelle Fultz | Open Q — resolve in Phase 1 |
| R2 | LMD un-batch + Spark driver API contracts unresolved | E05 could escalate to XL; +2–3 weeks | Edwin Ortiz | Open Q — resolve in Phase 1 |
| R3 | SCV + Omni-Channel + Einstein licensing not procured | Blocks everything | Troy / Jeff + SF commercial | Open Q12 — Phase 0 blocker |
| R4 | Compliance/Legal TBD — no queue owner, no disclosure script | E06 allergen path is go-live blocker; E08 Shield blocked | Compliance/Legal | Open Q7 — identify in Phase 1 |
| R5 | ~~Amazon Connect OEM vs BYOT~~ — **RESOLVED: AFCC native telephony** | Not a risk. Salesforce AFCC is the platform. | — | Closed |
| R6 | Coverage hours not decided (24/7 vs 5am–11pm CT) | Omni-Channel queue hours config + callback SLA clock start | Troy / Jeff | Open Q1 — resolve in Phase 1 |
| R7 | Item Signal triage owner not decided | E07 queue config blocked; can defer to Phase 2 (P1) | AJ Claessens + Catalog | Open Q8 |
| R8 | Complaint hand-back policy not written | E06 routing logic incomplete for complaint-to-CustomerCare path | Care Ops + CES | Open Q9 |
| R9 | Spanish bilingual coverage TBD | If required: adds ~S to E02 and training scope | WFM / CES | Open Q3 |
| R10 | Volume unknown (1,300 stores scale) | Staffing forecast for full scale is outside this estimate | Brayden Wood / WFM | Open Q2 |

---

## Summary Table

| | Traditional | Augmented | AI-Native *(conditional)* |
|--|-------------|-----------|--------------------------|
| **Timeline** | 14–23 weeks | 11–20 weeks | 9–15 weeks |
| **PS FTE avg** | ~3.0 | ~3.0 | ~2.2 |
| **PS FTE peak** | ~4.0 | ~4.0 | ~3.0 |
| **PS named roles** | SA (senior) + Dev (senior onshore) + Dev (regular offshore) + QA (offshore) + PM (half) + Voice SA (fractional) + C&A (fractional) | Same | Program Lead (half) + Intent Architect (full) + Agent Orchestrator (full) + Senior QA (half) + Voice SA (fractional) |
| **Pilot go-live** | FY27 Q1 *(Jan 2027 feasible if Phase 1 starts Oct 2026)* | FY27 Q1 *(Dec 2026–Jan 2027 feasible)* | FY27 Q1 *(Nov–Dec 2026 feasible, conditional)* |
| **Indicative price** | Pricing deferred | Pricing deferred | Pricing deferred |
| **Gate** | None | None | Requires daily PO + AI mandate |

**Recommended starting point:** Traditional lane (14–24 weeks) as the defensible benchmark. Present Augmented (11–20 weeks) as the AI-tooling uplift with no operating model change. AI-native (9–16 weeks) as the conditional ceiling if Walmart commits to the operating model.

**Narrow the range fast by:** Resolving E04/E05 API contracts (biggest band-wideners) and confirming Compliance/Legal owner in Phase 1.

---

*Pricing deferred — timeline, effort, and resourcing are complete. Add indicative pricing by supplying bill rates (`/commercials`).*
*Next: `/validate` for consistency check · `/narratives` or `/slides` to build the client story · `/export` to package.*

*Estimate authored Sept 11, 2026 · PRD v0.3 · Scopezilla KB: 20,022 atoms (commit 85c7bc71)*
