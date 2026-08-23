# Lab 5: Newsletter Analytics Collector and Dashboard

**Duration:** 120 minutes  
**Alignment:** LO2 | K5 | A4  
**Tools:** n8n Webhook, Schedule Trigger, Code, HTTP Request, Respond to Webhook

## Goal

Ingest MailerLite events, normalise and deduplicate them, calculate metrics at defined denominators and publish a self-contained dashboard.

## Deliverable

A webhook/poll collector, event fact table, KPI calculation and HTML dashboard with target, cohort and funnel views.

## Before you start

- Use a training n8n workspace and a non-production MailerLite group.
- Create credentials in n8n. Never paste tokens into a workflow, document or screenshot.
- Use the supplied sample data before connecting a live service.
- Record only safe evidence: IDs, statuses, timestamps, hashes and redacted payloads.

## Detailed procedure

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

## Acceptance checklist

- [ ] events normalised
- [ ] duplicates ignored
- [ ] metrics recomputable
- [ ] dashboard renders
- [ ] diagnosis cites cohort/funnel evidence

## Evidence to submit

- sample event payloads
- deduplication result
- metric dictionary
- dashboard HTML or screenshot
- diagnostic narrative

## Troubleshooting model

- invalid signature rejected
- duplicate event ignored
- late event updates the correct window
- empty denominator returns null not infinity

## References

- [n8n AI Agent node](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/)
- [n8n Human-in-the-loop for tool calls](https://docs.n8n.io/advanced-ai/human-in-the-loop-tools/)
- [n8n Form Trigger node](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.formtrigger/)
- [MailerLite Subscribers API](https://developers.mailerlite.com/api/subscribers)
- [MailerLite Campaigns API](https://developers.mailerlite.com/api/campaigns)
- [MailerLite Webhooks API](https://developers.mailerlite.com/api/webhooks)
