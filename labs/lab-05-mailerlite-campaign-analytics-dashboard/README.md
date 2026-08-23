# Lab 5: MailerLite Campaign Analytics Dashboard

**Duration:** 120 minutes

**Alignment:** LO2 | K5 | A4
**Tools:** n8n Webhook, Set, HTTP Request, Code, Respond to Webhook

## Goal

Fetch current MailerLite campaign statistics, normalise the report, calculate labelled metrics and publish a self-contained diagnostic dashboard.

## Deliverable

A live MailerLite campaign-report fetch, validated KPI calculation and HTML dashboard with status, target and funnel views.

## Before you start

- Use a training n8n workspace and a non-production MailerLite group.
- Create credentials in n8n. Never paste tokens into a workflow, document or screenshot.
- Use the supplied sample data before connecting a live service.
- Record only safe evidence: IDs, statuses, timestamps, hashes and redacted payloads.

## Detailed procedure

1. Import analytics-dashboard.json and inspect the connected dashboard request, MailerLite fetch, validation, metric and response path.
2. Attach the same n8n Header Auth credential used for MailerLite in Lab 4.
3. Open Workflow Configuration and replace the placeholder campaign ID with the approved training campaign ID.
4. Execute Fetch MailerLite Campaign and verify the response status is 200 before continuing.
5. Execute Validate Campaign Report and confirm campaign ID, name, status and statistics are present.
6. Open metric-dictionary.md and confirm each metric denominator matches Aggregate KPIs.
7. Execute Aggregate KPIs and manually recompute one delivery rate and one CTR.
8. Open the dashboard Webhook Test URL and verify the KPI cards, funnel and trend views render.
9. Verify the dashboard labels show the current campaign status and last refresh time.
10. Pin a report with zero sent messages and confirm rates show N/A rather than a division error.
11. Replace the campaign ID with an invalid value and confirm the workflow exposes a safe 404 diagnosis rather than rendering misleading metrics.
12. Restore the valid campaign ID and refresh the dashboard.
13. Compare actual results with targets and identify the largest material gap.
14. Write one evidence-based diagnosis and one controlled corrective action.
15. Export dashboard-sample.html and complete CHECKLIST.md.

## Acceptance checklist

- [ ] live report fetched
- [ ] response validated
- [ ] metrics recomputable
- [ ] dashboard renders
- [ ] diagnosis cites status/funnel evidence

## Evidence to submit

- MailerLite report response
- validated metric object
- metric dictionary
- dashboard HTML or screenshot
- diagnostic narrative

## Troubleshooting model

- 401 identifies a credential problem
- 404 identifies an invalid campaign ID
- missing statistics fail schema validation
- empty denominator returns null not infinity

## References

- [n8n AI Agent node](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/)
- [n8n Human-in-the-loop for tool calls](https://docs.n8n.io/advanced-ai/human-in-the-loop-tools/)
- [n8n Form Trigger node](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.formtrigger/)
- [MailerLite Subscribers API](https://developers.mailerlite.com/api/subscribers)
- [MailerLite Campaigns API](https://developers.mailerlite.com/api/campaigns)
- [MailerLite Webhooks API](https://developers.mailerlite.com/api/webhooks)
