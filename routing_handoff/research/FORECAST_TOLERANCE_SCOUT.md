# OWNER_HANDOFF: creative novelty scout, 4 October 2026 (Asia/Shanghai)

Packet: `outputs/creative_novelty_scout_20261004_claude_v1/`. The requested name `creative_novelty_scout_20261004_v1/` was already created by a concurrent session (07:42 to 08:02 UTC, lanes physics/algorithms/data_systems/evaluation, 82 files); it was not modified and this packet does not reconcile with it. Its strongest lead (PH1 passage-topology summaries, AL2 irreducible refusal explanation, DS1 identified sets) is different from this packet's (per-route forecast tolerance); the owner should read both OWNER_HANDOFFs.

Coordinator plus four separately owned lanes run in two waves
(P physics, A algorithms, D data/systems; then C independent critic). Candidate work and evaluation have separate
authorship. Lane files are preserved exactly as written; where the critic found overreach, the critic's wording
governs and is repeated here. Everything ran in an isolated Linux container (2 CPUs, 7 GB, Python 3.13.16, NumPy
2.5.3) on staged read-only copies of the frozen bench RB-1.0 and the delivered routing package. Nothing frozen,
concurrent or delivered was modified. No mentor forecasting model was touched. All evidence is
REAL_ROAD_CONTROLLED_HAZARD (real Nangok roads, generated HZ-1 hazards) or labelled SYNTHETIC. No real-fire
validation, no physical-safety claim, no "validated novelty".

Scout wall time: 07:39 to about 09:20 UTC (about 100 minutes), including coordination.

## 1. Bottom line

One direction survived adversarial testing and is worth implementing before 18 October: **the per-route forecast
tolerance (lane A HA1 + HA2)**. For every escape route the solver already certifies, we can compute, in closed form
and without re-solving, how many minutes earlier (delta_plus*) and later (delta_minus*) the forecast fire could arrive
before the route stops passing the peak, wait and destination-dwell conditions, including the full windowed set
R(r) when the answer is not a single interval. On the bench this was exact against brute force in two independent
checkers (19,591/19,591 and 432/432 points), produced a reproducible counterexample to a natural assumption (a
SLOWER fire breaks two real-road routes because the road they cross after burnout is still burning), and gave a
forecast-update check that was sound in 2,208 of 2,208 arrival-time updates. It is a dispatcher-facing number, it is
offline, and it reuses the certificate we already pay for.

It is NOT yet a complete object: the cumulative-dose budget is outside the closed form (synthetic counterexample
with a wait next to a sub-peak emitter reaches 750 kJ/m^2 > 600), and the update filter as written does not refuse
burn-duration (tau) or post-burn policy changes (critic shows tau x 2 and a PASSABLE-to-CLOSED flip pass the
arrival-only premise and are infeasible). Both gaps have known fixes (recheck dose at the window endpoints; refuse or
extend the premise for tau/policy). W-MAIN (2,456 nodes, longer routes, waits) is untested.

Everything else is either an engineering finding with presentation value (the delivered package re-validates the
road graph on every request, 4.7 to 4.8 s of a 5.0 s request on the 5,765-node fixture; a compact (A, tau) payload
is 100 to 1,000 times smaller than the dense schema with exact passability agreement), a surrogate-bias finding that
changes bench difficulty but not physics (HZ-1 MAX-over-emitters flux is 2 to 5 times lower than additive flux on
road cells), a one-sided bound that is correct on the bench generator but has no transfer path to the mentor's model
(T_lo pre-arrival certificate; the upper side is vacuous), or a proposal.

## 2. Top findings (status assigned by the critic, lane C)

