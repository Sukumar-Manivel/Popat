# Dataset Plan

> Status: collection in progress. No real participant data is stored in this repository.

## Why we build our own dataset

No public dataset records how developers use AI coding assistants **together with** company context such as roles, owned modules and access. Popat needs both, so we combine public data on normal behavior with a small, consent-based study.

## Sources

| Source | What it gives | Used for |
|---|---|---|
| DevGPT | Real developer–ChatGPT conversations shared on GitHub | How developers normally ask AI for coding help |
| WildChat / LMSYS-Chat-1M | Large collections of real user–AI chats (coding subset) | Broader normal-request patterns |
| CERT Insider Threat Dataset (CMU SEI) | Synthetic employee activity logs with labeled insider cases | Timing and behavior signals |
| **Our study** | Consent-based developer–AI sessions on open-source projects, with roles | Company-context features and labeled test cases |

Each public dataset will be used only under its license and terms of use; access and licensing are being verified before use.

## Our study

**Participants:** 10–30 consenting volunteers (students and junior developers), age 18+.

**Setting:** participants use an AI coding assistant on **open-source repositories**, so no real company code is involved. Each participant gets a role (for example backend developer, new joiner, tech lead) and an assigned area of the codebase.

**Groups**

- **Normal group (majority):** everyday tasks such as onboarding, bug fixing, writing documentation, code review and handoff notes. These include intentionally *broad* legitimate tasks, so the model learns that wide exploration is not automatically suspicious.
- **Role-play group (small, supervised):** participants act as an insider under study rules agreed with the supervising faculty member. These sessions are used mainly to **test** the model, not to train it.

**What is logged per session**

| Field | Description |
|---|---|
| session_id, user_id | Pseudonymous IDs |
| timestamp | Date and time |
| repo, files_in_context | Which project and files the AI could see |
| request_text, response_text | The interaction |
| role, assigned_modules | Participant's role and work area |
| label | normal / role-play (known to researchers only) |

## Consent and privacy

- Approval from the institution's ethics committee before collection
- Written informed consent; participants may withdraw at any time and have their data deleted
- No personal data in session content; participants are told not to enter any
- Pseudonymous IDs; the key linking IDs to names is stored separately and deleted after the study
- Data stored encrypted with access limited to the research team
- Only aggregate results and de-identified samples are published
- Compliance with India's Digital Personal Data Protection Act, 2023

## How the data will be used

- Train anomaly-detection models on **normal** behavior
- Evaluate on held-out participants and held-out repositories, so results are not inflated by memorization
- Report false alarms on legitimate broad-use tasks separately from overall accuracy
