# Agentic AI for Email Marketing Campaign - Learner Guide

**Course code:** TGS-2026064473  
**Version:** v1.0 (24 August 2026)

## Learning outcomes

- LO1: Implement email marketing campaigns in alignment to strategic marketing objectives.
- LO2: Analyse email marketing campaign outcomes using sales performance data and consumer insights.
- LO3: Analyse market trends and customer reviews.

## Platform and safety boundary

Use a training n8n workspace, a non-production MailerLite group and sample leads. Store tokens in n8n Credentials. Never paste a live token into workflow JSON, screenshots or assessment evidence.

## Topic 01: Implementing Agentic AI Email Marketing Campaigns

Consent -> brief -> specialist agents -> approval -> MailerLite schedule

- Translate strategy into a machine-readable campaign brief and acceptance contract.
- Separate audience, research, drafting and compliance roles across agent boundaries.
- Gate every external send or schedule action with deterministic human approval.
- Treat consent, unsubscribe state, secrets and audit evidence as first-class data.

### Lab 1: Consent-First Lead Capture to MailerLite

**Duration:** 75 minutes  
**Alignment:** LO1 | K3 | A1  
**Tools:** n8n Form Trigger, Code, IF, HTTP Request, MailerLite API

Build an n8n form workflow that validates explicit consent, normalises a lead, upserts the subscriber into a MailerLite group and preserves audit evidence.

#### Detailed procedure

1. Open n8n and create a new workflow named LAB01 - Consent-First Lead Capture.
2. Import lead-capture-mailerlite.json from this lab folder.
3. Open the Form Trigger and review the Name, Email, Company and explicit consent fields.
4. Compare the displayed consent wording with consent_text_version in the Normalize Lead node.
5. Create an n8n Header Auth credential for MailerLite; use Authorization with value Bearer plus your API token.
6. Attach the credential to the Upsert MailerLite Subscriber HTTP Request node; never paste the token into a node field.
7. Replace TRAINING_GROUP_ID in the Workflow Configuration node with a MailerLite group ID created for training.
8. Run the workflow in test mode and open the Form Trigger Test URL.
9. Submit a valid sample lead with consent checked and capture the Normalize Lead output.
10. Confirm the IF node routes the item to the approved branch only when consent is true.
11. Inspect the HTTP request body and verify email, fields, groups and opted_in_at match the contract.
12. Verify MailerLite returns 201 for a new subscriber or 200 for an existing subscriber.
13. Submit the same email again with an updated company and confirm the upsert is non-destructive.
14. Submit a row from sample-leads.csv with consent false and confirm no MailerLite call executes.
15. Export the execution evidence without the API token and complete CHECKLIST.md.

#### Acceptance criteria

- [ ] valid consent reaches MailerLite
- [ ] missing consent never calls MailerLite
- [ ] subscriber ID/status captured
- [ ] credential value absent from exports

#### Evidence

- test submission
- normalised payload
- MailerLite subscriber ID and status
- n8n execution ID

#### Troubleshooting

- invalid email returns a controlled form response
- unsubscribed status is never silently overridden
- 200 and 201 are both accepted
- 429 follows bounded exponential backoff

Working files: `labs/lab-01-*/`

### Lab 2: Campaign Brief and Audience Contract

**Duration:** 60 minutes  
**Alignment:** LO1 | K1-K2  
**Tools:** n8n Form Trigger, Edit Fields, Code, Structured Output

Turn a marketing objective into a validated campaign brief that every specialist agent consumes consistently.

#### Detailed procedure

1. Import campaign-brief-validator.json into n8n and open the Workflow Configuration node.
2. Open campaign-brief.json and read the objective, offer, audience, exclusions, voice, claims and schedule fields.
3. Set the campaign name and choose one measurable primary objective.
4. Define the eligible MailerLite group and at least two audience exclusions.
5. Write one primary CTA and the destination URL; do not use multiple competing CTAs.
6. Add brand voice attributes and prohibited tone or claim patterns from brand-guidelines.md.
7. List the approved evidence source IDs that agents may cite.
8. Set the intended schedule in local time and provide the explicit timezone identifier.
9. Execute the validator and inspect each validation_error item.
10. Remove the audience exclusions temporarily and verify the workflow fails closed.
11. Restore the exclusions, increment brief_version and execute again.
12. Confirm the final output contains a stable campaign_id and brief hash.
13. Save the approved brief as evidence for Lab 3.

#### Acceptance criteria

- [ ] all required fields pass
- [ ] exclusions present
- [ ] one CTA only
- [ ] brief version and hash generated

#### Evidence

- campaign-brief.json
- validator result
- audience inclusion/exclusion table
- brief version hash

#### Troubleshooting

- ambiguous objectives fail validation
- missing exclusions stop the pipeline
- unversioned edits create a new brief version
- PII is not inserted into prompts

