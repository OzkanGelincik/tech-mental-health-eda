# 🧠 Mental Health in Tech — Multi-Year OSMI EDA (2014–2023)

> **Core question.** Do visible, supportive workplace policies (benefits, care options, seek-help resources, anonymity, wellness communications) correlate with a higher likelihood of seeking treatment?

This repo contains a reproducible exploratory analysis of the Open Sourcing Mental Illness (OSMI) tech worker surveys. It harmonizes the 2014 instrument with the 2016–2023 waves so you can compare patterns over time and communicate actionable employer takeaways.

---

## 🗂️ Dataset Overview

| Item | Details |
|---|---|
| 📅 Years covered | 2014, 2016–2023 |
| 👥 Population | Tech workers (developers, engineers, IT professionals, etc.) |
| 🌍 Geography | Global (country reported; normalized) |
| 🎯 Goal | Understand how policy visibility & stigma relate to treatment-seeking |
| 📄 Files expected | `survey.csv` (2014) and `survey_YYYY.csv` for later years (same folder as the notebook) |
| 📦 Output | `osmi_wide_2014_2023.csv` — harmonized, analysis-ready table |

**Discovered files example:** https://osmhhelp.org/research.html

| Year | File |
|---:|---|
| 2014 | `survey.csv` |
| 2016 | `survey_2016.csv` |
| 2017 | `survey_2017.csv` |
| 2018 | `survey_2018.csv` |
| 2019 | `survey_2019.csv` |
| 2020 | `survey_2020.csv` |
| 2021 | `survey_2021.csv` |
| 2022 | `survey_2022.csv` |
| 2023 | `survey_2023.csv` |

---

## 🔑 Key Variables (harmonized)

- **Demographics:** `age`, `gender`, `country`, `family_history`
- **Outcomes:** `treatment` → `treat_bin` (Yes=1, No=0), `work_interfere`
- **Policy pillars (2014 wording; later waves regex-mapped):** `benefits`, `care_options`, `seek_help`, `anonymity`
- **Wellness communication:** `wellness_program`
- **Derived features:**
  - `support_score4 = benefits + care_options + seek_help + anonymity` (0–4)
  - `support_score5 = support_score4 + wellness_program` (0–5)
  - Optional: `gender_2` (Woman/Man), `age_cat_2` (<25, 25–34, 35–44, 45+), `work_interfere_cat_2` (Never/Rarely vs Sometimes/Often)

**Policy recoding to 2014 style**

| Source answers | Mapped to | Score |
|---|---|---:|
| Yes, Some did, Yes, they all did | **Yes** | 1 |
| No, None did, Not eligible for coverage / N/A, Not eligible for coverage / NA | **No** | 0 |
| I don’t know, I am not sure, Not sure | **Don’t know** | 0 |
| Missing | — | NaN |

> Later-year headers are normalized with exact renames when known and regex fallbacks for variants (e.g., “formally discussed … as part of a wellness campaign” → `wellness_program`; HTML bold/italic in “What country do you work in?” is handled).

---

## ✨ Project Highlights

- ✅ Harmonized eight survey waves to the 2014 instrument (robust header/regex mapping).
- ✅ Cleaned & normalized categorical answers; kept true missing as **NaN**.
- ✅ Built support scores (0–4 and 0–5) to capture policy availability + visibility.
- ✅ Country normalization (e.g., **United States of America → United States**).
- ✅ Clear visual story: point plots (with 95% CI), stacked bars, heatmaps, waffles/treemaps, wordclouds.
- ✅ Light stats & modeling: Welch’s *t*, pairwise comparisons, logistic regression, and leave-one-out ablation of the support score.

---

## 📊 Quick Findings (high-level)

