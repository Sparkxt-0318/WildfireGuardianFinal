# Owner handoff — specialized certified routing research (2026-10-03/04)

Root: `outputs/routing_specialized_research_20261003_v1/`. Coordinator plus six agents (A1 maths/adversary, A2
time/state, A3 bounds, A4 adaptive architecture, A5 independent evaluator, A6 literature). Every hypothesis, review,
failure, retirement and decision is in `EXPERIMENT_LEDGER.jsonl` (coordinator lines plus 150 merged agent lines).

## 1. Decision

**Go, narrowly: adopt the A4-C2 union zero-exposure screen as a drop-in change to the current validated
architecture.** It is exact (proved by A1, re-checked independently by A5 on every route), and it is the only component
that improved sealed-test performance.

**Do not claim a new routing algorithm.** The specialized exposure subsolvers (A2-C1 interval/potential solver, A2-C2
vector solver, A4-C1 windows) are exact and fast where exposure budgets bind. On the sealed road sets, however, no query
ever activated an exposure constraint, so they were never exercised. Their benefit remains a **narrow-domain,
development-only** result.

Recommended operational configuration: **CAND-2** (`a4_portfolio/FROZEN_v2_1.json`, config `CAND-2`). It behaves like
the screen on ordinary queries and keeps the faster exact subsolver for the rare exposure-active ones. The plain
validated architecture (AS-EAGER) stays the reference comparator and fallback.

## 2. What was measured (sealed final test, protocol v1.0)

Commitment `a628fe00…6be2` was verified at reveal. Arms were frozen before unsealing (`coordination/FINAL_ARMS.json`
`084bcc9d…`). Run 2026-10-04 06:11–10:44 on the single M5 Pro (load median 6.1). Limits per query: 60 s cold charged
(preparation, selection, failed attempts, search and verification all charged), 3,072 MB RSS, fresh process, BLAS = 1,
ε_fp band rule D1 for every arm.

| FINAL-ROAD (482 queries, W-MAIN 331 / W-SMALL 151) | Resolved | Median latency vs AS-EAGER [95% CI] |
|---|---|---|
| AS-EAGER (current validated architecture) | 482 (380 routes, 102 proven refusals) | 1 (median 0.664 s, p95 2.12 s, max 3.76 s) |
| CAND-2 | 482 | **0.922 [0.904, 0.940]**, p = 8e-34; median 0.560 s, p95 1.52 s; faster on 332/482 |
| CAND-2+VEC | 482 | 0.923 |
| SCREEN only (ablation) | 482 | 0.921, so the whole gain is the screen |
| Repaired label A* inside activation loop | 482 | 1.001 (n.s.) |
| A3-C3 corridor inside activation loop | 482 | 1.189 (slower) |

- **Correctness:** 0 disagreements for every arm on every set. Every road route passes the independent lane-F
  evaluator for all 30 members. The independent audit certified every route optimal and every refusal (516
  relaxation, 78 infeasible at issue, 18 disconnected).
- **Stress tracks @E200 / @E100:** same statuses and the same ~0.92 ratio. Zero exposure activations even at the lower
  budgets.
- **Memory:** peak RSS medians 645 MB (AS-EAGER) vs 634 MB (CAND-2). No memory claim.
- **Warm mode:** with equal per-instance preparation charging, CAND-2 per-query time is 0.29× AS-EAGER.
- **Synthetic sets** (FINAL-SYNTH-TINY 168, MEDIUM 36), on millisecond problems:
  - Resolved counts are equal, and the candidates are slower: CAND-2 3.6× on TINY, 1.8× on MEDIUM; C3 about 16×.
  - Full-member label A* and Dijkstra each leave one MEDIUM case unresolved, and are much slower.
- **Regressions:**
  - No query went from resolved to unresolved.
  - Small cold losses ≤ 0.19 s (CAND-2: 14 queries; CAND-2+VEC: 11, largest +0.53 s).
  - Synthetic slow cases are listed in `a5_evaluator/FINAL_EVAL_REPORT.md`.

The coordinator independently recomputed statuses, activation counts and median ratios from the raw row files and
matched A5.

**Absolute size of the gain:** about 0.04–0.1 s per query, on sub-second queries. It is statistically clear and
practically modest.

## 3. Development-only evidence (not sealed confirmation)

