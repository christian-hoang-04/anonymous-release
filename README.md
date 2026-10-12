# Language-Switching Heads in Multilingual LLMs

**Anonymous research artifact**

This repository contains the experiment code, prompt manifests, figures, tables, and saved outputs for a causal study of output-language selection in multilingual language models. It is organized around three questions: which individual attention heads influence output language, how post-training changes that dependence, and when head activations can redirect generation.

No author or affiliation metadata is included in this release snapshot.

## Current paper

**One Head Away from Language Confusion: Single Attention Heads Across Model Families**

[Download the current paper (PDF)](paper.pdf)

## Paper-first reading order

| Block | Question and evidence | Main code and artifacts |
|---|---|---|
| **Experiment 1 — Discovering language-switching heads** | Does ablating one head change the language of generated text? A 125-prompt FLORES screen selects candidates for 2,500-prompt follow-up; LCB evaluates explicit monolingual and crosslingual instructions. The cross-model suite covers 12 instruction-tuned models from seven families. | experiments/sweep.py, experiments/followup.py, experiments/lcb.py; tables/main_tables.md; results/ |
| **Experiment 2 — Post-training dependence** | How does the same head's causal effect differ between matched base and instruction-tuned checkpoints, and across released OLMo-2 training checkpoints? | experiments/sweep.py, experiments/lcb.py; results/RESULTS.md; tables/main_tables.md |
| **Experiment 3 — Mechanism and control** | When does an identified head act, what prompt information does it use, and can activation replacement or steering redirect language? Qwen2.5-1.5B and Gemma-3-1B are the detailed mechanism cases; steering includes selected additional models. | experiments/diagnose.py, experiments/steer.py, experiments/steer_quality.py; results/qwen-l22h6-mechanism/, results/gemma-l11h3-mechanism/, and results/*steer*/ |
| **Controls and supplementary analyses** | Pretraining-only contrasts, detector checks, random splits, mean-versus-zero ablation, additional languages, and comparisons with translation-head rankings test the limits of the main findings. | EXPERIMENTS.md, results/RESULTS.md, experiments/, figures/, tables/ |

The detailed experiment inventory preserves the order in which analyses were run for transparency. Use this table as the paper-oriented map; do not read that inventory as the paper's results sequence.

## Start with the saved results

Most readers do not need to download model weights or run GPU experiments. The checked-in output files support the paper tables and analyses.

~~~bash
python tables/main_tables.py
~~~

This regenerates tables/main_tables.md from the saved results using only the Python standard library. The complete run-by-run record is in results/RESULTS.md; EXPERIMENTS.md documents protocols, selection rules, and caveats.

## Reproducing model runs

Model sweeps require a CUDA-enabled PyTorch environment and substantial GPU memory. The recorded runs used one H100 80 GB GPU. Use the commands and model-specific precision in REPRODUCING.md; the model aliases and Hugging Face IDs are defined in experiments/sweep.py. Some model providers require accepting their terms or granting access before the weights can be downloaded.

## Repository map

- experiments/ — model sweeps, interventions, scoring, robustness, and analysis scripts.
- prompts/ — FLORES-derived prompt manifests and prompt construction code.
- results/ — saved generations, labels, summaries, manifests, and detailed result notes.
- figures/ — paper figures in PDF and PNG.
- tables/ — scripts and generated paper tables.
- tools/ — focused analysis helpers.
- requirements*.txt — recorded environment pins and experiment-specific dependency sets.

## Data and attribution

FLORES-derived prompt text is included in prompts/; the FLORES dataset is distributed under CC BY-SA 4.0 by its upstream dataset card. LCB test sets are fetched from the benchmark repository on first use rather than vendored here. See THIRD_PARTY_NOTICES.md for source links and attribution. Model weights are not included; their own access and license terms apply.

## Licensing

The source repository did not specify a project-wide code license, so this snapshot does not assign one. Public visibility alone does not grant reuse rights. The dataset-derived prompt files and upstream model/data assets remain subject to their respective terms; see THIRD_PARTY_NOTICES.md.
