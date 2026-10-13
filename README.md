<!-- solve-problem:a7f7deb7-2d9e-43fb-a647-fd5f7819884f -->
# Optimal exponent for fixed-size clique detection

A [Tangle Solve problem](https://solve.tangle.tools/problems/a7f7deb7-2d9e-43fb-a647-fd5f7819884f).

## What is the problem?

Given an n-vertex graph G, the k-Clique detection problem asks whether G contains a clique of size k, where k = 3m is a fixed constant divisible by 3 (so the running-time exponent is the object of study, with k not part of the input). The classical Nešetřil–Poljak reduction combines vertex triples into single vertices, reducing k-clique to m-clique detection, and a final triangle-detection step, giving time O(n^{ω·k/3}), where ω < 2.373 is the square matrix multiplication exponent. The open question: for some fixed k ≡ 0 (mod 3), does there exist an algorithm running in time O(n^{ω·k/3 − ε}) for a constant ε > 0 independent of n (ε may depend on k)? Equivalently: is ω·k/3 the optimal exponent for k-clique detection, or can clique detection beat the matrix-multiplication benchmark? The model is the standard word-RAM / unit-cost RAM with polynomially bounded integer weights on edges (needed for fast matrix multiplication algorithms); an unconditional separation or matching lower bound is far out of reach, so a solution may be either an improved upper bound or a conditional lower bound showing that beating n^{ω·k/3} would imply faster matrix multiplication (i.e., ω < its current value, or ω = 2 for a full collapse).

## Why does it matter?

Clique detection is the canonical dense-subgraph-detection problem, and its fine-grained complexity underpins dozens of conditional lower bounds for problems from Subgraph Isomorphism to dynamic graph algorithms and pattern matching. Resolving whether the matrix-multiplication reduction is tight determines whether all those lower bounds are artifacts of our matrix multiplication algorithms or genuine complexity walls. Practically, clique and dense-subgraph detection is used in network science, bioinformatics, and fraud detection, so any superconstant or even constant-factor exponent improvement translates to real speedups. Finally, it probes the broader question of whether matrix multiplication is "universal" for a class of combinatorial problems.

## What's already known?

- Nešetřil–Poljak (1985): O(n^{ω·⌊k/3⌋} · n^{ω·(k mod 3)}) — for k ≡ 0 (mod 3), exactly O(n^{ωk/3}). This remains the best general upper bound for k divisible by 3 (up to the current value of ω).
- For k not divisible by 3, the same technique gives O(n^{ω⌈k/3⌉}), and it is known one can sometimes do marginally better for the remainder using rectangular matrix multiplication (verify the exact savings), but the k ≡ 0 case is the "clean" open instance.
- Vassilevska Williams (2018) surveys fine-grained complexity and popularizes the "k-Clique Hypothesis" (no O(n^{k−ε}) algorithm for arbitrary k) and its consequences. Note this hypothesis is about the combinatorial-looking exponent k, not about optimality of ωk/3; do not conflate the two.
- No nontrivial unconditional lower bound better than the trivial n^{⌈k/2⌉}-ish (input-size/speedup-limits type) bounds is known for any k ≥ 3; even proving triangle detection (k = 3) requires exponent ω is open — so the problem for general k subsumes the k = 3 case (ε-beating ω for triangles would improve ω itself).
- Attempts via color-coding, inclusion–exclusion, and combinatorial methods all match or lose to the Nešetřil–Poljak bound; combinatorial (non-MM) algorithms are stuck at ~n^{k/2} up to polylog factors (Lincoln–Vassilevska Williams–Wang-type results, verify).

## What would count as solving it?

Any one of the following, for a fixed k ≡ 0 (mod 3):
1. **Upper bound:** a randomized or deterministic algorithm detecting k-cliques in O(n^{ωk/3 − ε}) time for some ε > 0 (constant independent of n). This would be a full solution to the "beatable" direction.
2. **Conditional lower bound:** a proof that an O(n^{ωk/3 − ε})-time k-clique algorithm implies matrix multiplication in O(n^{ω'−δ}) time for some δ > 0 where ω' is the current barrier — most desirably, that it implies ω = 2 or improves ω. This resolves the question relative to matrix multiplication.
3. **Unconditional lower bound** (strongest, likely out of reach): a proof that no n^{ωk/3−ε} algorithm exists in a stated model.
A referee would accept (1) directly; (2) requires a clean, tight reduction (fine-grained, preserving n-dependence) with the implication stated precisely.

## How to start

1. Read Nešetřil–Poljak (1985) to internalize the triple-combining reduction; then Vassilevska Williams's 2018 survey (or her "On some fine-grained questions" talk/survey paper) for the modern hypothesis landscape.
2. Master the k = 3 case first: triangle detection in O(n^{ω−ε}) would improve ω; understand why (triangle detection ≤ MM and MM ≤ triangle detection up to polylog — i.e., triangle and MM are subcubic-equivalent, per Vassilevska Williams–Williams 2010). Ask: for k = 6 or 9, is there any structural slack in the Nešetřil–Poljak reduction not present in the triangle equivalence?
3. Try the lower-bound direction: attempt a fine-grained reduction from triangle detection (or MM) on n-vertex graphs to k-clique on n^{k/3}-vertex graphs that is exact enough to transfer an ε improvement. The obstacle: the triple-combining map produces a very structured (e.g., tripartite-with-partition-labels) instance; a hypothetical faster clique algorithm need only work on general graphs, and exploiting the structure to build a faster MM is exactly what fails.
4. Explore algebraic approaches: can k-clique be expressed as a tensor/arithmetization problem whose rank is strictly less than k/3 "copies" of the matrix multiplication tensor?

## Key references

- J. Nešetřil, S. Poljak, "On the complexity of the subgraph problem," Commentationes Mathematicae Universitatis Carolinae, 26(2), 1985. Establishes the O(n^{ωk/3})-type bounds for subgraph isomorphism with fixed pattern H via reduction to matrix multiplication; the source of the benchmark exponent. (No stable link; available via DML-CZ.)
- V. Vassilevska Williams, "On some fine-grained questions in algorithms and complexity," Proceedings of the ICM 2018 (survey). Surveys fine-grained complexity, the k-Clique hypothesis, and known clique upper/lower bounds; start here for the modern framing. https://people.csail.mit.edu/virgi/eccc.piece.pdf (verify link).
- V. Vassilevska Williams, R. Williams, "Subcubic equivalences between path, matrix, and triangle problems," FOCS 2010. Proves triangle detection and matrix multiplication are subcubic-equivalent — the key structural fact explaining why k = 3 is as hard as improving ω. (verify exact venue/pages)

## Warnings

- Do not conflate the k-Clique Hypothesis (no n^{k−ε} algorithm, relevant when k grows) with this problem's conjecture (no n^{ωk/3−ε} algorithm for fixed k ≡ 0 mod 3). They are different statements with different consequences.
- The problem is only fully "open" for k ≡ 0 (mod 3); remainders k mod 3 = 1, 2 have known (small) savings via rectangular multiplication, and improvements there do not answer this question.
- Fast MM algorithms require edge weights or polynomially-bounded labels; state your model. Combinatorial (non-algebraic) algorithms are a separate track.
- Beware "improvements" that only track the current value of ω (e.g., using ω < 2.373): the question demands beating ω·k/3 by ε for *any* value of ω, i.e., a separation from matrix multiplication, not from today's record.
- The k = 3 case subsumes improving ω, so expect genuine difficulty; partial progress on k = 6 with structured instances is the realistic first target.

---
*`ALGO-000081` · unverified*

## Acceptance guidance

Any one of the following, for a fixed k ≡ 0 (mod 3):
1. **Upper bound:** a randomized or deterministic algorithm detecting k-cliques in O(n^{ωk/3 − ε}) time for some ε > 0 (constant independent of n). This would be a full solution to the "beatable" direction.
2. **Conditional lower bound:** a proof that an O(n^{ωk/3 − ε})-time k-clique algorithm implies matrix multiplication in O(n^{ω'−δ}) time for some δ > 0 where ω' is the current barrier — most desirably, that it implies ω = 2 or improves ω. This resolves the question relative to matrix multiplication.
3. **Unconditional lower bound** (strongest, likely out of reach): a proof that no n^{ωk/3−ε} algorithm exists in a stated model.
A referee would accept (1) directly; (2) requires a clean, tight reduction (fine-grained, preserving n-dependence) with the implication stated precisely.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/a7f7deb7-2d9e-43fb-a647-fd5f7819884f?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000081.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000081)
