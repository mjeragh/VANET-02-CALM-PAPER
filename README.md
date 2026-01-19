# CALM: Constrained Action Learning for Multi-agents

**Full Title:** Preventing Mode Collapse in Multi-Agent Reinforcement Learning for Cooperative Autonomous Driving

**Target Journal:** IEEE Transactions on Intelligent Transportation Systems (T-ITS)
- Impact Factor: ~8.5 (Q1)
- Scope: Intelligent transportation, autonomous vehicles, machine learning

**Authors:**
- Dr. Mohammad Jeragh (Corresponding Author)
- Dr. Ibrahim Alrashed

**Affiliation:** Computer Engineering Department, College of Engineering and Petroleum, Kuwait University

---

## Abstract

Mode collapse is a critical challenge in multi-agent reinforcement learning (MARL) for autonomous driving, where trained policies converge to repetitive, non-diverse behaviors. This paper presents CALM (Constrained Action Learning for Multi-agents), a novel hybrid approach combining behavioral cloning with reinforcement learning through KL divergence regularization. Using 47,874 human driving samples from the NGSIM dataset, CALM prevents mode collapse while maintaining task performance. Experiments on cooperative highway exit scenarios demonstrate that CALM achieves 35.22% action agreement with human drivers and 74.07% behavioral similarity, significantly outperforming entropy-regularization-only approaches (22.74% agreement). The decaying KL constraint enables smooth transition from imitation to optimization, producing diverse, human-like driving behaviors.

---

## Paper Structure

```
1. Introduction
2. Related Work
3. Problem Formulation
4. CALM: Proposed Method
5. Experimental Setup
6. Results
7. Discussion
8. Conclusion
```

---

## Repository Structure

```
VANET-02-CALM-PAPER/
├── README.md              # This file
├── .gitignore            # Git ignore patterns
├── main.tex              # Main LaTeX document
├── references.bib        # Bibliography
├── figures/              # Publication figures
│   ├── architecture.png
│   ├── action_distribution.png
│   ├── ablation.png
│   └── scalability.png
└── sections/             # LaTeX sections (if modular)
```

---

## Related Repositories

| Repository | Purpose |
|------------|---------|
| [VANET](https://github.com/mjeragh/VANET) | Main codebase (experiments, SUMO, MARL agents) |
| [VANET-journal-paper](https://github.com/mjeragh/coex-journal-paper) | CoEx paper (Phase 1, under review at T&F) |
| **This repo** | CALM paper (Phase 2, in preparation for T-ITS) |

---

## Collaboration

This repository syncs with Overleaf for collaborative editing.

**Overleaf Project:** (To be linked)

---

## Timeline

| Week | Milestone |
|------|-----------|
| 1 | GAIL baseline implementation |
| 2 | DAgger baseline implementation |
| 3 | Ablation studies |
| 4 | Scalability experiments |
| 5 | Statistical analysis (5 seeds) |
| 6 | Theory + Draft v1 |
| 7 | Full draft |
| 8 | Submission |

---

## Key Results (Current)

| Method | Agreement | Similarity | Mode Collapse |
|--------|-----------|------------|---------------|
| Entropy-only | 22.74% | 55.74% | Yes (100% Decel) |
| **CALM** | **35.22%** | **74.07%** | **No** |

---

**Last Updated:** January 19, 2026
