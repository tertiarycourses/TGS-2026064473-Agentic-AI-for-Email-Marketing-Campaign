# Lab 1: Consent-First Lead Capture to MailerLite

**Duration:** 75 minutes

**Alignment:** LO1 | K3 | A1
**Tools:** n8n Form Trigger, Code, IF, HTTP Request, MailerLite API

## Goal

Build an n8n form workflow that validates explicit consent, normalises a lead, upserts the subscriber into a MailerLite group and preserves audit evidence.

## Deliverable

A production-shaped lead form, MailerLite upsert, rejection path and evidence record.

## Before you start

- Use a training n8n workspace and a non-production MailerLite group.
- Create credentials in n8n. Never paste tokens into a workflow, document or screenshot.
- Use the supplied sample data before connecting a live service.
- Record only safe evidence: IDs, statuses, timestamps, hashes and redacted payloads.

## Detailed procedure

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

## Acceptance checklist

- [ ] valid consent reaches MailerLite
- [ ] missing consent never calls MailerLite
- [ ] subscriber ID/status captured
- [ ] credential value absent from exports

## Evidence to submit

- test submission
- normalised payload
- MailerLite subscriber ID and status
- n8n execution ID

## Troubleshooting model

- invalid email returns a controlled form response
- unsubscribed status is never silently overridden
- 200 and 201 are both accepted
- 429 follows bounded exponential backoff

## References

- [n8n AI Agent node](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/)
- [n8n Human-in-the-loop for tool calls](https://docs.n8n.io/advanced-ai/human-in-the-loop-tools/)
- [n8n Form Trigger node](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.formtrigger/)
- [MailerLite Subscribers API](https://developers.mailerlite.com/api/subscribers)
- [MailerLite Campaigns API](https://developers.mailerlite.com/api/campaigns)
- [MailerLite Webhooks API](https://developers.mailerlite.com/api/webhooks)
