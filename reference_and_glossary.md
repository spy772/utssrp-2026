# Reference & Glossary

*A pocket dictionary for the UTSSRP 2026 project. Every term gets one plain sentence — enough to
follow the notebooks, not a textbook. Skim it now, then come back whenever a word in a notebook,
on the poster, or at a debrief makes you pause.*

**How to use this.** The terms are split into **ML & UQ terms** and **Astronomy terms**, each in
roughly the order you meet them across the notebooks (explore → build the pipeline → quantify
uncertainty → conformal prediction). The
**Curated links** at the bottom are the few sources worth actually opening. If a definition here
and a notebook ever disagree, trust the notebook and tell your TA.

---

## ML & UQ terms

**Supervised learning** — teaching a model from examples where you already know the right answer
(inputs paired with their true labels), so it can answer for new inputs it has never seen.

**Regression** — supervised learning where the answer is a *continuous number* (like [Fe/H] in dex)
rather than a category; our whole project is regression.

**Feature** — one of the input numbers the model reads (here, each of the 110 Gaia XP coefficients
is a feature); the full feature vector for one star is a row of `X`.

**Label (target)** — the true answer we want to predict (here, primarily [Fe/H]); stored in `y` and
known only for the training stars.

**Model** — the rule that turns a star's 110 features into a predicted label; *fitting* (or
*training*) it means tuning that rule on examples with known answers.

**Train / calibration / test split** — three non-overlapping slices of the data: **train** fits the
model, **calibration** is held out to size and check uncertainty (this is what conformal prediction
needs), and **test** stays untouched until the very end for one honest score.

**Generalization** — how well a model does on new stars it never trained on, which is the only
performance that actually counts.

**Overfitting** — when a model memorizes quirks and noise in the training data and so predicts worse
on new stars; the opposite, **underfitting**, is being too rigid to capture the real signal.

**Hyperparameter** — a setting you choose *before* fitting (e.g. KNN's `n_neighbors`, RF's
`max_depth`, MLP's `alpha`), as opposed to the weights the model learns *during* fitting.

**Cross-validation** — picking a hyperparameter honestly by repeatedly fitting on parts of the
*training* data and scoring on the rest, never peeking at the test set (`GridSearchCV` automates it).

**(Data) leakage** — accidentally letting information from the test (or calibration) stars influence
training — e.g. computing scaling statistics on all the data at once — which inflates your score
dishonestly; a `Pipeline` prevents the most common form.

**Preprocessing** — cleaning and reshaping the raw inputs before the model sees them; our recipe is
two steps (normalize-by-G, then standardize).

**Normalize-by-G** — dividing each star's coefficients by its Gaia G-band brightness so we keep the
spectral *shape* (which encodes the physics) and discard overall brightness/distance.

**Standardize (z-score)** — rescaling each feature to mean 0 and standard deviation 1 so all 110
coefficients are on comparable scales, with the scaling statistics fit on the **training data only**.

**Pipeline** — a scikit-learn object that chains preprocessing and the model into one unit, so the
scaler is always fit on training data only and there is no leakage to track by hand.

**Baseline** — the simplest reasonable model (here **linear regression**) that every fancier model
must beat to justify its complexity.

**RMSE (root mean squared error)** — the typical size of a prediction error in the label's own units,
with big misses penalized extra; lower is better.

**MAE (mean absolute error)** — the average absolute prediction error, in the label's units; like
RMSE but less sensitive to a few large misses.

**Bias** — the *average signed* error (mean of predicted minus true); near zero means the model is
right on average, a nonzero value means it systematically over- or under-predicts.

**R² (coefficient of determination)** — the fraction of the label's variance the model explains,
where 1.0 is perfect and 0 is no better than always guessing the mean.

**Residual** — the leftover error for one star (predicted minus true, or true minus predicted); we
plot residuals across the Teff–log g plane to see *where* a model struggles, not just how much.

**Linear regression** — predicts the label as a weighted sum of the inputs; simple, fast,
interpretable, and our shared baseline (no hyperparameter to tune — that is the point).

**KNN (K-nearest neighbors)** — predicts a new star by averaging the labels of the *k* most similar
training stars ("you are like your closest neighbors"); **Student A's** model.

**Random forest (RF)** — averages the predictions of hundreds of decision trees, each grown on a
random slice of the data, for an accurate and stable predictor; **Student B's** model.

**Decision tree** — a flowchart of yes/no splits on feature values that lands each star in a leaf
with a predicted value; one tree alone overfits, which is why a forest averages many.

**Neural network (MLP, multilayer perceptron)** — a flexible function with internal "hidden" layers
of tunable weights, fit by gradient descent; the most expressive but fussiest model — **Student C's**.

**Ensemble** — combining several models (the trees in a forest, or several neural nets trained from
different random seeds) and using their average as the prediction and their spread as an uncertainty.

**Uncertainty** — the honest "plus or minus" attached to a prediction; turns a single number (a
*point estimate*) into a *range* (an *interval*).

**Aleatoric uncertainty** — irreducible noise in the data itself (photon noise; faint, uninformative
metal lines in metal-poor stars, where the [Fe/H] signal is genuinely weak) that *more of the same
data will not remove*.

**Epistemic uncertainty** — the model's own ignorance from limited or uneven training data; it is
largest where training examples are sparse and *shrinks as you collect more/better data*.

**Heteroscedastic** — when the noise level *changes across the data* (here, [Fe/H] error grows for
metal-poor stars and luminous giants), as opposed to *homoscedastic* (one constant noise level
everywhere).

**Native uncertainty** — an uncertainty read directly out of a model's own structure (KNN neighbor
scatter, RF tree spread, an MLP seed-ensemble spread) rather than from a separate calibration step.

