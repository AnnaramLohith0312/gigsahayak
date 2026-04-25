# GigSahayak — Gig Worker AI Assistant

> **AI-powered WhatsApp assistant helping gig & platform workers in Hyderabad access welfare schemes, optimize income, and resolve grievances.**

---

## What is GigSahayak?

GigSahayak (*Gig Helper*) is an intelligent, conversational assistant delivered over WhatsApp, built specifically for the ~2 lakh gig workers across Hyderabad employed with platforms like Swiggy, Zomato, Uber, Ola, Amazon, Flipkart, and Urban Company.

The platform bridges the awareness gap created by the **Telangana Gig Workers Welfare Act 2025**, helping workers unlock welfare benefits worth **₹10,000–50,000 per worker annually** through:

- Intelligent onboarding & worker profiling
- Personalised scheme matching from 15–20 welfare programmes
- Step-by-step application assistance with document validation
- Multi-platform income optimisation advice (peak hours, zones)
- Grievance categorisation & escalation support

---

## Key Features

| Feature | Description |
|---|---|
| 🗣️ Multi-language NLU | Supports Telugu, Hindi, English & code-switching |
| 🎯 Opportunity Matching | Top-5 welfare scheme recommendations in < 3 seconds |
| 📋 Application Assistant | Document checklist, validation & pre-fill guidance |
| 💰 Income Optimisation | Peak hours, zone heatmaps, multi-platform strategies |
| 🆘 Grievance Routing | Smart categorisation & escalation to authorities |
| 📚 RAG Knowledge Base | LLM + vector DB for accurate, up-to-date policy answers |
| 🔔 Proactive Notifications | Scheme deadlines, new opportunities & status updates |
| 🔐 Secure & Scalable | 99.5% uptime, 95% responses < 3s, TLS encryption |

---

## Tech Stack

- **Backend:** FastAPI (Python)
- **Database:** PostgreSQL + Redis (caching & session state)
- **AI / LLM:** LLM-based NLU engine with RAG (vector database)
- **Messaging:** WhatsApp Business API
- **Architecture:** Horizontally scalable microservices

---

## Project Structure

```
gigsahayak/
├── .kiro/specs/gigsahayak/
│   ├── requirements.md   # Detailed functional requirements
│   ├── design.md         # System design & architecture
│   └── tasks.md          # Development task breakdown
└── README.md
```

---

## Target Users

Gig & platform workers in Hyderabad working with:
Swiggy · Zomato · Uber · Ola · Amazon · Flipkart · Urban Company

---

## License

MIT
