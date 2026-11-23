# The ideas we didn't find: what we searched, and how

Three of the 21 ideas in Alpöge's note are marked NOT FOUND (rows 4, p. 3 and fn. 3 of the README table). A fourth, row 3, was NOT FOUND until we proved it in Lean ourselves and searched for the proof; it is now PARTIAL (see the end).

**What NOT FOUND means, and what it doesn't.** It means that the searches described below did not find the idea. It does not mean the idea is missing from the code. Every search here matches only the spellings, patterns and wordings we thought of, and an idea written differently would not match. Twice in this project a search like these missed code that was there: a pattern bug hid a used orthogonal-group lemma, and two ideas we had first labelled "not used" (Bost's slope inequality and the theta-series index) turned out to be used. We give no estimate of how likely the ideas are to be in the code.

## The searches

| Name | Data searched | How |
|---|---|---|
| **Statement and body patterns** | Text we extracted from the archive for all 1,060,824 declarations: each statement (theorems up to `:=`, `where` or `\|`; the first 900 characters of each definition) and each proof body | Case-sensitive regular expressions. Each match is labelled with whether the proof uses that declaration |
| **Source patterns** | The full text of all 175,187 `.lean` files | Case-sensitive regular expressions; each match is attributed to the declaration that contains it, then labelled used or not |
| **AI-assisted search with Claude** | The 182,848 declarations our name-matched dependency graph reached from the main theorem (later shown to be about a fifth of what the proof uses) | Claude Sonnet, then Claude Opus, searched for each idea starting from the note's wording |
| **Statement templates** | All 1,060,824 statements | Each idea written as Lean statements a formalizer might write; all statements ranked by TF-IDF word similarity |
| **Proof routes** | As statement and body patterns | For each idea, two or three proof sketches along different routes; for each sketch, patterns for the intermediate objects it needs |

**Used or not** comes from Lean's export of the main theorems, through the cut lists of [keithadler/rh-trim](https://github.com/keithadler/rh-trim): a declaration it cuts is unused, a plain declaration it keeps is used, and instances, attribute-tagged and build-only declarations stay undecided.

**How these searches did on ideas we knew were in the code.** Statement templates ranked the right code first or near the top for 3 of 5 known ideas, and missed 2 written with short private names (Vandiver's conjecture appears as `Sha`). The proof-route patterns led to the right code for both known ideas we tried them on (Vandiver's conjecture, and the derivatives of row 2, which the code calls "jets"). That is a small test, and it says little about ideas written in ways we did not anticipate.

Patterns below are given exactly as used (regular expressions). "0 matches" means the pattern matched no declaration in the data named.

---

## Row 4: W(1_S) ≥ 0 via a determinant; bootstrap

**The idea.** Prove the positivity W(1_S) ≥ 0 of the explicit-formula functional at an indicator function by writing it as a determinant. The note describes it as Stepanov's method with multiplicity one, followed by a bootstrap.

**What we searched for, and what came back:**
- **AI-assisted search:** both models reported it not found.
- **W applied to an indicator** (statement and body patterns, `W[A-Za-z]* \(?(Set\.)?indicator`): 0 matches.
- **Route A, Stepanov's auxiliary polynomial** (statement and body patterns):
  - `hasseDeriv`: 26 used declarations, including `exists_maximal_mem_pow_of_hasse_vanishing`: if the first N Hasse derivatives vanish, the function lies in the N-th power of the maximal ideal. This is Stepanov's multiplicity step; we did not find it connected to W.
  - `[Ww]ronskian`: 10 used. `[Ss]iegel[A-Z_]?[Ll]emma|siegel_lemma`: 1 used.
- **Route B, W as a Gram determinant** (statement and body patterns):
  - Explicit formula, `[Ee]xplicit[A-Z_]?[Ff]ormula|[Gg]uinand|[Ww]eil[A-Z_]?[Ee]xplicit`: 61 used.
  - Cauchy–Binet, `[Cc]auchy[A-Z_]?[Bb]inet|cauchyBinet`: 85 used; those we read are about minors of matrices indexed by Young diagrams.
- **Determinant positivity** (source patterns, `0 ≤ [^\n]{0,40}\.det\b|\.det\b[^\n]{0,30}≥ 0|PosSemidef[^\n]{0,60}det|det_nonneg|Matrix\.PosSemidef\.det`): 172 used. The ones we read are about heights and volumes of Gram matrices; we did not read all 172.
- **Statement templates:** only generic matches.

