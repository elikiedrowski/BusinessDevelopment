# Software Build Plan

> Owner: **Eli** (lead) + **Nagy** (technical execution), AI-augmented two-person team.
> Companion docs: [16 - Consumer Application Specification.md](16%20-%20Consumer%20Application%20Specification.md) and [17 - B2B Partner Platform Specification.md](17%20-%20B2B%20Partner%20Platform%20Specification.md).

## Mission

Build the software that lets a homeowner negotiate their own insurance claim from disaster to settlement — and monetize the resulting high-intent demand stream through a B2B partner platform. The ultimate target is replacing the public adjusting industry across all U.S. states and all insurable natural-disaster claim types.

## Operating assumptions

- **Team:** Eli + Nagy, AI-augmented (Claude / Cursor / Copilot etc.) for code generation, refactoring, testing, debugging.
- **Compensation:** Ownership-based, no labor-cost constraint. Calendar time and focus are the real constraints.
- **Cadence:** Nights and weekends until proof-of-concept; hurricane-season launch requires sprint-style founder focus from Phase 2A onward.
- **Stack:** Cloud-native, modern web. No Salesforce dependency.
- **Time pressure:** Bo's "Turboax" is rumored to be launching soon, and the 2026 Atlantic hurricane season is already beginning. We should aim for a usable Florida concierge beta by **August 1, 2026**, then improve weekly through peak hurricane season.
- **Infrastructure budget:** Ownership comp covers labor, but Phase 1–2A still requires real cash for LLM API ($1–3k/mo expected), hosting (Supabase/Neon, S3, Vercel), Twilio SMS, Resend email, Sentry/PostHog, DocuSign, outside counsel fees ($5–15k for the FL regulatory memo + disclaimer review), Vanta only when needed, and initial ad spend coordinated with Paymon. **Working assumption: $25–40k cash required to reach August 1 launch**, source TBD in Phase 2 business structuring. Flag immediately if this assumption is wrong.

## AI-assisted velocity — what to expect

AI assistance **does** accelerate:
- Application scaffolding, auth, CRUD plumbing
- API integration code and SDK boilerplate
- Test generation and refactoring
- Debugging and code review
- Documentation and migration scripts
- LLM prompt iteration with structured outputs

AI assistance **does not** materially accelerate:
- Regulatory research per state
- Partner relationship building and pre-commits
- Encoding Paymon's claims playbook into decision trees (Paymon's time is the bottleneck)
- Customer trust building / brand work
- Compliance audits (SOC 2, etc.)
- The actual lived calendar time for soft launches and partner onboarding

Realistic AI-augmented velocity for a two-person team working evenings + weekends: **roughly 2x** vs. a pre-AI two-person team. Faster on greenfield code, similar on integration and review work, slower than a full-time pair would be.

For the 2026 launch push, the plan assumes we do **not** wait for a fully automated marketplace. We launch a narrow, concierge-backed product first:

- Florida hurricane wind / roof only
- Web app only
- Human review of every AI output
- Manual partner routing by Eli / Lauren / Paymon
- Minimal B2B portal; internal admin tools first
- Weekly product releases during hurricane season

## Florida statutory framework — what shapes the product

Florida is our launch state, and Florida-specific insurance law materially shapes the product mechanics, partner economics, and forbidden language. The build plan must reflect these:

