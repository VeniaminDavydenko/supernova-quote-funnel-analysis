# The Revenue Question — Supernova Quote Funnel Analysis

A funnel analysis of a small-business **insurance quote flow**, built during the
Supernova BIA Externship (TripleTen, June 2026). It answers the question every
digital insurer eventually asks: *why do 1 in 3 applicants quit before they ever
see a price — and what is that leak worth?*

The headline finding: **one question does the most damage.** The Annual Revenue
question is both the biggest friction point in the form *and* the eligibility
gate — and asking it to do both jobs at once costs more potential premium than
the entire book captures today.

📊 **Final deliverable:** [`presentation/The_Revenue_Question_Supernova_Final.pptx`](presentation/The_Revenue_Question_Supernova_Final.pptx)

---

## The Business Question

Supernova sells small-business insurance through a nine-stage online quote form.
Every abandoned form is premium that walks away silently — no decline letter, no
signal, just a session that ends. The question isn't *"how many people finish?"*
— it's *"which stage kills the most quotes, why, and what would recovering it be
worth?"* This analysis walks the funnel stage by stage to tell friction problems
apart from eligibility problems and technical failures — because each one has a
different fix.

---

## About the Externship

The Supernova BIA Externship (via TripleTen, June 2026) simulates the day-to-day
of a business intelligence analyst at a digital insurance underwriter. The work
was structured as **120 numbered business questions across three topics**, each
answered with an Excel pivot table plus a short "why it matters" note — then a
finals phase where I chose one storyline, sized it in dollars, and defended it
in a timed final presentation to the program.

The same 9,817-quote dataset was examined through three different lenses:

1. **Topic 1 — Premium (the revenue lens, Q1–40):** how much money is in this
   book, and what drives it?
2. **Topic 2 — Rejections (the underwriting lens, Q41–80):** who gets declined,
   and why?
3. **Topic 3 — The quote journey (the conversion lens, Q81–120):** where do
   applicants leave, and what does it cost?

The final presentation is built on Topic 3, with the dollar figures from
Topic 1 and the decision logic from Topic 2 supporting it.

---

## Dataset

**9,817 insurance quote attempts**, one row per quote, provided by the externship
program. Each record carries the stage where an incomplete quote stopped
(`incomplete_stage`), the source app, state, industry code and category, the
underwriting decision and its free-text `rejection_log`, address-verification
artifacts (Google Place ID / map link), and a per-coverage premium breakdown
(Building, Contents, Flood, Earthquake, Crime, General Liability, Business
Interruption, EBI, and more).

Each workbook follows an `original data` → `clean data` pipeline plus pivot
tables:

| File | Contents |
|------|----------|
| `data/topic1_premium_analysis.xlsx` | Data cleaning + premium breakdown by category, industry, and source app (Q1–40) |
| `data/topic2_rejections_fus.xlsx` | Rejection rates by category and FUS grade distribution (Q41–80) |
| `data/topic3_funnel_dropoff.xlsx` | Stage-by-stage funnel drop-off analysis (Q81–120) |
| `data/finals_revenue_leak.xlsx` | Final EDA — sizing the revenue leak behind the presentation |

**Feature engineering.** The raw rejection log is free text, so a real part of
the work was parsing it into countable columns: the FUS fire-protection grade
(`Extracted_FUS`), underwriting rule IDs (`Extracted_Rule`), alert flags
(hazardous materials, cranes/heavy equipment, alcohol), revenue bands, a
has-Place-ID flag, and per-email roll-ups to study repeat applicants. One
engineered column (`Email_Domain`) turned out to be a dead end — emails are
hashed — which is documented honestly in the workbook rather than hidden.

**Known limits, named up front:** Ontario is ~90% of rows, so province
comparisons outside it aren't statistically meaningful; ~1,200 rows are missing
a category (mostly clustered in the never-priced cohort); and the data is a
training/synthetic set, so patterns are directional, not gospel.

---

## The Three Topics

### Topic 1 — Premium: sizing the book

Total premium across the dataset is **$23.08M**, spread over 2,419 priced
quotes averaging **$9,541** each (median $9,665 — pricing is symmetric, not
dragged by whales). B&P Services is the cash-cow category by volume; the
spread in average premium across categories is small (~$1,900), and the top
10% of accounts start at $13,928. Scenario questions put numbers on levers:
a +5% General Liability rate change is worth ~$361K, and dropping Flood
entirely would cost ~$276K. The most important derived metric: **revenue per
risk is $2,351** — total premium divided by all 9,817 attempts, not just the
priced ones. That gap between $9,541 per priced quote and $2,351 per lead *is*
the funnel problem, stated as money.

### Topic 2 — Rejections: how underwriting says no

Of the 5,492 quotes that reached a decision, **50.4% were rejected**. Parsing
the rejection log showed the machinery: a **FUS fire-protection grade of 10
(the worst) is an automatic decline** — all 743 such quotes were rejected —
and every alert flag (hazardous materials, heavy equipment, alcohol) is a hard
stop, not a warning. An integrity check confirmed zero rejections with no
logged reason. One subtle finding set up the finals: quotes without a verified
address get rejected more often (56.3%), but "fixing" that recovers $0 of
modeled revenue — the missing address is a symptom, and the real damage it
does shows up in Topic 3.

### Topic 3 — The quote journey: where the money walks away