| # | Finding | Status | Evidence |
|---|---|---|---|
| 1 | Per-route shift-feasibility set R(r) and tolerance delta_plus*/delta_minus* (A HA1) | MECHANISM_SUPPORTED for P0-GEO (peak, wait, dwell); NOT for the dose budget | 20 W-SMALL instances, 159 certified routes; closed form = brute force 19,591/19,591 (lane A) and 432/432 incl. window endpoints (critic, own checker); 87/159 finite delta_plus*, median 41 min, min 0.9 min; CLOSED policy always a half-line (40/40); PASSABLE: MULTI-03-Q02/Q09 feasible now, infeasible 9 to 22 min slower, feasible 21.4 to 22.4, infeasible to 35 min slower |
| 2 | No-replan filter from the per-cell box (A HA2) | MECHANISM_SUPPORTED for arrival-time updates with tau and policy fixed; NOT READY as a filter | 0 unsound accepts in 1,908 (lane A) + 300 non-uniform in-box (critic) updates; premise holds 61 % synthetic / 96 % member-swap; recall 74 % / 96 %; critic C2b: tau x 2, x 3, x 5 and a policy flip pass the A-only premise on MULTI-03-Q02/Q09 and are infeasible |
| 3 | HZ-1 MAX-over-emitters flux is biased low vs additive flux (P H2) | MECHANISM_SUPPORTED as surrogate bias; no physical-accuracy claim | road-cell dose ratio SUM/MAX median 1.8 to 5.1 per member (pooled 3.9); critic reproduced 1.906 / 3.212 / 1.772 on three instances with own integration; peak closures +7.6 %; closed time per road cell 2 to 3x; CAPSUM = min(Q, SUM) passes single-emitter, line-source and saturation limits; superposition is standard (Drissi 2014 eq. 1, full text) |
| 4 | Delivered package re-validates graph per request (D H2) | MECHANISM_SUPPORTED (measurement); remedy PROMISING_BUT_UNVALIDATED | core.py:135 calls validate_graph unconditionally although session.py:16 validated once; validate_graph 4.66 s (lane D, n=5) / 4.79 s (critic, n=3) of a 4.7 to 5.0 s request; search is 1 expansion; cache remedy not implemented (owner's package) |
| 5 | Compact hazard payload (D H1/H7) | MECHANISM_SUPPORTED (engineering, not novelty) | 960 member rows; edge_ok agreement 100 % on 64,000 samples (half endpoint-adversarial); dense exact schema 83.7 MB / 3.54 GB per member (W-SMALL / W-MAIN) vs (A, tau) 25.6 KB / 171 KB vs intervals 7.6 KB / 27 KB; intervals lose the dose channel, so ship (A, tau) |
| 6 | One-sided pre-arrival bound T_lo from a declared speed box (P H1) | MECHANISM_SUPPORTED on the bench generator only | 12 instances x 30 members (lane P) + 5 instances incl. 3 new (critic): 0 arrivals below T_lo at k >= 2, failures at k = 1; T_lo conservative by 6 to 30 min median; T_hi vacuous (0 to 25 % of road cells reached before H); adversarial +30 deg wind detected on only 2/12 |
| 7 | Feasibility is two-sided and policy-dependent (P H4) | MATHEMATICAL RESULT (elementary) | bad set [A, inf) monotone under CLOSED_AFTER_IGNITION; [A, A+tau] not monotone under PASSABLE_AFTER_BURNOUT; demonstrated on bench by finding 1 |
| 8 | Safe-departure windows per origin (A HA3/HA8) | PROMISING_BUT_UNVALIDATED | brute force only (1-min grid, 93 queries): CLOSED 20/20 prefixes (single deadline); PASSABLE 4/73 non-prefix, one two-window case [0,22] and [113,149] min (LINE-00-Q00); reverse solver not built |
| 9 | Member dominance pruning (A HA4) | REJECTED as pruning; MECHANISM_SUPPORTED as over-determination metric | 30/30 non-dominated under PASSABLE on 14/15 instances; leave-one-out changes a decision for 2 of 600 member-instance pairs |
| 10 | Single road-side flux sensor identifiability (P H5, D H4) | TOY DEMONSTRATION; pre-registered endpoint REJECTED AS WRITTEN (construction error retained) | MAX rule masks arrivals (series identical for different masked configurations) while SUM reveals them; two loggers at different front distances is the minimum design (proposal) |

## 3. Tested failures and corrections (retained)

* Lane A A1 pre-registration said a 0.25-min brute-force grid; a 1-min grid plus endpoint and midpoint probes was
  run. Unlabelled deviation; not material; must be labelled in any reuse.
* Lane A wording "exact two-sided shift-feasibility set R(r)" and "sound no-replan filter under both policies" is
  overreach; corrected wording in section 2 and in QUICK_TESTS.md.
* Lane P P1 tightness metric was mis-specified (labelled post-hoc replacement); P2 C1/C2 masking pair was mis-built
  (retained, post-hoc corrected pair labelled).
* Lane D D2 hypothesis as written (JSON parse dominates cold start) REJECTED: parse is 0.04 s; validation dominates.
* Lane A loads instances.jsonl rows containing descriptors_truth without popping them (unused; no truth value
  influenced any result; lane D pops them, which is the better practice).
* Lane A HA5/HA6 recorded as not novel (already in the frozen packet or trivial).
* alphaXiv search tools were unavailable in this session; one HAL paper returned 403; StochSIPP PDF not retrievable.

## 4. Recommendation

1. **Implement finding 1 as a reported field of every certified route** (delta_plus*, delta_minus*, and the window
   list under PASSABLE), with the dose budget rechecked at every window endpoint and at delta = 0, and with tau and
   post-burn policy declared as part of the premise. Effort: 1 to 2 days on the delivered package or as a
   post-processing script over its route output. Then run it on W-MAIN under a pre-registered protocol
   (IMPLEMENTATION_PATH.md).
2. **Fix the per-request graph re-validation** in the delivered package (hash-keyed cached validated graph,
   bounding-box line loop; exact, no result change). This is a presentation-reliability item for an offline venue
   with a 5-minute demo, not research.
3. **Record the HZ-1 MAX-vs-SUM finding as a bench-contract note** (candidate v1_1 variant CAPSUM). Do not change the
   frozen bench; do not claim physical accuracy for either rule.
4. **Do not pursue**: member dominance pruning, the dense-vs-compact payload as a research claim, the T_lo bound as a
   forecast product (keep as an internal screen idea), any Rothermel refit before 18 October.
5. **Next action for the owner**: decide whether the tolerance box becomes part of the KCF write-up; if yes, authorise
   the W-MAIN pre-registered run (IMPLEMENTATION_PATH.md step 2) and the two correctness fixes before it.

## 5. The strongest idea, in the required form

* **Strongest idea.** The per-route forecast tolerance: a closed-form, exact (for peak, wait and dwell conditions)
  set of time shifts of the forecast arrival field under which an already-certified route stays admissible, giving
  delta_plus* (how much faster the fire may be), delta_minus* (how much slower), the windowed set when non-monotone,
  and a cheap update check that avoids re-solving when a new forecast stays inside the box on the route's cells.
* **Why stronger than the current direction.** The current direction yields a 0.92x latency gain on a request whose
  cost is dominated by a 4.8 s validation defect, unpromoted specialists, and a bounds track whose V1 certificate was
  wrong. The tolerance is a new, defensible output (a number the fire department can act on), derived from the
  certificate we already compute, verified by two independent brute-force checkers with zero disagreement, and it
  reframes "re-solve on every forecast update" into "check a box on the route's cells", which was sound on all 2,208
  arrival-time updates tried. It needs no mentor payload to be built and tested.
* **Exactly what was demonstrated.** On 20 W-SMALL instances (real Nangok roads, generated hazards), 159 routes
  certified by an independent SIPP solver on the exact 2^-10 min lattice: closed-form R(r) equals brute-force
  recheck at every tested shift; 87/159 routes have a finite faster-fire tolerance (median 41 min, min 0.9 min);
  under CLOSED_AFTER_IGNITION the set is always a half-line; under PASSABLE_AFTER_BURNOUT two routes have windowed
  sets in the slower-fire direction (bench counterexample to monotonicity); the box filter had 0 unsound accepts in
  2,208 arrival-time updates; a SYNTHETIC counterexample shows the dose budget is not captured by the closed form; tau
  and policy changes are outside the premise and the filter as written does not refuse them.
* **What would falsify it next.** A closed-form vs direct-check disagreement on W-MAIN; a bench route with a wait
  whose dose crosses 600 kJ/m^2 inside the geometric window; a premise-true, tau-fixed, policy-fixed update whose full
  recheck fails; a premise hold rate below 5 % at realistic update magnitudes (sound but useless).
* **Verdict.** Deserves implementation plus one larger pre-registered experiment (W-MAIN, both policies, with dose
  endpoints and tau/policy guards). Not deployment, not a safety claim.
* **KCF SW-research criteria it supports** (no scores): design/methodology (a defined, controlled variable "minutes
  of forecast error tolerated", exact comparator, both policies as controlled conditions, explicit difference from
  trigger buffers which give a community lead time rather than a fixed-route tolerance); data
  collection/analysis/interpretation (counts, exact agreement, pre-registered failure conditions, byte-identical
  reproduction by an independent lane); creativity (two-sided, windowed, policy-dependent margin; the slower-fire
  counterexample found in the bench itself; update filter instead of re-solve); presentation (one sentence per
  route, computed offline). Research purpose is supported only if the write-up states the dispatcher question it
  answers.
* **Plain-English explanation the student can defend.** "Our planner already proves that an escape route is safe for
  every one of the 30 fire scenarios it was given. I added a second number to every route: how many minutes earlier,
  or later, the fire could actually arrive before that route stops being safe. I can compute it exactly without
  running the planner again, because each road cell only cares about one thing, when the fire reaches it. I checked
  the formula against brute force at about 20,000 points and it never disagreed. And it found something I did not
  expect on our real-road benchmark: a slower fire can break a route, because the road you planned to cross after it
  had burnt out is still burning. What it does not cover yet is the heat-dose budget and changes in how long cells
  burn; those I check separately."

## 6. Rubric map for the whole scout

* Research purpose: finding 1 and finding 8 give dispatcher-facing quantities (tolerance, deadline windows) that make
  the project's question ("how good must a forecast be before it deserves to change a protective decision")
  measurable per route; finding 3 and finding 6 sharpen what the bench can and cannot claim.
* Design/methodology: pre-registered tests with stated comparators and failure conditions in every lane; one
  unlabelled deviation recorded.
* Data collection/analysis/interpretation: all numbers are counts or exact agreements with seeds and manifests;
  critic reproduced lane A and lane P results byte-identically; findings 4 and 5 are reproducible measurements.
* Creativity: findings 1, 7, 8 and the measurement-design argument (finding 10).
* Presentation: finding 4 is the largest presentation risk found (offline venue, 5-minute demo); finding 5 removes a
  payload obstacle for an offline laptop.

## 7. Boundaries

Generated hazards, assumed speeds and passability, W-SMALL only for findings 1, 2, 8, 9; W-MAIN for parts of 3, 5, 6.
No mentor payload, no thermal truth, no historical fire. Timings are container timings under a lock and are not
comparable to Mac numbers. Independent review is same-model-family agent review, not human review. Prior-art searches
were bounded; trigger-buffer full texts were not read this session; "no match found" is not novelty.

## 8. Where things are

| Item | Path |
|---|---|
| All hypotheses incl. rejected | `IDEA_ATLAS.md` |
| Tests, raw evidence, comparators, limitations | `QUICK_TESTS.md`; lane `TESTS.md`, `*_RESULTS.json`, scripts |
| Prior art | `NOVELTY_CHECK.md`; lane `PRIOR_ART.md`; `lane_C_critic/CRITIC_REPORT.md` section 6 |
| What can be done before 18 Oct | `IMPLEMENTATION_PATH.md` |
| Critic report | `lane_C_critic/CRITIC_REPORT.md` |
| Coordination, brief, measurement log, timeline, manifest | `coordination/` |
