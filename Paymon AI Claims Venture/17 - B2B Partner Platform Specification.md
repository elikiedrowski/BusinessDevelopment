# B2B Partner Platform Specification

> Owner: **Eli** (product/architecture) + **Nagy** (engineering) + **Paymon** (partner relationships and pricing).
> Companion docs: [15 - Software Build Plan.md](15%20-%20Software%20Build%20Plan.md), [16 - Consumer Application Specification.md](16%20-%20Consumer%20Application%20Specification.md).

The B2B platform is where the business actually makes money. It turns the high-intent consumer activity from the claims app into qualified leads sold to partners who would otherwise pay heavily through Google Ads, LSA, or referral middlemen for the same audience.

For the 2026 hurricane-season launch, "platform" means an internal operator console plus compliant manual routing. A full self-service partner marketplace comes later, after Florida demand and lead value are proven.

## Strategic context

- **Consumer-side product = the demand generator.** It captures and qualifies homeowners in a way no other channel can.
- **B2B platform = the revenue engine.** Without it, we have a free tool with no business model.
- **The platform is the moat over time.** Paymon's existing law firm relationships are the warm starting channel, but a real partner network with real attribution data becomes hard for any competitor to replicate.

## Design principles

1. **Push leads, don't expect pulls.** Most attorneys and contractors won't log in to a portal to fetch leads. Leads arrive in their email, SMS, CRM, or phone — fast.
2. **Quality over volume.** Partners pay premium prices for high-intent leads with documented context. We compete on lead quality, not lead volume.
3. **Consent is sacred.** Every lead carries documented consent and revocation handling. No exceptions, ever. One bad partner abusing consent is an existential brand risk.
4. **Closed-loop attribution.** Partners report outcomes; we use outcome data to price future leads better. This is the long-term advantage.
5. **No-surprise pricing.** Partners always know the cost of a lead before they accept it. No bait-and-switch.

## Partner segments

| Segment | What they want | Lead value (rough order) | Notes |
|---|---|---|---|
| **Insurance attorneys** | Documented claims that are stuck after carrier denial or lowball | $$$ (hundreds to low thousands per qualified lead) | Paymon's strongest warm channel. **Florida-specific caveat:** HB 837 (March 2023) eliminated the one-way attorney fee statutes for property insurance claims. FL insurance attorneys can no longer expect the carrier to pay their fees when the homeowner prevails — their economics shifted hard. Lead values likely lower than pre-2023 industry assumptions. Validate against Paymon's current attorney conversations, not historical pricing. |
| **Personal injury attorneys with bad-faith practices** | Similar to insurance attorneys; specifically interested in bad-faith fact patterns | $$$ | Subset of above |
| **Public adjusters (when allowed)** | Cases too complex for self-service | $$ | Use carefully — must not undermine our "replace PAs" positioning |
| **Roofers** | Storm/hail damage homeowners with covered roof claims | $$ | Highest volume buyer category. **Florida-specific:** post-2019 AOB ban (HB 7065) means FL roofers can no longer take an Assignment of Benefits and bill the carrier directly — they bill the homeowner who then deals with the insurer. This shrinks roofer margins and may reduce their willingness to pay premium lead prices. Validate FL roofer pricing assumptions against post-AOB-ban economics. |
| **HVAC contractors** | Water/smoke damage homes needing system replacement | $$ | Volume buyer |
| **Drywall / general contractors** | Interior reconstruction work | $ | High volume, lower per-lead value |
| **Inspectors / estimators** | Pre-claim or claim-supporting inspections | $ | Often a precursor to the bigger lead |
| **Insurance brokers (loss-side)** | Brokers helping their clients with claims | $$ | Phase 4+ — interesting channel we haven't talked about |
| **Restoration companies** | Water/fire/mold remediation jobs | $$ | Volume buyer |

## End-to-end partner journey

### 1. Acquisition

- Paymon's warm calls and existing relationships (Phase 2 — primary channel)
- Direct outreach to top-tier attorneys / contractors in target geographies (Phase 3+)
- Inbound from our website's "Become a partner" page
- Industry conference and association presence (Phase 4+)

