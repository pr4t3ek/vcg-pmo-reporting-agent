# PMO Reporting Agent — Proof of Concept

An AI agent that drafts weekly project status reports for a consulting 
engagement, built in n8n. Submitted as part of a GenAI investment 
prioritisation case study for Vertex Consulting Group (fictional firm).

## What it does

Pulls project data from five enterprise systems, validates it, computes 
schedule and cost variance, drafts a status report, and routes it through 
two human approval gates before anything is published.

## Design principle

All figures are computed deterministically in code. The language model is 
only permitted to narrate those figures — the prompt forbids it from 
calculating or introducing any number it wasn't given. This is what makes 
the agent auditable and is the reason two approval gates are sufficient.

The agent holds no write access to source systems and cannot publish 
without both approvals.

## Demo data

Four projects are seeded so every branch fires in a single run:

| Project | Condition | Agent decision |
|---|---|---|
| Meridian Bank | Complete data, behind plan | Red draft — SPI 0.89, CPI 0.79, 17-day slip |
| Kestrel Logistics | Complete data, on track | Green draft — SPI 1.03, CPI 1.07 |
| Northwind Retail | Finance feed 18 days stale | Halted — missing data, routed to PM |
| Halcyon Pharma | No baseline, unexplained variance | Halted — confidence 0.35 vs 0.65 threshold |

Two of four halt. An agent that declines to draft when it cannot defend 
its conclusion is the point, not a limitation.

## Running it

1. Install n8n — `npx n8n` — or use n8n Cloud
2. Get a free Gemini API key from Google AI Studio
3. Workflows → Import from File → select the JSON
4. Open the "Google Gemini" node and add your key
5. Execute. It pauses at each approval gate with a form URL.
