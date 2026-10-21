# Lossfunk Autoresearch — Preliminary Results

Generated: 2026-10-19T19:52:31.291865+00:00

## Research question

When controlling for local replanning and inference computation, does explicit hierarchical subgoal representation provide an additional reliability advantage over flat receding-horizon world-model planning, and how does this advantage scale with horizon?

## Flat baseline compact validation

| Horizon | Successes | Episodes | Success rate | Wilson 95% CI |
|---:|---:|---:|---:|---:|
| 25 | 13 | 15 | 86.7% | 62.1%–96.3% |
| 50 | 7 | 15 | 46.7% | 24.8%–69.9% |
| 75 | 3 | 15 | 20.0% | 7.0%–45.2% |

Observed baseline success declined as horizon increased. These compact samples suggest a trend but do not establish a general scaling law.

## Matched pilot

- Same episodes and starting states: **True**
- Flat planning: **2/3** successes (66.7%)
- Hierarchical planning: **0/3** successes (0.0%)
- Flat wall time: **202.4234409030014 seconds**
- Hierarchical wall time: **34.263799706999635 seconds**
- Flat candidate scores: **54,000**
- Hierarchical high-level candidate scores: **108000**
- Hierarchical low-level candidate scores: **108000**
- Compute equivalence established: **No**

## Interpretation

The three-episode matched pilot favored flat planning in observed success. The hierarchical method was faster in wall-clock time but had more candidate scores. The results do not establish equal inference compute, and the sample is too small to support a definitive method ranking.

## Limitations

- Compact baseline validation used 15 episodes per horizon, not the intended full evaluation.
- The matched pilot used only three episodes at horizon 25.
- The hierarchical method scored more candidate costs than the flat method in this pilot.
- Candidate-score counts alone do not measure total inference compute.
- Wall-clock time is implementation- and hardware-specific.
- No definitive claim of hierarchical superiority or inferiority is supported.
- Exact challenge-specific requirements in autoresearch-stage.md were not independently verified.

## Generated figures

- `figures/01_flat_success_vs_horizon.png`
- `figures/02_matched_pilot_success.png`
- `figures/03_matched_pilot_runtime.png`
- `figures/04_candidate_scores.png`
- `figures/05_paired_episode_outcomes.png`

## Package contents

- `baseline_metrics.csv` — compact baseline metrics and Wilson intervals.
- `matched_pilot_metrics.csv` — matched-pilot success, runtime and candidate scores.
- `matched_pilot_episode_results.csv` — per-episode paired outcomes.
- `summary_metrics.json` — machine-readable results and limitations.
- `artifact_inventory.csv` — existing artifact hashes and sizes.
- `matched_pilot_source.json` — exact pilot JSON used.

**Submission caveat:** This is a preliminary results package. It does not represent completion of the intended full evaluation or a compute-matched study.