- **SB 2A (December 2022 special session)** changed first-party property claim timelines and bad-faith standards. Carriers now have shorter windows to acknowledge, investigate, and pay; the homeowner's notice and response windows also shifted. Our **deadline tracker must be Florida-statute aware on day one**, not generic.
- **HB 837 (March 2023)** eliminated the one-way attorney fee statutes for property insurance claims. Insurance attorneys can no longer expect carriers to pay their fees if the homeowner prevails — their economics shifted hard. This affects how aggressively attorneys will pursue claims and what they're willing to pay per lead. **Validate partner pricing assumptions against post-HB 837 attorney economics, not pre-2023 ones.**
- **HB 7065 (2019) + subsequent reforms** banned post-loss assignment of benefits (AOB) for residential property claims. **Florida roofers can no longer take an AOB and bill the carrier directly.** They have to bill the homeowner who then deals with the insurer. This shrinks roofer margins, may reduce their willingness to pay premium lead prices, and shifts who controls the post-claim repair money. Our roofer-partner pricing model must reflect this.
- **Florida Telephone Solicitation Act ("Florida mini-TCPA," FTSA, Fla. Stat. § 501.059)** is more restrictive than federal TCPA in some places and is heavily plaintiff-bar-friendly. Even though the 11th Circuit's January 2025 vacatur of the FCC's federal one-to-one consent rule (*Insurance Marketing Coalition Ltd. v. FCC*) loosened the federal floor, **Florida's FTSA still requires prior express written consent for automated calls/texts to FL residents and has its own private right of action.** Build to FTSA-grade consent, not the federal floor.
- **Florida's "Citizens" insurer-of-last-resort** is the single largest homeowners insurer in Florida and has its own claim handling quirks. A carrier-specific playbook for Citizens is high priority.

These statutes shape the product directly:
- Deadline tracker must be FL-statute-versioned
- Disclaimer + consent UX must be FTSA-grade for FL residents
- Roofer partner pricing must reflect post-AOB-ban economics
- Attorney partner pricing must reflect post-HB 837 economics
- Carrier playbooks must include Citizens specifically

## Architecture overview

```text
                    ┌──────────────────────────────────────────┐
                    │   Consumer App (Next.js + React Native)  │
                    │   ─ Policy upload + LLM extraction       │
                    │   ─ Claim intake + evidence              │
                    │   ─ Negotiation engine + drafting        │
                    │   ─ Deadline tracker + reminders         │
                    │   ─ Consent capture for B2B              │
                    └──────────────┬───────────────────────────┘
                                   │
                    ┌──────────────▼───────────────────────────┐
                    │   Shared Backend (TypeScript + Postgres) │
                    │   ─ Document store (S3, encrypted)       │
                    │   ─ Policy / claim / evidence models     │
                    │   ─ LLM orchestration layer              │
                    │   ─ Playbook rules engine                │
                    │   ─ Audit log (immutable)                │
                    │   ─ Notification service (SMS / email)   │
                    └──────────────┬───────────────────────────┘
                                   │
        ┌──────────────────────────┼──────────────────────────┐
        │                          │                          │
        ▼                          ▼                          ▼
┌───────────────┐         ┌─────────────────┐        ┌──────────────────┐
│  B2B Partner  │         │   Lead Routing  │        │  Disaster-Event  │
│   Platform    │◄────────┤   + Scoring     │◄───────┤    Ingestion     │
│   (Next.js)   │         │     Engine      │        │  (NOAA / FEMA)   │
└───────┬───────┘         └─────────────────┘        └──────────────────┘
        │
        ▼
┌─────────────────────────────────────────────┐
│  Partner Integrations                       │
│  ─ Push: email, SMS, webhook to CRM         │
│  ─ Pull: REST API for partner systems       │
│  ─ Voice routing for inbound partner calls  │
└─────────────────────────────────────────────┘
```

Key principles:
- **Consumer app and partner platform share the same backend** to avoid divergent data models.
- **The negotiation engine is its own service** — same engine, called from consumer-driving correspondence and from partner-facing case enrichment.
- **The audit log is immutable** — every AI output, user action, and partner handoff is captured for regulatory defensibility.

## Stack decisions