Working files: `labs/lab-02-*/`

### Lab 3: Multi-Agent Newsletter Editorial System

**Duration:** 120 minutes  
**Alignment:** LO1 | K1-K2 | A2  
**Tools:** n8n AI Agent, OpenAI Chat Model, Structured Output Parser, Loop Over Items, Code

Coordinate strategist, researcher, copywriter, critic and refiner agents through explicit schemas and a bounded revision loop.

#### Detailed procedure

1. Import multi-agent-newsletter.json and connect an approved n8n chat-model credential.
2. Open every AI Agent node and read its role, allowed inputs and required output.
3. Confirm the Workflow Configuration node points to the approved Lab 2 campaign brief.
4. Review approved-sources.md and add one current, authoritative source relevant to the campaign.
5. Run the workflow once with pinned sample data before using a live model.
6. Execute the Strategist Agent and verify it returns audience need, narrative arc and one CTA.
7. Execute the Research Agent and verify every proposed factual claim has a source_id.
8. Execute the Copywriter Agent and validate its output against newsletter-output-schema.json.
9. Inspect the Critic Agent scorecard for brand, evidence, compliance and actionability.
10. Force one low compliance score in pinned data and confirm the Refiner path executes.
11. Confirm revision_count never exceeds the configured maximum of two.
12. Confirm a still-failing draft is marked needs_human_review rather than silently accepted.
13. Inspect the final HTML and plain-text alternatives for the same message and CTA.
14. Save the final content JSON and critic scorecard for Lab 4.

#### Acceptance criteria

- [ ] five distinct roles visible
- [ ] source IDs preserved
- [ ] schema valid
- [ ] bounded revision loop
- [ ] no send/schedule action

#### Evidence

- agent outputs
- critic scorecard
- revision count
- final content JSON
- execution graph

#### Troubleshooting

- parser errors route to repair
- missing source IDs force revision
- loop limit escalates to human
- agent outputs never overwrite raw brief

Working files: `labs/lab-03-*/`

### Lab 4: Human Approval and MailerLite Scheduling

**Duration:** 105 minutes  
**Alignment:** LO1 | K4 | A3  
**Tools:** n8n Chat Trigger, AI Agent, Chat HITL Tool, HTTP Request, Code

Bind a human decision to the exact newsletter payload, create a MailerLite draft and schedule only the approved version.

#### Detailed procedure

1. Import hitl-mailerlite-scheduler.json and connect the same approved chat-model credential used in Lab 3.
2. Create an n8n Header Auth credential for MailerLite and attach it to both HTTP Request nodes.
3. Open approved-newsletter.json and replace placeholder sender, group and timezone values with training values.
4. Confirm the verified sender already exists in MailerLite.
5. Execute Canonicalize and Hash and record the payload_hash.
6. Open the Review Newsletter tool and confirm it presents subject, audience group, CTA URL, schedule and hash.
7. Run the workflow and choose Reject; verify neither MailerLite node executes.
8. Run again, choose Revise or deny, and confirm reviewer feedback is stored with the content version.
9. Run again, choose Approve, then change one character in the content before the hash check.
10. Verify the hash mismatch blocks the external write and returns the draft to review.
11. Restore the approved content and approve again.
12. Inspect Create Campaign Draft and confirm the response contains a campaign ID and draft status.
13. Inspect Schedule Campaign and verify the schedule is in the future with the intended timezone.
14. Run the recovery check and confirm it reads campaign status before any retry.
15. Capture the decision, campaign ID and scheduled timestamp as assessment evidence.

#### Acceptance criteria

- [ ] reject path makes no write
- [ ] hash mismatch blocks
- [ ] approved draft created
- [ ] campaign scheduled once
- [ ] evidence contains reviewer and campaign ID

#### Evidence

- review request
- decision payload
- hash comparison
- MailerLite campaign ID
- scheduled timestamp

#### Troubleshooting

- hash mismatch blocks write
- rejection returns feedback
- 422 exposes safe validation detail
- retry checks campaign status before a second write

Working files: `labs/lab-04-*/`

## Topic 02: AI-Driven Campaign Performance and Consumer Insights Analysis

MailerLite events -> normalised facts -> KPI model -> diagnostic dashboard

- Model campaign, delivery and subscriber events at the correct grain.
- Distinguish delivery, engagement and business outcomes in a metric tree.
- Use cohort and funnel views to diagnose where performance is lost.
- State limitations of open tracking, attribution and small samples.

### Lab 5: Newsletter Analytics Collector and Dashboard

**Duration:** 120 minutes  
**Alignment:** LO2 | K5 | A4  
**Tools:** n8n Webhook, Schedule Trigger, Code, HTTP Request, Respond to Webhook

Ingest MailerLite events, normalise and deduplicate them, calculate metrics at defined denominators and publish a self-contained dashboard.

