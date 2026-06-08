# UTSSRP 2026 — Start here

Welcome! Over the next two weeks (June 8–19, 2026) you'll build machine-learning models that read a
star's *Gaia* spectrum — really just **110 numbers** — and predict its physical properties:
temperature (`Teff`), surface gravity (`log g`), and chemical composition (`[Fe/H]`, the
metallicity). The catch, and the heart of the project, is **honesty**: every prediction should come
with a *calibrated uncertainty* — an interval that contains the truth as often as it claims to. You'll
start by getting to know the data, build a prediction pipeline, turn point predictions into intervals,
and finish with **conformal prediction**, a method that gives intervals a coverage guarantee. Along the
way you'll discover where those guarantees quietly break — which is exactly the kind of honest result
that makes good science.

---

## Quick start

The lab machines already have everything installed, so this is fast. If your TA has set up the
environment for you, you can skip straight to step 3.

1. **Create the conda environment** (one time, on a fresh machine):
   ```bash
   conda env create -f environment.yml
   ```

2. **Activate it and register the Jupyter kernel:**
   ```bash
   conda activate utssrp2026
   python -m ipykernel install --user --name utssrp2026 --display-name "Python (utssrp2026)"
   ```

3. **Launch JupyterLab** from the project folder:
   ```bash
   conda activate utssrp2026
   jupyter lab
   ```

4. **Open `01_explore_the_data.ipynb`** and pick the **Python (utssrp2026)** kernel (top-right of the
   notebook).

5. **Run the first cell** — titled *"Setup — run this first (you don't need to read it closely)"* —
   before anything else. It loads the data and defines the helper functions the rest of the notebook
   uses. Then run the notebook top to bottom.

**It works immediately.** The notebooks ship with a bundled synthetic ("mock") dataset
(`data/xp_apogee_mock.npz`, already in the repo), so everything runs on a fresh clone with no
downloads. The mock is tuned to behave like the real data, so the science story is the same either
way. Getting the real data (next section) is optional but recommended.

---

## Get the real data (optional but recommended)

The mock data is great for learning the moves; the **real** *Gaia* × APOGEE cross-match makes your
plots and results genuinely scientific. The raw archive is **~2.5 GB**, so it is **not** stored in
GitHub — you download it once. When the curated file is present, `load_data()` **prefers it
automatically** and no notebook edits are needed.

**Option A — your TA hands you the file.** Easiest path: ask your TA for `xp_apogee_real.npz`
(~120 MB) and drop it into `data/`. Done — re-run any notebook.