| Layer | Choice | Rationale |
|---|---|---|
| Frontend (consumer web) | Next.js 15 + React | Server components for fast first paint at the disaster moment; mature ecosystem |
| Frontend (consumer mobile) | React Native (Expo), Phase 3+ | Code-share with web; phone-first UX once Florida beta is proven |
| Frontend (partner platform) | Next.js | Same stack; minimal context-switch for the dev team |
| Backend | Node.js + TypeScript | Code-share with frontend; AI tools strongest in TypeScript |
| Database | Postgres (Supabase or Neon) | Battle-tested, RLS for multi-tenant isolation, pgvector for retrieval |
| Document storage | S3 + KMS encryption | Per-document encryption keys; standard pattern |
| LLM | Claude (primary) + GPT-5 (fallback) | Bake-off during Phase 1; Claude favored for long-context legal docs |
| Vector search | pgvector | Avoid a separate vector DB until volume warrants Pinecone or similar |
| Auth | Clerk or Supabase Auth | Skip building auth; both support enterprise SSO later |
| Notifications | Twilio (SMS) + Resend (email) | Standard; both have good APIs |
| Payments | Stripe | When we add paid tiers or partner billing |
| Background jobs | Inngest or Trigger.dev | For long-running LLM extractions and lead routing |
| Observability | Sentry + PostHog | Errors + product analytics from day one |
| Compliance scaffolding | Security checklist first; Vanta or Drata when needed | SOC 2 readiness before serious enterprise diligence, not before beta |

## Phased delivery roadmap

The timeline assumes a Phase 1 kickoff as soon as Paymon delivers 5–10 anonymized policies and confirms availability for the playbook session.

---

### Phase 1 — Compressed Feasibility Sprint (Weeks 1–4, target: May 20–June 16, 2026)

**Goal:** Decision-quality answer to "can we extract policy obligations from real homeowner policies with citation accuracy good enough to drive negotiation."

| Week | Workstream | Deliverable |
|---|---|---|
| 1 | Policy intake pipeline + extraction schema | PDF upload, text extraction, OCR fallback, page-numbered chunking, fixed JSON schema |
| 2 | LLM extraction + reviewer scoring | First-pass extraction, citation viewer, manual scoring on initial policy set |
| 3 | Model bake-off + draft correspondence test | Claude vs. GPT vs. Gemini on same policies; first Paymon-reviewed letters |
| 4 | Regulatory desk research + launch decision | FL-first PA / UPL memo (from engaged counsel), TCPA consent pattern, MVP scope estimate, go / no-go |

**Critical Week 2 action:** Outside counsel must be engaged by end of Week 2. Florida insurance-regulatory counsel with TCPA experience preferred. Without an engaged counsel by Week 2, the Week 4 memo slips and the August 1 launch slides.

**Timeline reality check:** This 4-week sprint has **zero slack**. If Paymon's policies arrive a week late, if counsel turnaround takes 3 weeks instead of 2, or if the bake-off needs an extra iteration, the August 1 launch slides into mid-August. Acceptable given hurricane-season pressure (peak Atlantic hurricane activity is mid-August through mid-October), but every founder needs to know there is no buffer.

**Output:** Go/no-go decision for the Florida concierge launch. If go, freeze Phase 2A scope only.

**Risks:**
- Extraction accuracy too low to be useful (kills the venture in current form — see pivot options below)
- Regulatory blocker discovered in Florida (forces wedge shift to a different state or a different posture)
- Paymon can't supply enough policies / playbook time in window (slips timeline)
- Outside counsel cannot be engaged in time (slips timeline)

**Pivot options if Phase 1 extraction fails:**

1. **Narrower extraction.** Drop the long tail of clauses; nail only deductibles, named perils, key exclusions, and statutory deadlines. May be enough to drive Phase 2A even if full-policy comprehension isn't ready.
2. **Human-in-the-loop extraction.** Cheaper than a PA but still labor-heavy. Reviewer fills in gaps the AI misses; user still gets a complete claim file. Cuts margin but proves the rest of the product.
3. **Pivot to carrier-letter analysis first.** Carrier denial / underpayment letters are shorter, more structured, and higher-value to analyze. Use the policy as supporting context rather than the primary input. May be a stronger wedge anyway.
4. **Fine-tuning on Paymon's anonymized historical claims.** Slower (weeks), more expensive, but produces a defensible private model. Only worth pursuing if frontier-model accuracy is close but not quite there.
5. **Different MVP entirely.** Kill the consumer-facing tool; pivot to an internal tool that improves PAs' own productivity (sell to PAs, not against them). Worst-case fallback that still uses what we've built.

