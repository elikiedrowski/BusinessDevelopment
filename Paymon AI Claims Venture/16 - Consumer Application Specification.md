# Consumer Application Specification

> Owner: **Eli** (product/UX) + **Nagy** (engineering).
> Companion docs: [15 - Software Build Plan.md](15%20-%20Software%20Build%20Plan.md), [17 - B2B Partner Platform Specification.md](17%20-%20B2B%20Partner%20Platform%20Specification.md).

The consumer-facing product is a homeowner's AI claim coach. It meets the user at the panic moment of a disaster, helps them understand what their policy may support, and walks them through handling their own claim — generating documents the user reviews and sends in their own name.

## Design principles

1. **Phone-first, panic-aware.** The user is finding us minutes-to-hours after a disaster, often on their phone, often stressed. Every screen has to be usable with one hand and a cracked focus.
2. **Software tool, not representation.** Every interaction reinforces that we are software the user uses, never a representative acting on their behalf. The user is always the sender, signer, and final decision-maker.
3. **Cited everything.** Every claim about the policy carries a citation back to the source clause. The user can always see why we said what we said.
4. **Deadline-aware.** The product proactively surfaces claim deadlines so the user doesn't forfeit rights by missing a window.
5. **Approval-gated.** No correspondence leaves the user's hands without their explicit review and approval. No partner gets contacted without their explicit consent.
6. **Calibrated confidence.** When the AI isn't sure, it says so and asks for more information or recommends qualified help.

## Target user

A homeowner of a $200–400k property who:
- Just experienced a covered loss (hurricane wind, water, fire, hail, etc.)
- Has either filed a claim or is about to file
- Doesn't understand their policy
- Is being offered a low settlement or fears they will be
- Doesn't want to immediately sign 10–40% of their claim away to a PA or attorney
- Has a smartphone, basic literacy, and roughly $0 to spend up front

Secondary user: same homeowner, weeks later on a laptop, working through the negotiation phase.

## End-to-end user journey

### 1. Arrival (Marketing → Landing → Trust)

The user lands on the site, usually from a disaster-event ad or a search after their insurance company offered them less than they expected.

- Disaster-event-aware landing pages (different copy for hurricane vs. wildfire vs. hail)
- Social proof: testimonials, success stories, trust signals
- Clear positioning: "We are software. You stay in control. You keep 100% of your claim."
- Single primary CTA: "See what your policy may support"

### 2. Onboarding (5 minutes)

- Account creation (email + phone; Apple/Google SSO; magic link)
- A few quick context questions: state, type of loss, when it happened, carrier
- Phone verification for SMS reminders (and to anchor the TCPA consent flow later)
- Disclaimer acknowledgment: "I understand this is software, not legal or public-adjusting representation"

### 3. Policy upload + AI summary (15–30 minutes)

- Upload policy PDF (or photograph each page if needed; OCR fallback)
- Optionally connect carrier portal where supported (later phase)
- Background processing: text extraction, chunking, LLM extraction with citations
- Within minutes, the user sees:
  - Plain-English policy summary
  - Coverage A/B/C/D limits and deductibles
  - Wind / hail / hurricane provisions called out
  - Named perils and key exclusions
  - Duties after loss (with deadlines highlighted)
  - Proof-of-loss requirements
  - Appraisal / mediation / dispute provisions
  - Source-clause citation on every fact
- "Things you might be missing" — flagged endorsements, sublimits, or surprises

### 4. Claim intake + evidence (variable, 30 minutes to a few days)

- Guided questionnaire specific to claim type (hurricane wind/roof, water, fire, etc.)
- Damage capture:
  - Upload photos and video (phone camera flow with annotation)
  - Upload existing carrier correspondence (claim number, first offer, adjuster letters)
  - Upload any estimates already obtained (roofer, contractor)
  - Optional: order a Matterport scan (~$250) through a recommended provider
- Evidence checklist: AI flags what's missing (e.g., "We didn't see photos of the interior; carriers often deny interior damage without them")
- Vision model classifies damage at a preliminary level (later phase)

**Florida AOB awareness:** When a Florida user uploads a roofer estimate or contractor agreement, the product checks for an Assignment of Benefits clause. Florida banned post-loss AOB on residential property claims in 2019 (HB 7065) and reinforced the ban in subsequent sessions. If detected, we surface a plain-English warning: *"This agreement appears to assign your claim rights to the contractor. Florida law restricts this for residential property claims since 2019. Review carefully before signing."* This is consumer protection AND product differentiation — homeowners don't know this.

### 5. Coverage vs. offer analysis

- If the carrier has already made an offer, the user uploads it
- Side-by-side: what the carrier offered vs. what the policy may support
- Plain-English explanation of the gap
- Specific categories where the offer appears to be underpaid
- Evidence the user would need to push back on each category
- Confidence indicator on each item

### 5b. Carrier estimate parsing (Xactimate / Symbility / Simsol)