**Where the file comes from (provenance).** The curated `xp_apogee_real.npz` is built from the Zenodo
record (<https://doi.org/10.5281/zenodo.14041773>, *"Closing the stellar labels gap,"* Laroche & Speagle
2025) by applying the paper's **exact quality cuts** (Teff/σ > 30, σ(log g) < 0.4, σ([M/H]) < 0.2,
0 < BP−RP < 4, 6 < G < 17.5, clean STARFLAG/ASPCAPFLAG) → the **full ~263k high-quality set** → a single
`.npz` (~120 MB, same format as the mock). The preparation script is part of the **instructor toolkit**
(not shipped in this student repo, since it's tied to the solutions) — your TA runs it and hands you the
result. Once `data/xp_apogee_real.npz` is present, `load_data()` uses it automatically.

**Working set & scaling up.** To keep in-class fits fast, `load_data()` returns a deterministic
**30,000-star subsample by default** (so everyone trains on the same stars and the notebook chain stays
aligned). Pass `load_data(n=100_000)` — or `load_data(n=None)` for **all** ~263k — to scale up; NB2's
last section turns this into a "does more data help?" experiment, and NB4 ties it to the real research
goal (label transfer to the 220M stars with XP spectra but no labels).

**Notes**
- **GaiaXPy is optional.** It's the official tool for reconstructing real *Gaia* spectra and is
  included in the environment, but the notebooks reconstruct example spectra from a stored basis and
  do **not** require it at runtime.
- `data/raw_zenodo/` and the raw downloads are **git-ignored** — they won't be committed, and they
  stay on your machine only.

---

## What's in this folder

```
utssrp-2026/
├── README.md                          ← you are here
├── reference_and_glossary.md          key terms (one line each) + curated links
├── 01_explore_the_data.ipynb          NB1 — explore the data (everyone)
├── 02_pipeline_common.ipynb           NB2 — build the baseline pipeline (everyone)
├── 02a_tune_knn.ipynb                 NB2 — your tuning lab · A = K-nearest neighbors
├── 02b_tune_random_forest.ipynb       NB2 — your tuning lab · B = random forest
├── 02c_tune_neural_net.ipynb          NB2 — your tuning lab · C = neural network
├── 03_uncertainty_common.ipynb        NB3 — quantify uncertainty (everyone)
├── 03a_uncertainty_knn.ipynb          NB3 — your UQ lab · A = K-nearest neighbors
├── 03b_uncertainty_random_forest.ipynb NB3 — your UQ lab · B = random forest
├── 03c_uncertainty_neural_net.ipynb   NB3 — your UQ lab · C = neural network
├── 04_conformal_prediction.ipynb      NB4 — intervals with a guarantee (everyone)
├── data/                              the datasets (mock shipped; real goes here)
├── models/                            your saved fitted models (experiment tracking)
├── outputs/                           saved predictions, intervals & experiment logs (passed between notebooks)
├── figures/                           saved plots, ready to reuse on your poster
├── communication/                     poster template, talk skeleton, rubric, figure checklist
└── environment.yml                    the conda environment
```

---

## The 4 notebooks & the schedule

The daily research block runs **2:30–5:30 PM**. Mornings are short courses and seminars; the table
below is the afternoon project work. A ★ marks a new topic Josh introduces in the room.

| Date | Afternoon focus | Notebook |
|------|-----------------|----------|
| Mon Jun 8 ★ | Data & EDA — "become one with the data" | **NB1** `01_explore_the_data` |
| Tue Jun 9 | Debrief the EDA + your first regression | NB1 → **NB2** |
| Wed Jun 10 ★ | Build the prediction pipeline (everyone) | **NB2** `02_pipeline_common` |
| Thu Jun 11 | Tune *your* model (each student their own) | **NB2** `02a/02b/02c_tune_…` |
| Fri Jun 12 ★ | Uncertainty quantification (common, then your model) | **NB3** `03_uncertainty_common` + `03a/03b/03c` |
| Mon Jun 15 ★ | Conformal prediction | **NB4** `04_conformal_prediction` |
| Tue Jun 16 | Your model + comparison (cross-check coverage) | NB4 |
| Wed Jun 17 | Shore up results + build poster/talk | `communication/` |
| Thu Jun 18 ★ | Feedback & polish | `communication/` |
| Fri Jun 19 | **Lightning talks + posters** (final-day reception) | — |

The poster tells the full story (question · data · method · results · **limitations** · next steps);
the lightning talk gives the question and the headline result and points people to the poster.
*Lightning-talk length is currently assumed to be **~3 minutes (3–4 slides)** — **confirm with your TA.***

---

## Who does what

You explore the data individually, then each own one model, then converge to cross-check and build a
shared set of figures. The model you pick in NB2 carries through NB3 and NB4.

| Student | Model (NB2–NB4) | EDA plots they lead in NB1 |
|---------|-----------------|----------------------------|
| **A** | K-Nearest Neighbors (KNN) | example spectra + signal-to-noise |
| **B** | Random Forest | coefficient scales + correlation heatmap |
| **C** | Neural network (MLP) | label corner plot + Kiel diagram |

**Linear regression** is the shared baseline in every worked example — everyone builds it, and you
compare your own model against it. In NB1, each person leads their assigned plots, skims the rest, and
the team shares everything at the debrief.

---

## How each notebook works

Every concept follows the same four-part rhythm so you're never stuck and never just copying.
**Explain** is a short plain-language intro with a reference link; **Worked example** is complete,
runnable code you run and watch work; **Your turn** is a guided task that adapts the worked example
(it runs as-shipped with a sensible default — your job is to change and extend it, never to fill a
blank page); **Explore** is one to three open prompts with no single right answer. Notebooks save key
predictions, intervals, and figures to `outputs/` and `figures/` so later notebooks (and your poster)
reuse them without re-running everything.

---

## If a cell errors

**Grab the TA.** The notebooks **never** `pip install` anything — the environment is already set up. If
a cell reports a missing package or won't run, it's a setup issue, not something to fix in the
notebook. Ask a TA or the doctoral mentor; they're around every afternoon.

---

## Links

- **The paper** — Laroche & Speagle 2025, *"Closing the stellar labels gap"* (arXiv):
  <https://arxiv.org/abs/2404.07316> · published version (ApJ 979, 5):
  <https://ui.adsabs.harvard.edu/abs/2025ApJ...979....5L>
- **The dataset** — Zenodo record (DOI 10.5281/zenodo.14041773):
  <https://doi.org/10.5281/zenodo.14041773>
- **scikit-learn** — the ML library we use:
  <https://scikit-learn.org/stable/> · Pipelines:
  <https://scikit-learn.org/stable/modules/compose.html#pipeline>
- **GaiaXPy** — official *Gaia* XP spectrum reconstruction (optional, real-data path):
  <https://gaiaxpy.readthedocs.io/en/latest/usage.html>
- **Conformal prediction primer** — Angelopoulos & Bates, *A Gentle Introduction to Conformal
  Prediction* (arXiv:2107.07511): <https://arxiv.org/abs/2107.07511>

For one-line definitions of every term (supervised learning, calibration, coverage, aleatoric vs
epistemic, marginal vs conditional, and more), see **`reference_and_glossary.md`**.
