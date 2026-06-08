# Lightning-talk skeleton

*A one-idea talk. Its only job: make the room curious enough to walk over to your poster.*

**Duration: ~3 minutes, 3–4 slides. (Confirm the exact time with your TA — this number is an
assumption.)** If it turns out to be 5 minutes, add one slide of detail; if 2, cut the method slide.

## The one rule
A lightning talk is **not** a mini-poster. You cannot fit your whole project into 3 minutes, so
don't try. Pick **one idea** — your single most interesting finding — and build the talk around it.
Everything else lives on the poster. The structure is a funnel:

> **Hook → Question → One headline result → "see our poster."**

For this project, the natural headline is the honest one: *our error bars are right on average but
wrong for the hard stars.* That is memorable and it is true. (You may instead headline "we can read a
star's metallicity from 110 numbers" if your team prefers — but the limitation is the stronger hook.)

---

## Slide-by-slide

### Slide 1 — Hook + Question  *(~45 sec)*
- **Hook (one sentence):** something that makes a tired afternoon audience look up. E.g. "Gaia gave us
  cheap, blurry spectra for 220 million stars — can we read a star's chemistry out of just 110
  numbers?" or "We built a model that's 90% right... and we're here to tell you where that 90% is a
  lie."
- **Question (one sentence):** state exactly what you predicted and the twist — *can we predict
  metallicity [Fe/H] from a Gaia spectrum, AND know when to trust the answer?*
- Title slide can double as this: project title, all three names, the program + date.
- *Spoken, not read:* don't fill the slide with text; a title and one image is plenty.

### Slide 2 — One headline result  *(~75 sec — the heart of the talk)*
- **Show ONE figure, big.** The strongest single image is your **conditional-coverage plot** (coverage
  vs log g or [Fe/H], with the 90% target line) — or, if you'd rather lead with success, the
  **predicted-vs-true** scatter.
- Say the result in plain words: "We get metallicities good to about 0.2 dex. We can even guarantee our
  error bars are right **on average** — 90% of the time the truth lands inside. **But** when we split
  the stars by type, the guarantee breaks: we **under-cover metal-poor stars and giants** and
  over-cover easy dwarfs. **Right on average, wrong in places.**"
- One number, one picture, one sentence of meaning. Resist adding more.

### Slide 3 — Why it matters + "see our poster"  *(~45 sec)*
- One line on *why the gap happens* (these stars carry less chemical information, so their errors are
  bigger and a one-size band can't keep up) and one line on the *fix* (group-conditional / adaptive
  conformal — on the poster).
- **Explicit handoff:** "The full story — the pipeline, all three models, and how we'd close the gap —
  is on our poster, number ___. Come find us." Point them there literally.

### (Optional) Slide 4 — only if you have time/slides to spare
- A single extra figure (e.g. the Kiel diagram, or the conformal-vs-native width comparison) *or* a
  one-line "what's next." Cut this first if you're over time.

---

## Timing & delivery
- **Rehearse with a timer.** 3 minutes is shorter than it feels; most first drafts run 5. Time it twice.
- **~45 sec per slide** is the rule of thumb for a 3-slide, 3-minute talk — leave a few seconds of slack.
- One idea per slide; speak *to* the figure, don't read bullet text aloud.
- Decide who says what (you have three speakers — e.g. one per slide, or one narrator) and practice the
  handoffs.
- End on a clean sentence and a clear pointer to the poster — that is the whole point of the talk.
- It's fine to *not* explain conformal prediction in detail; "a method that guarantees our error bars
  are right on average" is enough for 3 minutes. Depth belongs on the poster.
