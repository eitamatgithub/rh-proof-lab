# Riemann Hypothesis (or Riemann-Alpöge Theorem) Proof Lab

On 20 Nov 2026 Levent Alpöge released a Lean 4 proof of the Riemann Hypothesis (175,187 files, about 17.5M lines) together with a four-page note, [`remark.pdf`](https://alpo.ge/remark.pdf). On 21 Nov 2026 the Lean and Mathlib communities [certified](https://leanprover-community.github.io/blog/posts/riemann-hypothesis/) the formal proof.

The proof is checked, but it is huge and mostly unexplained. This lab is for work on understanding it:
- mapping how it is built;
- matching the author's ideas to the code;
- finding the parts that matter most.

Forks and contributions are welcome.

We don't re-check the proof here; the Lean community statement covers that.

## Studies

| Study | Question | Status |
|---|---|---|
| [Note → code](#study-1-note--code) | Where do the ideas in Alpöge's note appear in the Lean code? | First results, 22 Nov 2026 |

## Study 1: note → code

This study lists 21 ideas from the note and gives, for each, where we found it in the code, or that we didn't.

### Files

| File | What it is |
|---|---|
| [`inputs/note_items.json`](inputs/note_items.json) | The 21 ideas: timeline steps 1–19, the O(2, n) remark on page 3, and footnote 3. `note` is the note's own wording, transcribed by hand, so formulas are close but not character-exact. `hint` holds our explanations, kept separate from the note. |
| [`work/match/merged.json`](work/match/merged.json) | The verdict for each idea, with evidence. `meta` defines the verdicts and methods. |

Each item in `merged.json` has:
- `verdict`: the final verdict;
- `evidence`: Lean declarations, each with its module, its depth and what it shows;
- `agent_verdicts`: what each AI model concluded;
- `found_by`: which method found it;
- `correction`: where we overrode the AI search after checking the source.

### Results

| Verdict | Count | Ideas |
|---|---|---|
| **FOUND** | 4 | the √X chain of theta lifts (`PageWin`: √(x·δ₁) ≤ m ≤ 2√(x·δ₁)), the von Mangoldt sum Σ Λ(n)(X/n − 1)₊ − X log X + (1+γ)X (`GlobalTheoryTheory.G`), the Iwasawa tower, and a Vandiver-type bound on Sha of H¹(ℚ_l/ℤ_l(−e)) (`VVle`) |
| **PARTIAL** | 5 | Stepanov determinant, divisibility of the lift, Bost slope inequality, integral structure across Hecke characters, covolumes |
| **NOT_LINKED** | 6 | GGP determinants, seesaw/Bessel period, norms of lifts, the 2/3 cost barrier, the X^η bound, Behrend |
| **NOT_FOUND** | 6 | qN = Mᵏ + R, multiplicity 2 on O(2, n), the Gauss-sum cover of GL₂, W(1_S) ≥ 0 via a determinant, the relative trace formula, the derivation on a CM curve |

What the verdicts mean:
- **FOUND**: the idea is formalized in code the main theorem depends on. Every FOUND was read in the source by hand.
- **PARTIAL**: related code is there, but not the idea as the note states it.
- **NOT_LINKED**: the code exists, but our dependency graph shows no path from `riemannHypothesis` to it. *This is unconfirmed:* the graph matches names and can miss real uses, and since `Main` imports every module, imports can't settle it either.
- **NOT_FOUND**: an AI search found no trace. *This is weak evidence:* the AI search worked from the note's own words and missed objects whose code names are short (both models missed the Vandiver item, which the code writes as `Sha` and `QZ l`).

"Depth" is the number of steps from `riemannHypothesis` down to a declaration, in a dependency graph built by matching names. It is approximate.

### How it was made

The archive was read as text only. Nothing from it was built or executed.

1. A script mapped which declarations use which, starting from `riemannHypothesis`. This gave 182,848 declarations on the proof path, with the longest chain 54 steps deep.
2. **AI-assisted search.** For each idea, Claude searched the code and proposed where the idea is formalized, with evidence. Claude Sonnet ran on all 21 ideas, and Claude Opus reran the 15 that Sonnet left open. Every declaration Claude cited was checked to exist.
3. **Pattern search.** A separate search looked for the Lean form of each object, for example `Real.sqrt x` or a twist by `(-e)`. It found two ideas that both models missed.
4. Every FOUND was then checked by hand in the source, and one FOUND from the AI search was downgraded to PARTIAL.

## Other maps of the proof
- [jsmorph/rh](https://github.com/jsmorph/rh) — a mirror of the archive, with a diagram of the note's timeline.
- [riemannhypothesissolved.com](https://riemannhypothesissolved.com/) — an interactive map of the files, grouped into themes.
- [Lean community statement](https://leanprover-community.github.io/blog/posts/riemann-hypothesis/) and the verification overview at [math.mit.edu/~drew/grh](https://math.mit.edu/~drew/grh/).

Made with Claude (Anthropic). Corrections welcome.

## Citing

If you use this data, please cite it **together with** Alpöge's proof and note, the Lean community certification, Claude, Riemann's paper and Stepanov's method:

```bibtex
@misc{kavvenaki2026rhnote,
  author       = {Kav-Venaki, Eitam},
  title        = {Riemann Hypothesis Proof Lab},
  year         = {2026},
  month        = nov,
  howpublished = {\url{https://github.com/eitamatgithub/rh-proof-lab}}
}

@misc{alpoge2026note,
  author       = {Alp{\"o}ge, Levent},
  title        = {[Note on a proof of the {Riemann} Hypothesis found with {Claude}]},
  year         = {2026},
  month        = nov,
  howpublished = {\url{https://alpo.ge/remark.pdf}},
  note         = {Four-page note, dated 20 November 2026}
}

@misc{alpoge2026lean,
  author       = {Alp{\"o}ge, Levent},
  title        = {[{Lean} 4 formalization of the {Riemann} Hypothesis]},
  year         = {2026},
  month        = nov,
  howpublished = {\url{https://alpo.ge/lean.tar.gz.part01}},
  note         = {Lean 4 library in 14 parts (part01--part14), released 20 November 2026;
                  found and formalized with {Claude} ({Anthropic})}
}

@misc{leancommunity2026rh,
  author       = {{Lean and Mathlib communities}},
  title        = {Lean Verification of the {Riemann} Hypothesis},
  year         = {2026},
  month        = nov,
  howpublished = {\url{https://leanprover-community.github.io/blog/posts/riemann-hypothesis/}},
  note         = {Joint statement, endorsed by the {Lean FRO} and the {Mathlib Initiative}, 21 November 2026}
}

@misc{anthropic2026claude,
  author       = {{Anthropic}},
  title        = {Claude},
  year         = {2026},
  howpublished = {\url{https://www.anthropic.com/claude}},
  note         = {Large language model; Claude Sonnet and Claude Opus were used for this data set}
}

@article{riemann1859,
  author       = {Riemann, Bernhard},
  title        = {Ueber die {Anzahl} der {Primzahlen} unter einer gegebenen {Gr{\"o}sse}},
  journal      = {Monatsberichte der K{\"o}niglichen Preu{\ss}ischen Akademie der Wissenschaften zu Berlin},
  year         = {1859},
  month        = nov,
  pages        = {671--680},
  note         = {Volume for 1859, printed 1860}
}

@article{stepanov1969,
  author       = {Stepanov, S. A.},
  title        = {On the number of points of a hyperelliptic curve over a finite prime field},
  journal      = {Izvestiya Akademii Nauk SSSR, Seriya Matematicheskaya},
  volume       = {33},
  number       = {5},
  year         = {1969},
  pages        = {1171--1181},
  note         = {English translation: Math. USSR-Izvestiya 3(5) (1969), 1103--1114}
}
```

Titles in square brackets are descriptive: the sources give no title.
