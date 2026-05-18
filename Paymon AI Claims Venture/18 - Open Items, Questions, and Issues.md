# Open Items, Questions, and Issues — Working Log

> Consolidates the prior 06 (Open Questions & Risks), 11 (Questions for Paymon), and 18 (Issues to Raise With Paymon) into a single working log. Update items as they get resolved or new ones emerge.

## How to use this doc

- **Section A — Open questions and risks across the venture.** General running list; review periodically.
- **Section B — Priority issues to raise with Paymon.** Founder-level meeting agenda fodder, organized by priority.
- **Section C — Playbook-capture working-session questions.** Interview questions for working sessions when you need Paymon's domain expertise encoded into the product.
- **Section D — Items deferred from May 8.** Carryover from the May 8 meeting decisions.

When an item gets resolved, either move it to a "Resolved" subsection at the bottom or strike it through with the date. Don't delete — the history is useful.

---

# Section A — Open questions and risks across the venture

A running list of unresolved items spanning strategy, business model, partnership, technology, regulatory, and operations. When an item is specifically Paymon-dependent, the cross-reference points to its detailed treatment in Section B.

## Strategic / competitive

- **What's Bo's actual launch timeline for Turboax?** Worth pinging Valerie (carefully, given Eli's relationship there) to triangulate. (See Section B §5.1)
- **Could the insurance industry try to acquire and shelf this?** Paymon's threat call-out. Affects capitalization choices and long-term governance.
- **Is the disruptive framing defensible against a well-funded fast-follower?** Or is the moat really just Paymon's relationships + encoded playbook?

## Business model

- **What's the realistic CAC during peak disaster events?** Paymon has historical Noble numbers — get them. (See Section B §3.2)
- **What's the take rate B2B partners will accept?** Need a few real conversations with attorneys and roofers, not assumptions. (See Section B §4.1, §4.2)
- **What % of users actually convert to a B2B referral?** Drives whether free-to-consumer math actually works.
- **Is a small consumer fee ($50–$200) better than fully free?** Filters tire-kickers, generates float, but adds a conversion barrier.

## Partnership

- **Equity split among Paymon / Eli / Lauren?** Open. Drives every other partnership conversation. (See Section B §3.3)
- **What does Paymon's contribution look like financially?** Cash investment, marketing-spend commitment, sweat equity, or a mix? (See Section B §3.1)
- **IP ownership during Phase 1** — does Nagy's work flow through CRM Wizards or directly into the new entity? Affects how the work is structured.
- **Nagy equity structure** — international contractor complications. Lauren's action item.
- **Decision-making authority** — tie-breakers, who has final call on what? (See Section B §3.4)

## Technology

- **Can frontier LLMs hit ≥95% extraction accuracy on real policies with valid citations?** The single biggest unknown. Phase 1 Workstream A answers it.
- **Which model — Claude vs. GPT-5 vs. Gemini?** Probably doesn't matter for feasibility, will matter for production cost.
- **OCR quality on scanned-image policies?** Many homeowners only have a scanned PDF, not a digital one.
- **Vision model accuracy on damage classification?** Deferred until policy comprehension is proven.
- **Carrier estimate parsing (Xactimate / Symbility / Simsol)** — second-most-valuable AI capability after policy comprehension; how reliable is line-item extraction?
- **Long-term: do we need to fine-tune or train our own model?** Probably not for v1, but worth knowing the path.

## Regulatory

- **Which state is the strictest — and does our model survive there?** Workstream B answers this for FL first.
- **Does "software tool" framing actually hold under scrutiny, or is there state-level case law that pierces it?**
- **What's the disclaimer language pattern that's been tested?** TurboTax, LegalZoom, RocketLawyer — and what specifically got DoNotPay in trouble.
- **Do we need outside counsel for Phase 1, or can we do the desk research first?** Decided: counsel engaged by Week 2 with desk research feeding into the memo. (See Section B §1.4)
- **Florida-specific:** SB 2A, HB 837, AOB ban (HB 7065), FTSA — all directly shape product mechanics and partner economics. Covered in [15 - Software Build Plan.md](15%20-%20Software%20Build%20Plan.md).

## Operational

- **When do we actually need someone full-time?** Nights/weekends works for feasibility but won't scale to launch. Tie this to a clear funding/revenue trigger.
- **What's the right Phase 1 budget?** Working assumption: $25–40k for Phase 1–2A infrastructure (LLM API, hosting, counsel, etc.) — source TBD. (See Section B §3.1)
- **What's the cadence trigger for Phase 1 → Phase 2?** Specific deliverable acceptance (go/no-go on extraction accuracy) plus calendar date (June 16).

---

# Section B — Priority issues to raise with Paymon

Not gotchas. These are things Eli and Lauren need answers to before committing to the aggressive August 1, 2026 launch, and most of them only Paymon can answer. Sequence by priority when you next meet.

## Priority 1 — Critical-path dependencies (block August 1 launch)

These directly determine whether the August 1 Florida launch is achievable. They need answers within the next two weeks.

### 1.1 Policy supply timeline

**The ask:** 5–10 anonymized real Florida homeowners policies, ideally across 2+ carriers (Citizens, State Farm, Universal, Allstate, etc.), by **May 20, 2026**.

**Why it matters:** Phase 1 Week 1 starts with these. If they slip a week, every downstream date slides into mid-August.

**Questions for Paymon:**
- Can he get them by May 20? If not, by when?
- Does he have the anonymization workflow already, or do we need to build it?
- Are there any Noble contractual restrictions on using policies he handled at Noble for our purposes?

### 1.2 Paymon's weekly time commitment

**The ask:** Predictable weekly availability for playbook capture, draft review, and partner outreach.

**Why it matters:** Paymon is the single point of failure on three critical-path items: playbook encoding, partner pre-commits, and draft validation. Each requires hours of his time per week, every week, through August.

**Questions for Paymon:**
- What is his realistic weekly availability in May–August? (Hours/week, days that work)
- Can he commit to a recurring 2–3 hour playbook capture session?
- Is there other work demanding his time that might compete?
- At what trigger does he go all-in vs. side project?

### 1.3 Partner pre-commit reality check

**The ask:** Named, specific 3–5 partners pre-committed by end of July 2026.

**Why it matters:** The B2B platform launches empty without these. The plan currently treats his pre-commit claim as validated; it isn't yet.

**Questions for Paymon:**
- Which specific partners would he call first? Names, firms, geographies.
- What's his honest assessment of conversion likelihood for each?
- Has he already had any partner conversations about this venture, or will he be starting cold in May?
- Has FL HB 837 changed insurance attorneys' willingness to pay for leads? What is he hearing from his attorney contacts on current per-lead economics?
- What CRM or intake systems do those partners use? (Lead Docket, Filevine, Litify, email-only, phone-only?)

### 1.4 Outside counsel engagement

**The ask:** Engaged Florida insurance-regulatory counsel with TCPA experience by Week 2 of Phase 1.

**Why it matters:** Phase 1 Week 4 regulatory memo depends on counsel. Counsel turnaround is often 2–4 weeks for a focused memo.

**Questions for Paymon:**
- Does he have a recommended attorney he's worked with at Noble?
- Are there any conflicts of interest (Noble-related representation)?
- Is he willing to make the initial intro, or should Eli/Lauren handle the engagement directly?

## Priority 2 — Legal & IP risks (could materially constrain or end the venture)

### 2.1 Noble non-compete and IP assignment

**Why it matters:** If Paymon signed a non-compete or IP assignment with Noble that covers this venture's subject matter, we could face an injunction or IP claim from Bo at the worst possible moment (e.g., right after launch when we have real revenue and Bo has noticed).

**Questions for Paymon:**
- Did he sign a non-compete with Noble? Scope (geography, time, subject matter)?
- Did he sign any IP assignment that could cover insurance-claim AI ideas he pitched while there?
- How is his departure documented? Resignation, mutual separation, severance?
- Was the AI claims idea originated before his Noble tenure, during, or after?
- Has his attorney reviewed his Noble obligations specifically for this venture?
- **Suggested action:** Get a clean opinion letter from his counsel before Phase 2A launch confirming this venture is permissible.

### 2.2 Use of Noble materials

**Why it matters:** Even if there's no non-compete, using Noble's templates, client lists, playbooks, or contact databases as inputs to our product is high legal risk.

**Questions for Paymon:**
- Is he bringing any Noble materials (templates, contact lists, playbook documents)?
- His "playbook" — is that his own knowledge and methodology, or is it documented somewhere Noble could claim?
- Will any past Noble clients be early users or partners?
- **Suggested action:** Explicit policy: no Noble-owned materials enter the venture. All playbook capture done from scratch through new conversations.

### 2.3 Paymon's current Florida PA license status

**Why it matters:** If he still holds an active Florida PA license:
- Plus: credibility, can field complex cases through Phase 2A
- Minus: positioning risk — if a regulator argues that a licensed PA running a "software tool" is constructively practicing public adjusting, the whole regulatory wedge weakens
- Tradeoff: should he be a public-facing officer of the company, or stay quiet to keep the "software not representation" line clean?

**Questions for Paymon:**
- Is his FL PA license currently active?
- Is he willing to surrender / suspend it if counsel recommends?
- How public should his role be on the consumer-facing brand?

### 2.4 Florida Bar / industry hostility risk

**Why it matters:** Florida's insurance industry is well-organized and politically connected. Carriers, the Florida Bar's insurance section, or carrier-aligned legislators could push for regulatory action against us.

**Questions for Paymon:**
- Has he seen any patterns of industry retaliation against similar disruptive products?
- Does he have relationships with anyone on the regulatory side (DFS, OIR) we should brief proactively?
- Should we engage a lobbyist before launch, or wait until we have traction?

## Priority 3 — Financial & equity expectations

### 3.1 Cash investment vs. ownership

**Why it matters:** Phase 1–2A needs $25–40k of infrastructure cash (LLM API, hosting, counsel fees, etc.). Someone has to fund it. The "ownership-only comp" assumption doesn't cover external spend.

**Questions for Paymon:**
- Is he willing/able to contribute cash, and how much?
- If yes, does that adjust his equity stake upward?
- If no, who funds Phase 1–2A? Eli? A small friends-and-family round?
- Is he expecting Eli/Lauren to bear the cash burden in exchange for engineering ownership?

### 3.2 Marketing budget for August launch

**Why it matters:** Disaster-event ad campaigns require real spend. Even a modest hurricane-event burst is $5–25k in FL. That money has to come from somewhere.

**Questions for Paymon:**
- What ad spend did he run at Noble during hurricane events? Order of magnitude?
- Does he have existing FB / Google ad accounts and creative templates he'll bring?
- Who funds the initial ad spend — him, the venture, Eli?
- Are there partner-funded ad arrangements (attorneys paying for branded campaigns) we should consider?

### 3.3 Equity split among Paymon / Eli / Lauren

**Why it matters:** Open from May 8 meeting. Drives every other partnership conversation. Should be aligned before Phase 2A launch.

**Questions for Paymon:**
- What does he expect his stake to be? (No wrong answer, but we need the number)
- Does that change based on cash investment vs. relationship contribution?
- How should Nagy's equity grant interact with the founder split?
- What vesting schedule does he expect for himself and others?

### 3.4 Decision-making authority

**Why it matters:** When we disagree on product / legal / positioning, who decides? This needs to be settled before a real disagreement happens under launch pressure.

**Questions for Paymon:**
- Comfortable with a three-way founder vote with Eli as tie-breaker on technology, Paymon as tie-breaker on industry/marketing, Lauren as tie-breaker on business/legal?
- Or different structure?

## Priority 4 — Operational realities

### 4.1 Post-HB 837 attorney economics

**Why it matters:** Paymon's "law firms with multi-million marketing budgets ready to buy leads" pitch may have been pre-2023. HB 837 hit FL insurance attorneys hard.

**Questions for Paymon:**
- What is he hearing from his attorney contacts about post-HB 837 lead pricing?
- Are some firms exiting first-party property entirely? Which ones?
- Which attorneys are doubling down — bad-faith specialists, public adjuster-adjacent firms?
- What's a realistic per-lead price in Florida today, vs. 2022?

### 4.2 Post-AOB-ban roofer economics

**Why it matters:** Florida roofers used to take AOBs and bill carriers directly. Now they bill homeowners. Different cash-flow and risk profile changes their willingness to pay for leads.

**Questions for Paymon:**
- Are roofers in his network still aggressive on storm-damage marketing post-AOB-ban?
- Did some shift to "we'll inspect for free, you handle the claim, we do the work after settlement"? If yes, that's effectively our target consumer journey — partners are aligned by default.
- Realistic per-lead price for FL roofers today?

### 4.3 Citizens insurer-specific playbook

**Why it matters:** Citizens is the insurer of last resort for many FL homeowners. Almost every Florida claim will involve them. We need a Citizens-specific playbook for launch.

**Questions for Paymon:**
- How much of his Noble experience was Citizens claims?
- What are Citizens-specific tactics, denial patterns, and successful counter-strategies?
- Any Citizens-internal contacts useful for understanding their workflows?

### 4.4 Multi-state expansion realism

**Why it matters:** The plan extends FL → LA + TX in Phase 3. His relationships are concentrated in FL.

**Questions for Paymon:**
- Does he have warm partner contacts in LA or TX?
- Or is each state going to require fresh BD from cold?
- How long did it take Noble to build the FL network? Is that a fair benchmark for new-state buildout?

## Priority 5 — Competitive intelligence

### 5.1 Bo's Turboax — current state

**Why it matters:** Bo's launch timing is one of the biggest external risks. We need real intel, not speculation.

**Questions for Paymon:**
- What has he heard about Turboax since the May 8 conversation?
- Any new signal from Valerie? Other ex-Noble contacts?
- Any social / job posting signals that would indicate launch readiness (engineering hires, marketing hires)?
- Should we set up a periodic monitoring routine?

### 5.2 Other competitors in the space

**Questions for Paymon:**
- Who else is building anything adjacent? (Hover, EagleView, claim-management software vendors moving downstream into homeowner self-help)
- Any startups he's aware of trying similar disruption?
- Is anyone in the legaltech / insurtech space close to our positioning?

## Priority 6 — Brand & positioning

### 6.1 Paymon as the face — or not

**Why it matters:** Founder credibility is one of our few launch trust assets. But Paymon's PA past + Noble connection cuts both ways.

**Questions for Paymon:**
- Willing to be publicly named, photographed, and quoted on the site?
- Willing to author the thought-leadership post on broken public adjusting?
- Or does he need to stay quieter for Noble / industry reasons?

### 6.2 Testimonials from past Noble clients

**Why it matters:** With no operating history, real Florida-homeowner testimonials are our only social proof.

**Questions for Paymon:**
- Are there past Noble clients he could approach for a testimonial — even one that just speaks to his integrity and expertise generally, not Noble specifically?
- Any reputational risk in asking?

### 6.3 The "all-claim-types" ambition

**Why it matters:** The roadmap targets all perils + all states. Paymon's depth is property + hurricane.

**Questions for Paymon:**
- Beyond hurricane, where does he have real playbook depth? Water? Fire? Wildfire? Hail?
- Where does he have only general industry knowledge?
- For perils outside his depth, does he have domain experts in his network we could bring in for Phase 4?

---

# Section C — Playbook-capture working-session questions

These are interview questions for working sessions where the goal is to extract Paymon's claims expertise into product logic — decision trees, correspondence templates, deadline rules, evidence checklists, escalation patterns. Different use case than Section B (which is founder-level alignment). Use these to structure a 2–3 hour working session.

## Claim workflow

- For hurricane wind / roof damage, what are the first 10 questions a good desk adjuster asks?
- What facts determine whether a claim is easy, medium, or complex?
- What are the most common carrier lowball tactics?
- What are the most common homeowner mistakes that hurt their claim?
- What are the pre-litigation steps that must happen before an attorney can do anything useful?
- Which steps are state-specific vs. generally reusable?

## Policy interpretation

- Which policy clauses matter most for wind / roof claims?
- Which exclusions are most commonly misapplied by carriers?
- Which endorsements change the outcome most often?
- What are the top 10 clauses you would teach a new desk adjuster to find?
- What would make an AI-generated policy summary dangerous or wrong?

## Documentation and evidence

- What photos are required for a strong claim?
- What photos do homeowners usually forget?
- When is a roofer estimate enough?
- When is a third-party estimator / Matterport scan necessary?
- What does a "proof of loss" package need to include?
- What evidence makes the difference between a weak claim and a strong one?

## Correspondence

- What are the standard letters / emails in the workflow?
- What language tends to move carriers?
- What language should we avoid because it creates regulatory or legal risk?
- How should the system document contact attempts?
- What response deadlines matter?

## Business model validation

- Which 3–5 attorneys would you call first for a lead pre-commit?
- Which 3–5 roofers / contractors would you call first?
- What lead fields would those partners need to decide whether to pay?
- What is a realistic lead value by category?
- What did Noble historically pay in CAC for storm/disaster leads?

## Regulatory / positioning

- What states do you believe are most important for launch?
- Which states are most hostile to public adjusting or claims assistance?
- Where did Noble need certification most urgently?
- What exactly can a non-PA say or not say in your experience?
- What language would make you nervous if it appeared in the product?

---

# Section D — Items deferred from May 8 meeting

Carryover from the May 8 meeting decisions, kept here as the authoritative record.

- **Nagy partnership structure** — decided to revisit after internal discussion (NEEDS FURTHER DISCUSSION)
- **Nagy compensation structure** — Lauren's action item; equity vs. contractor TBD

---

# How to use this document

- **Before each Paymon meeting**, scan Section B priorities 1–3 and pick 2–4 items to raise
- **Before each working session with Paymon**, scan Section C and pick the topic area for the session
- **Update Section A** as the working venture-wide questions log
- **Resolved items**: append a "Resolved (date)" tag rather than deleting — the history is useful
- **Don't ambush Paymon** with the whole list in one meeting — these are real concerns and need real conversation, not a checklist sweep
- **Lauren should see this doc before each meeting** so she's not blindsided by anything sensitive (especially Section B Priority 2 legal items)