Completion rate is **67%** — 3,239 of 9,817 applicants abandon mid-form, and
the drop-off chart below shows exactly where. Just as important were the
controls that *ruled out* easy explanations: verified-address applicants don't
complete at a higher rate, no industry segment quits disproportionately, and
cosmetic fixes are small (auto-filling the address recovers only ~22 quotes).
The leak is structural — it's the form itself. This topic also surfaced the
"zombie" cohort: **~1,086 quotes that complete the form but are never priced**
(no decision, no premium, and almost none have a working map link) — a lookup
failure worth an estimated $10.4M, not an underwriting decline. Repeat
applicants told the same story from another angle: 1,305 emails tried more
than once, and repeat attempts complete at only ~35% — a signal of frustration,
not persistence.

### Finals — choosing and defending one story

The finals phase forced a choice: three defensible storylines, one
presentation. I chose the funnel story because it passes the
what / why / so-what test cleanly (Stage 2 is the biggest leak; it's a
sensitive money question asked too early; ~$30.9M is abandoned), and because
its recommendation — reorder the form, fix a lookup — is concrete and
low-risk, unlike Topic 2's natural recommendation of accepting worse fire
risk. The deck was built for a six-minute executive read: one claim per slide,
every rate stated with its denominator.

---

## The Funnel

Nine stages, in order: **S1 Location → S2 Revenue → S3 Years in business →
S4 Employees → S5 Claims history → S6 Contents → S7 Liability → S8 Info →
S9 Start date.** Only 67% of attempts (6,578) survive all nine and reach a
final price; 3,239 quotes (33%) abandon somewhere along the way.

---

## Key Findings

- **The Annual Revenue question (S2) is the single biggest leak** — 600 applicants
  quit there, more than at any other stage. It is the first sensitive money
  question in the form, and it arrives at stage two of nine.
- **The same question is also the eligibility gate.** Supernova declines firms
  over $2M in revenue — but only at the very end, so **about 35% of everyone who
  completes the form is then rejected for being too big.** Reducing friction
  means asking revenue later; filtering ineligible firms means asking it first.
  One question cannot do both.
- **Even finishing isn't safety:** about **1,000 completed quotes generate $0
  premium** — an estimated **$10.4M in pipeline** that finishes the form but is
  never priced. Most have no verified address or map link — an address-lookup
  failure, not an underwriting decline. The form completes; pricing never runs.
- **The leak is bigger than the book.** An estimated **$30.9M of potential
  premium is abandoned in the funnel** versus the **$23.1M captured today** —
  which makes the funnel the single biggest lever in the data.

---

## Visuals

### Drop-offs by Funnel Stage
![Drop-offs by funnel stage](visuals/chart_dropoff.png)

Each bar is the number of applicants who quit at that stage of the nine-step
form. The two highlighted stages are the story: **S2 Revenue (600 drop-offs)**
is the single worst stage — it's the first sensitive money question, and it
arrives before the applicant has invested much in the form. **S6 Contents
(575)** is a close second, where the form shifts from basic facts about the
business to detailed coverage questions. Notice the pattern around them:
drop-off falls once applicants get past S2 (they've committed), then spikes
again when the effort level jumps at S6. The easy stages in between — Years,
Claims, Liability — each lose only ~200.

### Premium Captured vs Abandoned in the Funnel
![Premium captured vs abandoned](visuals/chart_value.png)

This chart sizes the leak in dollars. The left bar is the premium the book
actually captures today ($23.1M) — every quote that finished the form and got
priced. The right bar is the estimated potential premium sitting in the 3,239
abandoned quotes ($30.9M), valued at the average premium of comparable
completed quotes. The abandoned side is *larger than the entire existing
book* — meaning even a modest recovery rate on funnel drop-offs would move
revenue more than any other lever in the data.

---

## Recommendations

1. **Split the revenue question.** Add a quick eligibility gate up front to
   filter the >$2M firms early; keep the detailed revenue question where rating
   actually needs it.
2. **Fix the address lookup.** Require a verified address (Place ID) at Stage 1
   so pricing always runs — recovering the $10.4M in completed-but-unpriced
   quotes.
3. **Re-test and measure.** A/B the new flow; track completion rate and
   priced-quote rate before rollout.

---

## Skills Demonstrated

- **Excel analysis at scale** — original → clean data pipelines, pivot tables,
  and lookup logic across four workbooks and 120 business questions.
- **Feature engineering from messy text** — parsing FUS grades, rule IDs, and
  alert flags out of a free-text rejection log so they could be counted.
- **Denominator discipline** — every rate in the analysis names its base
  (all 9,817 attempts vs 6,578 completions vs 5,492 decisioned quotes), because
  that's where funnel analyses quietly go wrong.
- **Ruling things out** — using control comparisons (address verification,
  segment mix, cosmetic fixes) to show the leak is structural, not
  demographic.
- **Sizing findings in dollars** — translating drop-off counts into revenue
  estimates an executive can rank ($30.9M abandoned, $10.4M unpriced).
- **Executive storytelling** — choosing one storyline out of three defensible
  ones and defending it in a timed presentation.

---

## Notes

- **Tools:** Excel (data cleaning, pivot tables, funnel analysis), PowerPoint.
- The dataset was provided by the externship program for training purposes and
  reflects a simulated/anonymized book of business, not live production data.
  Revenue-leak figures are estimates (abandoned quotes valued at the average
  premium of comparable completed quotes) and are labeled as such.
