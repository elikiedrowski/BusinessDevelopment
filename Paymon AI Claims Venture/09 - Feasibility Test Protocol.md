# Feasibility Test Protocol

This is the tactical test plan for Phase 1. It converts the feasibility study into concrete experiments Eli and Naggie can run.

## Test 1 — Policy extraction with citations

**Question:** Can an LLM extract policy facts accurately enough to support claim guidance?

**Inputs needed:**
- 5-10 anonymized homeowners policies
- At least 2 carriers
- At least 1 scanned / OCR-needed policy if possible
- Ground-truth answers from Paymon or a qualified reviewer

**Schema to extract:**
- Named insured
- Property address
- Carrier
- Policy period
- Deductibles
- Coverage A / B / C / D limits
- Wind / hail / hurricane provisions
- Water / mold sublimits if present
- Duties after loss
- Proof-of-loss requirements
- Appraisal / mediation / dispute provisions
- Relevant exclusions
- Relevant endorsements that modify base terms

**Scoring:**
- Correct value
- Citation present
- Citation actually supports the value
- Missed critical clause
- Hallucinated clause

**Pass condition:** No critical misses on exclusions, deductibles, limits, or endorsements in the sample set. Quantitative target ideally ≥95%, but critical-error rate matters more than average score.

## Test 2 — Carrier offer interpretation

**Question:** Can the system compare a carrier offer / estimate against the policy in a useful way?

**Inputs needed:**
- 3-5 anonymized carrier letters or estimates
- Corresponding policy
- Paymon's explanation of what is wrong or incomplete

**Output expected:**
- Plain-English summary of the carrier's position
- Gaps / underpaid categories
- Evidence needed to push back
- Suggested next action

**Pass condition:** Paymon agrees the analysis identifies the major issue and does not invent unsupported arguments.

## Test 3 — Draft correspondence quality

**Question:** Can the system draft letters that are useful, professional, and aligned with Paymon's playbook?

**Inputs needed:**
- Policy extract
- Claim facts
- Carrier offer
- Paymon's desired next step

**Output expected:**
- Email / letter draft in the homeowner's voice
- Clause citations
- Specific ask
- Deadline / response request
- No representation language

**Pass condition:** Paymon would be comfortable with the homeowner sending a lightly edited version.

## Test 4 — Regulatory-language boundary

**Question:** Can the UX and wording avoid drifting into representation or legal advice?

**Review checklist:**
- Does the product say "we will negotiate for you"? If yes, fail.
- Does it say "you should sue" or make legal conclusions? If yes, fail.
- Does it force review before sending anything? If no, fail.
- Does it explain uncertainty and recommend qualified help when needed? If no, fail.
- Does it keep the homeowner as the sender / actor? If no, fail.

## Test 5 — Lead qualification logic

**Question:** Can we identify when a user is a valuable partner lead without degrading the consumer experience?

**Inputs:**
- Claim type
- Location
- Estimated severity
- Carrier offer status
- Damage categories
- User intent / urgency

**Output expected:**
- Partner category recommendation
- Lead score
- Reason code
- Consent prompt language

**Pass condition:** Paymon and Lauren agree the scoring is commercially plausible and does not feel predatory.

## Phase 1 final package

The final feasibility packet should include:
- Prototype screenshots or demo
- Extraction scoring table
- Failure-mode summary
- Regulatory boundary memo
- Recommended MVP scope
- Engineering estimate
- Open risks that remain after testing
