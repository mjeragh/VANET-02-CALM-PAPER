# CALM Paper — Submission Readiness Status

**Target:** IEEE Transactions on Intelligent Vehicles (T-IV)
**Current branch:** `fix/reviewer-ready-calm-paper`
**Last updated:** March 12, 2026

---

## Completed Fixes (Text-only, no experiments needed)

All applied in commit `db239fc` on `fix/reviewer-ready-calm-paper`:

### Tier 1 — Presentation cleanup
- [x] Capitalize co-author name ("Ebrahim" not "ebrahim") — line 39
- [x] Rename figures: removed `week5_` prefix from all 5 PDFs in `figures/`
- [x] Remove placeholder grant number from acknowledgments

### Tier 2 — Structural (theory demotion)
- [x] Removed "Theoretical Analysis" from contributions list in intro
- [x] Renamed Section IV-D from "Theoretical Analysis" → "Analysis of the KL Constraint"
- [x] Removed vacuous Remark 2 (admitted bound evaluates to -0.86)
- [x] Removed trivial Proposition 2 (asymptotic convergence) + proof sketch
- [x] Fixed factor-of-2 error in Theorem 1: bound is `2J_max/λ(t)`, not `J_max/λ(t)`
- [x] Softened language: abstract, intro, conclusion no longer claim "theoretical contributions" or "provable guarantees"

### Tier 3 — Content additions
- [x] Added unconstrained MADDPG row to Table I (54.8% agreement, 0.003 entropy, 0.96 reward)
- [x] Added paragraph explaining why MADDPG's 54.8% agreement is a mode-collapse artifact
- [x] Added "Relation to Prior Work" subsection in Discussion reconciling with CoEx paper's 54.83% number
- [x] Added CoEx self-citation (`jeragh2026coex`) to references.bib

### Tier 4 — Methodological details (reproducibility)
- [x] Specified discrete MADDPG adaptation: categorical softmax with temperature scaling
- [x] Added NGSIM discretization thresholds: ±0.5 m/s² (accel), ±0.5 lanes (lane change), lane-change priority
- [x] Clarified QMIX-CALM integration: KL applied to softmax of Q-values, frozen BC Q-networks, averaged over agents

---

## Remaining Fixes (Require running experiments in VANET simulator)

All experiments run from `/Users/mohammadjeragh/Source/VANET/MyScenarios/AnyScenario/`.

### Fix 6: Tune entropy-only baseline (HIGH PRIORITY)
**Why:** Central claim ("entropy alone fails") rests on a single untuned α=0.01. A reviewer asking "did you try other values?" undermines the paper.

**What to do:**
1. Run entropy-only MADDPG with α_ent ∈ {0.001, 0.01, 0.05, 0.1, 0.5, 1.0}
2. Each α × 5 seeds = 30 runs total
3. Report best-performing α in the paper
4. If all fail → claim is bulletproof
5. If some succeed → reframe CALM's advantage as "does not require entropy tuning"

**Where in paper:** Update Section V-C (Baselines) and Table I

### Fix 7: Add learning curves figure (HIGH PRIORITY)
**Why:** Paper is about a training algorithm but shows no training dynamics.

**What to do:**
1. Plot reward vs. episode for all methods (mean ± std across 5 seeds)
2. One figure with subplots or overlaid curves
3. Save to `figures/learning_curves.pdf`
4. Add figure reference in Section VI (Results), before or after Table I

**Data source:** Should be in training logs. Check `results/` or `logs/` directories. If not logged, re-run with logging enabled.

### Fix 8: Equalize or justify training budgets (MEDIUM PRIORITY)
**Why:** DAgger=50 eps, QMIX-CALM=100, CALM/GAIL=300 — unfair comparison.

**Options (pick one):**
- Best: Re-run DAgger for 300 episodes and QMIX-CALM for 300 episodes
- OK: Use learning curves (Fix 7) to show each method plateaued before its budget ended
- Minimum: Add a paragraph in Section V-E justifying different budgets

### Fix 11: Add safety metrics (MEDIUM PRIORITY)
**Why:** T-IV reviewers expect transportation metrics, not just ML metrics.

**What to do:**
1. Extract from simulation logs: collision rate (%), exit success rate (%), average speed
2. Add a row or column to Table I, or a new small table
3. These numbers likely already exist — check SUMO TraCI logs

### Fix 12: λ₀ and δ ablation (MEDIUM PRIORITY)
**Why:** These are CALM's two key hyperparameters. No ablation = obvious reviewer request.

**What to do:**
1. Run CALM with λ₀ ∈ {0.5, 1.0, 2.0, 5.0} (fixing δ=0.999)
2. Run CALM with δ ∈ {0.99, 0.995, 0.999, 1.0} (fixing λ₀=2.0)
3. 7 configs × 5 seeds = 35 runs
4. Report as small table or figure in Section VI

### Fix 15: Add MAPPO or SAC baseline (LOW PRIORITY, nice-to-have)
**Why:** MAPPO is current MARL SOTA (Yu et al. 2022, already cited). SAC has automatic entropy tuning.

**What to do:**
1. Implement MAPPO or SAC agent (likely ~200 lines based on existing agent structure)
2. Run 5 seeds × 300 episodes
3. Add to Table I

---

## Estimated effort for remaining fixes

| Fix | Compute time | Human effort | Impact |
|-----|-------------|-------------|--------|
| 6 (entropy tuning) | ~6h (30 runs) | 2h setup + plotting | +5-8% |
| 7 (learning curves) | 0h if logged, ~12h if re-run | 2h plotting | +3-5% |
| 8 (budgets) | 0-8h depending on approach | 1h writing | +3-5% |
| 11 (safety metrics) | 0h (extract from logs) | 2h | +3-5% |
| 12 (ablation) | ~8h (35 runs) | 2h plotting | +3-5% |
| **Total** | **~14-34h compute** | **~9h human** | **+15-25%** |

With all fixes: estimated acceptance probability at T-IV rises from ~25-30% to ~45-55%.

---

## Implementation details for future agents

### Running experiments
```bash
cd /Users/mohammadjeragh/Source/VANET/MyScenarios/AnyScenario/
python run_any_scenario.py --scenario MultiExit --agent <agent_type> --seed <seed>
```

### Key code locations
- MADDPG agent: `maddpg_agent.py` — `Actor.forward()` for categorical softmax, `learn()` for KL penalty
- QMIX agent: `qmix_agent.py` — `learn()` lines 403-421 for CALM integration, `decay_bc_coef()` for λ decay
- NGSIM extraction: `scripts/ngsim/extract_features.py` — `_infer_action()` for discretization
- Config: `config/scenarios.yaml`

### CALM hyperparameters
- λ₀ (bc_coef): 2.0 — initial KL penalty weight
- δ (bc_decay): 0.999 — exponential decay rate per episode
- Temperature (τ): 1.0 — softmax temperature for action probabilities
- Entropy coef: 0.0 (CALM uses KL instead of entropy bonus)

### Seeds used in paper
{42, 123, 456, 789, 1024}
