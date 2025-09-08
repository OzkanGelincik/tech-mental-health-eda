# Mental Health in Tech — Exploratory Data Analysis (NYC DSA)

Exploratory analysis of the OSMI “Mental Health in Tech” survey.  
The notebook cleans the data, engineers interpretable features (support, stigma, and parity-gap indices), and uses seaborn to visualize relationships with treatment seeking and work interference.

> **Headline:** Higher perceived employer support is associated with higher treatment rates. Interview comfort and perceived consequences don’t always agree—about ~1/3 say “no interview gap” while still believing consequences are worse for mental health.

---

## Contents

- `notebooks/mental_health_in_tech_eda.ipynb` – main analysis
- (optional) `data/` – put the CSV here (not tracked)

---

## Data

- Source: OSMI “Mental Health in Tech” survey on Kaggle  
  (Download from the dataset page and place the CSV under `data/`.)

**Note:** This repo does not include the raw CSV. See the Kaggle license/terms.

---

## What the notebook does

### Cleaning & recodes
- **Timestamp** → parsed as `datetime`
- **Age** → numeric with outlier handling; binned to `<25, 25–34, 35–44, 45+`
- **Gender** → consolidated to `Man / Woman / Non-binary / GNC / Unknown`
- **Company size** → ordered categorical (`1–5, 6–25, 26–100, 100–500, 500–1000, 1000+`) and an ordinal score
- **US vs Non-US** from `Country` (with an `Unknown` bucket)
- **Supervisor / Coworkers comfort** → ordered categoricals (`No < Some of them < Yes`) and ordinal scores

### Feature engineering
- **Support score (0–5):** `benefits`, `care_options`, `seek_help`, `anonymity`, `wellness_program`  
  (`Yes`=1; `No/Don't know/Not sure`=0; normalized variant 0–1 included)
- **Stigma index:** negative consequence and willingness items (higher = worse stigma)
- **Parity gaps (−1/0/+1):**
  - `parity_gap_consequence`: mental vs physical consequences
  - `parity_gap_interview`: mental vs physical interview comfort
  - Categorical labels: *Physical safer / No gap / Mental worse*
- **Outcomes:**
  - `treat_bin` (0/1) from `treatment`
  - `work_interfere_sev` (0–3) from `work_interfere`

### Visuals (seaborn)
- **Support → Treatment** (with 95% CIs), plus **facets by company size**
- **Support → Work interference** (95% CIs)
- **Parity gap mismatch heatmaps** (counts & row %) for interview vs consequence
- **Treatment by age band & gender** (stacked bars)
- **Comfort group comparisons** (neither / coworkers-only / supervisor-only / both)

---

## Quick results (from this notebook)

- **Support & treatment:** strong positive association up to mid–high support; plateaus thereafter.  
- **Support & interference:** weak/flat overall; likely heterogeneous by subgroup.  
- **Gap mismatch:** ~64% agreement; ~1/3 say *no interview gap* but *worse consequences* for mental health.

> These are **associations**, not causal effects.

---

## Getting started

### Environment
Python ≥ 3.10  
Install packages (minimal set):
```bash
pip install pandas numpy seaborn matplotlib statsmodels
