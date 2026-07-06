# The Revenue Question — Supernova BIA Externship

**Veniamin Davydenko · Business Intelligence Analyst Externship (TripleTen × Supernova) · June 2026**

Funnel analysis of an insurance quote flow: where applicants drop off, what the leak is worth, and three concrete fixes to recover it.

📊 **Final deliverable:** [`T4/The_Revenue_Question_Supernova_Final.pptx`](T4/The_Revenue_Question_Supernova_Final.pptx)

## The problem

Supernova (a small-business insurance provider) was losing **1 in 3 quote applicants before they ever saw a price**. My job across the externship was to find where the funnel leaks, quantify it, and recommend fixes.

## Key findings

- Of **9,817** quote attempts in the dataset, only **67%** (6,578) reached a final price — **3,239 applicants (33%) abandoned before pricing**.
- The single biggest drop-off was the **Annual Revenue question** (~600 applicants quit at Stage 2) — the first sensitive money question in the form. It also doubles as the eligibility gate: firms over $2M in revenue are declined, but only at the very end, so ~35% of completers were rejected after doing all the work.
- Even completing the form wasn't safe: **~1,000 completed quotes generated $0 premium** — an estimated **$10.4M in pipeline** that finished the form but was never priced, traced to an address-lookup failure (no verified Place ID), not underwriting declines.
- The total leak is worth more than the **$23.1M** the book captures today, making the funnel the single biggest lever in the data.

## Recommendations

1. **Split the revenue question** — add a quick eligibility gate up front; keep detailed revenue where rating actually needs it.
2. **Fix the address lookup** — require a verified address (Place ID) at Stage 1 so pricing always runs.
3. **Re-test and measure** — A/B the new flow; track completion and priced-quote rates before rollout.

## Repo contents

| Path | What it is |
|---|---|
| `T1/` – `T3/` | Topic workbooks — staged analysis of the quote dataset in Excel |
| `T4/` | Final deliverable: presentation deck, study guide, and prep/delivery guides |
| `Questions.xlsx` | Working Q&A log for the externship |

## Tools

Excel (pivot tables, funnel analysis), PowerPoint.

## Data note

The dataset was provided by the externship program for training purposes and reflects a simulated/anonymized book of business, not live production data.
