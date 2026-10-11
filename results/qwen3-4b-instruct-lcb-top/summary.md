# qwen3-4b-instruct: Language Confusion Benchmark

Greedy 100 tokens, chat template. LPR = share of replies whose lines (5+ words) are all in the expected language, averaged over sources as in the benchmark. Δ = paired change against base on the non-English prompts both runs score, pooled, with a bootstrap 95% CI. Mean ablation uses the head's mean over the 2,500 FLORES prompts. Controls are random heads from the same layers, zero-ablated.

| condition | mono LPR | Δ mono | cross LPR | Δ cross | mono en LPR | English lines (mono / cross) |
|---|---|---|---|---|---|---|
| base | 0.982 |  | 0.893 |  | 0.975 | 0.00 / 0.08 |
| L24H27 zero | 0.982 | +0.000 [-0.010, +0.010] | 0.816 | -0.076 [-0.093, -0.061] | 0.995 | 0.00 / 0.15 |
| L24H27 mean | 0.978 | -0.004 [-0.014, +0.006] | 0.853 | -0.040 [-0.052, -0.027] | 0.985 | 0.00 / 0.11 |
| L24H12 zero (control) | 0.976 | -0.005 [-0.011, +0.000] | 0.893 | -0.001 [-0.007, +0.005] | 0.985 | 0.00 / 0.07 |
| L24H24 zero (control) | 0.994 | +0.010 [+0.001, +0.019] | 0.907 | +0.013 [+0.005, +0.024] | 0.985 | 0.00 / 0.07 |
| L24H28 zero (control) | 0.980 | -0.001 [-0.009, +0.006] | 0.895 | +0.002 [-0.006, +0.008] | 0.980 | 0.00 / 0.07 |

LPR per task and language

| condition | monolingual/fr | monolingual/de | monolingual/es | monolingual/it | monolingual/en | crosslingual/fr | crosslingual/de | crosslingual/es | crosslingual/it |
|---|---|---|---|---|---|---|---|---|---|
| base | 0.98 | 1.00 | 0.98 | 0.99 | 0.97 | 0.89 | 0.89 | 0.92 | 0.88 |
| L24H27 zero | 0.98 | 1.00 | 0.98 | 1.00 | 0.99 | 0.82 | 0.77 | 0.86 | 0.81 |
| L24H27 mean | 0.97 | 1.00 | 0.98 | 0.99 | 0.98 | 0.84 | 0.84 | 0.88 | 0.85 |
| L24H12 zero (control) | 0.97 | 1.00 | 0.98 | 0.99 | 0.98 | 0.88 | 0.90 | 0.91 | 0.88 |
| L24H24 zero (control) | 1.00 | 1.00 | 0.99 | 0.99 | 0.98 | 0.90 | 0.91 | 0.91 | 0.90 |
| L24H28 zero (control) | 0.98 | 1.00 | 0.98 | 0.99 | 0.98 | 0.89 | 0.90 | 0.91 | 0.88 |
