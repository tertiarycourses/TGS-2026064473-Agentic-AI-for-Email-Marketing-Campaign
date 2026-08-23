# Lab 2: Campaign Brief and Audience Contract

**Duration:** 60 minutes  
**Alignment:** LO1 | K1-K2  
**Tools:** n8n Form Trigger, Edit Fields, Code, Structured Output

## Goal

Turn a marketing objective into a validated campaign brief that every specialist agent consumes consistently.

## Deliverable

A campaign-brief intake form, JSON schema validator and audience/message contract.

## Before you start

- Use a training n8n workspace and a non-production MailerLite group.
- Create credentials in n8n. Never paste tokens into a workflow, document or screenshot.
- Use the supplied sample data before connecting a live service.
- Record only safe evidence: IDs, statuses, timestamps, hashes and redacted payloads.

## Detailed procedure

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

## Acceptance checklist

- [ ] all required fields pass
- [ ] exclusions present
- [ ] one CTA only
- [ ] brief version and hash generated

## Evidence to submit

- campaign-brief.json
- validator result
- audience inclusion/exclusion table
- brief version hash

## Troubleshooting model

- ambiguous objectives fail validation
- missing exclusions stop the pipeline
- unversioned edits create a new brief version
- PII is not inserted into prompts

## References

- [n8n AI Agent node](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/)
- [n8n Human-in-the-loop for tool calls](https://docs.n8n.io/advanced-ai/human-in-the-loop-tools/)
- [n8n Form Trigger node](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.formtrigger/)
- [MailerLite Subscribers API](https://developers.mailerlite.com/api/subscribers)
- [MailerLite Campaigns API](https://developers.mailerlite.com/api/campaigns)
- [MailerLite Webhooks API](https://developers.mailerlite.com/api/webhooks)
