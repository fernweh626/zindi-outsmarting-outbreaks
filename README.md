# Outsmarting Outbreaks Challenge — Competition Design, Partner Delivery & Benchmark

> **My role:** Competitions Lead at [Zindi](https://zindi.world) (April 2019 – November 2025). I ran this competition end to end as a product: data-owner engagement, data review, challenge design, metric choice, participant tooling and benchmark.
> **Competition page:** https://zindi.world/competitions/outsmarting-outbreaks-challenge
> **Data (not redistributed here):** https://zindi.world/competitions/outsmarting-outbreaks-challenge/data
> **Winning solution write-up (I helped the winners write it up and published it):** https://zindi.world/learn/winning-solution-to-sua-outsmarting-outbreaks-challenge

## The problem
Build a machine-learning model that predicts outbreaks of **climate-sensitive waterborne diseases**, using data on water sources, toilet quality, waste management, health facilities and climate from 2019 to 2023. The target is the number of cases per health facility, disease and month, across diseases including cholera, diarrhea, dysentery, typhoid, malaria, schistosomiasis and intestinal worms.

The aim is to let governments and health organisations **intervene earlier and in a more targeted way, and allocate resources better**, reducing disease incidence and building public-health resilience.

## Partners
- **Sponsors:** AWS, UNESCO and IRCAI
- **Data providers:** Sokoine University of Agriculture (SUA), Tanzania
- **Partner goal:** demonstrate to government what is possible with this data and a working model. Planned next steps were to put the best solution into a **dashboard** and take it to government.

## Product thinking
- **Users:** (1) data scientists on Zindi who build the models; (2) the partners (SUA and sponsors) who needed proof of what the data could do, as a case to make to government.
- **Success metric:** Mean Absolute Error (MAE). This is a regression problem on **counts**, so MAE is in the same units as the thing decision-makers care about (cases) and is easy to explain to a non-technical audience.
- **How to read MAE:** an MAE of 12.4 means the model's prediction for a given facility, disease and month is off by about 12 cases on average. The benchmark scored **12.41**, and the winning solution brought that down to roughly **6.5**, nearly halving the average error.
- **Key trade-offs:** realistic, multi-source data vs. a task participants can start quickly; an accessible compute environment for everyone vs. the cost and constraints of the cloud sponsor's credits; a benchmark that sets a baseline without capping what's possible.

## My contribution
- **Data-owner engagement:** worked in tandem with SUA, who are data-mature, to design the challenge around what their data could credibly support.
- **Data review and quality checks** before launch.
- **Challenge design:** wrote and published the challenge description, and chose the error metric.
- **Participant tooling:** wrote the starter notebook and benchmark (below).
- **Sharing the learning:** assisted the winning team in writing up their solution and published it on Zindi.
- **Making cloud credits inclusive.** AWS provided credits for participants to build on Amazon SageMaker. A large share of our community doesn't have a credit card, which AWS sign-up requires, and we did not want to exclude anyone. With our CTO and Zero-One (our development team), we created multiple accounts under a separate Zindi AWS account. I then distributed the environments to teams using mail merge and SQL, matching the right VMs to the right teams.

## Approach (benchmark)
| Step | Decision | Notes |
|---|---|---|
| Data integration | Join each facility to its nearest toilet, waste-management and water-source record (KD-tree on lat/lon) | Spatial only. See "What I learned" below |
| Model | `RandomForestRegressor` | Strong, low-tuning baseline for mixed numeric/categorical features |
| Train/test | Train on years < 2023, predict 2023 | Mirrors the forecasting use case |
| Metric | Mean Absolute Error | Interpretable in cases |

## Results & impact
- **Benchmark MAE:** 12.41 on the leaderboard
- **Winning MAE:** about 6.5
- Participants, submissions, and downstream partner outcomes (dashboard, government engagement): to be added.

## What I learned: a flaw in my starter notebook
During the competition a participant [pointed out](https://zindi.world/competitions/outsmarting-outbreaks-challenge/discussions/24226) that the starter notebook's data linkage was wrong. My join matched each facility to the **geographically nearest** toilet, waste and water record, but **ignored the date**. A December 2022 facility record could therefore be paired with, for example, a water record from December 2023. The participant found that dropping the linked datasets scored better than the starter, and others in the thread reported that scores of around 6 were reachable without the linkage at all.

**What this taught me as a product owner:**
- A starter notebook is not just a teaching aid. Participants treat it as the **spec of how the data fits together**, so a flaw in it becomes a flaw in everyone's approach.
- For time-indexed data, the starter needs an explicit **leakage and temporal-consistency review** (and ideally a second reviewer) before launch. A random validation split on top of a time-based test set hid the problem from the benchmark's own score.
- Join logic on multi-source data should be documented as an assumption participants are invited to challenge, with a visible known-issues note when questions come in.

I've left the notebook's outputs in place, and the flawed join as it was published, so you can see exactly what participants saw.

## Repo contents
```
notebooks/starter_notebook.ipynb   # participant-facing starter + benchmark (as published, with outputs)
data/                              # empty — download from Zindi (link above)
```
To run: download the data into `data/`, `pip install -r requirements.txt`, open the notebook.

## Attribution & licensing
The notebook was written by me while at Zindi for the competition. The competition data is © its owners and governed by the Zindi competition rules; it is not included here. The notebook's saved outputs include a few small previews of rows and maps.
