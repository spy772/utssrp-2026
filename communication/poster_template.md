# Poster template

*A section-by-section skeleton for your final poster. Fill each box with your own words and your
own figures — the prompts under each heading are scaffolding, not text to copy.*

**How to use this.** Your poster tells one honest story: *we predicted a star's metallicity from
its Gaia spectrum, we put error bars on it, and we found where those error bars are trustworthy
and where they quietly fail.* Six sections carry that story: **Question · Data · Method · Results ·
Limitations · Next steps**. Aim for short sentences, big readable figures, and one clear takeaway
per panel. A reader should get the gist in 60 seconds from the figures and headings alone.

> **No new notebook for the poster.** Every figure you put up was already made in NB1–NB4 and saved
> to `figures/` (via `savefig(...)`), or can be regenerated from the saved `outputs/` arrays using
> the *exact code you already wrote*. Do not build a new analysis for the poster — pull from what
> you have, polish it (see `figure_polishing_checklist.md`), and arrange it. If a figure isn't in
> `figures/` yet, re-run the cell that made it and add a `savefig("name")` line.

---

## 1 · Question  *(why should anyone care?)*

**Goal:** in 2–3 sentences, say what you set out to do and why it matters.

Prompts:
- What are you predicting? (Metallicity, **[Fe/H]** — how iron-rich a star is, in dex — from its
  Gaia XP spectrum, the 110 numbers that encode the star's light.)
- Why is this worth doing? (The **label-transfer** idea: precise labels are scarce and expensive;
  Gaia spectra exist for 220M+ stars. Learn the map on the overlap, apply it everywhere → map the
  chemistry of the Milky Way at scale.)
- What is the *uncertainty* angle, in one line? (A prediction without an honest error bar isn't
  science — we want every [Fe/H] to come with an interval we can trust.)

Keep it jargon-light here; this is the hook a passer-by reads first.

## 2 · Data  *(what did you learn from / predict on?)*

**Goal:** introduce the inputs, the labels, and where they came from — concretely, with a picture.

Prompts:
- **Inputs:** Gaia BP/RP "XP" spectra, compressed to **110 coefficients** (55 blue + 55 red). Low
  resolution, but cheap and all-sky.
- **Labels (the answers):** **APOGEE** high-resolution measurements of temperature (Teff), surface
  gravity (log g), and metallicity ([Fe/H]). The **Laroche & Speagle (2025)** cross-match pairs each
  spectrum with these labels — that pairing is our training data.
- **One EDA figure here.** Use one you already made in NB1: an example reconstructed spectrum
  (hotter vs cooler), or the **Kiel diagram** (log g vs Teff, colored by [Fe/H]) — the latter doubles
  as a one-glance "what kinds of stars are in our data" map.
- One sentence on the train / calibration / test split and *why* you held out a calibration set
  (you'll need it for the uncertainty step).

## 3 · Method  *(how does it work?)*

**Goal:** explain the pipeline at a level a smart non-expert follows. A small diagram beats a
paragraph.

Prompts:
- The pipeline in one arrow: **110 coefficients → normalize by brightness → standardize → model →
  predicted [Fe/H]**.
- Your model, named plainly: **KNN** ("predict from the most similar stars"), **Random Forest**
  ("a vote of many decision trees"), or **Neural network** ("a flexible function fit by gradient
  descent") — whichever you own. Note the **linear regression baseline** everyone compares against.
- One line on how you got an **interval**, not just a point: the shared **constant-σ** interval, plus
  your model's **native** uncertainty (neighbour scatter / tree spread / ensemble spread), then
  **split conformal** for a coverage guarantee.
- Define the one piece of UQ vocabulary you'll use in Results: **coverage** ("a 90% interval should
  contain the truth ~90% of the time").

## 4 · Results  *(what did you find?)*

**Goal:** show that the model works, then show how honest its error bars are. This is your biggest,
most visual section — let the figures lead, with a one-line takeaway under each.

Use the three figures you already saved:
- **Predicted-vs-true [Fe/H]** (`plot_pred_vs_true`, from NB2). Takeaway: points hug the 1:1 line;
  quote your RMSE qualitatively ("about 0.2 dex") and note your model beats the linear baseline.
- **Coverage / reliability** (`plot_reliability` or the `empirical_coverage` number, from NB3/NB4).
  Takeaway: the constant interval is ~90% **on average (marginal coverage)**, and **split conformal
  pins that average guarantee** that your model's native band drifts off (the native band adapts its
  width per star, but its overall coverage misses the target — show this with the `compare_intervals`
  coverage-vs-width panel).
- **Conditional coverage** (`plot_reliability` sliced by **log g** and/or **[Fe/H]**, from NB4 §5).
  Takeaway: coverage is **not** flat across the plane — it sags in one corner. *This figure sets up
  your Limitations section; don't bury it.*

Reporting tips:
- Quote numbers **qualitatively** ("around 0.2 dex", "close to 90%") — your exact value depends on
  whether you ran the mock or the real data. Let the figure carry the precision.
- Pair every coverage claim with a **width**: "90% coverage at a typical width of ~X dex." Coverage
  alone can be gamed by a uselessly wide band.

## 5 · Limitations  *(where does it break? — the part graders look for)*

**Goal:** the honest heart of the poster. State plainly that your intervals are *right on average
but wrong in places*, and name where.

The story (make it yours, but this is the spine):
- Split conformal gives a real, distribution-free **marginal** guarantee — right *on average* across
  all stars.
- But "right on average" can hide "wrong for a subgroup." Binning coverage by **log g** and **[Fe/H]**
  reveals a **conditional gap**: the intervals **under-cover metal-poor stars and luminous (low-log g)
  giants**, and **over-cover metal-rich dwarfs**. (Physical reason: [Fe/H] leaves only faint marks on
  these spectra — weakest for metal-poor stars, and giants sit in the sparse, most-extrapolated corner
  of the data. Less information → bigger, more variable errors that one fixed-width band can't track.)
- **The punchline:** split conformal fixed the *marginal* coverage but the *conditional* gap
  **persists** — a single global interval is honest about the average and quietly optimistic for the
  hard stars. **"Right on average, wrong in places."**
- Frame this as a **discovery, not a failure**. You found the limitation, you measured it, and you
  can show it. That is exactly the scientific honesty the project is about.
- One sentence acknowledging the data: results are shown on the mock / real cross-match; APOGEE labels
  themselves carry uncertainty (the irreducible floor).

## 6 · Next steps  *(what would you do with more time?)*

**Goal:** show you know how to fix what you found. Keep it to 2–3 concrete, credible ideas.

Prompts (pick from these — the first two directly target your conditional gap):
- **Group-conditional ("Mondrian") conformal:** run the conformal recipe *separately* in each log g (or
  [Fe/H]) bin so each region gets its own interval width — directly closes the gap you measured.
- **Conformalized Quantile Regression (CQR):** start from a model that predicts a low and high quantile
  so the band *widens automatically* where the data are noisy, then conformalize the edges.
- **More / better data in the sparse corner** (more metal-poor stars, more giants) to shrink the
  *epistemic* part of the error.
- **Predict Teff and log g too**, with their own intervals, for a full label set.

---

## Layout & polish reminders
- Reading order should be obvious (numbered sections, top-left to bottom-right, or clear columns).
- Figures do the talking; text is captions and connective tissue. Every figure gets a **one-line
  takeaway** caption.
- Run every figure through `figure_polishing_checklist.md` before you print.
- Title states the result, not just the topic (e.g. "Trustworthy metallicities from Gaia spectra —
  and where the trust runs out"), with all three names + the program/date.
