# Grading rubric — poster & lightning talk

*What the graders look for, and what each level looks like in practice. Use this as a checklist while
you build, not just after. The five criteria come straight from the kickoff deck (S4): clear framing ·
correct understanding of the methods · honest, well-labeled figures · an honest account of uncertainty
and limitations · clear delivery.*

> The single thing that most separates an excellent project from an adequate one in this program is
> **honesty about limitations** — naming where your method breaks and showing it. A polished poster
> that claims everything worked perfectly scores *below* a plainer one that says "right on average,
> wrong for metal-poor stars and giants — here's the plot."

| Criterion | Excellent | Adequate | Needs work |
|---|---|---|---|
| **Clear framing** — is the question and its importance obvious? | Opens with a crisp question and *why it matters* (label transfer at scale) in a sentence a non-expert gets. The whole poster/talk follows one storyline; a 60-second skim lands the point. | The question and motivation are stated and findable, but take some hunting, or the "why it matters" is thin. The thread mostly holds. | No clear question, or the goal is buried in jargon. Reader can't tell what you predicted or why. |
| **Correct understanding of the methods** — do you actually get what you ran? | Explains the pipeline (110 coeffs → preprocess → model → predict), your model in plain words, and conformal as a *distribution-free, marginal* guarantee — correctly, with jargon defined on first use. Can answer "why a calibration set?" and "what does coverage mean?" | Methods are described correctly at a high level; minor imprecision or one undefined term, but no real misconception. | A core idea is wrong (e.g. "conformal assumes Gaussian errors," "90% coverage means 90% chance for this star," scaler fit on the test set), or the method is a black box you can't explain. |
| **Honest, well-labeled figures** — do the plots stand on their own? | Every figure has axis labels + units, readable fonts, colorblind-safe colors, and a one-line takeaway caption. Plots are the right ones for the claim (pred-vs-true, coverage, conditional coverage) and are not misleading. Passes the figure-polishing checklist. | Figures are mostly clean and labeled; a couple of small lapses (a missing unit, a tiny font, a caption that restates the title instead of the takeaway). | Unlabeled axes, illegible text, default rainbow/red-green colors, chartjunk, or a figure that doesn't support the claim it sits under. |
| **Honest account of uncertainty & limitations** — *the heart of the project* | Reports coverage **with width**, distinguishes **marginal vs conditional** coverage, and shows the **conditional gap** (under-covers metal-poor stars & luminous giants, over-covers dwarfs) as a measured, plotted discovery. States *why* it happens and a credible fix (group-conditional / CQR). "Right on average, wrong in places," owned proudly. | Acknowledges uncertainty and at least one real limitation, with a coverage number; but the conditional gap is mentioned without being shown, or width/sharpness is omitted, or the "why/fix" is hand-wavy. | Reports only a point prediction or only a single coverage number; claims the method "works" with no limitations; hides or omits where it breaks. |
| **Clear delivery** — poster legible, talk lands | Poster has obvious reading order and breathing room; talk is one idea, well-timed, rehearsed, with a clean handoff to the poster. Speakers know their parts; figures (not bullet text) carry the message. | Poster is readable and the talk gets the point across, but runs long/short, is a little cramped, or the handoffs are rough. | Wall of text, no clear order, talk over time or read off slides, or no clear takeaway by the end. |

## How to use this before the showcase
- **Self-grade once mid-build** (Wed Jun 17) and once after polish (Thu Jun 18). Find your weakest row
  and spend your next hour there.
- The two rows most teams under-invest in are **honest figures** and **honest limitations** — and they
  are weighted heavily here. Budget time for them deliberately.
- For figures, run `figure_polishing_checklist.md`. For limitations, make sure your conditional-coverage
  plot is on the poster and you can say in one sentence where coverage breaks and why.