**Prediction interval** — a range around a prediction (e.g. "[Fe/H] between −0.55 and −0.25") meant
to contain the true value at a stated **confidence level**, usually 90%.

**Calibration (of intervals)** — whether the promise holds: a method is *calibrated* when its
measured coverage matches the level it claims; **overconfident** intervals are too narrow (the
dangerous failure), **underconfident** ones are too wide and uselessly vague.

**Coverage** — the fraction of stars whose true label actually lands inside their interval; an honest
"90% interval" has empirical coverage near 90%, measured on held-out data.

**Marginal coverage** — coverage averaged over *all* stars (the 90%-overall number); what conformal
prediction guarantees.

**Conditional coverage** — coverage *within every subgroup* (e.g. for luminous giants specifically,
or metal-poor stars), which is what we really want — and which a single one-size-fits-all interval can
miss even while looking fine on average ("right on average, wrong in places").

**Reliability (diagram)** — a diagnostic plot of measured coverage versus the target level, usually
binned by a feature like Teff or log g, so a sagging bin instantly reveals where coverage breaks.

**Sharpness (width)** — how *narrow* the intervals are; among calibrated methods, narrower is better,
so the real goal is the *sharpest intervals that still cover*.

**Conformal prediction** — a recipe that turns *any* model's predictions into intervals with a
coverage guarantee, by measuring the model's actual misses on a held-out calibration set and sizing
the intervals from them — no assumption about the shape of the error distribution.

**Exchangeability** — the one assumption conformal needs: the calibration and test stars are
"interchangeable" (drawn from the same pool, in no special order), so a new star's error behaves like
just one more calibration error; it is *weaker* than assuming Gaussian noise.

**Nonconformity score** — a number measuring how badly the model missed on a calibration star (for us
the absolute residual, |true − predicted|); big score means a surprising, hard-to-fit point.

**Calibration quantile (q̂)** — the (slightly corrected) ⌈(n+1)(1−α)⌉/n-th quantile of the
calibration nonconformity scores; for a 90% interval it is roughly the 90th-percentile error, and the
conformal interval is simply prediction ± q̂ (note: q̂ is a half-width *in the label's units*, not a
probability).

**Distribution-free guarantee** — conformal's promise that coverage is *at least* the target level for
*any* error distribution, relying only on exchangeability, not on a bell-curve assumption; the
guarantee is **marginal** (on-average), which is exactly why a conditional gap can still hide inside it.

