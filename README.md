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
| [Note → code](#study-1-note--code) | Where do the ideas in Alpöge's note appear in the Lean code? | Updated 23 Nov 2026 |

## Study 1: note → code

This study lists 21 ideas from the note and gives, for each, where we found it in the code, or which searches did not find it.

### Results

**9 found, 9 partial, 3 not found.** One row per idea, in the note's order: timeline steps 1–19, then the remark on page 3 and footnote 3.

| # | Idea in the note | Verdict | In the code |
|---|---|---|---|
| 1 | Stepanov determinant | PARTIAL | Stepanov's multiplicity step: Hasse derivatives vanishing to order N put a function in the N-th power of the maximal ideal (`exists_maximal_mem_pow_of_hasse_vanishing`). A step-determinant constant for each lift in the chain is a p-adic unit (`stepDeterminantConstant_unit_independent`). We did not find the determinant itself. |
| 2 | An invariant derivation on a CM curve | PARTIAL | The invariant differential of a CM elliptic curve, supersingular reduction, the inseparable Frobenius, and the p-adic size of theta "jets" (`norm_jet_atU`). We did not find the exact bound p^(a/p + a/p³ + …). |
| 3 | qN = Mᵏ + R, a sign-only argument | PARTIAL | The method is used: zeros of a quadratic form counted with additive characters (`sum_psi_mul`: Σ_t ψ(t·c) = q·[c = 0]), the main term split off and the rest bounded, (#{xᵀSx = β} − q^(n−1))² ≤ qⁿ (`card_quadric_sub_sq_le`), and an exact count with a definite sign for sums of binary forms (`card_sum_zero`). We did not find the path quadric itself, the weights, or the sign from Bertini. See [row 3 in Lean](#row-3-in-lean). |
| 4 | W(1_S) ≥ 0 via a determinant; bootstrap | **NOT FOUND** | Our searches did not find a statement about W at an indicator. The proof gets its multiplicity from the chain of lifts (row 8). *Related:* the Hasse-derivative step of row 1. |
| 5 | The von Mangoldt sum Σ Λ(n)(X/n − 1)₊ − X log X + (1+γ)X; the Guinand–Weil sign | FOUND | Defined verbatim as `GlobalTheoryTheory.G`, with twisted versions `sumLamChi` and `gChi`. |
| 6 | W(1_(0,1] + X^η·1_(0,1/X]) ≥ 0 gives no zeros past 1 − η | FOUND | `zeroFree_of_rpow_window`: a bound on the prime sum over windows [Y, Y^(1+δ)] gives zero-freeness past θ. |
| 7 | Determinants of GGP periods; interlacing | PARTIAL | The GGP interlacing condition (`interlaces_of_OK`, `pairOK_of_interlace`). We did not find determinants of the periods. |
| 8 | Iterated theta lifts, multiplicity m ≈ √X | FOUND | `PageWin`: √(x·δ₁) ≤ m ≤ 2√(x·δ₁), with the lifts `TLiftK`. |
| 9 | Norms of lifts: Chowla–Selberg, Weyl, the Siegel mass formula | PARTIAL | All three tools are used (`siegel_mass_rank_one`, Chowla–Selberg, the Weyl dimension formula). We did not trace the norm computation itself. |
| 10 | A lower bound on the divisibility of the m-fold lift | PARTIAL | A prime-power divisibility statement for theta lifts modulo an Eisenstein term (`exists_primePow_dvd_mod_eisenstein_of_theta`). |
| 11 | The Iwasawa tower via pigeonhole | FOUND | `exists_iwasawaRank_bound_of_ferreroWashington`: a uniform Iwasawa rank bound up the cyclotomic tower. |
| 12 | Seesaw: the period of the lift is a Bessel period | FOUND | `exists_seesaw_intertwiners_of_triple` and `FourierJacobiPeriodGivesBesselPeriodOfLift`. |
| 13 | Bost slope inequality | FOUND | `SlopeInequality`: for an injective map φ of Euclidean lattices with ‖φ(x)‖ ≤ C‖x‖, slope(L) ≤ μ_max(M) + log C. A Hermitian version over number fields (`SizeIneq`). |
| 14 | The cost of each lift; the 2/3 barrier | PARTIAL | A per-lift constant (`perLiftC`). We did not identify the 2/3 barrier. |
| 15 | An integral structure across Hecke characters (a "gcd") | PARTIAL | Divisor Galois representations (`DivAll`, `IsDivRep`) and a common sublattice of two lattices (`exists_common_sublattice`). We did not find the gluing across Hecke characters. |
| 16 | Relative trace formula; projected Poincaré series | PARTIAL | Poincaré series and their pairing with Bessel periods (81 declarations). Our searches did not find a relative trace formula (the pattern `[Rr]elative[A-Z_]?[Tt]race|relativeTrace` matched only traces of field extensions). |
| 17 | A weakening of Vandiver's conjecture | FOUND | `VVle` bounds Sha of H¹(ℚ_l/ℤ_l(−e)); the code writes these objects as `Sha` and `QZ l`. |
| 18 | Covolumes, the theta series' index, congruences between lifts and forms | FOUND | `covolume_eq_sqrt_det_gram`, `theta_lattice_index` (the relative index of the theta lattices is idx^N) and `hecke_lift_separated_of_lattice_congruence`. |
| 19 | Choosing the Iwasawa tower with Behrend | FOUND | Behrend sets (`Formalism.BehrendSets`, sets free of k-term progressions): 34 of their 89 declarations are used, although the note calls the idea unnecessary. |
| p. 3 | Multiplicity 2 on O(2, n) via a natively automorphic determinant | **NOT FOUND** | Our searches did not find signature (2, n), GSpin, Borcherds products or Heegner divisors. The proof works with unitary groups. *Related:* a Wronskian of E₄ and E₆ (`wronskian_E₄_E₆_ne_zero`), for SL₂. |
| fn. 3 | A normalized Gauss sum on the \|μ_K\|-fold cover of GL₂ | **NOT FOUND** | Our searches did not find a cover of GL₂, Kubota's symbol or higher reciprocity laws. The note says Gauss-sum cancellation was "replaced by iterated Parseval". *Related:* Stickelberger's factorization of Gauss sums. |

What the verdicts mean:
- **FOUND**: the idea is formalized in code the proof uses, and we read it in the source by hand.
- **PARTIAL**: related code is used, but not the idea as the note states it.
- **NOT FOUND**: our searches did not find the idea. This does not mean it isn't in the code: the searches match only the patterns and wordings we wrote, and an idea written differently would not match. Row 3 was NOT FOUND until we proved it ourselves and searched for the proof.

### The 3 not-found ideas

Every search, with the exact patterns and match counts: **[not_found_explained.md](not_found_explained.md)**. In short:

| Row | Idea | What our searches did not find | What we know the proof uses in that role |
|---|---|---|---|
| 4 | W(1_S) ≥ 0 via a determinant | W applied to an indicator (0 matches); among 172 used determinant-positivity statements, the ones we read are about heights and Gram volumes | multiplicity m ≈ √X from the chain of lifts (row 8). Related: Stepanov's Hasse-derivative step |
| p. 3 | Multiplicity 2 on O(2, n) | Borcherds, Heegner, discriminant forms, vector-valued modular forms, Mp₂: 0 matches each; the signature-(2, n) matches we read are Clifford algebras used as exterior algebras | unitary groups (8,177 used declarations with a unitary signature). Related: a Wronskian of E₄ and E₆, on SL₂ |
| fn. 3 | Gauss sum on a cover of GL₂ | Kubota, Hilbert symbols, n-th order Gauss sums, reciprocity laws: 0 matches each | iterated Parseval, as the note says (`ThetaParseval`, 3,079 used). Related: Stickelberger's factorization of Gauss sums |

**How we searched:** case-sensitive regular expressions over the statements and proof bodies of all 1,060,824 declarations and over the full text of all 175,187 files, for the objects that two or three possible proofs of each idea would need; an AI-assisted search with Claude; and similarity ranking against Lean-style statements of each idea. We tested the pattern searches on ideas known to be in the code first. **A "0 matches" is about our patterns, not about the code.** One of our patterns missed a used declaration because of a regex bug, and row 3 was found once we searched for a proof instead of for objects. We give no estimate of how likely these three ideas are to be in the code.

### Row 3 in Lean

Row 3 used to be a fourth not-found idea. To know what to look for, we proved its three parts in Lean 4 with Mathlib, for every finite field 𝔽_q:
1. **The identity** q·N = Mᵏ + R, for any weights t, by orthogonality of additive characters.
2. **The bound** |S_h| ≤ q^((k−1)/2)·‖t‖_∞^(k−2)·‖t‖₂² for each h ≠ 0 (so R, a sum of q − 1 of them, is of square-root size), by iterating Parseval along a transfer chain.
3. **The sign** for t ≡ 1: q·N = qᵏ + (q − 1)·q^⌈k/2⌉, so R > 0, by a recursion on the number of variables.

Searching the archive for these found the method (part 1, and the unweighted shape of parts 2 and 3) in code the proof uses, but not the path quadric or the note's weighted sign argument. So row 3 is now PARTIAL. The Lean files are not in this repository yet.

"Used by the proof" comes from Lean's own export of the main theorems, through the cut lists published by [keithadler/rh-trim](https://github.com/keithadler/rh-trim) (see step 5 below).

**What changed since 22 Nov:** the first version had 4 found, 5 partial, 6 not linked (code exists, use unconfirmed) and 6 not found. Lean's export settled all six "not linked" ideas (3 found, 3 partial). Three not-found ideas turned out partial (rows 2, 3 and 16), and two partial ideas turned out fully there (rows 13 and 18). `previous_verdict` in `merged.json` records each change.

### Files

| File | What it is |
|---|---|
| [`inputs/note_items.json`](inputs/note_items.json) | The 21 ideas: timeline steps 1–19, the O(2, n) remark on page 3, and footnote 3. `note` is the note's own wording, transcribed by hand, so formulas are close but not character-exact. `hint` holds our explanations, kept separate from the note. |
| [`work/match/merged.json`](work/match/merged.json) | The verdict for each idea, with evidence. `meta` defines the verdicts and methods. |

Each item in `merged.json` has:
- `verdict`: the final verdict, and `previous_verdict` where it changed;
- `summary`: what we found;
- `evidence`: Lean declarations, each with its module and what it shows;
- `related`: for NOT FOUND ideas, code that is related but is not the idea;
- `agent_verdicts`: what each AI model concluded;
- `found_by`: which method found it;
- `correction`: where we overrode the AI search after checking the source.

### How it was made

The archive was read as text only. Nothing from it was built or executed.

1. **A dependency graph by name matching.** A script mapped which declarations use which, starting from `riemannHypothesis`. What it finds is right (99.8%), but it sees only about a fifth of what the proof uses: 182,848 declarations, where Lean's export shows 913,694. The `depth` field in `merged.json` comes from this graph and is approximate.
2. **AI-assisted search.** For each idea, Claude searched the code and proposed where the idea is formalized, with evidence. Claude Sonnet ran on all 21 ideas, and Claude Opus reran the 15 that Sonnet left open. Every declaration Claude cited was checked to exist.
3. **Pattern search.** A separate search looked for the Lean form of each object, for example `Real.sqrt x` or a twist by `(-e)`. It found two ideas that both models missed.
4. **Whole-archive text search.** About 40 text patterns over all 175,187 files, for the ideas still unmatched.
5. **Lean's export.** rh-trim cuts the archive down to what Lean's export of the main theorems needs. A declaration it cuts is unused, and a plain declaration it keeps is used. Instances, attribute-tagged and build-only declarations stay undecided (about 5%). This settled every "not linked" idea.
6. **Proof routes.** For each idea still open, two or three proof sketches along different routes, the objects each route needs, and a count of every declaration that mentions them, used or not. Each check was first run on ideas we knew were present.
7. **Manual check.** Every FOUND was read in the source by hand, and one FOUND from the AI search was downgraded to PARTIAL.

## Other maps of the proof
- [keithadler/rh-trim](https://github.com/keithadler/rh-trim) — the archive cut down to what the proof uses, from Lean's export. Our "used by the proof" data comes from its cut lists (Apache-2.0).
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
