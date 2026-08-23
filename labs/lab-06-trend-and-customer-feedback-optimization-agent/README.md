# Lab 6: Trend and Customer Feedback Optimization Agent

**Duration:** 90 minutes

**Alignment:** LO3 | K6 | A5
**Tools:** n8n Code, AI Agent, Structured Output Parser, IF, Chat HITL Tool

## Goal

Combine campaign trends with de-identified feedback, require evidence cards and produce a reversible experiment recommendation for human approval.

## Deliverable

A trend/feedback analysis workflow with theme counts, confidence, counter-evidence, priority score and experiment contract.

## Before you start

- Use a training n8n workspace and a non-production MailerLite group.
- Create credentials in n8n. Never paste tokens into a workflow, document or screenshot.
- Use the supplied sample data before connecting a live service.
- Record only safe evidence: IDs, statuses, timestamps, hashes and redacted payloads.

## Detailed procedure

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

## Acceptance checklist

- [ ] PII removed
- [ ] themes trace to sources
- [ ] trend has baseline/window
- [ ] counter-evidence present
- [ ] experiment has guardrails and approval

## Evidence to submit

- de-identified feedback
- theme table
- trend chart
- evidence card
- approved experiment contract

## Troubleshooting model

- insufficient sample returns monitor
- untraceable claim fails schema
- PII detector blocks prompt
- irreversible change requires escalation

## References

- [n8n AI Agent node](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/)
- [n8n Human-in-the-loop for tool calls](https://docs.n8n.io/advanced-ai/human-in-the-loop-tools/)
- [n8n Form Trigger node](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.formtrigger/)
- [MailerLite Subscribers API](https://developers.mailerlite.com/api/subscribers)
- [MailerLite Campaigns API](https://developers.mailerlite.com/api/campaigns)
- [MailerLite Webhooks API](https://developers.mailerlite.com/api/webhooks)
