# Naggie Technical Brief

Purpose: give Naggie a clean technical starting point for Phase 1 without asking him to digest the full meeting transcript.

## What we're testing

Can we build an AI-assisted insurance claim tool that helps homeowners represent themselves, starting with one narrow claim type?

The first feasibility target is **policy comprehension with citations**. If this fails, the rest of the platform is premature.

## First prototype

Build a small local or lightweight web prototype that:
- Accepts a homeowners policy PDF
- Extracts text with page references
- Runs a model extraction against a fixed schema
- Returns structured JSON
- Includes citations to exact policy text
- Provides a simple reviewer view to mark each extracted field correct / incorrect

## Required extraction schema

Start with:
- Carrier
- Policy period
- Property address
- Coverage A/B/C/D limits
- Deductibles
- Wind / hail / hurricane coverage
- Relevant exclusions
- Relevant endorsements
- Duties after loss
- Proof-of-loss requirements
- Appraisal / mediation / dispute provisions

## Design constraints

- No production polish.
- No full app.
- No lead marketplace.
- No automated legal advice.
- Every extracted claim must cite source text.
- Every output should be easy for Eli / Paymon to review manually.

## Suggested implementation path

1. Parse PDFs and store page text.
2. Build a policy extraction prompt using a strict JSON schema.
3. Run the same document against 2-3 model options if easy.
4. Create a scoring CSV / simple UI for review.
5. Document failure modes.

## Definition of done

Phase 1 prototype is done when Eli can sit with Paymon and review:
- 5-10 anonymized policies
- Extracted fields
- Citations
- Accuracy score
- Failure modes
- Recommendation: proceed, narrow scope, or stop

## Inputs needed before starting

- 5-10 anonymized policy PDFs
- A sample carrier estimate / offer letter if available
- Paymon's answer key or availability for review
- Preferred model API keys / budget
