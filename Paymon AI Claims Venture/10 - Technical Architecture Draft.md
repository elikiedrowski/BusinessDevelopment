# Technical Architecture Draft

> **Status:** Original architecture sketch. The current authoritative architecture overview is in [15 - Software Build Plan.md § Architecture overview](15%20-%20Software%20Build%20Plan.md). This doc retains useful data-object schemas (Policy, Claim, Evidence item, Partner lead) that supplement 15 — Nagy should reference both when starting the prototype.

Early architecture sketch for feasibility. This is not a production commitment.

## Guiding principle

Use boring infrastructure and frontier models. The hard part is the claim workflow, not web app plumbing.

## Prototype architecture

```text
User Uploads
  -> Document Store
  -> OCR / Text Extraction
  -> Clause Chunking
  -> Retrieval Index
  -> LLM Extraction
  -> Structured Policy JSON
  -> Reviewer / Scoring UI
  -> Claim Guidance + Draft Correspondence
```

## Core modules

### Document ingestion

- Accept PDFs, images, and carrier letters
- Preserve original file
- Extract text with page numbers
- Run OCR where text extraction fails
- Store per-page text and metadata

### Policy extraction

- Chunk policy by sections / endorsements
- Retrieve relevant clauses by question
- Extract into a strict JSON schema
- Require citation for every field
- Validate citations against source text

### Claim intake

- Guided questionnaire for one MVP claim type
- Structured fields, not freeform only
- Capture cause of loss, date of loss, location, known damage, carrier actions, user goals

### Guidance engine

- Combines policy JSON + claim intake + Paymon playbook rules
- Produces next-step recommendation
- Shows confidence and required evidence
- Escalates to human / partner when confidence is low

### Drafting engine

- Produces homeowner-owned drafts
- Inserts cited policy clauses
- Avoids representation language
- Requires user approval before sending

### Lead routing

- Computes partner category and lead score
- Captures consent
- Sends minimal required information to partner
- Tracks outcome for revenue attribution

## Data objects

### Policy

- `policy_id`
- `carrier`
- `insured_name`
- `property_address`
- `policy_period`
- `coverages`
- `deductibles`
- `exclusions`
- `endorsements`
- `duties_after_loss`
- `dispute_provisions`
- `citations`

### Claim

- `claim_id`
- `policy_id`
- `date_of_loss`
- `cause_of_loss`
- `damage_categories`
- `claim_status`
- `carrier_offer_amount`
- `evidence_items`
- `tasks`
- `deadlines`

### Evidence item

- `evidence_id`
- `claim_id`
- `type`
- `source_file`
- `captured_at`
- `description`
- `linked_policy_clause`
- `confidence`

### Partner lead

- `lead_id`
- `claim_id`
- `partner_category`
- `geo`
- `lead_score`
- `reason_codes`
- `consent_status`
- `sent_at`
- `outcome`

## Model strategy

Run a bake-off during feasibility:
- Same policies
- Same extraction schema
- Same scoring rubric
- Compare Claude, OpenAI, and Gemini if available

Pick based on:
- Citation reliability
- Critical-error rate
- Cost per policy
- Latency
- JSON/schema compliance

## Production concerns to defer

- SOC 2 implementation details
- Full partner portal
- Marketplace bidding
- Multi-state rule engine
- Automated damage valuation
- Fine-tuning
- Custom model hosting
