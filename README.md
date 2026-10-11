# Popat 🦜

**Detecting AI-assisted source code theft by insiders**

> 🚧 **Status: research in progress.** The problem framing, architecture and data collection plan are done. Data collection is underway; the detection model and evaluation are next. The dashboard in this repo runs on **synthetic demo data only**.

---

## The problem

Developers now build company software with AI coding assistants such as Claude Code, Codex and GitHub Copilot. These assistants can read an entire repository, which creates a new insider-threat path:

1. An employee asks the approved AI assistant to describe the project in detail.
2. The answer is plain text, so it can leave the company through copy-paste, a personal device, or even a photo of the screen.
3. Another AI can use that text to regenerate a close copy of the project elsewhere.

Traditional **data loss prevention (DLP)** looks for files and known patterns leaving the network. A natural-language description of a codebase looks like ordinary text, so it slips past. And because the copy is *regenerated* rather than copied, classic clone detection struggles to match it to the original.

### Why it matters

- **Insiders already defeat DLP by changing the format of data.** In January 2026, a former Google engineer, Linwei Ding, was convicted on 14 counts of economic espionage and trade-secret theft. Prosecutors said he copied source content into Apple Notes and converted it to PDFs before uploading, which helped him avoid immediate detection. An AI-written summary is a far easier way to change the format.
- **Source code is already the most common sensitive data sent to AI tools.** Harmonic Security found that 65% of the sensitive technical data employees put into GenAI tools was proprietary source code (Q3 2025). Netskope reported source code as the most frequent type of sensitive data posted to ChatGPT.
- **Rebuilding software has become cheap.** Legal commentary in 2026 notes that AI has collapsed the cost of reimplementing software, so trade-secret protection and technical safeguards matter more than ever.

### The research gap

Prior work covers pieces of this: insider-threat detection on activity logs (UEBA), DLP for AI prompts, detection of LLM-paraphrased code, and code leakage *to* AI providers. To our knowledge, no published work detects insiders who use an approved AI assistant to extract a project **in a rebuildable form**, while keeping false alarms low on legitimate use such as onboarding and documentation.

---

## Our approach

**Key idea: an insider can disguise the question, but not the answer.** To be useful for theft, what the AI returns has to contain real implementation detail. So Popat looks at *what information leaves*, accumulated over time per person, rather than only at how requests are worded.

Popat works at the **AI-interaction layer** (an enterprise AI gateway or the AI tool's audit logs), where a request is visible no matter how its answer leaves the building afterwards.

**Two levels of scoring**

| Level | Question it answers |
|---|---|
| Session | How much standalone implementation detail did this interaction reveal? |
| Employee | Across weeks, how much of the codebase has this person's AI activity covered, and does it fit their role and their own normal behavior? |

**Planned signals** (being refined during research)

- **Coverage:** share of the codebase described in detail across all of a user's sessions
- **Standaloneness:** whether outputs point *into* the repository or are written to work *without* it
- **Scope vs. role:** activity on modules outside the person's normal area
- **Behavioral drift:** change from the person's own history
- **Context:** timing signals the company already holds, such as notice periods

**Modelling plan**

- Anomaly detection trained on **normal** developer–AI behavior (Isolation Forest, One-Class SVM, autoencoder)
- A lightweight supervised classifier, compared with a small fine-tuned transformer
- Baselines: keyword DLP rule, request-text-only classifier, volume-only behavior analytics

**Design principle:** Popat produces a **risk score with evidence for a human analyst**. It never accuses, blocks or punishes anyone automatically.

See [`docs/architecture.md`](docs/architecture.md) for the system design.

---

## Repository layout

```
popat/
├── README.md
├── docs/
│   ├── architecture.md    system design and data flow
│   └── dataset_plan.md    data collection plan, consent and privacy
└── dashboard/
    └── index.html         analyst dashboard demo (synthetic data)
```

### Try the dashboard

Open `dashboard/index.html` in any browser. No install or server is needed. Every name and number in it is randomly generated demo data.

---

## Roadmap

- [x] Problem framing and related-work review
- [x] System architecture
- [x] Data collection plan with consent and privacy safeguards
- [x] Analyst dashboard prototype (demo data)
- [ ] Collect consent-based dataset of developer–AI sessions *(in progress)*
- [ ] Feature extraction pipeline
- [ ] Anomaly detection and classifier models
- [ ] Evaluation: accuracy, false alarms on legitimate use, robustness
- [ ] Connect real model output to the dashboard
- [ ] Paper submission to a security workshop

---

## Ethics

Popat is a **defensive** research project. Real data is collected only from consenting participants working on open-source projects, under the plan in [`docs/dataset_plan.md`](docs/dataset_plan.md). Any production use of employee monitoring must follow applicable law, including India's Digital Personal Data Protection Act, 2023, and must inform employees.

## References

- The Register, *Ex-Googler convicted for stealing AI secrets* (Jan 2026): https://www.theregister.com/2026/01/30/google_engineer_convicted_ai_secrets_china/
- Harmonic Security, Q3 2025 GenAI data exposure research: https://www.businesswire.com/news/home/20251113987633/en/
- Netskope findings via TechRadar: https://www.techradar.com/pro/lots-of-sensitive-data-is-still-being-posted-to-chatgpt
- Park et al., *Detection of LLM-Paraphrased Code* (2025): https://arxiv.org/abs/2502.17749
- Finkman et al., *CodeCloak* (2024): https://cris.bgu.ac.il/en/publications/codecloak-a-method-for-mitigating-code-leakage-by-llm-code-assist/
- IOActive, *Husn Canaries* (2026): https://www.ioactive.com/husn-canaries-defense-in-depth-for-ai-coding-assistant-governance/
- Reed Smith, *Clean room coding in the age of AI*: https://www.reedsmith.com/our-insights/blogs/technology-law-dispatch/102nbig/clean-room-coding-in-the-age-of-ai-who-owns-the-code/

## Author

**Sukumar Manivel** · [GitHub](https://github.com/Sukumar-Manivel)
