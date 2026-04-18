# Split Decision

**Play it: <https://craftkeg.github.io/split-decision/>**

A single-page browser game that teaches the difference between **fixed-time** and **anytime-valid** statistical inference — staged inside the November 2024 Tyson vs. Paul broadcast at AT&T Stadium.

You are the PM on call for a Netflix-scale launch. Eight rounds, ten seconds each, seven release cells. One cell has a real regression hiding in the noise. Find it before the bell. Pick wrong and you ship a false alarm. Pick nothing and the broken cell ships live.

Pick a corner before the opening bell:

- **Red Corner — Fixed-Time.** Sharp Wilson/Newcombe intervals. Every peek is another shot at being fooled by noise.
- **Blue Corner — Anytime-Valid.** Confidence sequences. Wider bands, but they stay valid every time you look.

## Play

Hosted: <https://craftkeg.github.io/split-decision/>

Or open `index.html` directly in any modern browser. No build step, no dependencies — one HTML file with inline CSS/JS.

## How scoring works

| Outcome | Score | Counter |
| --- | --- | --- |
| Click the real broken cell | +1 | "Caught" |
| Click any other cell | −1 | "FA" (false alarm) |
| Don't pick before the bell | −1 | "Miss" (timeline shows a grey dot) |

The pill labelled `R x/8` shows your round, with `FIXED-TIME` or `ANYTIME-VALID` next to it indicating which corner you chose.

## The statistics

Both corners compare two arms of streaming traffic per cell at α = 0.10.

**Fixed-time corner** uses the Newcombe hybrid-Wilson interval for a difference of proportions:

```
CI = [pT_lo − pC_hi, pT_hi − pC_lo]
```

where `pT_*` and `pC_*` are 90% Wilson bounds on each arm.

**Anytime-valid corner** uses the asymptotic e-process radius from Lindon (2022):

```
z_AV(g, n, α) = √( (g + n) / n · ln( (g + n) / (g · α²) ) )

CI = (p̂T − p̂C) ± z_AV · √( p̂T(1−p̂T)/nT + p̂C(1−p̂C)/nC )
```

with `g = 1`, `n = nT + nC`. This is a confidence sequence: it is valid at every sample size simultaneously, not just at a pre-specified one. The cost is wider intervals (≈2× at typical sample sizes), the benefit is that "peeking" no longer inflates the false-positive rate.

Both methods are computed live every frame so you can watch the intervals contract as the round plays out.

## Credits

- **Travis Brooks** — for the framing that turned this into a PM monitoring a live event rather than an abstract A/B exercise.
- **Matthew Wardrop** and **Michael Lindon** — for feedback that shaped the gameplay and statistical framing.

The anytime-valid radius is a JS port of `z_radius` from the [`avlm` R package](https://github.com/michaellindon/avlm) by **Michael Lindon**, which implements the methodology from:

> Lindon, M. (2022). *Anytime-Valid Linear Regression.* arXiv:2210.08589 — <https://arxiv.org/abs/2210.08589>

`avlm` is MIT-licensed.

Commentary lines are stylised dramatisations of the actual Netflix broadcast (Mauro Ranallo, Andre Ward, Rosie Perez, Jessica McCaskill).

## License

MIT — see [LICENSE](LICENSE).