#### Detailed procedure

1. Import analytics-dashboard.json and open the workflow notes that separate ingestion from presentation.
2. Open campaign-events.csv and identify the event grain, campaign key, subscriber key and timestamp.
3. Pin the first ten CSV rows at the Normalize Events node for a credential-free test.
4. Execute Normalize Events and verify UTC timestamps, allowed event types and source hashes.
5. Run the Deduplicate Events node twice and confirm the second run adds no duplicate facts.
6. Open metric-dictionary.md and confirm each metric denominator matches the Code node.
7. Execute Aggregate KPIs and manually recompute one delivery rate and one CTR.
8. Inspect the cohort output for new leads, existing customers and dormant subscribers.
9. Open the dashboard Webhook Test URL and verify the KPI cards, funnel and trend views render.
10. Select one campaign and one cohort and verify the dashboard labels reflect the filter.
11. Introduce one duplicate click event and confirm unique-click metrics do not double-count it.
12. Introduce a campaign with zero delivered events and confirm rates show N/A rather than a division error.
13. Compare actual results with targets and identify the largest material gap.
14. Write one evidence-based diagnosis and one controlled corrective action.
15. Export dashboard-sample.html and complete CHECKLIST.md.

#### Acceptance criteria

- [ ] events normalised
- [ ] duplicates ignored
- [ ] metrics recomputable
- [ ] dashboard renders
- [ ] diagnosis cites cohort/funnel evidence

#### Evidence

- sample event payloads
- deduplication result
- metric dictionary
- dashboard HTML or screenshot
- diagnostic narrative

#### Troubleshooting

- invalid signature rejected
- duplicate event ignored
- late event updates the correct window
- empty denominator returns null not infinity

Working files: `labs/lab-05-*/`

## Topic 03: AI Agent Analysis of Market Trends and Customer Feedback

Signals -> evidence -> themes -> confidence -> controlled optimization

- Separate observed signals from agent inference and recommended action.
- Combine quantitative trends with review themes and unsubscribe reasons.
- Use confidence, materiality and reversible-test thresholds before changing a campaign.
- Write approved learnings back into the next campaign brief without contaminating raw data.

### Lab 6: Trend and Customer Feedback Optimization Agent

**Duration:** 90 minutes  
**Alignment:** LO3 | K6 | A5  
**Tools:** n8n Code, AI Agent, Structured Output Parser, IF, Chat HITL Tool

Combine campaign trends with de-identified feedback, require evidence cards and produce a reversible experiment recommendation for human approval.

#### Detailed procedure

1. Import trend-feedback-agent.json and inspect the analysis window and minimum-sample configuration.
2. Open customer-feedback.csv and identify which columns contain direct identifiers.
3. Execute De-identify Feedback and confirm email addresses and names are absent from the agent input.
4. Run the theme classifier with pinned sample data and inspect source_ids for every theme.
5. Verify the structured output contains finding, sample_size, window, confidence, counter_evidence and source_ids.
6. Force one finding to omit source_ids and confirm the evidence validator rejects it.
7. Compute the six-week CTR trend and compare it with the baseline in the workflow.
8. Combine the trend with feedback-theme counts; distinguish observed facts from inference.
9. Calculate priority for at least two improvement options using impact, confidence, reversibility and effort.
10. Select the higher-priority reversible test and define a single primary metric.
11. Add unsubscribe, complaint and bounce rates as guardrail metrics.
12. Define control/treatment allocation, analysis window and stop rule.
13. Submit the experiment contract to the human review tool and inspect approve/reject behavior.
14. Save the approved experiment contract and one rejected unsupported recommendation as evidence.

#### Acceptance criteria

- [ ] PII removed
- [ ] themes trace to sources
- [ ] trend has baseline/window
- [ ] counter-evidence present
- [ ] experiment has guardrails and approval

#### Evidence

- de-identified feedback
- theme table
- trend chart
- evidence card
- approved experiment contract

#### Troubleshooting

- insufficient sample returns monitor
- untraceable claim fails schema
- PII detector blocks prompt
- irreversible change requires escalation

Working files: `labs/lab-06-*/`

## Assessment preparation

The Written Assessment contains six open-ended K-coded questions. The Practical Performance contains five connected A-coded tasks. Both are open book and 60 minutes.

## References

- [n8n AI Agent node](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/)
- [n8n Human-in-the-loop for tool calls](https://docs.n8n.io/advanced-ai/human-in-the-loop-tools/)
- [n8n Form Trigger node](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.formtrigger/)
- [MailerLite Subscribers API](https://developers.mailerlite.com/api/subscribers)
- [MailerLite Campaigns API](https://developers.mailerlite.com/api/campaigns)
- [MailerLite Webhooks API](https://developers.mailerlite.com/api/webhooks)
