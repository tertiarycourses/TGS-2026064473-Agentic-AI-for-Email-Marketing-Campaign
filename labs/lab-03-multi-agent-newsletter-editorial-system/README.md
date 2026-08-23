# Lab 3: Multi-Agent Newsletter Editorial System

**Duration:** 120 minutes

**Alignment:** LO1 | K1-K2 | A2
**Tools:** n8n AI Agent, OpenAI Chat Model, Structured Output Parser, Loop Over Items, Code

## Goal

Coordinate strategist, researcher, copywriter, critic and refiner agents through explicit schemas and a bounded revision loop.

## Deliverable

An importable multi-agent n8n workflow that produces a grounded newsletter draft with quality scores and provenance.

## Before you start

- Use a training n8n workspace and a non-production MailerLite group.
- Create credentials in n8n. Never paste tokens into a workflow, document or screenshot.
- Use the supplied sample data before connecting a live service.
- Record only safe evidence: IDs, statuses, timestamps, hashes and redacted payloads.

## Detailed procedure

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

## Acceptance checklist

- [ ] five distinct roles visible
- [ ] source IDs preserved
- [ ] schema valid
- [ ] bounded revision loop
- [ ] no send/schedule action

## Evidence to submit

- agent outputs
- critic scorecard
- revision count
- final content JSON
- execution graph

## Troubleshooting model

- parser errors route to repair
- missing source IDs force revision
- loop limit escalates to human
- agent outputs never overwrite raw brief

## References

- [n8n AI Agent node](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/)
- [n8n Human-in-the-loop for tool calls](https://docs.n8n.io/advanced-ai/human-in-the-loop-tools/)
- [n8n Form Trigger node](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.formtrigger/)
- [MailerLite Subscribers API](https://developers.mailerlite.com/api/subscribers)
- [MailerLite Campaigns API](https://developers.mailerlite.com/api/campaigns)
- [MailerLite Webhooks API](https://developers.mailerlite.com/api/webhooks)
