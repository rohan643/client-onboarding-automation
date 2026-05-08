<div align="center">

# ⚡ Client Onboarding Automation

**Zero-touch client onboarding — from payment to kickoff in 12 minutes, fully automated**

[![n8n](https://img.shields.io/badge/n8n-Core-EA4B71?style=for-the-badge&logo=n8n&logoColor=white)](https://n8n.io)
[![Stripe](https://img.shields.io/badge/Stripe-Payments-635BFF?style=for-the-badge&logo=stripe&logoColor=white)](https://stripe.com)
[![Notion](https://img.shields.io/badge/Notion-Workspace-000000?style=for-the-badge&logo=notion&logoColor=white)](https://notion.so)
[![DocuSign](https://img.shields.io/badge/DocuSign-eSignature-FFB600?style=for-the-badge)](https://docusign.com)

</div>

---

## The Problem This Solves

Before this system, onboarding a new client took **4+ hours** of manual work:
- Manually send contract
- Wait for signature, chase if needed
- Create Notion workspace by hand
- Set up Slack channel
- Send welcome email manually
- Schedule kickoff call manually
- Add to project management tool

This system does all of it in **under 12 minutes**, triggered the moment Stripe processes payment.

---

## Full Onboarding Flow

```
Stripe Payment Confirmed
         │
         ▼
┌─────────────────────┐
│  Extract client data │  ← Name, email, plan, company from Stripe metadata
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Send DocuSign      │  ← Auto-fills service agreement with client details
│  Contract           │
└──────────┬──────────┘
           │ (webhook: contract signed)
           ▼
┌─────────────────────┐
│  Create Notion      │  ← Duplicates master template, sets permissions,
│  Workspace          │    pre-populates with client info & project scope
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Create Slack       │  ← #client-acmecorp channel, invites client,
│  Channel            │    posts welcome message with Notion link
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Send Welcome Email │  ← Personalized email with next steps,
│  Sequence (3 emails)│    Notion link, Slack invite, kickoff scheduling link
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Send Kickoff       │  ← Cal.com link auto-sent, or auto-schedules
│  Calendar Invite    │    based on client's availability preferences
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Internal Slack     │  ← Alerts team: "New client onboarded: Acme Corp"
│  Alert              │    with link to Notion workspace
└─────────────────────┘

Total time: ~8-12 minutes
Human intervention required: 0
```

---

## What Gets Created Automatically

### Notion Workspace
- Client info page (auto-filled)
- Project scope & deliverables
- Weekly check-in template
- File library
- Feedback log
- Invoice tracker

### Welcome Email Sequence
```
Email 1 (immediate): Welcome + access links
Email 2 (+1 day):    "What to expect in week 1" + onboarding checklist
Email 3 (+3 days):   Kickoff call reminder + agenda
```

### Slack Channel
- Channel name: `#client-{company-slug}`
- Auto-pinned: Notion workspace link, kickoff call link
- Welcome message from your agency bot

---

## Tech Stack

| Tool | Role |
|---|---|
| **n8n** | Workflow orchestration |
| **Stripe** | Payment trigger + client data source |
| **DocuSign** | Contract generation + e-signature |
| **Notion API** | Workspace creation from template |
| **Slack API** | Channel creation + messaging |
| **Gmail/SMTP** | Welcome email sequence |
| **Cal.com** | Kickoff scheduling |
| **Airtable** | Client registry + onboarding audit log |

---

## Setup

### 1. Configure Stripe Webhook
```
Endpoint: https://your-n8n.com/webhook/stripe-payment
Events: payment_intent.succeeded, checkout.session.completed
```

### 2. Configure Required Credentials in n8n
- Stripe API Key
- DocuSign OAuth
- Notion Integration Token
- Slack Bot Token
- Gmail OAuth or SMTP
- Airtable API Key

### 3. Set Template IDs
```yaml
# config.yaml
notion:
  template_page_id: "abc123..."
  parent_page_id: "def456..."
docusign:
  template_id: "xyz789..."
slack:
  workspace_id: "T0123..."
cal:
  booking_link: "https://cal.com/your-agency/kickoff"
```

### 4. Import & Activate the n8n Workflow
Import `client-onboarding-main.json` → add credentials → activate.

---

## Error Handling

Every step has a failure branch:
- **DocuSign fails** → Slack alert to team, manual send fallback
- **Notion API error** → Retry 3x, then Slack alert
- **Slack channel exists** → Append timestamp to name, continue
- **Any failure** → Log to Airtable error table with full context

---

## Results

```
Before:  4 hours manual work per new client
After:   12 minutes, 0 human touches

Scale:   Handles 50+ simultaneous onboardings
Errors:  < 0.3% failure rate across 200+ onboardings
```

---

<div align="center">

**Built by [Rohan Mukherjee](https://github.com/rohan643) @ Apex Automation Co.**

</div>
