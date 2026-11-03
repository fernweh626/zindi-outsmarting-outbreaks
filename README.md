# Outsmarting Outbreaks Challenge — Competition Design & Benchmark

> **My role:** Competitions Lead at [Zindi](https://zindi.world) — I ran this competition end to end and wrote the starter notebook participants built from.
> **Competition page:** https://zindi.world/competitions/outsmarting-outbreaks-challenge
> **Data (not redistributed here):** https://zindi.world/competitions/outsmarting-outbreaks-challenge/data

## The problem
Forecast the number of **waterborne / water-related disease cases** reported by health facilities each month, across diseases including cholera, diarrhea, dysentery, typhoid, malaria, schistosomiasis and intestinal worms. Earlier, more accurate forecasts let health authorities pre-position supplies, staff and prevention campaigns before an outbreak peaks.

- **Partner / sponsor:** TODO
- **Decision this supports:** TODO (e.g. resource allocation by facility/district)

## My contribution (product & delivery)
- Ran the competition lifecycle. TODO: edit to what you actually owned
- Packaged a multi-source dataset: monthly health-facility case counts (target `Total`, 2019–2022 for training, 2023 held out) plus supporting **toilet, waste-management and water-source** datasets, and a variable dictionary of environmental / climate indicators.
- Wrote a starter notebook that shows how to **join geospatial context to each facility** and produce a valid submission.
- TODO: metric choice, partner management, community engagement

## Approach (benchmark)
| Step | Decision | Why |
|---|---|---|
| Data integration | Nearest-neighbour join (KD-tree on lat/lon) from each facility to the closest toilet, waste and water-source records | Brings WASH infrastructure context into a facility-level model without needing exact IDs |
| Validation | Time-based split: train on years < 2023, predict 2023 | Reflects the real forecasting use case and avoids leakage |
| Model | `RandomForestRegressor` | Strong, low-tuning baseline for mixed numeric/categorical features |
| Metric (benchmark) | Mean Absolute Error | Interpretable in "cases" |

## Results & impact
- Participants: TODO
- Benchmark → winning score: TODO
- Other outcomes: TODO

## Repo contents
```
notebooks/starter_notebook.ipynb   # participant-facing starter + benchmark
data/                              # empty — download from Zindi (link above)
```
Outputs are stripped from the notebook. To run: download the data into `data/`, `pip install -r requirements.txt`, open the notebook.

## Attribution & licensing
The notebook was written by me while at Zindi for the competition. The competition data is © its owners and governed by the Zindi competition rules; it is not included here.