**Backup partner pre-commit sources (if Paymon's channel underdelivers):**

The Phase 2A launch depends on 3–5 partner pre-commits from Paymon's network. If those calls don't convert, we need fallback channels ready:

- **Eli's CRM Wizards customer base** — current and prior CRM Wizards clients may include FL-based attorneys, roofers, or contractors who'd be natural early partners
- **Lauren's enterprise network** — broader business relationships that could be tapped for warm intros
- **State bar association referral lists** — Florida Bar maintains practice-area directories for insurance / bad-faith attorneys
- **Direct outreach to known plaintiff-side insurance firms** — short list of FL firms with public marketing budgets and a hurricane-claim practice
- **Inbound from the Paymon-authored thought-leadership post** — attorneys / contractors finding us organically pre-launch

Identify and warm 2–3 of these backup partners by end of Phase 1 so they're available if Paymon's pipeline slips.

### 2. Application + vetting

- During the 2026 beta, partner onboarding is manual and relationship-led by Paymon
- We verify: state license, bar status, BBB rating, insurance, reviews
- Background check for principal owners (Phase 3+)
- Geographic and service-category configuration
- Pricing tier accepted
- Master Services Agreement and Data Protection Addendum signed (DocuSign)
- Banking info captured for payouts and/or billing

### 3. Onboarding

- Partner sets up:
  - Lead delivery preferences (email, SMS, webhook to their CRM, phone routing)
  - Coverage hours and capacity (max leads per day, per week)
  - Geographic precision (state, metro, ZIP)
  - Claim-type / service-category preferences
  - Pricing tier (volume vs. premium)
- Welcome experience: how leads will look, what to do when one arrives, how to report outcomes

### 4. Active operation — lead flow

When a consumer in our app reaches a referral-appropriate moment:

1. Lead-scoring engine assesses fit (claim type, geography, severity, partner capacity)
2. Internal operator confirms recommended partner fit
3. Consumer consents in the app before any contact information is shared
4. During the beta, lead is pushed manually or semi-manually:
   - Email with structured info
   - SMS summary to the partner contact
   - Phone call / warm handoff where appropriate
5. Partner has a contractual window to engage and report status
6. If partner doesn't engage in window, operator may reoffer to next-best partner after checking consent scope

### 5. Outcome tracking and attribution

- During beta, partner reports outcome via email link, simple form, or manual operator update
- Status workflow: contacted → engaged → retained → settled / lost / closed
- Settlement / fee data captured where partner is willing to share
- Closed-loop data feeds lead-scoring and pricing improvements

### 6. Billing and payouts

- Two pricing models running in parallel:
  - **Per-lead flat fee** — billed at acceptance
  - **Revenue share on closed leads** — billed at settlement, requires trust + attribution
- Most partners likely start on per-lead flat fee (simpler trust gate)
- Stripe ACH billing on net-30 or pre-paid lead packs
- Volume discounts negotiated in MSA

### 7. Partner ongoing engagement

- Monthly reports: leads received, accepted, converted, closed
- Quarterly business reviews for top-tier partners
- Pricing renegotiation based on demonstrated value
- Disputes process (lead quality, contact issues)

## Feature inventory by phase

### Phase 2A — Florida Concierge Beta (target: August 1, 2026)

- Manual partner approval; no self-service marketplace
- Partner configuration: geo, service category, capacity, lead-delivery channel
- Internal admin console for Eli / Lauren / Paymon
- Consent record viewer attached to every lead
- Manual / semi-manual email and SMS lead delivery
- Manual outcome reporting via simple form
- Manual invoicing or Stripe invoice on per-lead flat fee
- Audit log of every lead sent, with consent record attached
- Partner misuse / complaint log

### Phase 2B — Productized Florida MVP

- Partner sign-up form with manual approval
- Partner records with license / bar / insurance verification fields
- Rules-based lead scoring
- Rules-based partner matching
- Email/SMS lead delivery templates
- Partner status-update links
- Basic internal attribution dashboard
- Stripe billing workflow for per-lead flat fee
- Revocation and suppression-list handling

### v1 (Phase 3 — Multi-State Hurricane)

- Self-service partner application flow with vetting checkpoints
- CRM webhook integrations for the top 5 legal/contractor CRMs
- Phone-routing lead delivery option (Twilio)
- Partner dashboard: lead inbox, status, billing, reports
- Lead-scoring engine v1 (rules-based)
- Volume-tier discount logic
- Reoffer logic when partner doesn't engage in SLA window
- DPA template auto-executed at onboarding

### v2 (Phase 4 — Multi-Peril)

- Marketplace-style bidding for premium leads
- Revenue-share pricing model option (closed-loop attribution required)
- Lead-scoring v2 (ML-assisted, using historical outcome data)
- Partner API for full programmatic integration
- White-label lead routing for very large partners
- Cohort and territory exclusivity offerings
- Dispute and chargeback workflow

### v3 (Phase 5 — National Scale)

- Partner-association integrations (state bar associations, contractor associations)
- Partner-acquisition affiliate program ("Refer a partner, earn share")
- Carrier-partnership product (carriers paying us for the data layer, very carefully designed to avoid acquire-and-shelf risk)
- International expansion partner infrastructure

## Critical technical components

### Lead-scoring engine

Inputs:
- Claim type, peril, severity estimate
- Geography (state, metro, ZIP)
- Estimated claim value (from policy extraction + damage signal)
- Carrier offer status (denied, lowballed, no offer yet)
- User engagement signals (time spent, evidence completeness)
- Consumer intent signal (explicitly asked for help vs. browsing)

Outputs:
- Lead tier (cold / warm / hot)
- Reason codes
- Recommended partner shortlist
- Suggested pricing tier
- Time-sensitivity rating

Initially rules-based; in Phase 4, augmented with ML on accumulated outcome data.

### Consent management system

Every lead must carry a verifiable consent record. The system stores:
- Timestamp of consent
- Exact UI text presented to the user (versioned)
- User identifiers (account, IP, device fingerprint)
- Channels consented to (SMS, email, phone)
- Specific partner(s) or partner categories consented to
- Time-bounded scope (consent expires after N days of inactivity)
- Revocation log
- Suppression-list state
- Partner notification that consent was revoked, where applicable

The audit trail must be sufficient to defend a lead-generation complaint — assume one will come eventually. Legal counsel should approve the exact consent language and routing pattern before beta launch.

**Federal vs. state TCPA framing:** In January 2025, the 11th Circuit vacated the FCC's "prior express written consent" rule that would have required one-to-one consent and topic-relevance for telemarketing calls/texts (*Insurance Marketing Coalition Ltd. v. FCC*). The federal floor is now back to the pre-rule TCPA standard. **However:**

- Florida's mini-TCPA (the **Florida Telephone Solicitation Act**, Fla. Stat. § 501.059) has its own prior-express-written-consent regime, has a private right of action, and has been actively litigated. **Build to FTSA grade for any FL contact.**
- Maryland, Oklahoma, and several other states have similar mini-TCPAs.
- The FCC could re-issue a revised rule; the plaintiff bar is highly active in this space.
- Industry best practice is to keep consent **specific to named partner(s) or narrow categories, time-bounded, revocable, and fully audit-logged** regardless of the federal floor.

Operating conservatively here costs us very little and protects against the lawsuit surface that will inevitably grow as the platform scales.

### Partner CRM webhook layer

We can't ask partners to build to our API. We build to theirs.

Phase 2B launch delivery:
- Email delivery with structured payload + parseable subject line
- Partner status-update links / simple forms
- Generic webhook only if a launch partner already has a clean intake endpoint and the integration is faster than manual routing

Phase 3 integrations:
- Lead Docket (popular among PI / insurance attorneys)
- Filevine
- Litify (Salesforce-based; lots of attorneys)
- HighLevel (used by many contractor agencies)
- Zoho CRM
- HubSpot

### Phone-routing lead delivery

For partners who want hot leads as phone calls:
- During beta: manual warm handoff where appropriate
- Phase 3+: Twilio bridge where consumer agrees to be connected and partner gets inbound call with context
- Recorded with consent (varies by state)
- Fallback to voicemail-with-callback if partner doesn't pick up in N seconds
- Pricing premium for phone-routed leads

### Disaster-event ingestion (cross-cutting)

Although this serves the consumer-side marketing engine, it also drives partner alerts:
- NOAA / National Weather Service feeds
- FEMA disaster declarations
- USGS earthquake feeds
- Local emergency-management alerts
- Wildfire perimeter feeds (Cal Fire, etc.)

When a major event hits a partner's geography, we alert them: "Demand is about to surge in your area. Capacity?"

## Pricing model details

### Per-lead flat fee (default starting model)

- Cold lead: $25–75 (browsing user, no claim yet)
- Warm lead: $75–250 (claim in progress, consented)
- Hot lead: $250–1500 (claim with documented dispute, consented, phone-routable)
- Premium attorney lead with bad-faith fact pattern: $1500–5000+

For the 2026 Florida beta, use simple founder-negotiated launch pricing first. The numbers above are target ranges to validate, not committed price cards.

### Revenue share (Phase 4+ for trusted partners)

- 10–20% of fees collected on closed leads
- Requires partner cooperation on outcome reporting
- Lower commercial-risk for partner; longer payback for us
- Only offered to partners we've validated through flat-fee history

### Volume tiers

- Bronze (1–10 leads/month): flat list price
- Silver (10–50): 10% discount
- Gold (50–200): 20% discount + dedicated success contact
- Platinum (200+): custom pricing, territory considerations

### Geographic / category exclusivity

- Some partners will pay premium for exclusivity in a metro for a claim type
- Be careful — exclusivity reduces market efficiency
- Only offered post-MVP, only in specific high-margin scenarios

## Open questions for the B2B side

- How do we handle partners who decline a lead but still get contacted by the consumer? (Probably treat as no-charge.)
- Do we charge partners for cancelled / fraud-detected leads? (No — they should be confident in lead quality.)
- Do we ever offer a phone-only product to partners who refuse to integrate with anything digital? (Yes for hot-tier; routed through our switchboard.)
- How do we handle partners who try to convert our consent flow into their own marketing list? (Contractual prohibition; revocation of partnership on detection.)
- Do we want certification tiers (BBB-rated, our own quality badge) shown to consumers when partners are recommended? (Yes by Phase 3.)
- Do we operate a partner-of-last-resort fund — small reserve to refund consumers if a partner behaves badly? (Phase 4 consideration; protects brand.)
