# Meeting Notes & Brainstorm — Lauren Kiedrowski / Patrick Cameron

**Date:** April 9, 2026
**Attendees:** Lauren Kiedrowski, Patrick Cameron
**Purpose:** ParkM project status + brainstorm new business ideas Patrick wants us to help shape

---

## Part 1 — ParkM Status Update

### Business health
- ParkM closed **54 deals in March 2026**, more than double the 22 deals closed in March 2025.
- Another sales team is being onboarded but is not yet productive — natural expansion point for any use case we ship.
- Top business priority is the marketing side. Robert Phillips is being brought in to fix email deliverability (80,000 emails/month generating only ~20 leads).
- **Opportunity for us:** Patrick explicitly flagged that downstream systems impact from the deliverability fix "may be an opportunity for you guys too." Worth actively tracking.

### Phase 1 status (current project)
- Katie estimates they are 75–80% complete with phases 1–3.
- Delays driven by ParkM-side tasks, not our delivery.
- Scope expanded when Sadie requested a more robust 94-item list for rep steps, requiring full UAT of each item.
- **Eli built a testing spreadsheet** to make UAT easier for a team unfamiliar with acceptance testing — Patrick specifically called this out as the right move.
- Katie told Patrick "2–4 weeks" to go-live; Patrick is pushing for 2.
- Patrick will whisper in Chad's ear to apply pressure on Katie. Waiting 4 weeks yields no better data.

### Go-live strategy (Patrick's suggestions to increase comfort)
- **Option A:** Split the inbound email flow — run live traffic through the system in parallel to observe behavior.
- **Option B:** Just turn it on. Risk is low; bad advice is still reviewable by an experienced human.
- ParkM is generally unsophisticated with technology, so "training wheels" artifacts (testing spreadsheets, opinionated testing guidance) help things move faster.
- **Action for Eli:** have a go-live runbook ready so we are not the blocker when Chad applies pressure.

### Next ParkM use case: Google Reviews
- Chad's #1 priority after phase 1 ships because it drives *immediate* leads.
- Low-disruption: no existing operational workflows to retrain — leads simply pump into Zoho as they come in.
- We should be ready to scope this the moment phase 1 ships.

### Process lesson learned: copy the sponsor from day one
- Eli did not copy Chad on weekly updates from the start. Chad was on the initial meetings, so he should have been kept in the loop throughout.
- Eli's concern: copying Chad now risks undermining the relationship he has built with Katie and Sadie.
- **Agreed going-forward rule:**
  - For any new use case, copy Chad (or equivalent sponsor) from the very first kickoff so they see every stage-gate.
  - Copy Patrick as well when he is helping shepherd the project.
- Worth codifying as an internal SOP so we do not re-litigate per project.

### Why the near-term ParkM roadmap is low-risk
- The priorities Chad cares about (Google Reviews, rep digest, etc.) do not touch existing operational systems.
- Leads can be pumped into Zoho. No workflow retraining needed.
- Easier to show value fast without disruption — plays to our strengths.

---

## Part 2 — Patrick's New "Jelly" Ideas

> Lauren's framing: these are **loss leaders** — things we would build at our own expense to generate pipeline, win future client business, and/or sell as a product. Patrick wants our thoughts on how we might bring technology value in these two areas.

### Idea 1 — Sell 90 Score (Rep Productivity Benchmark)

**Patrick's concept:** A HubSpot-grader-style free tool that scores a sales org's rep productivity 0–100, then recommends AI-driven improvements to help them reach "Sell 90."

**Brainstorm:**