---

### Phase 2A — Florida Concierge Hurricane Beta (Weeks 5–10, target: June 17–July 31, 2026)

**Initial wedge:** Florida hurricane wind / roof damage. Single state, single peril, single workflow.

**Goal:** A real homeowner can upload a policy, document hurricane roof damage, receive a human-reviewed draft next step, and optionally consent to a partner referral before peak 2026 hurricane season.

#### Product scope

- Consumer web app only (mobile-responsive; no native app yet)
- Auth, onboarding, Florida-only claim intake
- Policy upload, OCR fallback, extraction with citations
- Claim file: photos, carrier letters, estimates, notes
- Paymon playbook v0 for hurricane wind / roof
- Draft correspondence generator with required human review
- Deadline tracker v0 for Florida-specific contractual / statutory windows
- Consent-to-share flow for partner referrals
- Internal admin console for Eli / Lauren / Paymon
- Manual lead routing to 3–5 pre-committed partners
- Audit log for policy extraction, AI output, human review, user approval, and partner handoff

| Subphase | Weeks | Deliverable |
|---|---|---|
| Foundation | 5–6 | Auth, onboarding, policy upload, extraction results, claim workspace |
| Claim + evidence | 6–7 | Damage intake, photo/doc upload, evidence checklist, carrier-letter upload |
| Playbook + drafting | 7–8 | Paymon-reviewed decision tree, first 3 letter templates, human review queue |
| Consent + manual B2B | 8–9 | TCPA consent capture, internal lead routing, partner notification by email/SMS |
| Beta hardening | 10 | End-to-end QA, privacy/ToS/disclaimer pass, first real-user concierge pilot |

**Launch target:** Florida concierge beta live by **August 1, 2026**.

**Beta success criteria:**
- 10–25 Florida homeowners complete a guided claim during hurricane season
- 3–5 Paymon-approved partners are ready to receive manually routed leads
- Every AI-generated letter is reviewed by Eli / Paymon before release to user
- At least 3 paid or payment-committed partner handoffs
- Zero TCPA / PA / UPL incidents
- Paymon validates that drafts are professionally usable

---

### Phase 2B — Productized Florida MVP (Weeks 11–20, target: August–October 2026)

**Goal:** Reduce concierge labor while hurricane-season traffic is live. Turn the manual beta into a repeatable Florida product and prove B2B unit economics.

#### Workstreams

**Consumer app** — see [16 - Consumer Application Specification.md](16%20-%20Consumer%20Application%20Specification.md) for detail.

| Subphase | Weeks | Deliverable |
|---|---|---|
| Automation hardening | 11–13 | Better extraction validation, confidence thresholds, versioned prompt/rule releases |
| Carrier estimate parsing | 12–14 | Parse Xactimate / Symbility / Simsol estimate exports (PDF and structured where available); extract line items, depreciation, ACV vs. RCV, O&P treatment; side-by-side gap analysis vs. policy entitlement |
| Claim workflow | 12–15 | Carrier response handling, improved deadline tracker (FL-statute-versioned), task list, document vault |
| Notifications | 14–16 | SMS/email reminders, calendar export, opt-out handling, FTSA-grade consent records |
| Document delivery | 16–18 | Email delivery with receipts; certified mail/e-sign evaluated but not required for MVP |
| Florida launch iteration | 18–20 | Product analytics, support workflows, weekly defect burn-down |

**B2B partner system** — see [17 - B2B Partner Platform Specification.md](17%20-%20B2B%20Partner%20Platform%20Specification.md) for detail.

