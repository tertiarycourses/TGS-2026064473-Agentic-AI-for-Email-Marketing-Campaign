# Lab 4: Human Approval and MailerLite Scheduling

**Duration:** 105 minutes  
**Alignment:** LO1 | K4 | A3  
**Tools:** n8n Chat Trigger, AI Agent, Chat HITL Tool, HTTP Request, Code

## Goal

Bind a human decision to the exact newsletter payload, create a MailerLite draft and schedule only the approved version.

## Deliverable

A human-review gate, payload hash check, MailerLite draft creation and schedule transaction with recovery controls.

## Before you start

- Use a training n8n workspace and a non-production MailerLite group.
- Create credentials in n8n. Never paste tokens into a workflow, document or screenshot.
- Use the supplied sample data before connecting a live service.
- Record only safe evidence: IDs, statuses, timestamps, hashes and redacted payloads.

## Detailed procedure

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

## Acceptance checklist

- [ ] reject path makes no write
- [ ] hash mismatch blocks
- [ ] approved draft created
- [ ] campaign scheduled once
- [ ] evidence contains reviewer and campaign ID

## Evidence to submit

- review request
- decision payload
- hash comparison
- MailerLite campaign ID
- scheduled timestamp

## Troubleshooting model

- hash mismatch blocks write
- rejection returns feedback
- 422 exposes safe validation detail
- retry checks campaign status before a second write

## References

- [n8n AI Agent node](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/)
- [n8n Human-in-the-loop for tool calls](https://docs.n8n.io/advanced-ai/human-in-the-loop-tools/)
- [n8n Form Trigger node](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.formtrigger/)
- [MailerLite Subscribers API](https://developers.mailerlite.com/api/subscribers)
- [MailerLite Campaigns API](https://developers.mailerlite.com/api/campaigns)
- [MailerLite Webhooks API](https://developers.mailerlite.com/api/webhooks)