Most carrier offers arrive as an estimate report generated by industry estimating software — Xactimate is dominant, Symbility and Simsol are also common. These aren't simple PDFs; they have line-item structure, depreciation treatment, ACV vs. RCV splits, overhead and profit ("O&P") line treatments, and unit-price assumptions that vary by region.

- Upload the carrier's estimate (typically a PDF export; sometimes an ESX file from Xactimate)
- Extract line items with quantity, unit price, depreciation, and totals
- Flag issues commonly seen:
  - Missing line items (e.g., flashing, drip edge, code upgrades)
  - Aggressive depreciation
  - O&P excluded when it should be applied
  - ACV being treated as final settlement when RCV should be recoverable
  - Unit prices below regional norm
- Generate plain-English summary of where the estimate appears low
- Generate draft supplement-request correspondence with specific line-item citations

This subsystem is the second-most-valuable AI capability after policy comprehension. The carrier's estimate is the document that anchors the entire negotiation; if we can parse it well, we can drive the rest.

### 6. Negotiation engine — guided correspondence

- The system drafts the next letter or email for the user
- Drafts cite specific policy clauses
- Drafts include a specific ask and a response deadline
- Drafts are in the homeowner's voice, sent from their email, signed by them
- The user reviews and edits before anything goes out
- During the 2026 concierge beta, every AI-generated draft is also reviewed internally before the user sees it
- Optional: e-sign and certified-mail integration for evidentiary weight
- Every sent document is logged with timestamp and content hash

### 7. Deadline tracker + reminder system

