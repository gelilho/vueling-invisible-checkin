# Invisible Check-In

**AI-powered automated check-in for Vueling**
*NextGen Aviation Challenge &middot; 4YFN / MWC 2026*

---

## The Idea

What if 38 million passengers never had to check in again?

**Invisible Check-In** is an AI-powered pipeline that proactively checks in every eligible, opted-in passenger &mdash; documents verified, seat assigned, boarding pass delivered. The passenger does nothing.

The system runs up to **15 days before departure** (early document validation) and executes full check-in at **48 hours** before the flight. Eight orchestrated stages complete in **under 15 seconds** per passenger.

---

## The Problem

| Metric | Value | Source |
|--------|-------|--------|
| Airport counter check-ins | **11.4M / year** | 38.2M pax &times; 30% (SITA 2023) |
| Passenger time wasted | **3.2M+ hours / year** | 38.2M pax &times; 5 min average |
| Complaints linked to check-in | **60K+ cumulative** | AirAdvisor public data |

The model is **reactive** &mdash; the airline waits for the passenger to act instead of acting for them.

---

## The Solution &mdash; 8-Stage Pipeline

```
1. Booking       ──→  Retrieve booking details
2. Passenger     ──→  Profile & passenger data
3. Identity [AI] ──→  AI document verification (Gemini 2.0 Flash)
4. Bags & Seats  ──→  Baggage & seat status check
5. Channels      ──→  Delivery preferences
6. Check-In [AI] ──→  Automated check-in generation
7. Notify        ──→  Boarding pass delivery
8. AI Nudge [AI] ──→  Contextual upsell / alert (configurable LLM prompts)
```

Three stages are AI-powered (marked `[AI]`), using **Google Gemini 2.0 Flash** for both vision (document scanning) and text generation (contextual nudges).

---

## Quantified Impact

Automating check-in for eligible passengers delivers measurable impact across **six dimensions**:

| Dimension | KPI | Basis |
|-----------|-----|-------|
| Customer time saved | **1.6&ndash;3.2M hours / year** | 19.1M pax (50% opt-in) &times; 5&ndash;10 min |
| Customer satisfaction | **+10&ndash;15 points** | Top-5 satisfaction driver (IATA GPS 2024) |
| Airport staff efficiency | **~30% fewer counter agents** | 11.4M &rarr; 3.4&ndash;5.7M counter pax |
| Customer care reduction | **20&ndash;30% fewer support cases** | Check-in issues eliminated upstream |
| Upsell opportunity | **+8&ndash;10M&euro; / year** | ~10M pax &times; 3% conversion &times; ~30&euro; |
| Automation efficiency | **&lt;15 seconds end-to-end** | Full 8-stage pipeline execution |

---

## Smart Eligibility

Not every passenger can be auto-checked in. The pipeline verifies **profile completeness, document status, ticketing, and special service requests** before proceeding. If any gate fails, it falls back gracefully with a clear, actionable message.

**Pilot scope**: single passenger, simple domestic trip, no restrictions &mdash; hardest cases come later.

---

## What the POC Does Today

The proof of concept is **built and running**:

- **Full 8-stage pipeline** &mdash; booking &rarr; identity AI &rarr; check-in &rarr; boarding pass &rarr; AI nudge
- **AI document verification** &mdash; upload a passport or DNI, Gemini 2.0 Flash extracts data in real time
- **Contextual AI nudge** &mdash; configurable LLM prompts with passenger context (destination, trip days, language)
- **Orchestration dashboard** &mdash; every stage timed, logged, and observable
- **Privacy by design** &mdash; document images are processed by AI and immediately discarded, never stored

---

## Architecture

```
┌─────────────────────────────────────────────────┐
│               App Layer (Next.js)               │
│        Pages  ·  Layout  ·  API Routes          │
├─────────────────────────────────────────────────┤
│              Components Layer                    │
│  Pipeline views  ·  Boarding pass  ·  Dashboard  │
├─────────────────────────────────────────────────┤
│             Hooks Layer (Orchestration)           │
│         usePipeline  ·  useOrchestration         │
├─────────────────────────────────────────────────┤
│            Business Logic Layer                   │
│  Pipeline stages  ·  Gemini AI  ·  Validator     │
├─────────────────────────────────────────────────┤
│             Foundation Layer                      │
│     Types  ·  Constants  ·  Utils  ·  Data       │
└─────────────────────────────────────────────────┘
```