- **Six exposure-active W-MAIN queries** (already exposed, used to develop the methods):
  - CAND-1 was 2.4× faster in total than AS-EAGER, winning 6/6.
  - The A2-C1 subsolver alone ran at 0.53× AS-EAGER's time; with windows, 0.43×.
  - Representation size was 27–27,000× smaller than the dense state-tick table on these queries.
  - Worst case: A1 built a gadget with one affine piece per tick, so there is no asymptotic size guarantee.
- **Multi-active regime:**
  - The one DEV road query with 2 active members (W-SMALL, @E200) is unresolved by every arm. The r3 vector DP hits
    60 s; A2-C2-r1 caps on an exact Pareto blow-up at about 6 s.
  - On 30 synthetic multi-member cases, A2-C2-r1 resolved 2 that r3 could not (development only).
- **Fresh generated road hazards** (HZ-1 on real Nangok roads) almost never make exposure budgets bind. This is why
  the targeted regime is rare and the sealed test could not confirm subsolver gains.

## 4. Circling (central modeling issue): diagnosis and owner decisions

The 534-traversal STRESS-10-Q07 route came from the tie order of the bound + deep-tie label A* (it dives depth-first
and prefers driving over a one-tick wait). P0 did not force it: same-arrival alternatives exist.
- A 39-edge loop-free route passes 30/30 members.
- A 19-edge "hold at origin, then drive the shortest legal path" route (2.197 min driving, certified minimal) also
  passes 30/30.

Further findings:
- In general P0 can genuinely require cycles (CE3: no waitable node).
- In the sealed evaluation no road route from any arm repeated an edge.
- The experimental P1 stage (earliest arrival, then minimum driving time; same feasible set, so P0 certificates
  transfer) returned 380/380 P1-optimal routes. It reduced driving on 0 of them, because the current subsolvers already
  return non-circling routes.

**Decisions needed from you** (presented together; none blocks the result above):
1. **Adopt P1 reporting** (keep P0 as the certified quantity, report a P1 route)? This costs a small extra stage.
2. **Secondary objective** for P1:
   - (a) driving time (implemented default);
   - (b) edge count;
   - (c) lowest worst-member exposure;
   - (d) fewest waits.
3. **Holding policy (P2):** restrict where or how long vehicles may hold? This shrinks the feasible set; refusals
   would be P2 refusals, not P0.
4. **Traversal caps or penalties (P3):** only if circling must be excluded even when it is the only way to pass time.
   Caps make the state history-dependent and can refuse valid P0 routes.
5. **Destination question:** is "first arrival at a destination that is then safe for a 30-minute dwell" the intended
   evacuation question?

## 5. Correctness findings that matter beyond this study

- **F1 defect in the prior label engine** (`routing_controlled_validation_20261003_v1/engine.py`, `engine_bound.py`; the
  same pattern is in `routing_novelty_validation_20261003_v1/astar_exact.py`):
  - A WAIT label at a waitable destination can dominate an EDGE goal arrival at the same (state, tick). This gives
    false refusals or late arrivals reported as optimal.
  - It was found independently by A1, A3, A5 (and confirmed by A2, A4); executable cases are in `a1_math/counterexamples/`.
  - Earlier "zero disagreement" claims for that engine (fresh48, the 240 audit cases) hold only on generators without
    waitable destinations, which is why they missed it.
  - TESolver, r3 and the validated ActiveScenarioArm are unaffected.
  - STRESS-10-Q07 remains optimal because it arrives exactly at the valid union bound L.
- **Origin = destination while infeasible at issue:** `engine.py` returns an inadmissible zero-leg route (CE7);
  standalone r3 misses the zero-leg route (CE8).
- **Floating point:** the frozen TESolver refuses a route whose exact dose equals the budget (its float sum is 8e-12
  high). Decision D1 (ε_fp = 1e-6 band, strict recertification, UNRESOLVED_FP_BAND) now governs every arm.

## 6. Novelty (A6, primary sources; see NOVELTY_MATRIX.md)

No component is new at the literature level:

| Component | Verdict | Closest prior work |
|---|---|---|
| Wait-potential transform | Equivalent | Dean 2004 |
| Interval/envelope labels | Specialization | Lera-Romero 2022, Ioachim 1998 |
| Activation | Equivalent | Righini & Salani |
| Screen | Equivalent | Standard bound check |
| Windows and corridor | Specialization | Known iterative deepening and edge-pruning ideas |
| P1 | Equivalent in concept | Himmel/Bentert et al. |
| CAND-2 as a whole | Combination | — |

