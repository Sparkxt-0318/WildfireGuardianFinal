<!-- Copy of a6_literature/NOVELTY_MATRIX_FINAL.md (A6 final, sha256 6d2fac6ab0659bad…) placed at delivery root by the coordinator. -->
# Novelty matrix — final (A6, 2026-10-03)

Prepared by agent A6 (literature and novelty review), WildfireGuardian specialized-routing team.

Scope: claim-level overlap between the **frozen** candidates and primary literature.
- CAND-1.1 (A4 cascade with the A2-C1-r1 subsolver).
- FROZEN_C3 (A3-C3-r2 corridor subsolver).

This file supersedes nothing. The working history (Phases 1–3, including superseded verdicts) is in
`NOVELTY_MATRIX.md`. The bibliography with access depth for every source ID is in `SOURCES.md`.

## 1. Method and limits

**Search method.**
- WebSearch / WebFetch and direct PDF or full-text retrieval from arXiv, NSF PAR, publisher-hosted working papers,
  author pages and Optimization Online. 78 catalogued sources.
- The alphaXiv search tool was not available in this session.

**Access-depth codes** (what A6 actually read):
- **FT**: full text, cited sections read.
- **FT-p**: full text, named sections only.
- **ABS**: abstract or landing page.
- **SEC**: known through a cited secondary source, or bibliographic memory (flagged in `SOURCES.md`).

**Not openly accessible.** Two potentially decisive sources:
- Orda & Rom 1991, *Minimum weight paths in time-dependent networks* (Networks 21);
- Ioachim et al. 1998, Networks 31 (SPPTW with linear node costs).

Their content is known only through Dean (2004 report) and the Lera-Romero et al. preprint.

**Interpretation rule.** "No matching paper found" means only that a bounded search on 2026-10-03 did not find one. It
never establishes first-ever novelty.

**No performance content.** A6 ran no code and read no A5 timed results. Every "benefit" below is conditional on A5's
equal-limit evaluation.

## 2. Problem-class positioning

P0 is earliest admissible arrival with M ≈ 30 scenario-wise additive exposure budgets, on a turn-state lattice with
gated waiting. Its pieces are established:
- **Problem class.** P0 is a **time-dependent shortest-path problem with resource constraints** (SPPRC: Irnich &
  Desaulniers S28 ABS; Dabia et al. S30 FT-p; Lera-Romero et al. S31 FT-p).
- **Waiting.** It adds a **location- and time-dependent waiting policy**: Orda & Rom S07 FT-p; Dean S09 FT-p;
  Cai–Kloks–Wong S12 ABS; He et al. S19 FT-p.
- **Complexity context.** Static RCSP is NP-hard (S42). Scenario min–max SP is NP-complete with 2 scenarios (S44 ABS).
  Waiting limits at a node subset are NP-hard (S19 Thm 4). Generalized time-dependent objectives are NP-hard, and cost
  profiles can have 2^Ω(n) bend points (S16 FT-p, Thms 1–2).
- **Circling is expected.** Forbidden waiting creates loops and even infinite optimal paths in continuous time (S07
  §3.2). Maximum-wait rules force cycles, and earliest-arrival walks keep removable cycles (S24 FT-p). Restricting to
  simple paths is NP-hard (S25 ABS).
- **Applications.** Dose-aware evacuation routing exists: S50 tank-farm thermal dose (weighted bi-objective Dijkstra);
  S51 threat-field exposure with waiting on a time-expanded graph; S55 / S71 cumulative-dose A* and Dijkstra; S52
  RESCUE; S53 time-expanded wildfire evacuation flows. Robust shortest paths over scenarios use min–max or min-regret
  objectives (S44, S69, S70) and robust RCSP (S46, S47). **None located enforces per-scenario cumulative budgets under
  a time objective with refusal certificates.**

## 3. Claim-level matrix (final verdicts)

Verdicts:
- **EQ** = equivalent to published work;
- **SPEC** = specialization of a known method;
- **COMB** = composition of known pieces without a located precedent for the composition;
- **PNIS** = possibly new in scope (bounded search; not first-ever).

