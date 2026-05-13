# Product Requirements

Working v0 product definition for the consumer-facing insurance claim assistant. This is intentionally scoped to support the Phase 1 feasibility study, not the full company vision.

## Product thesis

Help a homeowner understand their policy, document their loss, and negotiate their own claim before signing away 10-40% of claim value to a PA or attorney.

## Initial user

An ordinary homeowner after a disaster event who:
- Has filed or is about to file a claim
- Does not understand their policy
- Has photos / documents but does not know what matters
- Received a low offer or fears receiving one
- Wants help without immediately hiring a PA or attorney

## Recommended MVP wedge

**Florida hurricane wind / roof damage.**

Why:
- High disaster-event demand
- Strong contractor / attorney lead value
- Paymon's deepest likely playbook
- Clear consumer pain
- Narrow enough for feasibility

## Core user journey

1. User arrives from disaster-event ad or referral.
2. User enters location, carrier, claim status, and damage type.
3. User uploads policy PDF and any carrier correspondence.
4. System extracts coverages, deductibles, exclusions, limits, duties after loss, and relevant endorsements.
5. User uploads photos, estimates, and receipts.
6. System builds a claim file and identifies evidence gaps.
7. System explains what the carrier owes / may owe in plain language with citations.
8. System drafts next-step correspondence for the user to review and send.
9. System tracks deadlines, carrier responses, and follow-up steps.
10. System recommends optional partner help when appropriate: roofer, estimator, attorney, PA fallback.

## MVP must-have capabilities

- Policy PDF upload
- OCR fallback for scanned PDFs
- Clause-level extraction with citations
- Plain-English policy explanation
- Claim intake questionnaire
- Evidence checklist
- Carrier offer / estimate upload
- Draft correspondence generation
- User review-before-send guardrail
- Timeline / task tracker
- Basic lead handoff workflow

## MVP should-not-have capabilities

- Full multi-state support
- Full partner marketplace
- Automated legal conclusions
- Automated communication sent without user approval
- Vision-based final damage valuation
- Public adjuster representation language
- Broad claim-type coverage on day one

## Product guardrails

- The user represents themselves.
- The product generates drafts and guidance, not binding legal advice.
- Every recommendation should show why it was made.
- Every policy claim should cite the source clause.
- When confidence is low, the product should ask for more evidence or recommend human review.

## Product success signal

For the v0 prototype, success is not "the claim is won." Success is:
- The user understands their coverage better than before
- The system catches meaningful policy / evidence issues
- The generated draft is good enough for Paymon to say, "Yes, this resembles what I would tell them to send"
- The lead handoff is valuable enough that a partner would pay for it
