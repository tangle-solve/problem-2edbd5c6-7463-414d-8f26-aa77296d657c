<!-- solve-problem:2edbd5c6-7463-414d-8f26-aa77296d657c -->
# Paid verification when fraud is vanishingly rare

A [Tangle Solve problem](https://solve.tangle.tools/problems/2edbd5c6-7463-414d-8f26-aa77296d657c).

## What is the problem?

**Formal setting.** A smart-contract system (the "consensus computer" of Luu et al. 2015) lets solvers/verifiers submit computational results and allows others to challenge them. Consider a game with:

- **Solvers** who may submit fraudulent results; committing fraud yields payoff *G* if unchallenged, penalty *P* (or *0* if not staked) if caught.
- **N verifiers** who each choose *verify* (cost *c* > 0, catches fraud with probability 1) or *skip* (cost 0). Verification effort is unobservable; rewards for catching fraud go to whoever verifies (possibly shared among verifiers).
- The system designer chooses reward/penalty parameters.

**The verifier's dilemma:** if penalties are strong enough that fraud is *deterred* in equilibrium, then no verifier ever earns a fraud-detection reward, so verification is pure cost — the only symmetric equilibria are "nobody verifies." But if nobody verifies, fraud becomes profitable, so equilibria oscillate or collapse into mixed strategies where fraud happens with positive probability and/or verification is underprovided. Detection reward *R* and challenge probability *q* satisfy a fixed-point relation: deterrence requires expected fraud reward < 0, but then verifiers' expected reward from verifying is < *c*.

**The open problem:** construct a mechanism (cryptographic, game-theoretic, or economic — e.g., mandatory escrow, randomized "verification bounties", fraud proofs with deposits) in which (a) independent verification is sustained in equilibrium — verifiers strictly prefer verifying — while (b) fraud occurs with probability → 0, robustly to (c) free-riding among verifiers and (d) *outside* adversaries who attack the system's security budget rather than seeking fraud rewards.

## Why does it matter?

Truebit, Optimism/Arbitrum-style fraud-proof systems, and any layer-2 relying on "at least one honest watchful party" all need verification to be *individually rational*, not just assumed. If verification is not incentivized, the economic security assumption behind optimistic rollups is unfounded and bridges/exit fraud proofs can be gamed during low-fraud periods. Solving this would let designers replace the "1-of-N honesty" assumption with a verified equilibrium property, and would connect to broader mechanism design for public-good provision under rare disasters.

## What's already known?

- **Luu et al. (2015)** formalized the verifier's dilemma for consensus computers, showing participants rationally skip verification when rewards don't cover cost; they analyze when bribing or non-verification destabilizes correctness.
- **Truebit (Teutsch et al., ~2017)** proposed the main known mitigation: an *artificial* error-injection mechanism ("forced errors"/jackpots) that keeps verification rewarding even absent real fraud — i.e., deliberately re-creating fraud events so verifiers have something to catch. Whether this survives sophisticated attackers and how jackpot sizing trades off against cost is not fully analyzed (verify).
- **Inspection-game literature** (e.g., Dresher, Avenhaus) characterizes mixed-strategy equilibria: inspection probability q* = G/(G+P) style formulas; these give positive fraud probability, not vanishing fraud.
- **Volunteer's dilemma** results show provision probability *decreases* with N under fixed costs — exactly the free-rider obstacle.
- Shortcomings: existing fixes either re-introduce fraud-like events (costly, attacks the "vanishingly rare" premise), rely on altruism/1-of-N honesty, or ignore outside attackers who extract value from disrupting verification rather than committing payable fraud.

## What would count as solving it?

Any of the following, in increasing strength:

1. **Impossibility result:** a proof that under any mechanism with verification cost c > 0, per-verifier reward budget linear in honest operation, and no trusted third party, no equilibrium has fraud probability → 0 with all verifiers strictly preferring to verify.
2. **Construction with proof:** a mechanism M and equilibrium analysis showing verification is strictly incentive-compatible (each verifier's expected gain > c) and fraud probability → 0 as parameters scale, including free-riding (N verifiers, unobservable effort) and a malicious outside attacker model.
3. **Tight bounds:** matching upper bound (mechanism cost) and lower bound (minimum subsidy/burn required) on the steady-state cost of sustaining verification at fraud rate ε.
4. **Empirical/agent-based validation** is weaker but acceptable as a complement, not a substitute for the equilibrium proof.

## How to start

1. Read Luu et al. 2015 §4 (verification incentives in consensus computers) to fix notation; then find and read the Truebit whitepaper's error-injection mechanism (verify citation details) — it is the closest existing construction.
2. Write down the simplest model: one solver, N verifiers, binary verify/skip, fixed reward R for detection. Solve the mixed equilibrium; identify the deterrence-vs-participation fixed point and prove the impossibility in this restricted model. This is likely a publishable lemma on its own.
3. Model Truebit-style forced errors as a lottery and compute the required jackpot as a function of N and c; check whether outside attackers can profitably drain it.
4. Main obstacles: unobservable verification effort (moral hazard), free-riding scaling with N, and the self-defeating nature of deterrence. Candidate tools: revelation-principal mechanisms, contest/tournament design, verifiable random functions for audit lotteries, or stake-slashing that pays auditors from solver bonds.

## Key references

- Luu, Teutsch, Kulkarni, Saxena (2015), *Demystifying Incentives in the Consensus Computer*, IACR ePrint 2015/702. https://eprint.iacr.org/2015/702 — Formalizes why consensus-computer participants rationally skip verification and analyzes resulting security failures.
- Teutsch, Reitwiessner et al., *Truebit: A Scalable Verification Solution for Computation* (whitepaper, ~2017) (verify) — Proposes verification games with error injection/jackpots to keep verifiers paid absent natural fraud.
- Avenhaus, von Stengel, Zamir, *Inspection Games* (Handbook of Game Theory, 2002) — General theory of inspector/inspectee mixed-strategy equilibria.

## Warnings

- The phrase "vanishingly rare" is the crux: mechanisms that *manufacture* fraud-like events (Truebit) arguably sidestep rather than solve the problem; be explicit about whether your solution allows this.
- Unobservable effort means naive "pay for verifying" schemes fail — you cannot pay for effort you cannot see, only for caught fraud.
- Outside attackers break the usual assumption that challengers profit only from detection; they may attack to deplete the reward pool or trigger griefing.
- Do not confuse 1-of-N honesty assumptions with equilibrium verification; a referee will demand strict individual rationality or a precise behavioral assumption.
- Beware of "solutions" that only shift costs into solver deposits; check solvers' participation constraints and who bears the deadweight loss.

---
*`BCGT-000188` · flagged for review*

## Acceptance guidance

Any of the following, in increasing strength:

1. **Impossibility result:** a proof that under any mechanism with verification cost c > 0, per-verifier reward budget linear in honest operation, and no trusted third party, no equilibrium has fraud probability → 0 with all verifiers strictly preferring to verify.
2. **Construction with proof:** a mechanism M and equilibrium analysis showing verification is strictly incentive-compatible (each verifier's expected gain > c) and fraud probability → 0 as parameters scale, including free-riding (N verifiers, unobservable effort) and a malicious outside attacker model.
3. **Tight bounds:** matching upper bound (mechanism cost) and lower bound (minimum subsidy/burn required) on the steady-state cost of sustaining verification at fraud rate ε.
4. **Empirical/agent-based validation** is weaker but acceptable as a complement, not a substitute for the equilibrium proof.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/2edbd5c6-7463-414d-8f26-aa77296d657c?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: BCGT-000188.

- [Public source catalog](https://drewstone.github.io/problems/#/p/BCGT-000188)