1. **Low-touch "3 questions + 3 system connections" version** (Patrick's preference)
   - OAuth into Gmail/Outlook, CRM (HubSpot/Salesforce/Zoho), Calendar.
   - Score dimensions:
     - *Selling Time %* — calendar meetings with external domains / total working hours
     - *Pipeline Hygiene* — stale deals, missing next steps, empty required fields
     - *Follow-through* — response latency on inbound
     - *Admin Drag* — internal vs. external email ratio
     - *Activity Cadence* — touches per open opportunity
   - Output: shareable scorecard + "how to get to 90" recommendations.

2. **Solving the "can't measure admin" problem Lauren raised**
   - Don't try to classify every internal email. Invert the question: measure *external-facing output* and treat everything else as admin leakage. You don't need to see inside admin work — you just need the ratio.
   - Lightweight connectors (Chrome extension, MCP-style) beat heavy integrations for a free offer.
   - Benchmark against anonymized cohort data — defensibility grows with adoption.

3. **Monetization ladder**
   - **Free:** one-time score + PDF report (the Techstars giveaway).
   - **Paid:** monthly tracking, team-level breakdowns, AI coaching recommendations, stack integrations.
   - **Enterprise:** consulting engagement leading to custom build — our wheelhouse.

4. **Content flywheel**
   - Every run generates anonymized data Patrick can blog about ("we scored 500 Series A sales teams; here's what the top decile does differently").

---

### Idea 2 — Spend Your Benjamin (Product Feedback Tool)

**Patrick's concept:** Customers are shown the future product roadmap and given a virtual $100 to allocate across items. Forced prioritization reveals where they actually see value. Patrick has run this manually at JumpCloud and other companies and wants to productize the orchestration, capture, and feedback loop.

**Brainstorm:**

1. **Addressing Lauren's confidentiality concern**
   - Do **not** automate the roadmap reveal. Keep that human (pre-recorded video or live session) — you are sharing some of a company's most sensitive information.
   - Automate only capture, aggregation, and closing the loop.
   - Use watermarked, time-boxed video links (track viewers, auto-expire) and NDA click-through gates.

2. **The tool itself**
   - Customer gets a branded link → watches pre-video → drags sliders to allocate $100 across N roadmap items → leaves optional comments per item → submits.
   - Dashboard for the product team: aggregate allocation, variance across customer segments (ARR tier, vertical, tenure), auto-clustered open-text themes.
   - Auto-generated "close the loop" email to each customer: "Here's what you told us, here's what we're prioritizing, here's why."

3. **Differentiator vs. ProductBoard**
   - ProductBoard collects *requests*. Spend Your Benjamin forces *prioritization under scarcity* — fundamentally different and more useful signal. The $100 constraint is the whole point.

4. **Execution questions to work through**
   - Do customers actually receive a $100 gift card as the incentive, or is the $100 purely a thought exercise?
   - How do you handle customers who forward the video to a competitor? (Webinars have the same leak risk.)

5. **Go-to-market hook**
   - Free for first N customer sessions, then paid.
   - Patrick's blog post becomes top-of-funnel content.
   - Target PMs/CPOs at Series B–C companies via the Techstars community.

---

### Strategic observation tying both ideas together

Both Sell 90 and Spend Your Benjamin are **benchmarking products disguised as free tools**, where the long-term moat is the cross-customer dataset. If we build one, the foundation (auth, data warehouse, anonymization) should anticipate building both. Shared plumbing, two front-ends.

Patrick explicitly wants a free offer he can socialize through the Techstars community. David at Techstars has told him this would "break the demand funnel" — more inbound than we could handle. Patrick will tap out before we do because we can scale delivery and he cannot.

---

## Part 3 — Nearmap / Roofing Cross-Pollination

Patrick does not currently have a business opportunity for us at Nearmap, but the discussion surfaced a potentially valuable angle.

### What Nearmap does
- Aerial imagery via fixed-wing aircraft (not drones).
- Sub-3-inch resolution. Flies Colorado 3x/year.
- Sells **roof scores** to insurance companies and roofing companies — a major revenue driver.
- Uses AI on their imagery to measure roofs and detect changes (e.g. identifying properties without permits for government customers).
- Resolution is detailed enough to notice minute changes like holes in a backyard.

### The opportunity Lauren raised
Lauren specifically wondered whether **Noble Public Adjusting** (Eli's customer) uses Nearmap or similar aerial imagery for assessing insurance claims.

**Questions worth chasing with Eli / Noble:**
- Does Noble currently use aerial imagery in their claim assessment workflow? If so, whose?
- If not, is there a workflow where we integrate Nearmap (or comparable) data into their claim process?
- Could we build a thin tool that ingests aerial roof scores + historical imagery and produces a claim-ready damage report?
- This is a potential warm intro Patrick could make later if we develop a point of view first.

---

## Part 4 — Meta: Lauren's Business Development Gap

Lauren openly acknowledged she has been chasing known relationships without anything strategic or formalized to build a long-term business funnel. Some plays out, some does not, some is deferred.

**Reframe:** The Sell 90 and Spend Your Benjamin work is not just loss-leader product building — it is *the* mechanism to build that formalized funnel. These should be treated as the business development motion itself, not as side projects.

---

## Part 5 — Action Items & Next Steps

### Immediate (this week)
- [ ] Make sure Eli has phase 1 go-live runbook ready — Patrick is about to push Chad to accelerate Katie.
- [ ] Decide on split-flow vs. full-switch go-live approach and communicate to ParkM.

### Near-term (next 2–4 weeks)
- [ ] Scope Google Reviews use case for ParkM. Copy Chad from kickoff this time.
- [ ] Ask Patrick what downstream systems work the Robert Phillips deliverability fix may create.
- [ ] Codify the "copy the sponsor from day one" rule as an internal SOP.

### Brainstorm follow-ups for Patrick
- [ ] Pick one of the two jelly ideas (Sell 90 or Spend Your Benjamin) to prototype first. Sell 90 has broader appeal; Spend Your Benjamin has tighter ICP and may be faster to build.
- [ ] Decide branding: white-label under Patrick's name, co-brand, or stand up our own.
- [ ] Define investment appetite — weeks or months of loss-leader build time?

### Nearmap / Noble angle
- [ ] Separate conversation with Eli about Noble's current aerial imagery workflow.
- [ ] If there is a gap, develop a point of view on a roof-score-to-claim-report tool before asking Patrick for a warm intro.

### Open questions for the next Lauren/Patrick conversation
- Which jelly idea does Patrick want to prioritize?
- What does Patrick's Techstars free-offer launch timeline look like?
- Is there a specific first customer at ParkM who would be a good "Spend Your Benjamin" pilot?
