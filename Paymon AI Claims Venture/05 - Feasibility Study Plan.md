# Feasibility Study Plan (Phase 1)

> Owner: **Eli + Naggie**. Output is the deliverable for the next Paymon review meeting.

## Goal

Validate that the technically risky parts of the product are buildable, and produce a credible scope (time, materials, effort) for a Phase 2 build. Not a working product — a **decision-quality answer** to "can this thing actually be built?"

## Success criteria for Phase 1

A Phase 1 deliverable should answer:

1. Can a current frontier LLM extract structured data from a real insurance policy with citation accuracy good enough to drive negotiation?
2. What is the regulatory envelope — which states are hardest, and can our software-tool framing survive them?
3. What is the realistic effort estimate (engineering weeks) and stack to ship an MVP for a single claim type?
4. What are the showstoppers we discovered, if any, and what would a v0 actually cost?

If we can answer those four questions, Phase 2 (business structuring + investment) has the inputs it needs.

## Sequenced workstreams

### Workstream A — Policy comprehension prototype *(highest risk, do first)*

**Why first:** If LLMs can't do this reliably, the venture doesn't work. Every other technical question is downstream of this.

**Approach:**
- Source 5–10 real homeowners insurance policies across different carriers (Paymon can supply via Noble's archive — anonymized)
- Build a narrow prototype that extracts: coverage limits, exclusions, deductibles, named perils, claim procedures, statutory deadlines
- Every extracted fact must have a citation back to the source clause
- Manually validate output against ground truth

**Pass criteria:** Define a small ground-truth schema and target very high accuracy, ideally ≥95%, with valid citations. If the prototype misses critical exclusions, limits, or endorsements, the user-facing product is not ready even if the average score looks acceptable.

**Deliverable:** Prototype + accuracy report + qualitative writeup on failure modes.

**Estimated effort:** 1–2 weekends of Naggie's time + Eli review, assuming policies are supplied cleanly and anonymized up front.

### Workstream B — Regulatory landscape research

**Approach:**
- Map state-by-state public adjuster regulations (especially: FL, TX, LA, CA, CO — disaster-prone states)
- Map state-by-state UPL (unauthorized practice of law) statutes around insurance disputes
- Study disclaimer / positioning approaches used by TurboTax, LegalZoom, RocketLawyer, and what failed for DoNotPay
- Identify the strictest state we'd need to survive
- Identify any blocker states where the model fundamentally doesn't work

**Deliverable:** 2–3 page memo with a per-state risk matrix and a clear "go / caveat / no-go" verdict on each priority state.

**Estimated effort:** Eli desk research first; outside counsel review if the model still looks viable after the technical prototype.

### Workstream C — Negotiation engine concept

**Approach:**
- Sit with Paymon for 2–3 hours and document Noble's actual playbook for one claim type (recommend: hurricane wind/roof damage — high frequency, well-codified)
- Translate the playbook into a decision tree + correspondence template library
- Validate: given a policy + a damage description + a carrier offer, can the LLM output the correct next-step letter and reasoning?

**Deliverable:** Encoded playbook for one claim type + concept demo of the negotiation flow.

**Estimated effort:** Paymon time is the bottleneck here, not engineering.

### Workstream D — MVP scope and effort estimate

**Approach:** With A/B/C done, write the actual MVP scope:
- Single claim type (hurricane wind/roof)
- Single state to launch in (likely FL — Paymon's strongest network)
- Minimal lead routing to 3–5 launch partners Paymon can pre-commit
- Realistic engineering effort estimate

**Deliverable:** Phase 2 scope document with timeline and rough budget.

## What we're explicitly NOT doing in Phase 1

- Building UI polish
- Production infrastructure
- B2B partner portal (deferred to MVP)
- Any marketing / customer acquisition work
- Vision / damage assessment prototype (defer until policy comprehension is proven)
- Any commitment to long-term tech stack

## Recommended MVP claim type

**Hurricane wind / roof damage in Florida.**

- High frequency (hurricane season generates concentrated demand)
- Well-codified — Paymon has the deepest playbook here
- Florida is Paymon's strongest network for B2B partner pre-commits
- Roofers are the most leveraged B2B buyer — predictable revenue per lead

Other claim types (water damage, fire, hail) come after this is proven.

## Cadence

- Weekly check-in between Eli and Naggie during the study
- Bi-weekly read-out to Paymon and Lauren
- Whole-group review at the end of Phase 1 to decide on Phase 2 commitment

## What we need from Paymon to start

- 5–10 anonymized real policies for Workstream A
- 2–3 hours of his time for Workstream C playbook encoding
- A list of 3–5 attorneys / roofers he believes would pre-commit as launch partners
