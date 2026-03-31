# Microsoft Viva Engage — Community Health Score Blueprint

> **Most dashboards count posts. This one asks if anyone's actually listening.**

**🔗 [Live Dashboard →](https://sanamjummani.github.io/Microsoft_VivaEngage_CHS_Blueprint/)**

A full-lifecycle analytics framework built for Microsoft Viva Engage — measuring what post counts miss. Designed and validated on real public data from GitHub Discussions and StackExchange across 22+ communities.

Built as part of the **Purdue BAIM × Microsoft Industry Practicum, Spring 2026**.

---

## The Problem

Enterprise social networks like Viva Engage host thousands of internal communities — but leadership had no reliable way to know which ones were thriving vs. quietly dying.

Raw activity metrics are misleading. A community can log thousands of posts and still be broken — if nobody replies, if one person dominates all the conversation, or if the tone is toxic. **High activity does not equal community health.**

The ask: design a Community Health Score that gives HR, leadership, and community managers an early-warning system and a clear picture of where to intervene — and why.

---

## What We Built

### 1. The CHS Framework — 5 Health Themes, 19 Sub-Metrics

Every metric grounded in literature (14+ academic papers and industry reports). Every formula justified and calibrated. Weights adjust by community type.

```
CHS = (w₁ × Engagement) + (w₂ × Responsiveness) + (w₃ × Content Quality) + (w₄ × Vitality) − Risk Penalty
```

| Theme | What It Measures | Key Metrics |
|---|---|---|
| **T1 Engagement & Activity** | Is anyone home? | Active User Rate, Gini Coefficient, Replies-to-Post Ratio, Unique Contributors/Thread |
| **T2 Responsiveness & Dialogue** | Does anyone care when you post? | Median Time to First Reply, 24-Hour Coverage, Thread Continuity, Reply Reciprocity |
| **T3 Content Quality & Sentiment** | Is content useful or noise? | Sentiment Balance Score, Helpful Thread Ratio, Average Reply Richness |
| **T4 Community Vitality** | Is the community self-renewing? | New Contributor Rate, Retention Ratio, Topic Novelty Index, Lurker Conversion Rate |
| **T5 Risk & Fragility** | What warning lights are on? | Ghost Post Rate, Super-User Dependency, Toxic Sentiment Rate, Activity Decline Trend |

Weights are **not one-size-fits-all.** A new community, a technical community, a support community, and a mature community are scored by different standards — because health is defined relative to purpose.

| Theme | 🌱 New | ⚙️ Tech/IT | 🤝 Support | 🏛️ Mature |
|---|---|---|---|---|
| T1 Engagement | 0.30 | 0.20 | 0.25 | 0.15 |
| T2 Responsiveness | 0.20 | **0.35** | 0.20 | 0.25 |
| T3 Content Quality | 0.20 | 0.30 | **0.35** | 0.25 |
| T4 Vitality | **0.30** | 0.15 | 0.20 | **0.35** |
| T5 Risk Penalty | Always subtracted | Always subtracted | Always subtracted | Always subtracted |

### 2. The Data

Real communities, real data — not simulated or synthetic.

**GitHub Discussions** (via GraphQL API): `vercel/next.js`, `ohmyzsh/ohmyzsh`, `vuejs/core`, `sveltejs/svelte`, `denoland/deno`, `dotnet/runtime`, `moby/moby`, `expressjs/express`, `lodash/lodash`

**StackExchange** (via SEDE): Python, JavaScript, Java, C#, ReactJS, Data Science, Machine Learning, Docker, TypeScript, Flask, Yammer, Microsoft Graph, Streamlit

Communities were tiered as Popular / Mid / Niche / Dead to validate whether the framework scores matched known community lifecycle stages.

### 3. The Dashboard

**[→ Open Live Dashboard](https://sanamjummani.github.io/Microsoft_VivaEngage_CHS_Blueprint/)**

An interactive single-page dashboard built in pure HTML/CSS/JS — no frameworks, no dependencies. Features:

- **Snapshot View** — CHS score, 5 theme bars, radar chart, community selector
- **Deep Dive** — sub-metric breakdown per theme with score explanations
- **Nudges** — 41 prioritized, role-targeted interventions triggered by live metric values
- **Compare View** — side-by-side CHS scoring across all 22 communities

### 4. The Nudge System — 41 Actionable Interventions

The dashboard doesn't just show a score — it tells you what to do next. Every metric threshold triggers a specific, role-targeted nudge.

```
Priority system:
🔴 CRITICAL  →  Maximum 1 shown at a time. Act immediately.
🟡 WARNING   →  Maximum 2 shown. Address within 1–2 weeks.
🟢 OPTIMIZE  →  Maximum 3 shown. Enhancement opportunities.
```

Example nudges:
- GPR > 40% → *"Set a team norm: No post goes unanswered for >48 hours"*
- SUDI > 80% → *"Identify and train 3 backup experts in your domain"*
- RR < 50% → *"Send a personalized DM to members who haven't posted in 30 days"*

---

## Key Findings

From analyzing 22 real communities across both platforms:

**1. Ghost posts are the #1 community killer.** Ghost post rate ranged from 12% (sveltejs/svelte, one of the healthiest) to 88% (vercel/next.js). The single strongest predictor of community decline.

**2. Activity ≠ health.** moby/moby appeared engaged on raw post counts — but one user contributed 52% of all content. High super-user dependency with low participation breadth: a community one resignation away from collapse.

**3. Community type fundamentally changes the score.** GitHub technical repos and StackExchange niche communities score very differently on identical raw metrics. A universal formula mis-classifies both. Context-adjusted weights are essential for honest health signals.

---

## Dashboard Score Interpretation

| CHS Score | Status | Recommended Action |
|---|---|---|
| 80–100 | 🟢 Thriving | Monitor and replicate |
| 65–79 | 🟢 Healthy | Maintain momentum |
| 50–64 | 🟡 Moderate Risk | Identify lowest theme, trigger alerts |
| 35–49 | 🟠 Declining | Immediate review, leadership visibility needed |
| < 35 | 🔴 Critical | Community revival program required |

---

## Repo Structure

```
Microsoft_VivaEngage_CHS_Blueprint/
│
├── index.html                              ← Live interactive dashboard (v5)
├── README.md
│
├── data/
│   ├── github_data.csv                     ← GitHub Discussions dataset (626 rows, 9 communities)
│   ├── stackexchange_data.csv              ← StackExchange SEDE dataset (12,976 rows, 13 communities)
│   └── Data_Dictionary.xlsx               ← Field definitions for both datasets
│
├── analytics/
│   ├── CHS_Analytics_Reference.xlsx       ← Pre-computed CHS scores for all 22 communities
│   │                                          (Executive Dashboard · Type Classification ·
│   │                                           Metric Deep Dive · Risk & Nudges)
│   │                                          Note: scores are pre-computed from Python.
│   │                                          Full formula logic → CHS_Computation_Logic.docx
│   └── CHS_Computation_Logic.docx         ← Every metric formula mapped to its CSV column,
│                                              plain-English explanations, constraint notes,
│                                              and what requires GPT-4/BERTopic in production
│
├── docs/
│   ├── CHS_Metrics_Framework.pdf          ← Full framework: all 5 themes, formulas, weights
│   ├── CHS_Nudges_Reference.pdf           ← All 41 nudges with triggers and priority logic
│   └── Microsoft_Problem_Statement.pdf    ← Original project brief
│
└── presentation/
    └── Microsoft_CHS.pptx                 ← Portfolio deck
```

---

## Methodology & Constraints

**What we built:** The full CHS framework (5 themes, 19 sub-metrics, formulas, community-type weights), the data pipeline, and the interactive dashboard.

**What we scoped out:** GPT-4 sentiment analysis (T3 Sentiment Balance Score) and BERTopic topic modeling (T4 Topic Novelty Index) were designed into the framework but not implemented due to API budget constraints. The architecture fully supports them — they're the next layer to add.

**Why public data:** Viva Engage data is internal and inaccessible for academic projects. GitHub Discussions and StackExchange mirror Yammer's threaded discussion model closely enough to validate the framework's logic at scale.

---

## Literature Foundation

The framework is grounded in 14+ sources including:

- Iriberri & Leroy (2009) — Community lifecycle stages
- Harshil Panchal — *"Social Resilience: Autopsy of Friendster"* — super-user dependency risk
- Gaurang Panpalia — PubMed survival analysis, arXiv commitment conversion
- Gabriel Ponsot — Reddit interconnectivity, Signal-to-Noise methodology
- Khoros/Lithium Community Health Index — 24-hour coverage benchmark
- Hassan El Habbal (WWW 2018) — Thread-ending and unanswered content risk
- Sana Majeed — TAM model, perceived usefulness and self-worth in community retention
- GroupOS — Engagement depth and lurker-to-contributor dynamics

---

## Team

**Purdue BAIM × Microsoft Industry Practicum — Spring 2026**

A team of M.S. in Business Analytics & Information Management (BAIM) students at Purdue University, in collaboration with Microsoft and under faculty mentorship.

---

## License

This project was built for academic and portfolio purposes under the Purdue BAIM × Microsoft Industry Practicum. Data sourced from public APIs (GitHub GraphQL, StackExchange SEDE). Framework methodology is original work of the student team.

---

*Built for the question Microsoft couldn't answer yet.*
