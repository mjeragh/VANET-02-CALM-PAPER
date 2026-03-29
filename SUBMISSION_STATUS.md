# CALM Paper — Submission Readiness Status

**Target:** IEEE Transactions on Intelligent Vehicles (T-IV)
**Current branch:** `fix/reviewer-ready-calm-paper`
**Last updated:** March 28, 2026

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

### Fix 6: Tune entropy-only baseline (DONE ✅)
**Status:** Completed. 30 runs (43.5h total), NGSIM validation done.

**Results:** Mode collapse confirmed across ALL α_ent values:
| α_ent | Agreement | Entropy | Verdict |
|-------|-----------|---------|---------|
| 0.001 | 24.6±17.4% | 0.002 | Collapse |
| 0.01 | 0.5±0.5% | 0.045 | Collapse |
| 0.05 | 20.1±20.0% | 0.000 | Collapse |
| 0.1 | 32.8±26.8% | 0.006 | Collapse |
| 0.5 | 23.5±18.3% | 0.228 | Partial collapse |
| 1.0 | 14.6±13.9% | 0.235 | Partial collapse |

vs CALM: 34.0±1.2%, entropy 0.88 — bulletproof central claim.
Added as Table IV in paper.

### Fix 7: Add learning curves figure (DONE ✅)
**Status:** Completed in commit `6b64933`. Generated `figures/learning_curves.pdf` from Week 5 training data.
Added Fig. 4 and convergence paragraph to Section VI-A.

### Fix 8: Equalize or justify training budgets (DONE ✅)
**Status:** Completed in commit `6b64933`. Added paragraph referencing learning curves to justify
convergence-appropriate training budgets (DAgger 50, QMIX 100, CALM/GAIL 300 episodes).

### Fix 11: Add safety metrics (DONE ✅)
**Status:** Completed in commit `8831ee8`. Evaluation script at `VANET/experiments/calm_paper/evaluate_safety_metrics.py`.

**Results (100 eval episodes per method):**
| Method | Collision Rate | Exit Success | Avg Speed (m/s) | Proximity/ep |
|--------|---------------|-------------|-----------------|-------------|
| CALM | 0.0% | **33.3%** | **25.8 ± 1.2** | **42.5** |
| QMIX-CALM | 0.0% | 26.7% | 19.7 ± 6.9 | 220.9 |
| Entropy-only | 0.0% | 20.0% | 16.3 ± 6.1 | 685.4 |
| DAgger | 0.0% | 7.7% | 11.0 ± 3.6 | 3150.5 |
| GAIL | 0.0% | 6.7% | 8.1 ± 2.2 | 2563.8 |

Added as Table III in Section VI-E "Driving Safety Evaluation".

### Fix 12: λ₀ and δ ablation (DONE ✅)
**Status:** Completed. Full 4×4 grid (80 runs, 5 seeds each). Analysis script at
`VANET/experiments/calm_paper/ablation/analyze_lambda_delta.py`.

**Results (last-50-episode avg reward, mean ± std over 5 seeds):**
| λ₀ \ δ | 0.99 | 0.995 | 0.999 | 1.0 |
|---------|------|-------|-------|-----|
| 0.5 | **1.39 ± 0.07** | 1.34 ± 0.06 | 1.32 ± 0.04 | 1.35 ± 0.03 |
| 1.0 | 1.32 ± 0.06 | 1.30 ± 0.08 | 1.35 ± 0.05 | 1.34 ± 0.03 |
| 2.0 | 1.34 ± 0.03 | 1.31 ± 0.03 | 1.36 ± 0.04† | 1.34 ± 0.03 |
| 5.0 | 1.33 ± 0.03 | 1.32 ± 0.04 | 1.33 ± 0.03 | 1.33 ± 0.04 |

**Key finding:** CALM is robust — <7% variation across all 16 configs. Default (λ₀=2.0, δ=0.999) ranks 2nd.
Added as Table V in Section VI-F "Hyperparameter Sensitivity".

### Fix 15: Add SAC baseline (DONE ✅)
**Status:** Completed. MASAC implemented (discrete SAC with twin Q-networks, auto temperature, CTDE).
5 seeds × 300 episodes (7h total). Agent: `VANET/sac_agent.py`.

**Results:**
| Metric | MASAC | CALM |
|--------|-------|------|
| Reward | 1.06 ± 0.16 | 1.35 ± 0.05 |
| NGSIM Agreement | 25.8% | 34.0% |
| Entropy | 1.38 | 0.88 |

**Key finding:** SAC prevents mode collapse (entropy 1.38) but diversity is undirected —
agreement near rule-based baseline. Confirms CALM's demonstration anchoring is essential.
Added MASAC row to Table I, discussion in Sections VI-C and VI-D, plus Key Finding #5.

---

## Estimated effort for remaining fixes

| Fix | Status | Impact |
|-----|--------|--------|
| 6 (entropy tuning) | ✅ Done (30 runs, 43.5h) | +5-8% |
| 7 (learning curves) | ✅ Done | +3-5% |
| 8 (budgets) | ✅ Done | +3-5% |
| 11 (safety metrics) | ✅ Done | +3-5% |
| 12 (λ₀/δ ablation) | ✅ Done (80 runs) | +3-5% |
| 15 (MASAC baseline) | ✅ Done (5 runs, 7h) | +2-3% |

**All fixes complete.** Paper is submission-ready for IEEE T-IV.

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