- **Policy visibility ↔ treatment:** Higher support scores are associated with markedly higher treatment-seeking rates.
- **Work interference matters:** Respondents reporting *Sometimes/Often* interference seek treatment substantially more than *Never/Rarely* (t-tests in the notebook).
- **Gender differences:** Trends are often cleaner (more monotonic) among men; women sometimes show plateaus at higher support levels.
- **Employer actions:** Make policies explicit (not buried), communicate them (wellness campaigns), and protect anonymity. These are cheap wins with real signal.

> ⚠️ These are associations, **not** causation. Later waves have smaller *N*. Interpret directionally.

---

## 🧭 Repo Structure

| Path | What |
|---|---|
| `mental_health_in_tech_EDA.ipynb` | Main notebook (discovery → harmonization → analysis → visuals) |
| `survey.csv`, `survey_2016.csv` … `survey_2023.csv` | Raw OSMI CSVs (place alongside the notebook) |
| `osmi_wide_2014_2023.csv` | Harmonized, analysis-ready dataset (written by the notebook) |
| `README.md` | This file |

---

## ⚙️ Tech Stack

- **Language:** Python (3.12)
- **Notebook:** Jupyter
- **Libraries:** pandas, numpy, matplotlib, seaborn, scipy, scikit-learn, pywaffle, squarify, wordcloud

---

## ▶️ How to Run

1. Download data from OSMI and place files in the same folder as the notebook: `survey.csv` (2014) and any subset of `survey_2016.csv` … `survey_2023.csv`.
2. Install deps:
   ```bash
   pip install pandas numpy matplotlib seaborn scipy scikit-learn pywaffle squarify wordcloud
3.  Open mental_health_in_tech_EDA.ipynb and run all cells. You will get osmi_wide_2014_2023.csv plus plots and tables used in the slides.

---

## 🧱 Harmonization Details (what the code does)

- Discovery: Finds survey.csv + survey_*.csv and builds a {year: path} map.
- Column mapping: exact renames (when known) + regex fallbacks (e.g., wellness program, seek-help resources, “country you work in”).
- Recoding: Policy answers mapped to Yes/No/Don’t know → {1,0,0}, keeping NaN.
- Scores:
- support_score4 = benefits + care_options + seek_help + anonymity
- support_score5 = support_score4 + wellness_program
- Outcome: treat_bin from treatment (yes/no).
- Country cleanup: synonym normalize (e.g., “United States of America” → “United States”).

⸻

## 📈 Visuals you’ll see

- Support → Treatment point plots with 95% CI (overall + faceted by gender/age/company size)
- Stacked/100% bars for policy visibility (Yes/No/Don’t know)
- Heatmaps for stigma vs. leave comfort (counts and row %)
- Waffle / Treemap snapshots for quick composition views
- Wordcloud (gender string cleaning demo)

⸻

## 🧪 Methods Cheat-Sheet

- Welch’s t-test: Compare treatment rate across two groups (e.g., Never/Rarely vs Sometimes/Often in work_interfere_cat_2).
- Pairwise tests: across support groups (interpret with multiple-comparisons caution).
- Logistic regression: treat_bin ~ support_score + gender + age (+ optional controls).
- Ablation (leave-one-out): Recompute support score 5×, each time dropping one policy; compare slope/marginal effect on treat_bin to quantify each policy’s contribution to the bundle.

⸻

## 📎 Files

| File | Description |
|---|---|
| `mental_health_in_tech_EDA.ipynb` | Full pipeline: discovery → harmonization → EDA → models → figures |
| `osmi_wide_2014_2023.csv` | dataset generated by the notebook (Raw data not included) |
| `Download` | Harmonized, Download OSMI CSVs and save locally as noted above |

⸻

## 👨🏻‍💻 Author

Ozkan Gelincik — Data Scientist
🔗 LinkedIn: https://www.linkedin.com/in/ozkangelincik


⸻


## 📝 Notes & Ethics

- This is EDA, not causal inference. Treat relationships as directional, not definitive.
- Please cite OSMI when using these data; follow their usage guidelines.
- Contributions welcome—PRs for new visuals, models, or improved harmonization!



