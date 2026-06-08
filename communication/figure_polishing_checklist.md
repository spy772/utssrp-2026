# Figure-polishing checklist

*Run every poster/talk figure through this before it goes up. Two minutes per figure now saves you a
grade-band on "honest, well-labeled figures." A good figure is readable from across the room and tells
its story without you standing next to it.*

> **No new notebook.** Every final figure comes from code you already wrote in NB1–NB4. Re-run the cell
> that made it (or regenerate from the saved `outputs/` arrays), tidy it with the points below, and
> `savefig("name")` into `figures/`. You're polishing existing plots, not building new analysis.

## The checklist

- [ ] **Axis labels + units on both axes.** "Teff [K]", "log g [dex]", "[Fe/H] [dex]", "empirical
      coverage". No bare "x"/"y", no unlabeled axes. (Coverage axes go 0–1 or 0–100%.)
- [ ] **Readable fonts.** Title and labels legible from ~2 m away — bump sizes up for the poster; on a
      laptop they'll look too big, which is correct. Tick labels not microscopic.
- [ ] **Colorblind-safe colors.** Use the shared `viridis` / project palette from `set_plot_style()`;
      **avoid red–green contrasts** and the default rainbow. If color carries meaning, add a colorbar
      (labeled!) or a legend; don't rely on color alone to distinguish two lines.
- [ ] **One-line takeaway caption.** Say what the reader should *conclude*, not what the plot *is*.
      Good: "Coverage sags for metal-poor stars — the band is too narrow there." Bad: "Coverage vs
      [Fe/H]." (You wrote these as the markdown takeaways under each notebook figure — reuse them.)
- [ ] **Consistent style via the shared helpers.** Call `set_plot_style()` and use the canonical
      plotters (`plot_pred_vs_true`, `plot_residuals`, `plot_reliability`, `compare_intervals`) so all
      three teammates' figures match — same fonts, colors, and look across the poster.
- [ ] **Reference lines where they help meaning.** The 1:1 line on predicted-vs-true; the dashed
      **target = 90%** line on coverage plots. These let a reader judge "good/bad" at a glance.
- [ ] **Coverage never shown without width.** If a figure or panel makes a coverage claim, the
      sharpness (median width) is shown alongside it (e.g. the two-panel `compare_intervals`), so a
      vacuously wide interval can't masquerade as a win.
- [ ] **Save at good resolution.** Use `savefig(...)` (the helper saves to `figures/` at poster-friendly
      dpi); for print aim ~300 dpi. Check the saved PNG/PDF isn't blurry or pixelated when enlarged.
- [ ] **No chartjunk.** Drop 3-D effects, heavy gridlines, redundant legends, decorative backgrounds, and
      anything that doesn't carry information. White space is your friend.
- [ ] **Honest axes.** Don't crop or zoom to exaggerate a result; start counts/coverage axes sensibly
      (coverage at 0 or just below the lowest bin, not at 0.85 to make a small dip look like a cliff).
- [ ] **Sanity-check the numbers.** The figure agrees with what you say in text (qualitatively — "about
      0.2 dex", "near 90%"); no leftover placeholder titles or a different model's data.

## Quick reference — which figure for which poster claim
| Claim | Figure | Helper |
|---|---|---|
| "The model works" | predicted-vs-true [Fe/H] with the 1:1 line | `plot_pred_vs_true` |
| "Where it does well / poorly" | residuals across Teff or log g | `plot_residuals` |
| "Our error bars are right on average" | coverage number / reliability vs target | `empirical_coverage`, `plot_reliability` |
| "Conformal pins the 90% guarantee the native band drifts off" | coverage + median-width side by side | `compare_intervals` |
| "Right on average, wrong in places" | coverage binned by log g or [Fe/H] | `plot_reliability(feature=...)` |
| "What kinds of stars are these" | Kiel diagram colored by [Fe/H] | (from NB1) |