| # | Claim (as realized in the frozen candidates) | Closest primary works (depth) | Already covered | What differs | Final verdict |
|---|---|---|---|---|---|
| 1 | Waiting cost absorbed into a cumulative node potential (U = V − Φ), so waiting chains are constant; wait closure = running minimum within allowed-wait components (A2-C1-r1) | **Dean 2004 eqs. 3–4 (FT-p)**: cumulative waiting cost CCW, Q = P + CCW, windowed minimum − CCW, forbidden waiting = ∞ unit cost, O(mT) (Chabini & Dean); Desaulniers & Villeneuve 2000 (ABS, constant-rate case); Johnson-type potentials (SEC) | The transform and the sliding/running-minimum closure for time-varying, gated waiting costs | Forward direction; applied on piecewise-affine segments | **EQ** |
| 2 | Exact piecewise-affine segment labels over integer ticks, per-tick min-merge, label-correcting over dirty intervals (A2-C1-r1) | Profile search link/merge (S15 FT-p, S16 FT-p); partial dominance with interval domains (S31 FT-p; Ioachim via S31); Dean's PWL chronological algorithm (S10 FT-p); Orda & Rom function label correcting (S07 FT-p, S65 SEC) | Exact piecewise-linear value functions of time propagated with lower-envelope merges | The function is the TE minimum-dose value per (turn state, tick) on a lattice | **SPEC** |
| 3 | Exact budget cut on segments; goal test on EDGE arrivals before closure or pruning | Feasible-time domain restriction (S31 FT-p); RCSP feasibility (S28 ABS, S36 ABS); extension-set dominance (S30 FT-p Def. 1) | Domain cuts by resource feasibility; dominance only with identical extensions | P0 goal semantics (enter destination by an edge) | **SPEC** (correctness requirements) |
| 4 | IDEP as a whole: segment-compressed forward Chabini–Dean-type DP + exact budget cut + A*-key/incumbent pruning + lazy window-limited exact dose functions + resumable windows | Rows 1–3; A* (S05 SEC); pulse/WC-BA* incumbent pruning (S36 ABS, S38 FT-p); DDD (S20 ABS) | Each ingredient | The integrated scalar solver for budgeted earliest arrival with gated, time-varying waiting cost on a turn-state lattice | **COMB** (pending P3-F1) |
| 5 | Relaxation then activation of the most violated scenario budget, with conditional certificates | DSSR (S33 ABS); CBS (S06 ABS); C&CG (S48 ABS); dynamic neighbourhood augmentation (S63 FT-p) | Exact relaxation, add violated constraints, re-solve; certificates by relaxation logic | Scenario-wise integrated-dose budgets | **EQ** (scheme) |
| 6 | Union zero-exposure relaxation (union SIPP) as lower bound / first stage | SIPP (S01 FT); relaxation bounds (S17 FT-p) | Safe-interval earliest arrival; admissible relaxations | Union of 30 members' forbidden sets; waiting only at waitable nodes | **SPEC** |
| 7 | Union zero-exposure screen in place of 30 per-member checks (A4-C2) | Conservative broad-phase screening (S77 SEC); relaxation-feasibility logic (S33) | Sound sufficient-condition check with exact fallback | HZ-1-specific lemma: route ends before the earliest member hazard arrival W(c), so every integral is exactly 0 | **EQ** (standard bound check; the lemma is model-specific) |
| 8 | Horizon windows (⌈1.25 L⌉ + m_max, ×2 growth, snap 0.8 K, refusals only at K, resume) (A4-C1, A2 PO10) | Iterative deepening (S76 SEC); DDD (S20, S21); deadline-bounded TE (S09, S11) | Progressive horizons with exact final step | Proved window/refusal/resume semantics | **SPEC** |
| 9 | Bound-derived corridor: edges with forward earliest arrival ≤ U and backward time bound ≤ U, then unchanged TE/r3 on the compacted subproblem (FROZEN_C3) | Forward+backward arc elimination (Aneja et al. 1983; Dumitrescu & Boland 2003 — S73 SEC); bounded-search valid domains (S38 FT-p Lemma 1); time-dependent backward search on lower bounds (S17 FT-p); FIFO TD Dijkstra (S10 FT-p, S27 SEC) | Network reduction by bounds before an exact solver | Time-dependent turn-state version with forbidden departures, block backward bounds, deadline windows | **SPEC** |
| 10 | Floating-point discipline: ε-band decisions, exact-rational recheck, Neumaier prefix sums, runtime per-state error bound (D1 / D1.1, A2 L16b) | Floating-point filters with exact fallback (S74 SEC); compensated summation and running error analysis (S75 SEC) | Decide when certified, otherwise go exact or abstain | Applied to budget decisions in route certification | **EQ** (engineering practice) |
| 11 | P1 lexicographic (arrival, then driving time) track against circling, P0 untouched | Walk criteria and combinations under waiting constraints (S24 FT-p); time + extra cost (S16 FT-p); lower-bound-attained certificate (standard) | Secondary criteria on earliest-arrival walks | Exact P1 DP over (state, tick) with M budgets as a separate track | **EQ** (concept) / **SPEC** (implementation) |
| 12 | Interval/safe-interval dominance with time-dependent wait dose (anticipated C6; not used by the frozen IDEP, which keeps only per-tick minima) | MO-SIPP (S02 FT: constant costs, wait everywhere); constant-rate wait cost (S31, S66); extension dominance (S30 Def. 1) | Constant-cost and constant-rate cases; the generic sufficient condition | Time-dependent wait dose + gated waiting | **SPEC** |
| 13 | Exact cascade / portfolio with cheap diagnostics (CAND-1.1 configuration structure) | Rice (S57 SEC); SATzilla (S61 ABS); Kotthoff (S59 FT-p); Kerschke (S62 ABS) | Per-instance staged solvers; charged selection cost | All stages exact; routing-specific stages | **EQ** (framework) |
| 14 | **CAND-1.1 as an integrated system** (support → union relaxation → screen → activation with windows → IDEP / r3 → optional P1) | All of the above | — | One certifying pipeline for scenario-budgeted earliest arrival with gated waiting | **COMB**; **PNIS only as an integrated application system** |
| 15 | **FROZEN_C3 as an integrated subsolver** | Row 9 + exact TE (S09, S11) | — | — | **SPEC** |
| 16 | Application formulation: scenario-wise thermal-dose budgets + refusal certificates for evacuation routing | S50–S55, S71 (dose minimization); S44, S46, S47, S69, S70 (robust objectives) | Dose-aware routing; scenario robustness as an objective | Per-scenario budget constraints under a time objective, with explicit cap ≠ refusal semantics | **PNIS** (formulation only; generated hazards, not real-fire validated) |