**Split (inductive) conformal** — the simplest conformal method we use: scores on the calibration
set → take the q̂ quantile → form constant-width intervals on the test set.

**CQR (Conformalized Quantile Regression)** — a stretch upgrade that starts from a model predicting a
low and a high quantile (so the raw band is already *wider where the data are noisier*), then
conformally adjusts those edges, giving intervals whose *width adapts* to each star.

**Mondrian (group-conditional) conformal** — a stretch upgrade that runs the whole conformal recipe
*separately inside each group* (e.g. a cool-star bin and a hot-star bin), so each region gets its own
q̂ and conditional coverage is restored where a single q̂ failed.

---

## Astronomy terms

**Spectrum** — a star's brightness plotted against wavelength (color); its smooth overall shape and
the dark absorption dips stamped on it encode the star's physical properties.

**Absorption line** — a narrow dip in the spectrum where atoms or molecules in the star's outer
layers soak up specific wavelengths; the pattern and depth of these dips fingerprint the star.

**Continuum** — the smooth, slowly-varying backbone of the spectrum (roughly a blackbody curve set by
temperature) underneath the absorption lines.

**Effective temperature (Teff)** — the surface temperature in kelvin (our stars roughly 3,000–6,500 K;
the Sun ≈ 5,772 K); it sets the spectrum's overall shape, making it the *easiest* of the three labels.

**Surface gravity (log g)** — the base-10 log of surface gravity in cgs units; it separates compact
**dwarfs** (high log g ≈ 4–4.6, like the Sun) from puffed-up **giants** (low log g ≈ 1–3), and is of
*medium* difficulty.

**Metallicity ([Fe/H])** — the log iron-to-hydrogen ratio relative to the Sun ([Fe/H] = 0 is solar,
−1 is one-tenth solar and typically old); it leaves only shallow marks on a low-res spectrum, making
it the *hardest* label and our primary target.

**dex** — one unit on a base-10 log scale, i.e. a factor of 10 (so [Fe/H] = −1 dex means 1/10 the
solar iron fraction); both log g and [Fe/H] are quoted in dex.

**Metals** — in astronomy, *every* element heavier than hydrogen and helium (not just literal metals).

**[α/M] (alpha abundance)** — the abundance of α-process elements (O, Mg, Si, Ca, Ti…) relative to
overall metal content; an age/origin tracer, available in our data as an *explore* label, not a
prediction target.

**Gaia BP/RP (XP) coefficients** — the model inputs: Gaia (ESA) compresses each low-resolution
spectrum from its Blue and Red Photometers ("XP" = "BP or RP") into **110 numbers** (55 BP + 55 RP),
the amplitudes of a fixed set of basis functions; blurry but cheap and available for 200M+ stars.

**APOGEE** — a high-resolution infrared spectroscopic survey (part of SDSS) whose ASPCAP pipeline
measures precise Teff, log g, and [Fe/H]; these are our training "ground-truth" labels (themselves
imperfect, which contributes to the aleatoric floor).

**Kiel diagram** — a plot of Teff (x, hot to the **left**) versus log g (y, dwarfs at the **bottom**),
colored by [Fe/H], on which stars fall onto structure — the main sequence, the giant branch, the red
clump — that you rebuild from the data in NB1.

**Hertzsprung–Russell (H–R) diagram** — the classic temperature-versus-luminosity version of the same
idea; the Kiel diagram swaps in *measured surface gravity* for luminosity, so you can draw it straight
from spectroscopic labels.

**Label transfer** — the project's motivation: learn the spectrum→label mapping on the ~200k stars
that have *both* a cheap Gaia XP spectrum and a precise APOGEE label, then apply it to the 200M+ stars
with XP spectra only — carrying scarce labels onto abundant data to map the Milky Way at scale.

**Signal-to-noise ratio (S/N or SNR)** — how strong the real signal is compared with the measurement
noise; high-S/N spectra are cleaner and easier to read, and our cross-match already keeps high-S/N
stars.