**What we know the proof uses in this role.** Its multiplicity comes from the chain of m ≈ √X theta lifts (row 8, found), and its window bound for zero-freeness (row 6, found) comes from that.

**Limits of these searches.** Determinants and positivity statements are very common in the code, and we read only part of the matches, so a version written in other terms could be among them.

---

## Page 3: multiplicity 2 on O(2, n) via a natively automorphic determinant

**The idea.** A determinant built from automorphic forms on the orthogonal group O(2, n) that vanishes to order 2. The note presents it as the step after which more multiplicity from more theta lifts became the plan.

**What we searched for, and what came back:**
- **AI-assisted search:** both models reported it not found.
- **Orthogonal groups** (source patterns, `Matrix\.orthogonalGroup|specialOrthogonalGroup|\bLorentzGroup\b|OrthogonalGroup\b|\bSO\(|\bO\(2`): 0 matches. **This pattern missed a used lemma**, `weil_orthogonal_group_invariance`, because of the `\b` after `OrthogonalGroup`; we found it later. It states the invariance of the Weil representation under the isometry group {g : gᵀBg = B} of a bilinear form, for any form, with no signature.
- **Signature (2, n), GSpin, Type IV** (source patterns, `signature.{0,20}\(2|\bGSpin\b|[Tt]ypeIV|SpinGroup|CliffordAlgebra`): 59 used. The ones we read use `CliffordAlgebra` as an exterior algebra.
- **Route A, Borcherds products** (statement and body patterns), each 0 matches:
  - `[Bb]orcherds`
  - `[Hh]eegner`
  - `[Dd]iscriminant[A-Z_]?[Ff]orm|dual[A-Z_]?[Ll]attice[^\n]{0,20}quotient`
  - `[Vv]ector[A-Z_]?[Vv]alued[A-Z_]?[Mm]odular`
  - `Mp2|[Mm]etaplectic[A-Z_]?SL|Mp₂`
  - `[Kk]udla` matched 390 used declarations. We did not check whether any of them concern Kudla–Millson theta lifts.
- **Route B, determinants of modular forms** (statement and body patterns):
  - `[Rr]ankin[A-Z_]?[Cc]ohen`: 0 matches.
  - `[Ww]ronskian`: 10 used, including `wronskian_E₄_E₆_ne_zero`: the Wronskian of the Eisenstein series E₄ and E₆ is nonzero on the upper half-plane. It is on SL₂, and it shows non-vanishing, not vanishing to order 2.
  - Serre derivative, `[Ss]erre[A-Z_]?[Dd]eriv|[Rr]amanujan[A-Z_]?[Tt]heta`: 3 used.
- **A lead we read:** a folder of "Capelli determinant" files. The ones we read are Lie-algebra cohomology for unitary discrete series.
- **Statement templates:** the best match scored 0.38, the lowest of the four ideas we couldn't find at first.

**What we know the proof uses in this role.** Unitary groups: the pattern `\(ε : Fin [a-z0-9]+ → Bool\)|\{ε : Fin [a-z0-9]+ → Bool\}` for a unitary signature matches 8,177 used declarations. Its multiplicity comes from a chain of theta lifts between definite unitary groups (row 8).

**Limits of these searches.** Small orthogonal groups can be rewritten as unitary groups through exceptional isomorphisms, so an O(2, n) argument could be written in unitary terms that none of these patterns match.

---

## Footnote 3: a normalized Gauss sum on the |μ_K|-fold cover of GL₂

**The idea.** Average over the family χ ↦ χ(discriminant) using a normalized Gauss sum of order |μ_K|, which comes from a theta function on the |μ_K|-fold metaplectic cover of GL₂. Its reciprocity law produces a complete-graph quadratic form, which simplifies to the path quadric of row 3.

**What we searched for, and what came back:**
- **AI-assisted search:** both models reported it not found.
- **Route A, Kubota–Patterson** (statement and body patterns):
  - Each 0 matches:
    - `[Kk]ubota`
    - `[Hh]ilbert[A-Z_]?[Ss]ymbol|hilbertSymbol`
    - n-th order Gauss sums, `gaussSum[^\n]{0,80}orderOf|orderOf[^\n]{0,80}gaussSum`
    - reciprocity laws, `[Rr]eciprocity[A-Z_]?[Ll]aw|[Cc]ubic[A-Z_]?[Rr]eciprocity|[Ee]isenstein[A-Z_]?[Rr]eciprocity`
  - `[Pp]ower[A-Z_]?[Rr]esidue`: 4 used, about Frobenius acting as a power residue.
  - `[Pp]atterson`: 2 used, both about a hypothesis named after Kazhdan and Patterson on unitary Eisenstein series at −ρ, not a cover of GL₂.
