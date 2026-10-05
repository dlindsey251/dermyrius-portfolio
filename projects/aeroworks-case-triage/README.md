# AeroWorks AI Case Triage

An AI-assisted case intake and human approval workflow built with Microsoft Power Platform.

## Overview

New rows in the Dataverse **AeroWorks Cases** table trigger a Power Automate flow. An AI prompt summarizes and classifies the request, and the flow writes the structured results back to the case. Requests requiring review are sent to an approver.

The approver remains the decision-maker:

- **Approve:** set **Human Review Required** to **No**.
- **Reject:** keep **Human Review Required** as **Yes** so the case remains flagged for follow-up.

## Flow

1. Trigger when a Dataverse row is added.
2. Analyze the new request with an AI prompt.
3. Parse the response as JSON.
4. Update the case with the AI summary, confidence, category, department, priority, and review flag.
5. Start an approval when human review is required.
6. Update the review flag based on the approval outcome.

## Validation

Both decision paths were run end to end.

| Case | Decision | Flow | Human Review Required |
|---|---|---|---|
| AW-000004 | Approve | Succeeded | No |
| AW-000005 | Reject | Succeeded | Yes |

The example request was intentionally vague: “I don't know what's going on. Please help me.” The AI summary was “Customer request is unclear and lacks specific details,” with confidence **0.80**, prompting human review.

## Evidence

Screenshots are in [`screenshots/`](screenshots/):

- [Completed flow layout](screenshots/flow-layout.jpg)
- [Successful approved run](screenshots/approved-run.jpg)
- [Successful rejected run](screenshots/rejected-run.jpg)
- [Final Dataverse review flags](screenshots/final-case-flags.jpg)

## Scope

This is a portfolio demonstration in a personal Power Platform environment, not a production deployment for AeroWorks. AI output is advisory. A human makes the final decision. The rejection path flags the case for follow-up and does not change its Status field.