- Every relevant statutory and contractual deadline surfaced and tracked
- SMS + email reminders (3 days out, 1 day out, day of)
- Calendar export (Apple / Google / ICS)
- Missed-deadline recovery guidance (some are recoverable, many aren't)

### 8. Carrier response handling

- User uploads the carrier's response (email, letter, revised offer)
- AI analyzes the new position
- Presents possible next steps: accept, counter, request more information, consider appraisal, or ask for qualified help
- New draft correspondence generated as appropriate

### 9. Resolution paths

The user reaches one of several outcomes:
- **Settled** — user accepts an offer they are comfortable with. Celebrate. Ask for review.
- **Settled with help** — the user accepted a partner referral (attorney, PA) midway. Track partner outcome.
- **Stuck** — claim is stalled. Recommend a partner. Capture consent. Pass lead.
- **Litigated** — user has retained counsel. Continue as a passive document organizer for them.

### 10. Long-term home + post-claim relationship

- Document vault for the resolved claim (policy, claim file, correspondence, settlement)
- Anniversary reminder when policy renews (suggest review)
- Future loss support (return user)
- Optional content / educational hub on insurance literacy

## Feature inventory by phase

### Phase 2A — Florida Concierge Beta (target: August 1, 2026)

- Account creation, auth (Clerk or Supabase Auth)
- Mobile-responsive web app only; no native app yet
- Florida-only eligibility gate
- Policy PDF upload + OCR fallback
- LLM extraction with citations for hurricane wind/roof claims
- Plain-English policy summary
- Claim intake questionnaire (hurricane wind/roof)
- Photo/document upload (no vision classification yet)
- Carrier offer / letter upload + side-by-side issue spotting
- Draft correspondence generator (first 3 templates: request for itemized estimate, supplemental documentation request, response to denial / underpayment)
- Internal human review queue for every draft before the user sees it
- User review-and-approve workflow before any document leaves
- User-controlled email transmission of drafts to carrier
- Deadline tracker v0 with manual verification
- TCPA-compliant consent flow before any B2B referral
- Audit log of every AI output and user action
- Soft launch only in Florida

### Phase 2B — Productized Florida MVP

- Improved extraction validation and confidence thresholds
- Carrier response handling
- Expanded draft library (appraisal-related language only after legal review)
- SMS + email reminders
- Calendar export
- Document vault
- Product analytics and support tooling
- Email delivery receipts where technically available
- Partner referral screen with explicit consent capture
- Florida event-specific landing pages

### v1 (Phase 3 — Multi-State Hurricane)

- React Native mobile app at parity with web
- Spanish-language UX
- Certified mail integration (Lob or similar) for evidentiary correspondence
- E-sign integration (DocuSign or Dropbox Sign)
- Carrier-specific tactic playbooks for top 5 carriers
- Vision-model preliminary damage classification
- Partner-recommendation screen with consent capture
- Settlement-tracking and partner outcome feedback loop
- Trust signals: reviews, success stories, BBB equivalent
- Improved disaster-event onboarding (event-specific landing pages)

### v2 (Phase 4 — Multi-Peril)

- Water damage flow (interior flooding, plumbing)
- Hail damage flow
- Wildfire flow
- Wind/tornado flow
- Freeze / pipe burst flow
- Per-peril evidence checklists
- Per-peril playbooks
- Multi-claim support per user (some users will be repeat customers)
- Carrier portal integrations where they exist (start with one or two cooperative carriers)

### v3 (Phase 5 — National Scale)

- Self-service partner-referral marketplace from within the consumer app
- Educational content library (policy literacy, what to do before a disaster)
- Pre-disaster mode: hurricane-approaching prep, photo inventories, policy review
- Possible carrier partnership program (e.g., progressive carriers who want to surface us to their policyholders)
- Long-term home / household account: track multiple properties, multiple claims over time

## Specific UX flows worth calling out

### Concierge review flow (2026 beta)

During the Florida beta, AI output is not immediately shown as final guidance.

1. User uploads policy, claim facts, photos, and carrier documents
2. AI produces extraction, issue summary, and draft correspondence
3. Internal reviewer checks citations, tone, forbidden language, and Paymon playbook fit
4. Reviewer either approves, edits, requests more information, or escalates to Paymon
5. User receives the approved draft and remains the final sender / decision-maker
6. Every AI output, reviewer action, and user approval is logged

### Consent-to-share flow (TCPA / B2B referral)

When the user reaches a moment where a partner referral is appropriate, the flow is:

1. We explain in plain language what's being recommended and why
2. We disclose the specific partner or partner category that may contact them and through what channels
3. We disclose how partner compensation works (we get paid for the referral)
4. We require an explicit checkbox + signature for SMS / phone outreach
5. We log the consent with timestamp, IP, device, and exact consent-screen text shown
6. The user can revoke at any time, and revocation is propagated to partners where applicable

### Document-out flow (sending to carrier)

When a draft is ready:

1. User reviews the full draft
2. User can edit any line
3. User reviews the cited clauses
4. User chooses transmission method: email for beta; certified mail, e-fax, and carrier portal are later options
5. User signs (typed signature or e-sign as needed)
6. We capture: sent timestamp, hash of final content, delivery receipt where applicable
7. Reminder set for carrier response window

### Confidence and escalation flow

When the AI's confidence is low or the situation calls for human review:

1. Plain-language explanation: "This is more complex than what we can guide you through with confidence."
2. Options: (a) try anyway with extra warning, (b) ask for help from a partner
3. If they choose partner help, the consent-to-share flow runs

## Technical implementation notes

### LLM orchestration

- Long-context model (Claude with prompt caching) for policy ingestion
- Structured outputs (JSON schema) for every extraction
- Retrieval over chunked policy clauses for citation generation
- Streaming responses for draft correspondence so the user sees progress

### Mobile vs. web

- Web (Next.js) is the primary build target through the 2026 hurricane-season launch
- React Native (Expo) version targeted for Phase 3
- Same backend, same data models, same auth tokens
- Native camera handoff for damage photo capture (especially important on iOS)

### Notifications

- Twilio for SMS (TCPA-compliant flow)
- Resend for transactional email
- Push notifications via Expo (Phase 3+)

### Document storage

- S3 with per-user KMS keys
- Original documents retained for the life of the claim plus retention policy
- Anonymized derivative data may flow to a separate dataset for model improvement (with consent)

### Performance targets

- First contentful paint under 1.5s on 4G
- Policy upload to first summary in under 3 minutes
- Draft generation in under 30 seconds streamed

## Trust & brand plan for August 1 launch

Brand new domain, no reviews, no BBB rating, no press. A panicked Florida homeowner who just lost half their roof to a hurricane has no reason to upload their insurance policy to us. Trust has to be deliberately constructed before launch, not earned organically after.

**Pre-launch trust assets (must have by August 1):**

- **Founder bios with photos** on the site — Paymon's industry credibility, Eli's CRM Wizards background, Lauren's enterprise experience. The "who is behind this" answer must be immediately findable.
- **5+ testimonials from real Florida homeowners** Paymon worked with at Noble (with their permission) — these are the only social proof we'll have at launch.
- **BBB application submitted** (rating won't arrive for months but the application itself is a trust signal).
- **Privacy policy + Terms of Service + clear "we are software, not a public adjuster" page** prominently linked.
- **Press kit and 1-page explainer** Paymon can send to insurance-industry contacts and any journalists.
- **One Paymon-authored thought-leadership post** on the broken state of public adjusting — establishes credibility and frames the launch narrative.
- **Comparison page**: us vs. PA vs. attorney vs. doing nothing. Honest about when each is the right call.
- **Security and privacy callouts**: encryption, audit log, "your data is yours" language.

**Launch-time trust mechanisms:**

- Free phone consultation with a real human (concierge beta is the differentiator)
- Visible "Florida homeowners only for now" copy — signals we're not pretending to be national
- Public commitment: "We never share your information without your explicit consent, every time"

**Post-launch trust accumulation:**

- Settlement-story collection workflow built into the resolution step
- Review-request flow to BBB, Google, Trustpilot
- Local press / community outreach in Florida (especially in disaster-hit metros)
- Insurance-industry-association outreach (cautiously — there will be hostility)

## Open questions for the consumer side

- Do we accept claims that are already in litigation? (Probably not — passive document vault only.)
- Do we accept commercial / business policies? (Phase 5 question.)
- Do we want a referral program where existing users invite neighbors after a disaster?
- Do we ever charge consumers (premium tier vs. always-free)?
- Do we want to capture pre-disaster policies (i.e., users sign up BEFORE a loss happens)?
