# Erdős problem #1: disproof

[![CI](https://github.com/tadamcz/erdos1/actions/workflows/ci.yml/badge.svg)](https://github.com/tadamcz/erdos1/actions/workflows/ci.yml)

> **Note.** This README, the documentation in `Challenge.lean` and `formalization.yaml` were machine-written by Claude (Anthropic)
> at the direction of Tom Adamczewski, from the FrontierMath Erdős paper, the benchmark files and the module documentation inside
> the proof files, and reviewed by him. The Lean proofs themselves were written by GPT-6 Astra, as described below.

Machine-checked disproof of [Erdős problem #1](https://www.erdosproblems.com/1) in Lean 4 with Mathlib, found autonomously by a
pre-release version of **GPT-6 Astra** (OpenAI) in the **FrontierMath Erdős** benchmark (Adamczewski and Bloom, 2026). The
repository packages the AI-written proofs for the [Palomar registry](https://palomar-registry.org/): `Challenge.lean` is the
small statement a reader audits, `Solution.lean` proves it, and [Comparator](https://github.com/leanprover/comparator) checks that the two
statements coincide and that only the standard axioms are used.

## The result

A finite set `A ⊆ {1, …, N}` is *sum-distinct* (or *dissociated*) if the map `S ↦ ∑_{a ∈ S} a` on subsets `S ⊆ A` is
injective. Erdős asked whether a sum-distinct set of size `n` in `{1, …, N}` forces `N ≫ 2^n`, that is, whether there is
an absolute constant `C > 0` with `N > C · 2^n`. He called this "perhaps my first serious problem" and dated it to 1931
[Er98]; erdosproblems.com lists a $500 prize. Taking `A = {1, 2, 4, …, 2^{n-1}}` shows `N ≤ 2^{n-1}` is possible, and
the best previous construction is Bohman's `N ≤ 0.22002 · 2^n` [Bo98]. The best lower bound is `N ≫ 2^n / √n`, first
proved by Erdős and Moser [Er56].

The conjecture is **false**: for every `ε > 0` there are arbitrarily large `n` and sum-distinct sets `A ⊆ {1, …, N}`
with `|A| = n` and `N ≤ ε · 2^n`. Formally, the theorem proved is the negation of the Formal Conjectures statement
`Erdos1.erdos_1`. As noted in the FrontierMath Erdős paper, the argument as formalized is ineffective: it gives no bound
on how large `n` must be in terms of `ε`.

The compared declaration, from `Challenge.lean`:

```lean
theorem erdos_1.disproof : ¬ (∃ C > (0 : ℝ), ∀ (N : ℕ) (A : Finset ℕ) (_ : IsSumDistinctSet A N),
    N ≠ 0 → C * 2 ^ A.card < N) := by
  sorry
```



For disproofs the benchmark file states the conjecture `erdos_N` and its negation `erdos_N.disproof`, both with `sorry`, and the model fills in exactly one. Here the compared theorem is the negation, written out explicitly instead of via `type_of%` (see *Edits* below).

**Fidelity.** None known between the compared theorem and the conjecture as stated on erdosproblems.com. The formal conjecture
`Erdos1.erdos_1` (Formal Conjectures) reads: there is `C > 0` such that `C · 2^|A| < N` for every `N ≠ 0` and every sum-
distinct `A ⊆ {1, …, N}`; the compared theorem is exactly its negation. The hypothesis `N ≠ 0` only excludes the
degenerate empty interval. The disproof is ineffective (no explicit `n(ε)`).

## Provenance

**Benchmark.** FrontierMath Erdős (Adamczewski and Bloom, 2026) evaluates AI systems on 68 open Erdős problems selected by Thomas F. Bloom, in the Lean proof
assistant, autonomously and under a fixed, disclosed budget ($300 and 72 hours of working time per attempt in the default configuration). The
agent works in a network-isolated Docker container with a Lean 4 toolchain (v4.27.0) and Mathlib, SageMath and Python; its final
`Spec.lean` is checked in a separate pristine container by Comparator against the trusted statement, permitting only `propext`,
`Quot.sound` and `Classical.choice`. The benchmark, harness and statements are public at
[epoch-research/LeanOpenProblems](https://github.com/epoch-research/LeanOpenProblems); the paper is in preparation. No human saw or steered the proof search.

**Statement.** The definitions and the statement come verbatim from [`FormalConjectures/ErdosProblems/1.lean`](https://github.com/google-deepmind/formal-conjectures/blob/488aade228ec37880b8fec178c173c07d279bb53/FormalConjectures/ErdosProblems/1.lean) in Google DeepMind's Formal Conjectures at commit `488aade228ec`, where the problem is stated with `sorry` as open. The benchmark isolated the selected statement into [`apn/data/erdos/Isolated/Erdos1.erdos_1.lean`](https://github.com/epoch-research/LeanOpenProblems/blob/77882c437ca1dfefab3b27fa00f1d29788100311/apn/data/erdos/Isolated/Erdos1.erdos_1.lean) (with a `.disproof` negation added), and that file is exactly what the model received.

**Resolutions.** Several independent attempts resolved this statement; all verified files are included.
"Default configuration" is the deepagent-based agent with subagents, memory and an offline arXiv snapshot under the benchmark's
budget of $300 and 72 hours of working time per attempt; "ReAct agent, larger budget" is a basic agent under a $1,000 budget.
**Cost** is computed from the attempt's exact token counts (from the harness's eval logs) at GPT-6 Astra's standard rates as provided
by OpenAI on 3 September 2026: $10 per million input tokens, $50 per million output tokens, $1 per million cache-read tokens and
$12.50 per million cache-write tokens. The harness itself metered spend at stand-in GPT-5.6 Sol prices, which is what the `usd` figure
in each file name reflects. **Working time** is the harness's `working_time` (time the agent was actually working, excluding waits on
API retries and rate limits), read from the harness's eval logs; the `h` figure in each file name is instead wall-clock time.
The Inspect transcripts are linked for the record (access may be restricted).

| Module | Role | Attempt | Cost | Working time | Tokens, millions (input / output / cache read / cache write) | Inspect log |
|---|---|---|---|---|---|---|
| `Erdos1/Resolutions/Erdos1_219usd_38h.lean` | **primary** (wired to `Solution.lean`) | default configuration, 28 Aug 2026 (benchmark run) | $405 | 27.1 h | 0.05 / 3.3 / 74 / 13.4 | [transcript](https://viewer.hawk.hawkbench.com/permalink/sample/nazKiHC3d3BqkobCeJMJqN) |
| `Erdos1/Resolutions/Erdos1_735usd_86h.lean` | alternate | ReAct agent, larger budget, 26 Aug 2026 (re-run) | $1,384 | 84.0 h | 0.41 / 8.6 / 410 / 43.4 | [transcript](https://viewer.hawk.hawkbench.com/permalink/sample/dUHKNhUXgLuAKTA9ekYyWf) |

## Proof account

The accounts below paraphrase the module documentation the model wrote inside each file; they describe the Lean proofs actually
present. They are not a human verification of the mathematics beyond what Comparator establishes.

**`Erdos1_219usd_38h`** (default configuration, 28 Aug 2026 (benchmark run)). Constructs, for large `n`, an `n × n` rational matrix whose zero-sum lift is admissible for an open cube and whose determinant is small; integral changes of basis, saturated bidiagonal perturbations and a binary-block construction (weights `a i * 2^j` of a relation-free family have distinct subset sums) transfer this to sum-distinct subsets of `{1, …, N}` with `N ≤ ε · 2^n`. The final theorem `ErdosCounter.no_uniform_subset_bound` refutes the uniform bound.

**`Erdos1_735usd_86h`** (ReAct agent, larger budget, 26 Aug 2026 (re-run)). Builds a full-rank cyclic lattice from cyclic power and reversed-pair determinants (`Erdos1CyclicLatticeConstruction.low_determinant_family`) and transfers low-determinant lattices to sum-distinct sets (`Erdos1LatticeReduction.low_determinant_implies_negation`).

**Informal summary from the FrontierMath Erdős paper** (Thomas F. Bloom, appendix; a fuller sketch is on the problem page of
erdosproblems.com): The argument uses linear algebra to construct a sequence of `n × n` rational matrices, with `n → ∞` and determinant `→
0`, with specific properties that allow one to construct from this matrix large dissociated sets. The second author of
the paper reinterpreted it in terms of lattices; a detailed sketch is available on erdosproblems.com. The paper judges
the two disproofs (the two files in this repository) to be essentially the same argument.

## Repository layout

- `Challenge.lean` — the statement surface: definitions copied verbatim from the benchmark statement and the compared theorem with `sorry`.
- `Solution.lean` — imports the primary resolution module, in whose environment the compared theorem is proved.
- `Erdos1.lean`, `Erdos1/Resolutions/` — the AI-written proof module(s); `Erdos1.lean` imports the primary one.
- Alternate resolutions are built as the separate Lake library `Erdos1Alternates`; each is a self-contained copy of the statement preamble plus its own proof, so they are never imported together.
- `comparator.json` — Comparator configuration naming `Erdos1.erdos_1.disproof`.
- `formalization.yaml` — structured metadata (provenance, sources, classification, automation, review) in the mathlib-initiative v0.4 format.
- `provenance/` — SHA-256 sums of the benchmark output files and unified diffs from them to the modules here.
- `scripts/verify-comparator.sh` runs the pinned Comparator, lean4export, NanoDa and Landrun locally (Linux); `scripts/validate-formalization.rb` checks the metadata file.
- `.github/workflows/ci.yml` — builds the project and runs Comparator (layout from the Palomar template; the template's doc-gen4 job is omitted because the modules import all of Mathlib).

## Edits relative to the benchmark output

The proof modules are the model's final `Spec.lean` files, verified by the benchmark, with only the following mechanical changes; the
exact diffs are in `provenance/`. The toolchain was moved from Lean v4.27.0 / Mathlib (via Formal Conjectures at commit
`488aade2`) to Lean v4.28.0 / Mathlib v4.28.0, the oldest release Palomar accepts; the only change this required is the
`loopless` adjustment listed below for the files it affects.

- `Erdos1_219usd_38h.lean` (SHA-256 of the benchmark output: `5993f549cf4e885f29b83fe4d9b4d1f9de89f7136c25ff3d3c86310c87fd1d96`):
  - line 1: `import FormalConjecturesUtil` → `import Mathlib`
  - removed the sorry'd stub of the original conjecture `erdos_1` (lines 22–31 of the original) together with its docstring
  - restated `Erdos1.erdos_1.disproof` explicitly (the original used `¬ (type_of% @Erdos1.erdos_1)`, which referenced the removed stub) and added a docstring
- `Erdos1_735usd_86h.lean` (SHA-256 of the benchmark output: `fb76ac09bccc62b1b8a1cec1120a9f0c4615a2f968a75cfe5da275cacad40fc4`):
  - line 1: `import FormalConjecturesUtil` → `import Mathlib`
  - no other changes (react-agent file: statement contained only the proved direction)

## Verification

```sh
lake exe cache get
lake build
ruby scripts/validate-formalization.rb
./scripts/verify-comparator.sh   # Linux: Comparator + NanoDa under Landrun
```

CI runs the same checks. The compared theorem depends on no `sorry` and on no axioms beyond `propext`, `Quot.sound` and
`Classical.choice`. This repository is prepared for submission to Palomar through the
[submission form](https://submit.palomar-registry.org/) with the full commit SHA; registration is a separate step by the maintainer.

## Licence and attribution

This repository snapshot is licensed under the Apache License 2.0 (see `LICENSE`). The benchmark statement it reproduces is
from Formal Conjectures, © The Formal Conjectures Authors, Apache-2.0 (see `NOTICE`). Cited papers,
erdosproblems.com and Mathlib retain their own licences.