Defensible contribution:
- a proved, exactly certifying composition specialized to scenario-wise thermal budgets with explicit
  cap-versus-proof semantics;
- independent-oracle agreement;
- the measured screen latency gain.

No first-ever claim, no general superiority over A*, no physical-safety claim. "No matching paper found" for the
scenario-budget formulation is not proof of novelty.

## 7. Limits and boundaries

- **Real versus assumed:**
  - Roads are historical Korean OSM (Nangok).
  - Hazards, members, thresholds and destination conditions are generated or assumed.
  - Travel speeds and passability are assumed.
  - Track 3 (real-observation/payload integration) is NOT EVALUABLE: the mentor forecast payload has not arrived and
    no eligible timed thermal-road reference exists.
- **What is certified:** results are conditional on the 30 planning members, the 2^-10 min lattice, HZ-1 parameters,
  and the support rule. Caps, timeouts, RSS limits and band cases are UNRESOLVED, never infeasible.
- **Assumptions not independently proved:** SIPP exactness on the union geometry (empirically tick-identical to TE on
  72/72 instances); lane-F and bench geometry correctness. See `MATHEMATICAL_FOUNDATIONS.md`, open obligations.
- **Process incidents (all in the ledger):**
  - An API outage killed A5 once; its detached jobs continued.
  - A3 edited a file after it was frozen. The bytes were later restored and hash-verified, CAND-1 v1.0 never produced
    evaluation rows, and the charter now makes frozen files immutable.
  - Coordinator timestamps before COORD-TSFIX were hand-estimated.
  - The KSA session shared the lock protocol; no other owner ran during the final window.
- **Selection after development:** the CAND-2 window-regrowth rule was chosen after seeing DEV results and only affects
  cost.

## 8. Blockers and next steps

1. **Merge the zero screen into the validated pipeline** (small change; keep A5's lane-F recomputation in acceptance).
2. **Find a regime where exposure binds before claiming subsolver value.** Use calibrated thermal inputs (the mentor
   payload) or pre-registered hazard families that make budgets bind. Then rerun the frozen CAND-2 vs AS-EAGER on a
   new sealed set. This is the strongest remaining hypothesis: the exact interval subsolver gives 2–4× on
   exposure-active queries.
3. **Multi-member conflicting budgets** are the open algorithmic problem. The exact Pareto set blows up, and only
   stronger dose-to-go bounds (weak so far) or a different relaxation could help.
4. **Open proof obligations:** SIPP exactness; an end-to-end fp bound for TE table evaluation; an exact road recheck
   for band cases.
5. **Your circling-policy decisions** (§4).

## 9. Where things are

| Item | Path |
|---|---|
| Mathematics | `MATHEMATICAL_FOUNDATIONS.md` (copy of `a1_math/MATHEMATICAL_FOUNDATIONS.md`); `a1_math/CERTIFICATE_AUDIT_CHECKLIST.md`; reviews in `a1_math/reviews/` |
| Novelty | `NOVELTY_MATRIX.md` (copy of `a6_literature/NOVELTY_MATRIX_FINAL.md`); `a6_literature/SOURCES.md` |
| Protocol, frozen | `a5_evaluator/EVAL_PROTOCOL.md` v1.0 (`0ada3c01…`); `coordination/DECISION_D1…`, `DECISION_D2…` |
| Final results | `a5_evaluator/FINAL_EVAL_REPORT.md`, `a5_evaluator/results/FINAL/` (TABLES.md, raw rows, FILE_MANIFEST 151/151) |
| DEV results | `a5_evaluator/DEV_EVAL_REPORT.md` |
| Frozen code | `a4_portfolio/FROZEN_v2_1.json` (CAND-2/+VEC/+P1/SCREEN; `0f06b7b6…`), `FROZEN_v2.json`, `FROZEN_v1_1.json`, `FROZEN.json` (historical); `a3_bounds/FROZEN_C3.json` (`d44982c3…`) |
| Reproduction | `REPRODUCE.md`; `FILE_MANIFEST.json` covers this delivery |