| Subphase | Weeks | Deliverable |
|---|---|---|
| Partner records | 11–12 | Partner profiles, geo/service config, license/approval status |
| Lead routing v1 | 13–15 | Rules-based lead scoring, partner matching, email/SMS delivery |
| Outcome tracking | 15–17 | Partner status updates by link/form; internal attribution dashboard |
| Billing | 17–19 | Stripe invoice/manual billing workflow for per-lead flat fee |
| Disaster landing pages | 18–20 | Florida event-specific pages and campaign attribution hooks |

**Compliance + legal scaffolding** (continuous)

- TCPA-compliant consent flow with documented evidence
- Disclaimer framework reviewed by outside counsel
- SOC 2 readiness checklist started; Vanta / Drata only if partner diligence requires it before Phase 3
- Privacy policy + Terms of Service drafted
- DPA template for B2B partners
- Data retention, revocation, and partner misuse policies drafted

**Productized MVP success criteria:**
- 50+ Florida homeowners complete a guided claim
- 10+ paid or payment-committed B2B partner handoffs
- Zero TCPA / UPL incidents
- Paymon validates the negotiation drafts are professionally usable
- Unit economics are at least directionally validated

---

### Phase 3 — Multi-State Hurricane Scale (Months 6–9, target: November 2026–February 2027)

**Goal:** Become the default tool for hurricane-damage homeowners across the entire U.S. coast.

- Expand from Florida into LA + TX first, then SC, NC, GA, AL, MS, FL Panhandle, southern VA
- Add Spanish-language UX (Florida + Texas have significant Spanish-speaking homeowner bases)
- Scale partner network to 30+ active partners across regions
- SOC 2 Type I achieved if enterprise partner diligence requires it
- Implement carrier-specific playbook layers (State Farm, Citizens, Universal, Allstate, etc.)
- Add vision-based damage classification (preliminary; not a final valuation)
- Disaster monitoring becomes proactive — pre-position ad spend before landfall
- Prepare for 2027 hurricane season with multi-state playbooks and partner coverage

**Critical dependencies:**
- Phase 2A/2B Florida unit economics validated
- Paymon's playbook encoded for at least 3 major coastal carriers
- Outside counsel green-lights each new state's regulatory posture

---

### Phase 4 — Multi-Peril Expansion (Months 9–15)

**Goal:** Expand from hurricane-only to the full set of claim types where homeowners need help.

Add, in roughly this priority order:
1. **Water damage** (interior flooding, plumbing) — high frequency year-round
2. **Hail damage** — high frequency Midwest / Plains
3. **Wildfire** — California, Colorado, Oregon, Washington
4. **Wind / tornado** — Plains states, Tornado Alley
5. **Freeze / pipe burst** — winter storm events
6. **Theft, vandalism, lightning** — long-tail but well-defined