## 4. Contribution statement (approved wording for reports; does not overclaim)

> We implement exact, certifying solvers for earliest admissible arrival under scenario-wise integrated
> thermal-exposure budgets on a turn-constrained road lattice with gated waiting, time-dependent forbidden departures
> and destination dwell windows. The solvers compose established techniques: a zero-exposure union relaxation
> (safe-interval type), a conservative zero-exposure screen, relaxation-then-activation of violated scenario budgets
> (as in decremental state-space relaxation / constraint generation), deadline windows, and forward/backward-bound
> corridor reduction (as in classical arc elimination). For the single-active-scenario subproblem they use the
> classical location-dependent-waiting-cost dynamic program (Dean 2004; Chabini & Dean), carried on piecewise-affine
> segments over integer ticks with exact budget cuts and A*-style ordering; time-varying waiting exposure is absorbed
> into a cumulative node potential, so waiting chains are constant. The defensible contributions are threefold. First,
> the explicitly proved adaptation lemmas for this setting (edge-entry goals, gated waiting, window and refusal
> semantics, floating-point filtering with exact fallback). Second, exhaustive agreement with independent oracles on
> small instances. Third, whatever equal-budget, held-out measurements show, reported per factor with regressions. We
> claim no new algorithmic principle, no general superiority over A* or the time-expanded DP, no first-ever novelty,
> and no physical safety. Roads are historical OSM geometry; hazards, members and destinations are generated.

## 5. Claims not supported

- "A new algorithm or algorithm family" for time-dependent routing, or "first" anything.
- "Our potential/interval method is novel." The transform is Dean 2004 / Chabini & Dean; interval function labels are
  profile search and partial dominance.
- "Beats A*" in general. A* is a search order. Only matched-semantics, equal-limit comparisons on stated benchmarks
  are admissible.
- "The screen / corridor / windows are contributions." They are standard bound checks, network reduction and
  iterative deepening.
- "P1 solves circling." That holds only where P1 is certified. Development showed UNRESOLVED_P1 on 3 of 4
  exposure-binding road queries.
- Any real-fire, life-safety or forecast-skill claim.

## 6. Remaining falsification tests

