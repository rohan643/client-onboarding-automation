# ⚡ Client Onboarding Automation

**Before:** 4 hours of manual work per new client.
**After:** 12 minutes. Zero touches.

---

### The Trigger

Stripe payment confirmed → everything below happens automatically.

---

### What Runs

```
[1] Extract client data from Stripe metadata
        ↓
[2] Send DocuSign service agreement (pre-filled)
        ↓ (fires when signed)
[3] Create Notion workspace from template
        ↓
[4] Create Slack channel #client-{name}
        ↓
[5] Send 3-email welcome sequence
        ↓
[6] Send kickoff calendar invite (Cal.com)
        ↓
[7] Post internal Slack alert to team
```

---

### Files

```
client-onboarding-automation/
├── workflow/
│   └── onboarding.json        # n8n workflow export
├── config/
│   └── config.yaml            # Template IDs, channel names, etc.
└── templates/
    └── welcome-email.md       # Welcome email template
```

### Configure

```yaml
# config/config.yaml
notion_template_id: "abc123..."
docusign_template_id: "xyz789..."
slack_workspace_id: "T0123..."
cal_booking_link: "https://cal.com/rohan/kickoff"
```

### Import

1. Import `workflow/onboarding.json` into n8n
2. Configure credentials: Stripe, DocuSign, Notion, Slack, Gmail, Cal
3. Set Stripe webhook → `https://your-n8n/webhook/stripe-payment`
4. Activate

---

**Error handling:** Every step has a failure branch that posts to Slack and logs to Airtable. No silent failures.

---

<sub>[@rohan643](https://github.com/rohan643)</sub>
