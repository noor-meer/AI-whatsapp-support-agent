# AI WhatsApp Customer Support Agent

An AI-powered customer support agent that automatically answers customer questions on WhatsApp, intelligently escalates urgent issues to a human team via Slack, and logs every conversation for review — built entirely with no-code/low-code automation.

## 🎯 What It Does

- Receives customer messages in real-time via WhatsApp
- Uses Google Gemini AI to generate helpful, context-aware responses
- Automatically detects escalation-worthy messages (refunds, complaints, angry customers) and routes them to a human support team via Slack instead of letting AI handle them
- Logs every conversation — both AI-handled and human-escalated — to Google Sheets for tracking and analysis

## 🏗️ Architecture

Customer (WhatsApp)
↓
Twilio (WhatsApp Business API gateway)
↓
n8n Webhook (workflow trigger)
↓
Escalation Check (keyword-based logic)
├── Escalation needed → Slack Alert → Google Sheets Log
└── Normal query → Google Gemini AI → Twilio (reply) → Google Sheets Log

## 🛠️ Tech Stack

- **n8n** — workflow automation and orchestration
- **Twilio** — WhatsApp Business API integration
- **Google Gemini API** — AI-generated customer responses
- **Slack API** — human escalation alerts
- **Google Sheets API** — conversation logging

## 💡 Key Technical Decisions

- **Rule-based escalation over AI judgment**: Used explicit keyword matching rather than asking the AI to judge urgency. This makes escalation behavior predictable, auditable, and fast — critical qualities for a support tool where false negatives (missing an angry customer) are costly.
- **Dual logging paths**: Every conversation is logged regardless of whether it was handled by AI or a human, giving full visibility into support volume and AI performance over time.

## 📋 Setup

1. Import `workflow.json` into your n8n instance
2. Set up credentials for: Twilio, Google Gemini API, Slack, Google Sheets
3. Replace placeholder values (`YOUR_GEMINI_API_KEY_HERE`, `YOUR_TWILIO_ACCOUNT_SID`) with your own credentials
4. Configure your Twilio WhatsApp Sandbox to point to your n8n webhook's Production URL
5. Activate the workflow in n8n

## 🔧 What I Learned

- Debugging live webhook-based systems using execution logs and tunnel traffic logs (ngrok) rather than guesswork
- The distinction between n8n's Test URL (manual trigger) and Production URL (always-on, for Active workflows) — and how mixing them up causes silent failures
- Structuring conditional branching logic for reliable, auditable automation decisions
- Working with multiple third-party APIs (Twilio, Gemini, Slack, Google Sheets) in a single orchestrated pipeline

## 📌 Future Improvements

- Add sentiment analysis instead of pure keyword matching for escalation detection
- Support multiple languages
- Add a `/summary` command for daily analytics
- Migrate from Twilio Sandbox to a verified WhatsApp Business number for production use