**GaiaXPy** — the official Gaia tool that reconstructs a sampled flux-versus-wavelength spectrum from
the 110 coefficients; needed *only* for the real-data path (the shipped notebooks use a mock and do
not require it — if a cell errors, ask your TA).

---

## Curated links

A short, vetted list — these are the ones worth opening. (Project paper and data first, then ML,
then UQ/conformal, then astronomy.)

### The project paper & data

- **Laroche & Speagle (2025), "Closing the stellar labels gap: Stellar label independent evidence for
  [α/M] information in Gaia BP/RP spectra"** (ApJ 979, 5) — the science behind our hard-region story.
  - arXiv preprint: https://arxiv.org/abs/2404.07316
  - Published version (ADS): https://ui.adsabs.harvard.edu/abs/2025ApJ...979....5L
  - Dataset (Zenodo, DOI 10.5281/zenodo.14041773): https://doi.org/10.5281/zenodo.14041773

### Machine learning (scikit-learn)

- **scikit-learn User Guide — Pipelines: chaining estimators** (why a Pipeline prevents leakage and
  the `step__param` naming): https://scikit-learn.org/stable/modules/compose.html#pipeline
- **KNeighborsRegressor — API reference** (Student A): https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsRegressor.html
- **RandomForestRegressor — API reference** (Student B): https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html
- **MLPRegressor — API reference** (Student C, the neural net): https://scikit-learn.org/stable/modules/generated/sklearn.neural_network.MLPRegressor.html
- **LinearRegression — API reference** (the shared baseline): https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html
- **GridSearchCV — tuning hyperparameters by cross-validation**: https://scikit-learn.org/stable/modules/grid_search.html#exhaustive-grid-search

### Uncertainty quantification & conformal prediction

- **A Gentle Introduction to Conformal Prediction and Distribution-Free Uncertainty Quantification** —
  Angelopoulos & Bates (the project's main UQ/conformal primer; defines coverage, the exact
  ⌈(n+1)(1−α)⌉ quantile, and marginal vs conditional coverage): https://arxiv.org/abs/2107.07511
- **Aleatoric and Epistemic Uncertainty in Machine Learning: An Introduction to Concepts and Methods**
  — Hüllermeier & Waegeman (the two flavors of uncertainty, in depth): https://arxiv.org/abs/1910.09457
- **Calibration (statistics) — Wikipedia** (plain-language "of the events called 90% likely, 90%
  actually occur" framing): https://en.wikipedia.org/wiki/Calibration_(statistics)
- **scikit-learn User Guide — 1.16. Probability calibration** (reliability diagrams and the
  over/under-confident vocabulary; written for classifiers, so use it for the *concept*):
  https://scikit-learn.org/stable/modules/calibration.html
- **Conformalized Quantile Regression (CQR)** — Romano, Patterson & Candès (the original adaptive-width
  method, a NB4 stretch): https://arxiv.org/abs/1905.03222
- **Awesome Conformal Prediction** — a curated, maintained hub of conformal tutorials, papers, and
  software (including Mondrian/group-conditional methods) for going deeper:
  https://github.com/valeman/awesome-conformal-prediction

### Astronomy & the data

- **Gaia BP/RP (XP) spectra — ESA/Gaia overview** (the 55+55 coefficient representation):
  https://www.cosmos.esa.int/web/gaia/iow_20220131
- **GaiaXPy documentation** (reconstruct/calibrate real spectra from the coefficients — real-data path
  only): https://gaiaxpy.readthedocs.io/en/latest/usage.html
- **APOGEE / ASPCAP (high-resolution labels, SDSS DR17)**: https://www.sdss4.org/dr17/irspec/  ·
  ASPCAP pipeline: https://www.sdss4.org/dr17/irspec/aspcap/
- **Hertzsprung–Russell diagram — Wikipedia** (background for the Kiel diagram):
  https://en.wikipedia.org/wiki/Hertzsprung%E2%80%93Russell_diagram

---

*If anything here is unclear, that is a great question for your doctoral mentor or TA — asking is part
of the research, not a sign you are behind.*
</content>
</invoke>
