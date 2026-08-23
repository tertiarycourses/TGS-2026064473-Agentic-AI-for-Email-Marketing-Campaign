# Lab 4: Human Approval and MailerLite Scheduling

**Duration:** 105 minutes

**Alignment:** LO1 | K4 | A3
**Tools:** n8n Manual Trigger, Gmail Send and Wait, IF, Code, HTTP Request

## Goal

Bind a human decision to the exact newsletter payload, create a MailerLite draft and schedule only the approved version.

## Deliverable

A human-review gate, payload hash check, MailerLite draft creation and schedule transaction with release evidence.

## Before you start

- Use a training n8n workspace and a non-production MailerLite group.
- Create credentials in n8n. Never paste tokens into a workflow, document or screenshot.
- Use the supplied sample data before connecting a live service.
- Record only safe evidence: IDs, statuses, timestamps, hashes and redacted payloads.

## Detailed procedure

1. Import hitl-mailerlite-scheduler.json and inspect the connected approval, hash-check, draft and schedule paths.
2. Attach an n8n Gmail credential to Human Newsletter Approval and replace the reviewer address with a training reviewer.
3. Create an n8n Header Auth credential for MailerLite and attach it to both HTTP Request nodes.
4. Open approved-newsletter.json, replace placeholder sender, group and timezone values, then copy the approved fields into Load Pending Newsletter.
5. Confirm the verified sender already exists in MailerLite.
6. Execute Canonicalize and Hash and record the payload_hash.
7. Open Human Newsletter Approval and confirm it presents subject, audience group, CTA URL, schedule and hash.
8. Run the workflow and choose Reject; verify neither MailerLite node executes.
9. Run again, choose Decline, and confirm the decision record contains the content version and approved payload hash.
10. Run again, choose Approve, then change one character in the content before the hash check.
11. Verify the hash mismatch blocks the external write and returns the draft to review.
12. Set review_expires_at to a past timestamp and confirm the expiry guard also blocks release.
13. Restore the approved content and approve again.
14. Inspect Create MailerLite Campaign and confirm the response contains a campaign ID and draft status.
15. Inspect Schedule MailerLite Campaign and verify the schedule is in the future with the intended timezone.
16. Capture the decision, campaign ID and scheduled timestamp as assessment evidence.

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
- expired approval blocks release
- rejection records the decision without an external write
- 422 exposes safe validation detail
- failed draft or schedule responses stop the workflow

## References

- [n8n AI Agent node](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/)
- [n8n Human-in-the-loop for tool calls](https://docs.n8n.io/advanced-ai/human-in-the-loop-tools/)
- [n8n Form Trigger node](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.formtrigger/)
- [MailerLite Subscribers API](https://developers.mailerlite.com/api/subscribers)
- [MailerLite Campaigns API](https://developers.mailerlite.com/api/campaigns)
- [MailerLite Webhooks API](https://developers.mailerlite.com/api/webhooks)
