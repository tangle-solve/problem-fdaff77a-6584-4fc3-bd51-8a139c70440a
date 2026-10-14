<!-- solve-problem:fdaff77a-6584-4fc3-bd51-8a139c70440a -->
# Practical epsilon-sure mining equilibria

A [Tangle Solve problem](https://solve.tangle.tools/problems/fdaff77a-6584-4fc3-bd51-8a139c70440a).

## What is the problem?

Bitcoin-style proof-of-work mining is modeled as a game: miners (players) each control a fraction of total hashing power, choose which block to mine on (fork choice) and possibly withhold/publish blocks (selfish-mining-style strategies), and receive rewards under a ledger protocol's reward rule. The protocol designer wants the honest strategy (always extend the canonical tip, publish immediately) to be an equilibrium, so no coalition of bounded size profits from deviating.

Colordag (Abraham, Dolev, Eyal, Halpern 2023) constructs a DAG-based protocol with a "colored" reward scheme under which honest mining is an *ε-sure* Nash equilibrium (or coalition-proof equilibrium) — roughly, deviations increase a deviator's payoff by at most ε, with high probability over mining randomness, in the limit / under asymptotic parameters. The open problem: obtain the same style of high-probability, per-deviation best-response guarantees **with explicit, small constants** — realistic confirmation times, bounded DAG storage, and reward parameters that don't blow up — and **under variable/block rewards** ( fees varying per block, changing hashpower shares) rather than fixed block rewards. Formal target: for a protocol Π with reward function r, show that for any coalition of relative hashpower α ≤ α*, any strategy profile σ deviating from honesty, Pr[payoff_σ − payoff_honest > ε] ≤ δ, with ε, δ and protocol parameters (confirmation depth k, memory m, reward split) bounded by explicit functions of α* and network delay-to-interval ratio. Ambiguity note: the census doesn't pin down the exact equilibrium notion (ε-sure vs. ε-whp; Nash vs. coalition-proof) — the solver should state which refinement is targeted.

## Why does it matter?

Colordag-type incentive compatibility currently comes with asymptotic or impractical parameters, so no deployed chain can use it. Explicit bounds with variable rewards would let DAG ledgers (throughput-oriented systems like those surveyed in Aumayr et al.) claim incentive compatibility against selfish mining and withholding attacks at real confirmation depths. It bridges the gap between idealized fixed-reward theory and fee-driven revenue that actual miners face; without it, "incentive-compatible blockchain" remains a limit statement, not an engineering spec.

## What's already known?

- **Colordag (2023)**: defines colored-DAG reward scheme where deviators' payoffs are ε-lower than honest with high probability; the equilibrium holds asymptotically / with large parameters (large confirmation horizon, high ε tolerance). Details of exact parameter dependence should be read from the paper (verify exact constants).
- **FairChain / Aumayr et al. (2026, arXiv:2503.10185)**: provides a different practical guarantee — believed to relate to fair reward distribution or transaction fairness on DAGs; it is not the ε-sure best-response guarantee sought here, hence only "related."
- Standard toolbox: selfish-mining analyses (Eyal–Sirer style), 𝜀-Nash notions with probabilistic guarantees, and Ledger Combinators / DAG confirmation analyses give concentration tools (Chernoff bounds on block counts) but don't compose with reward games.
- Gap: no published result gives non-asymptotic ε-sure equilibrium with explicit small (k, m) under heterogeneous rewards.

## What would count as solving it?

1. **Upper bound (positive result)**: a protocol with an explicit reward rule and confirmation/storage parameters (k, m as concrete numbers or tight functions of α*, delay, and reward variance) plus a proof that for all coalitions with hashpower ≤ α*, every deviation loses ≥ −ε with probability ≥ 1−δ, under variable rewards (e.g., i.i.d. or bounded-variation fee streams).
2. **Lower bound**: a proof that no reward rule with confirmation depth below some k achieves ε-sure equilibrium under variable rewards, showing the tradeoff is inherent.
3. **Matching construction**: parameters achieving the lower-bound tradeoff up to constants, with a computable reward function.
A referee needs the equilibrium definition stated, all constants explicit, and the reward model formalized.

## How to start

1. Read Colordag (arXiv:2308.11379) Sections defining the game and Theorem stating the ε-sure equilibrium; extract the exact parameter dependencies and identify which step is asymptotic.
2. Formalize variable rewards: start with block rewards drawn i.i.d. from a bounded distribution; extend to Markovian later. Re-derive Colordag's key lemma (deviator's colored sub-DAG grows slower) with concentration bounds that handle reward variance.
3. Compute Colordag's implicit constants numerically — a paper-style "how bad is it really" experiment may reveal which parameter actually blows up and guide where to tighten.
4. Main obstacle: withholding attacks interact with reward randomness — a deviator can condition publication on realized fees, breaking the symmetry Colordag exploits under fixed rewards; consider fee-smoothing (time-averaged rewards over a window) as a fix.

## Key references

- I. Abraham, D. Dolev, I. Eyal, J. Halpern, *Colordag: An Incentive-Compatible Blockchain*, arXiv:2308.11379, 2023. Defines the colored-DAG reward game and proves an ε-sure equilibrium for honest mining (asymptotically / with large parameters). https://arxiv.org/abs/2308.11379
- V. Aumayr et al., 2026, arXiv:2503.10185. Provides a different practical guarantee on DAG ledgers (fairness/reward related; verify exact content on retrieval). https://arxiv.org/abs/2503.10185
- (Background, not in census) I. Eyal, E. G. Sirer, *Majority is not Enough* (2014): selfish mining baseline that incentive analyses must defeat. (verify citation details)

## Warnings

- The exact Colordag equilibrium notion (ε-sure vs. ε-whp, Nash vs. resilient-to-coalitions) must be quoted from the paper; getting this wrong invalidates comparisons.
- Variable rewards change the game: with fees, deviators get information advantages, and reward-rule monotonicity arguments may fail.
- The Aumayr et al. 2026 reference postdates Colordag and its exact contribution is uncertain from the census — verify before relying on it.
- Don't conflate confirmation-depth security (probabilistic finality) with incentive security; both must hold simultaneously with explicit constants.

---
*`BCGT-000253` · flagged for review*

## Acceptance guidance

1. **Upper bound (positive result)**: a protocol with an explicit reward rule and confirmation/storage parameters (k, m as concrete numbers or tight functions of α*, delay, and reward variance) plus a proof that for all coalitions with hashpower ≤ α*, every deviation loses ≥ −ε with probability ≥ 1−δ, under variable rewards (e.g., i.i.d. or bounded-variation fee streams).
2. **Lower bound**: a proof that no reward rule with confirmation depth below some k achieves ε-sure equilibrium under variable rewards, showing the tradeoff is inherent.
3. **Matching construction**: parameters achieving the lower-bound tradeoff up to constants, with a computable reward function.
A referee needs the equilibrium definition stated, all constants explicit, and the reward model formalized.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/fdaff77a-6584-4fc3-bd51-8a139c70440a?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: BCGT-000253.

- [Public source catalog](https://drewstone.github.io/problems/#/p/BCGT-000253)
