# ShiftReady

**Hand-off practice. Unit early warning.**

ShiftReady is a prototype early warning for hospital workforce readiness. It shows a unit nurse educator when many new nurses miss the same critical cue in practice, so the educator can decide what to review.

**Live demo: https://shiftready-mauve.vercel.app**

> **Training prototype. Not a clinical tool.**
> The demo's seeded patient charts, learner records and incident reports are synthetic. The clinical content is **not clinically reviewed** and is not clinical guidance. No clinician has reviewed it, and nothing was tested in a hospital. Please do not type real patient information into the demo.

Built for the Future Health Digital Innovation Challenge, Challenge 06: Population and Workforce Health Early Warning.

## Try it

Open the [live demo](https://shiftready-mauve.vercel.app) and choose a workspace. No personal account is needed.

| Button | What you see |
|---|---|
| **Continue as Guest — Educator** | The dashboard for synthetic unit 4-West at a fixed demo date. It shows an Alert for recognizing deterioration: about 75% of learners missed a critical cue, with 4 incident reports against a threshold of 2.875 (seeded synthetic data). Open the evidence page to see why the Alert fired. |
| **Continue as Guest — Learner** | "Bed 12: Patient hand-off". Read the synthetic chart, choose "Start hand-off", type a hand-off and submit it. It is saved and locked. When scoring runs, a Claude model scores it and the page shows counts with a link to item feedback. |

Things to know before you start:

- You read a short notice before entering.
- A guest session lasts seven days. You can also create a demo account (username and password) to come back to your saved work. Demo accounts are not verified, have no password recovery, and expire 60 days after the last sign-in.
- AI scoring runs within a small daily budget. When the budget is used up, the page says "Scoring is paused for this demo" and your hand-off is still saved.
- Your submitted hand-off is saved. When scoring runs, the app sends it to Anthropic's Claude API.
- A guest's own hand-off does not change the demo Alert. The Alert is computed from seeded synthetic data.

## How it works

1. **Practice.** A new nurse reads a synthetic patient chart and types a hand-off, either in one free-text box or in four SBAR sections (guided practice: the interface supplies the section order). Either way, one text is submitted and locked.
2. **Scoring.** A Claude model reads the hand-off against a list of key items. Code then checks every quote against the learner's own text, sets confidence tiers and applies dependency rules. Results are counts, such as "8 of 11 key items covered". There is no grade and no single score.
3. **Early warning.** Per unit, a fixed rule combines two signal types: practice misses on critical cues, and incident reports compared with an eight-week baseline. One signal alone is a quiet Watch. Two signals in the same cue family within 7 days raise an Alert.
4. **Explanation.** Each Alert shows its signals, weeks, baseline, threshold and rule version.
5. **Response.** From an Alert, an educator can draft a short pre-shift huddle with a Claude model, then edit and approve it. In the demo, a guest educator's huddle stays private and can be previewed as a learner would see it.

The early warning views show no individual data. Groups under 10 are not shown, percentages are rounded, and the views are not for employment, evaluation or disciplinary use.

## What is built

| Feature | Status |
|---|---|
| Unit Early Warning rule, with groups under 10 not shown | Built |
| Educator dashboard, Alert banner and evidence page | Built |
| Back-test harness, single seed and 10-seed summary | Built |
| Synthetic Bed 12 chart, from a hand-built synthetic FHIR R4 bundle | Built |
| Typed hand-off: start, submit, lock (free text or SBAR sections) | Built |
| Quote check, confidence tiers and dependency rules, in code | Built |
| Guest access and demo accounts | Built |
| Hand-off scoring with a Claude model | Partial |
| Results page | Partial |
| Educator huddle draft and approval | Partial |
| Educator review queue | Partial |
| Voice escalation call, policy Q&A, more patients | Planned |
| Clinical review of the content | Not done |

**What Partial means here**

- **Scoring and results:** two live scoring results exist, both from November 18, 2026 (one on a local build, one on the live site). The rest of the results page was verified on test rows, not on model output.
- **Huddle:** a guest educator's huddle is private, so a learner never sees it in the demo.
- **Review queue:** it covers seeded synthetic items only.

## Results so far

All numbers below are labeled as measured or modeled. None of them describes real nurses or real patients.

**Back-test of the alert rule (modeled, 10-seed mean)**

| Measure | Result | Design target | Met? |
|---|---|---|---|
| Sensitivity | 33% | at least 80% | No |
| Positive predictive value | 25% | at least 60% | No |
| False Alerts per 26 control unit-weeks | 0.07 | at most 1 | Yes |

The back-test generator and the alert rule share assumptions. This tests the logic, not real-world prediction.

**Live scoring (measured, two observations)**

- Local build: 8 of 11 key items covered, 1 missed, 2 not counted; 1 critical item missed.
- Live site: 6 of 11 key items covered, 3 partly covered, 2 held for educator review; 0 critical items missed.

**Tests (measured):** 1,410 automated tests, all passing locally on November 18, 2026.

**Not done:** no formal user study and no clinical evaluation.

## Tech stack

- Next.js 16 and TypeScript, deployed on Vercel
- Postgres on Supabase, with plain SQL migrations
- Claude, through the Anthropic SDK, for hand-off scoring and huddle drafts
- Vitest for tests

## Data and privacy

- The project was built with no real patient data, hospital documents or real names. The demo's seeded patient charts, learner records and incident reports are synthetic.
- The demo stores what a visitor creates: a temporary guest profile or a demo account (a username and a salted password hash), a keyed hash of the network address, and any hand-off text the visitor submits.
- Usage and audit records of AI calls are kept after guest cleanup, and audit records can contain excerpts of a submitted hand-off.
- A visitor could still type real information. This cannot be fully prevented, so please do not.

## About

ShiftReady is a solo project by Simranpal Singh, a computer science student, built and tested with AI coding assistance in November 2026. I am not a clinician. The first step on the path to a pilot is review by qualified nursing educators and clinicians.

The source code is in a private repository. This repository holds the project description and the link to the live demo.