**Tech stack**: Next.js 16 &middot; TypeScript (strict) &middot; Google Gemini 2.0 Flash &middot; Tailwind CSS 4 &middot; REST API

---

## Pipeline Flow

```
                    ┌──────────────────┐
                    │  Flight/Passenger │
                    └────────┬─────────┘
                             │
        ┌────────────────────┼────────────────────┐
        ▼                    ▼                    ▼
   ┌─────────┐        ┌──────────┐         ┌─────────┐
   │ Booking │──→     │ Passenger│──→      │Identity │
   │ Lookup  │        │  Data    │         │ AI [V]  │
   └─────────┘        └──────────┘         └────┬────┘
                                                │
                              ┌─────────────────┤
                              ▼                  ▼
                         ┌─────────┐      ┌──────────┐
                         │Bags &   │──→   │ Channel  │
                         │Seats    │      │ Prefs    │
                         └─────────┘      └────┬─────┘
                                               │
                              ┌────────────────┤
                              ▼                ▼
                         ┌──────────┐    ┌──────────┐
                         │Check-In  │──→ │  Notify  │
                         │ AI [T]   │    │  Comms   │
                         └──────────┘    └────┬─────┘
                                              │
                                              ▼
                                        ┌──────────┐
                                        │ AI Nudge │
                                        │  [T]     │
                                        └──────────┘

[V] = Gemini Vision    [T] = Gemini Text
```

---

## AI Integration &mdash; Configurable LLM Prompts

All AI interactions use **template-based prompts** that are centralized, documented, and easily editable:

| Prompt | Purpose | Gemini Mode |
|--------|---------|-------------|
| Document Scan | Read passports, DNIs, ID cards from image | Vision |
| Check-In Confirmation | Warm, contextual confirmation message | Text |
| Document Issue | Explain problem and next steps | Text |
| Bag Nudge | Smart, contextual baggage suggestion | Text |

Each prompt receives **real passenger context** (name, destination, trip days, language, party info) via template variables like `{{name}}`, `{{destination}}`, `{{trip_days}}`.

---

## Roadmap

| Phase | Milestone | Timeline |
|-------|-----------|----------|
| POC | 4YFN / MWC 2026. Full E2E pipeline. AI doc verification. AI nudge. | **Now** |
| Pilot | 5 domestic routes. Smart eligibility gate. IRROPS fallback. Real DCS. | Q3 2026 |
| Scale | Camera scan. Decoupled upsell engine (D-7/D-3). All domestic routes. | Q3+ 2026 |
| International | EU routes. Multi-leg. Partner airlines. Decision Engine. IATA One ID. | 2027 |

---

## The Bigger Vision &mdash; Decision Engine

Invisible Check-In is the **first module** of a broader AI platform:

| Module | Status |
|--------|--------|
| **Invisible Check-In** | POC live |
| Disruption Manager | Planned |
| Smart Upsell & Cross-Sell | Planned |
| Doc Pre-Validator | Planned |
| Loyalty Activator | Planned |

An orchestration layer that anticipates, decides, and acts before the customer even knows there is something to do.

---

## Risks We're Managing

| Risk | Mitigation |
|------|------------|
| Seat revenue cannibalization | Default random seat + "upgrade before check-in" prompt |
| Coercion perception | Transparent opt-in rules, editable results |
| Document/visa exceptions | Graceful fallback to manual check-in |
| Irregular operations (IRROPS) | Automation pauses, human handoff |
| Regulatory requirements | Smart eligibility gates per route |

---

## Presentation

The full pitch deck is available as a live interactive presentation:

**[View the Pitch Deck](https://gelilho.github.io/vueling-invisible-checkin/deck/pitch-deck.html)**

You can also download it as a PDF:
1. Open the deck link above in Chrome
2. Press `Cmd+P` (or `Ctrl+P`)
3. Set: **Save as PDF**, **Landscape**, **Margins: None**, **Background graphics: ON**
4. Save &mdash; you get a clean 7-page PDF

---

## Data Sources

- IAG Full Year Results 2024
- Vueling ESG Report 2024
- SITA Passenger IT Insights 2023
- IATA Global Passenger Survey 2024
- IdeaWorksCompany Ancillary Revenue Report 2024
- CAPA Centre for Aviation
- AirAdvisor public data

---

## Author

**Angel Garcia**
NextGen Aviation Challenge &middot; 4YFN / MWC 2026

---

*Built with Next.js, TypeScript, Google Gemini 2.0 Flash, and Tailwind CSS.*