| ID | Test | Kills / downgrades |
|---|---|---|
| P3-F1 | Read the full texts of Orda & Rom 1991, Ioachim et al. 1998 and Dean's thesis on PWL TDSP algorithms (cited as [7] in S10) | IDEP-as-a-whole COMB → SPEC if any contains PWL/segment label correcting with time-varying waiting costs and forbidden waits |
| P3-F2 | A5 equal-limit pairs A2-FULL vs AS-EAGER-REPLICA and A2-WIN vs TE-WIN on DEV and held-out sets | Representation-benefit claim if there is no gain in resolved count or charged time, or any status/arrival disagreement |
| P3-F3 | Batz–Sanders-type gadget (non-waitable origin, 2^k shifted dose dips): IDEP pieces vs ticks, and time vs TESolver | Must be reported as the worst case; any "always compresses" wording |
| P3-F4 | Corridor-size distribution on held-out queries, split into refusals, loose L and tight L | Corridor benefit outside the regime where it holds (development showed regressions on refusals) |
| P3-F5 | Screen: unsound passes vs A5's independent checker on held-out queries | Screen soundness (must be 0); any resolved-count claim (latency only) |
| P3-F6 | FP_BAND / FP_BOUND abstention counts per arm | "Exact" arms that abstain (e.g. C3 inside CAND-1 gave FP_BOUND on 6/6 development activation queries) |
| P3-F7 | P1 arrival equals P0 arrival on every query; UNRESOLVED_P1 rate | Any P1 claim beyond certified queries |
| P3-F8 | Factorial attribution: credit only for factors that A5's configurations isolate | Per-component credit not isolated by the configurations |

## 7. Key sources and access depth (full list: `SOURCES.md`)

| ID | Source | Depth | Role in verdicts |
|---|---|---|---|
| S09 | Dean 2004, Min-cost paths in TD networks with waiting policies (Networks 44) | FT-p (§1–2, eqs. 1–4) | Row 1 (EQ): cumulative waiting-cost potential + windowed minimum |
| S10 | Dean 2004, Shortest paths in FIFO TD networks (MIT report) | FT-p | Rows 2, 9: function-label correcting; PWL chronological algorithm; FIFO TD Dijkstra |
| S07 | Orda & Rom 1990, J. ACM 37 | FT-p | Waiting models; loops and infinite paths under forbidden waiting |
| S65 | Orda & Rom 1991, Networks 21 | SEC | Unread decisive check (P3-F1) |
| S01 | Phillips & Likhachev 2011, SIPP | FT | Row 6 |
| S02 | Ren et al. 2022, MO-SIPP (RA-L) | FT | Row 12: constant costs, wait everywhere |
| S15 / S16 | Batz et al. TCH; Batz & Sanders ESA 2012 | FT-p | Row 2; worst-case 2^Ω(n) bend points |
| S30 | Dabia et al. 2011 WP / 2013 TS | FT-p (WP) | Rows 3, 12: PWL arrival-function labels; extension dominance |
| S31 | Lera-Romero et al. 2020 preprint / IJOC 2022 | FT-p | Rows 2, 3, 5: partial dominance on interval domains; time-indexed completion bounds; DNA |
| S29 | Ioachim et al. 1998 | ABS + SEC | Unread decisive check (P3-F1) |
| S33 | Righini & Salani 2008, DSSR | ABS | Row 5 |
| S38 | Ahmadi et al. AAAI-21 | FT-p | Rows 4, 9: bounded valid domains, incumbent pruning |
| S17 | Nannicini et al. 2012 | FT-p | Rows 6, 9: backward search on lower bounds |
| S24 | Himmel/Bentert et al. 2020 | FT-p | Row 11; circling as an expected phenomenon |
| S73 | Aneja et al. 1983; Dumitrescu & Boland 2003 | SEC | Row 9: arc elimination by forward+backward bounds |
| S74 / S75 | FP filters (Fortune & Van Wyk; Shewchuk); Neumaier; Higham | SEC | Row 10 |
| S50 / S51 | Chou et al. 2025; Cooper & Cowlagi 2018 | ABS / FT | Row 16 |
| S44 / S69 / S70 | Yu & Yang 1998; Conde et al. 2018; Hansknecht et al. 2018 | ABS | Row 16 (robust objectives, not budgets) |

## 8. Verdict history (abridged; details in `NOVELTY_MATRIX.md`)

- **Phase 1** (abstract-level for several sources):
  - interval dose dominance C6 = PNIS;
  - symbolic dose propagation C1 = COMB;
  - per-member bounds C2 = SPEC.
- **Phase 2** (full texts of MO-SIPP, Dabia, Lera-Romero):
  - C6 → SPEC;
  - C1's representation → EQ (application COMB);
  - C2 confirmed SPEC.
- **Phase 3** (frozen candidates; Dean 2004 re-read):
  - waiting-potential transform = EQ (Dean / Chabini & Dean). A6's own Phase-1 "suffix-minimum" hint is corrected to
    cite Dean;
  - IDEP-as-a-whole = COMB;
  - screen = EQ;
  - corridor = SPEC;
  - windows = SPEC;
  - FP discipline = EQ;
  - P1 = EQ/SPEC;
  - integrated CAND-1.1 = COMB, PNIS only as an application system.
