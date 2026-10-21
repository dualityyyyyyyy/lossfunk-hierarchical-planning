# Hierarchical Planning for World-Model-Based Control

**Lossfunk Autoresearch Challenge — Preliminary Experimental Results**

## Overview

This project investigates whether explicit hierarchical subgoal representations improve the reliability of world-model-based control compared with flat receding-horizon planning, and how the difference changes as task horizon increases.

Experiments use the **Hi-LeWM PushT environment** to examine task success, planning runtime, and candidate-scoring workload.

## Research Question

> When controlling for local replanning and inference computation, does explicit hierarchical subgoal representation provide an additional reliability advantage over flat receding-horizon world-model planning, and how does this advantage scale with task horizon?

## Planning Approaches

### Flat Receding-Horizon Planning

The planner evaluates candidate action sequences toward the final goal, executes actions, receives updated observations, and replans.

### Hierarchical Planning

A high-level planner selects intermediate subgoals, while a low-level planner evaluates actions to reach those subgoals. The system executes actions and replans as needed.

## Experimental Setup

- **Environment:** Hi-LeWM PushT
- **World model:** Pretrained checkpoint
- **Baseline task horizons:** 25, 50, and 75 steps
- **Baseline evaluation:** 15 episodes per horizon
- **Baseline seeds:** 42, 43, and 44
- **Matched pilot:** Three shared episodes at horizon 25
- **Compute environment:** Google Colab with an NVIDIA T4 GPU

The baseline evaluation provides an initial performance profile across task horizons. The matched pilot provides preliminary observations of the two planning approaches on the same selected episodes.

## Results

### 1. Flat-Planning Performance Across Horizons

| Task Horizon | Successful Episodes | Success Rate |
|---|---:|---:|
| 25 | 13/15 | 86.7% |
| 50 | 7/15 | 46.7% |
| 75 | 3/15 | 20.0% |

The observed success rate declined as the task horizon increased. This suggests that longer-horizon tasks are more challenging for the flat-planning baseline in this evaluation.

### 2. Preliminary Flat vs. Hierarchical Comparison

| Metric | Flat Planning | Hierarchical Planning |
|---|---:|---:|
| Successful episodes | 2/3 | 0/3 |
| Success rate | 66.7% | 0% |
| Runtime | 202.42 seconds | 34.26 seconds |
| Candidate scores | 54,000 | 216,000 |

The pilot recorded two successes for flat planning and none for hierarchical planning. Hierarchical planning completed the pilot in less wall-clock time but evaluated four times as many candidate scores.

These results are exploratory because the comparison contains only three episodes and the candidate-scoring workloads were not matched.

### 3. Planning Computation

Instrumentation recorded planner calls and candidate-scoring counts. The hierarchical planner was instrumented separately at its high and low levels.

| Planner Component | Planner Calls | Candidate Scores |
|---|---:|---:|
| Flat planner | 180 | 54,000 |
| Hierarchical high-level planner | 120 | 108,000 |
| Hierarchical low-level planner | 180 | 108,000 |
| Hierarchical total | 300 | 216,000 |

These measurements highlight the importance of accounting for planning computation when comparing different planning architectures. Candidate-score counts are not, by themselves, a complete measure of inference cost.

## Visual Results

The `figures/` directory contains the generated experimental plots:

1. `01_flat_success_vs_horizon.png` — baseline success rate across task horizons.
2. `02_matched_pilot_success.png` — success comparison for the matched pilot.
3. `03_matched_pilot_runtime.png` — measured pilot runtime.
4. `04_candidate_scores.png` — candidate-scoring workload.
5. `05_paired_episode_outcomes.png` — episode-level outcomes.

## Repository Structure

```text
lossfunk-hierarchical-planning/
├── README.md
├── docs/
│   ├── FINAL_REPORT.md
│   ├── RESEARCH_SPEC.md
│   ├── BASELINE_PROTOCOL.md
│   ├── BASELINE_INSTRUMENTATION.md
│   ├── ENVIRONMENT.md
│   ├── HUMAN_INTERVENTIONS.md
│   └── RUN_LOG.md
├── src/
│   └── experiment and instrumentation scripts
├── configs/
│   └── environment and experiment configuration records
├── results/
│   ├── baseline_metrics.csv
│   ├── baseline_validation_source_copy.csv
│   ├── matched_pilot_metrics.csv
│   ├── matched_pilot_episode_results.csv
│   └── summary_metrics.json
├── figures/
│   └── experimental plots
├── logs/
│   └── selected validation logs and summaries
└── prompts/
    └── research prompts and workflow records
```

The directory listing above describes the intended organization; individual files should be checked against the actual repository contents.

## Key Findings

- Flat-planning success decreased across the three tested task horizons.
- The hierarchical pipeline executed successfully, allowing high-level and low-level planning computation to be instrumented.
- The preliminary matched pilot showed lower wall-clock runtime for hierarchical planning but a substantially larger candidate-scoring workload.
- The current experiments do not establish whether hierarchical planning provides a general reliability advantage.

## Limitations and Future Work

The baseline evaluation is a compact validation rather than the full planned evaluation. The matched pilot contains only three episodes and does not equalize candidate-scoring workload or establish equivalent inference-computation budgets.

Further work should expand evaluation across horizons, increase the number of paired episodes, control planning and inference budgets, and assess uncertainty in the resulting success rates.

Accordingly, the findings should be interpreted as **preliminary experimental observations**, not a definitive ranking of flat and hierarchical planning.

## Reproducibility

The repository includes the available research documentation, experimental summaries, plots, and selected instrumentation artifacts. Consult `docs/` for the research specification, baseline protocol, instrumentation notes, environment information, and run log.

The pretrained checkpoints and dataset are not included in this repository. Refer to the upstream Hi-LeWM project and its official checkpoint documentation for the original implementation and required assets.

---

*This repository documents an ongoing research investigation into hierarchical planning, task-horizon sensitivity, and computational accounting in world-model-based control.*