- **Covers** (source patterns, `[Mm]etaplectic|[Kk]ubota|NFoldCover|nFoldCover|CentralExtension`): 114 used. The ones we read concern the Weil representation, which lives on a double cover.
- **Route B, Stickelberger and Gross–Koblitz** (statement and body patterns):
  - `[Ss]tick`: 148 used, including `pAdicFrobeniusStickelberger`: finite-field Gauss sums of powers of a character, factorized p-adically with digit-sum valuations.
  - `[Gg]ross[A-Z_]?[Kk]oblitz|[Mm]orita|padicGamma|PadicGamma`: 18 used.
  - `jacobiSum|[Jj]acobi[A-Z_]?[Ss]um`: 6 used.
  - `[Hh]asse[A-Z_]?[Dd]avenport`: 0 matches.
- **Route C, the family average** (statement and body patterns):
  - Complete-graph form, `∑ i[^\n]{0,30}∑ j[^\n]{0,30}i < j[^\n]{0,40}x i \* x j|completeGraph`: 0 matches.
  - Character orthogonality, `sum_char|DirichletCharacter\.sum|MulChar\.sum_eq|orthogonality`: 273 used.
- **Gauss sums in general** (source patterns, `\bgaussSum\b`): 55 used.
- **Statement templates:** the top matches were the Stickelberger Gauss sums.

**What we know the proof uses in this role.** The note says Gauss-sum cancellation was "replaced by iterated Parseval". The proof's theta-lift code sits in a folder named `ThetaParseval` (3,079 used declarations).

**Limits of these searches.** The Stickelberger and Kazhdan–Patterson code is close to this idea, and a cover could be written as a cocycle or central extension in terms our patterns do not match.

---

## Row 3 (now PARTIAL): qN = Mᵏ + R

Row 3 was NOT FOUND by the same searches: the path-quadric pattern `x \(?[a-z]\)? \* x \(?[a-z] \+ 1\)?|[Pp]athQuadric` (source patterns) had 0 matches, and so did the other forms we tried (superdiagonal and tridiagonal matrices, path and complete graphs).

We then proved its three parts in Lean 4 with Mathlib, for every finite field:
1. the identity q·N = Mᵏ + R for any weights t;
2. the bound |S_h| ≤ q^((k−1)/2)·‖t‖_∞^(k−2)·‖t‖₂² for h ≠ 0, by iterated Parseval;
3. for t ≡ 1, the count q·N = qᵏ + (q − 1)·q^⌈k/2⌉, so R > 0.

A search for code matching these proofs (17 statement and body patterns, plus a ranking of declarations by the Mathlib constants they share with our proofs, then reading the top 16) found the method in code the proof uses:
- `sum_psi_mul`: Σ_t ψ(t·c) = q·[c = 0], the orthogonality step of part 1, with essentially our proof.
- `card_quadric_sub_sq_le`: (#{x : xᵀSx = β} − q^(n−1))² ≤ qⁿ for a nondegenerate quadratic form. It is q·N = main term + R with a square-root bound on R, unweighted.
- `card_sum_zero`: q·#{Σᵢ Q(xᵢ) = 0} = q^(2M) + (q − 1)(ηq)^M, an exact count with a definite sign for sums of binary forms. It gives the same numbers as part 3 for even k.

That search did not find the path quadric itself, the weights t(xᵢ), or the sign from Bertini and middle cohomology, so row 3 is PARTIAL: the method is in the code, the note's instance of it was not found.

**The lesson for the other three:** an idea we couldn't find by searching for its objects turned out to have its method in the code once we had a proof to search for. The same could happen with rows 4, p. 3 and fn. 3.

## What could find more

- **Exact dependency edges from Lean's export.** rh-trim gives used or unused, not who uses whom. The full export would show what the window bound (row 6) and the chain of lifts (row 8) rest on.
- **Proving more steps ourselves,** as we did for row 3, and searching for code that matches the proofs.
- **The author,** who could say which routes reached the code.
