# Technology Scope

> Owner: **Eli** (lead) + **Nagy** (technical execution).

This document captures the full technical surface area of the product as discussed May 8. Phase 1 feasibility study won't tackle all of it — it will validate the riskiest pieces. The remainder is architecture context for the eventual build.

## Product surface area — seven components

### 1. Policy ingestion & comprehension

**What it does:** User uploads their insurance policy (PDF, often 50+ pages of dense legalese). The system extracts the structured data needed to assess any claim against it.

**Outputs:**
- Plain-English policy summary the homeowner actually understands
- Structured extraction: coverage limits, exclusions, deductibles, claim procedures, statutory deadlines, named perils, sub-limits
- Flagging of "deny-delay-defend" trigger language and known carrier playbook clauses
- Source-clause citations for every extracted fact (defensibility)

**Hard parts:**
- Policy variation across carriers, states, and policy types
- Hallucination risk on legal language — citations must be exact
- Exhibits, endorsements, and riders that modify base policy
- Scanned-image policies (OCR + comprehension stack)

**Why it's the biggest risk:** If LLMs can't reliably extract policy details with citations, the rest of the product is built on sand. **This is the first thing to prototype.**

### 2. Damage documentation intake

**What it does:** Captures evidence of loss in the form an insurance company will accept.

**Inputs:**
- Photos and video from the homeowner's phone
- Receipts, repair estimates, prior inspection reports
- Optional integration with **Matterport** (~$250 per scan) for professional 3D evidence — Paymon's call

**Outputs:**
- Vision-model damage classification (water, wind, fire, hail, structural, etc.)
- Rough scope estimation
- Structured claim file linking evidence to specific policy coverages

**Considerations:**
- Vision models hallucinate damage assessments — need confidence thresholds and human review for edge cases
- Chain-of-custody / metadata preservation for legal defensibility
- Integration with existing tools homeowners may already have (e.g., Hover, EagleView)

### 3. Claim negotiation engine — *the core IP*

**What it does:** This is the value-creation layer. Compares the carrier's offer against what the policy actually entitles the homeowner to, and walks them through negotiating the gap.

**Capabilities:**
- Side-by-side: carrier estimate vs. policy-derived entitlement
- Auto-generated correspondence: demand letters, statutory notices, deadline reminders, counter-arguments
- Pre-litigation step tracking — these are state-statute requirements that, if skipped, kill any future legal case. This is where we genuinely add value over a confused homeowner with ChatGPT.
- Contact log: every email, call attempt, response, with timestamps for evidentiary value
- Suggestion engine for next-best-action based on where the negotiation has stalled

**Integration with Paymon's playbook:**
- Encode Noble's actual escalation patterns (state-statute timing, carrier-specific tactics, language that has historically moved the needle)
- This is the unfair advantage — it's not just LLM + policy text, it's LLM + policy + decades of negotiation experience encoded as system prompts and decision trees

### 4. Regulatory positioning — built into the product

**What it does:** Keeps us classified as a software tool, not a public adjuster or attorney. This is a **product-design constraint, not a legal afterthought.**

**Design rules:**
- The user is always the named party on every communication. AI generates drafts the user reviews and sends.
- Disclaimer framework analogous to tax/legal software ("we are software, not your licensed representative")
- No language in the product that implies representation or fiduciary duty
- UX flow forces user review-and-approve before any document goes out

**What this avoids:**
- State-by-state public adjuster certification, if the software-tool framing holds
- Unauthorized practice of law (UPL) claims
- The regulatory wedge that limits Bo's Turboax to existing Noble territories

**What needs to be researched (Phase 1):**
- Which states have the most aggressive PA / UPL statutes — the product has to survive the strictest
- Whether any state requires special handling even for software tools
- Disclaimer language patterns from TurboTax, LegalZoom, RocketLawyer, DoNotPay (and what got DoNotPay in trouble)

### 5. Hybrid human assist

**What it does:** Provides a fallback to a human when the AI is insufficient — for trust, for edge cases, and for the bimodal user base Lauren called out.

**Two user modes:**
- **Self-serve power users** (Eli is one) — fully automated, never want to talk to a person
- **Hand-holding users** — need a human voice for legitimacy, especially at launch when the brand is unknown

**Components:**
- AI receptionist on inbound calls (we already built one for Noble — leverageable)
- Live chat with human agents for stuck users
- Escalation paths into the negotiation flow
- Brand legitimacy at launch ("who is this company?") — humans solve this faster than UX polish does

### 6. B2B lead routing & partner system

**What it does:** Turns user activity in the platform into the actual revenue stream.

**Capabilities:**
- Lead scoring: claim size, complexity, geography, services needed, intent strength
- Partner portal where attorneys, roofers, HVAC, drywall, PAs subscribe and configure their geo + service preferences
- Bidding/auction for leads in contested geographies (Google Ads / Angie's List analog)
- Fallback referral path — if the user gives up on self-service, route to a partner with revenue share
- Reporting back to partners on lead outcomes (closes the loop, increases their willingness to pay more)

**Integration considerations:**
- CRM/intake systems on the partner side — most attorneys and contractors won't manually pull from a portal
- TCPA / consent compliance for any contact info passed
- Data minimization — only share what the partner needs

### 7. Acquisition funnel infrastructure

**What it does:** Captures the disaster-event traffic Paymon's ads drive.

**Capabilities:**
- Disaster-event responsive landing pages (sub-state geo-targeted)
- Sign-up → policy upload → guided claim, completable in minutes from a phone
- Real-time spin-up of new geo-targeted campaigns when a disaster event hits
- Integration with ad platforms for attribution and re-targeting

## Cross-cutting concerns

### Data and ML strategy
- Model selection: frontier LLMs (Claude / GPT) for policy comprehension; vision models for damage classification
- Self-hosted vs. API trade-off — likely API for v1 (speed to market), evaluate self-hosting once volume warrants
- Build a private dataset from anonymized real claims as a long-term moat (this is the hardest thing for a competitor to replicate)

### Security and privacy
- Insurance policies and claim documents are highly sensitive PII
- SOC 2 path early — required for any serious B2B partnerships
- Encryption at rest, in transit, granular access controls

### Stack thoughts (for Phase 1 prototype only — not a long-term commit)
- LLM: compare Claude / OpenAI / Gemini on the same anonymized policy set; pick the best extraction + citation behavior, not the brand
- Vector store: Pinecone / pgvector for policy clause retrieval
- App: whatever lets Nagy ship fastest — Next.js + Supabase or similar
- Avoid premature investment in production infra during feasibility

## Existing assets we can leverage

- **AI receptionist for Noble** — already built; good starting point for the human-assist component
- **CRM Wizards Salesforce/FSL expertise** — relevant if we go toward a Salesforce-based backend, less so if we go cloud-native
- **Paymon's encoded playbook** — the negotiation patterns from running Noble; this is essentially proprietary training data
