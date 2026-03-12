# CALM: Constrained Actor-Learning from deMonstrations

**Full Title:** CALM: Constrained Actor-Learning from Demonstrations for Mode Collapse Prevention in Multi-Agent Cooperative Driving

**Target Journal:** IEEE Transactions on Intelligent Vehicles (T-IV)
- Impact Factor: ~8.2 (Q1)
- Scope: Intelligent vehicles, cooperative driving, autonomous systems

**Authors:**
- Dr. Mohammad Jeragh (Corresponding Author) — mohammad.jeragh@ku.edu.kw
- Dr. Ebrahim Alrashed — ebrahim.alrashed@ku.edu.kw

**Affiliation:** Computer Engineering Department, College of Engineering and Petroleum, Kuwait University

**Status:** Pre-submission — reviewer-ready fixes applied, experimental work remaining

---

## Abstract

Mode collapse is a critical challenge in MARL for autonomous driving, where policies converge to a single repeated action. CALM prevents this through BC pre-training on NGSIM data followed by RL fine-tuning with a decaying KL-divergence constraint. In a 25-run study (5 methods x 5 seeds), DAgger achieves highest NGSIM agreement (45.3%), while CALM achieves highest reward (1.35) with entropy 0.88, eliminating mode collapse.

---

## Repository Structure

```
VANET-02-CALM-PAPER/
├── README.md              # This file
├── SUBMISSION_STATUS.md   # Submission readiness and remaining work
├── main.tex               # Main LaTeX document (IEEEtran, 8 pages)
├── references.bib         # Bibliography (38 cited, ~61 total entries)
├── main.pdf               # Compiled PDF
└── figures/
    ├── method_comparison.pdf
    ├── significance_heatmap.pdf
    ├── action_agreement.pdf
    ├── action_entropy.pdf
    └── reward_comparison.pdf
```

---

## Key Results (Current)

| Method | Agreement | Similarity | Entropy | Reward |
|--------|-----------|-----------|---------|--------|
| MADDPG (unconstrained) | 54.8%* | 68.0% | 0.003 | 0.96 |
| DAgger | **45.3% ± 0.9%** | **98.1%** | 0.63 | 0.44 |
| QMIX-CALM | 40.1% ± 2.3% | 75.8% | 0.70 | 1.01 |
| CALM | 34.0% ± 1.2% | 74.1% | 0.88 | **1.35** |
| Entropy-only | 14.6% ± 23.8% | 39.2% | 0.06 | 0.97 |

*54.8% is a mode-collapse artifact — 99.7% "Maintain" matches the dominant NGSIM action

---

## Related Repositories

| Repository | Purpose |
|------------|---------|
| [VANET](https://github.com/mjeragh/VANET) | Simulator codebase (SUMO, MARL agents, experiments) |
| [VANET-journal-paper](https://github.com/mjeragh/coex-journal-paper) | CoEx paper (submitted to JITS, MS #269158218) |
| **This repo** | CALM paper (targeting IEEE T-IV) |

---

## Branch Structure

- `master` — Last stable draft (Week 8 final polish)
- `fix/reviewer-ready-calm-paper` — **Active branch** with reviewer-ready text fixes (current)

---

**Last Updated:** March 12, 2026