Each peril requires:
- Playbook capture from Paymon (or domain expert if Paymon's depth is hurricane-specific)
- Per-state regulatory review for that peril
- Partner network in the relevant geographies and service categories
- Carrier-specific tactic encoding
- Updates to the LLM negotiation engine prompts

By end of Phase 4:
- ~10 supported claim types
- Available in ~25 states (all major disaster-prone states)
- SOC 2 Type II achieved
- Paymon's playbook is in production for all major perils
- B2B partner network in the hundreds

---

### Phase 5 — National Scale & Full Disruption (Months 15–24+)

**Goal:** Replace the public adjusting industry as the default first stop for an underpaid claim. Available in all 50 states, for all common claim types.

- Self-service partner onboarding (productized B2B funnel)
- Marketplace-style bidding for high-value leads
- White-label or carrier-partnership product lines explored (carefully — avoiding the acquire-and-shelf risk)
- Vertical AI model improvements: potentially fine-tune on accumulated anonymized claim data (this becomes a real moat — competitors can't replicate it)
- International expansion considered (UK, AU, CA have similar dynamics)
- Series A or strategic capital raise to fund expansion (if ownership comp is no longer enough to retain talent)

---

## Critical path

The work items that block everything else, in order:

1. **Policy extraction accuracy** (Phase 1) — venture-defining
2. **Florida regulatory boundary** (Phase 1) — must confirm software-tool posture and forbidden language before beta
3. **Paymon's playbook encoding bandwidth** (Phase 1–2A) — single point of failure
4. **TCPA-compliant consent flow** (Phase 2A) — legal blocker before any partner handoff
5. **Internal admin + human review queue** (Phase 2A) — what makes aggressive launch defensible
6. **First 3–5 partner pre-commits** (Phase 2A) — without these the B2B platform launches empty
7. **Disaster-event traffic during 2026 season** (Phase 2A/2B) — proves the unit economics or forces acquisition pivot
8. **Outside counsel regulatory memo per state** (Phase 3–4) — gates each state expansion
9. **SOC 2 Type II** (Phase 4) — required for serious enterprise partners

## Compliance and legal sequencing

| Phase | Compliance work |
|---|---|
| 1 | Florida PA / UPL desk research. TCPA consent pattern. Outside counsel intro conversations. Privacy policy + ToS first draft. |
| 2A | Disclaimer framework approved before beta. TCPA flow audit. Human review policy. DPA template for launch partners. |
| 2B | Revocation handling, partner misuse policy, data retention policy, billing terms, security baseline checklist. |
| 3 | SOC 2 Type I if needed. Per-state regulatory memos for LA/TX and next expansion states. Insurance-industry vendor compliance reviews. |
| 4 | SOC 2 Type II achieved. Per-state per-peril regulatory updates. Outside counsel retained on retainer for ongoing review. |
| 5 | Active regulatory monitoring. Lobbying / industry-association engagement. Carrier-partnership compliance posture. |

## Risk register

| Risk | Phase | Mitigation |
|---|---|---|
| Policy extraction accuracy too low | 1 | Workstream A is the gate — kill the venture or pivot wedge if it fails |
| Aggressive launch creates regulatory mistakes | 2A | Florida only, human review of every AI output, counsel-approved disclaimers, no automated sending |
| Bo's Turboax launches first and captures mindshare | 2 | Pre-write the positioning response now: "Bo's tool is still public adjusting — 10–20% of your claim. Ours keeps 100%." Have a press / Paymon-authored blog post ready to publish within 48 hours of Turboax launch. Monitor Valerie's signals and other ex-Noble channels for launch warning. |
| Insurance industry acquire-and-shelf attempt | 3+ | Capitalization strategy avoids carrier money; defensive corporate structure |
| Paymon bandwidth becomes the constraint | 2–4 | Encode his playbook into rules engines aggressively; train backup domain expert |
| A state regulator declares us unauthorized practice | Any | Pause that state immediately; have outside counsel response template ready |
| LLM cost scales faster than B2B revenue | 3+ | Self-host smaller models for routine extraction once volume warrants |
| Partner network refuses TCPA-compliant push model | 2 | Have manual fallback path; some partners may need phone-routing not data |
| 2026 hurricane season is mild (low demand validation) | 2A/2B | Use Florida beta to validate workflow; prepare water/hail expansion earlier if demand is weak |
| Eli or Nagy bandwidth collapses under day-job pressure | All | Build with maintainability and AI documentation so the other can carry alone short-term |

## What this plan deliberately does NOT include

- A precise founders-go-full-time milestone — but Phase 2A requires explicit weekly availability commitments
- Investor pitch or fundraising plan — separate workstream
- Marketing creative and ad strategy detail — Paymon's lane, separate doc
- Detailed financial model — Phase 2 deliverable
- Hiring plan — labor-cost-free assumption holds for now; revisit at Phase 4 if ownership comp doesn't retain the team

## What we'll know by end of Phase 1 (Week 4) that we don't know now

- Whether policy extraction is good enough to build on
- Whether Florida is safe enough for a concierge beta
- A real engineering estimate for Phase 2A (current 6-week estimate has high execution risk)
- Whether Paymon's playbook is sufficient to encode hurricane wind/roof, or whether we need additional domain expertise
