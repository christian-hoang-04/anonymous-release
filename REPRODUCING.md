# Reproducing the experiments

This guide describes the checked-in scripts and the environment recorded for the experiments. Begin with the committed results/ artifacts; a full model rerun requires model downloads, benchmark data, and GPU compute.

## Environment

The main sweeps were recorded with Python 3.12.13, CUDA-enabled PyTorch 2.10.0, Transformers 5.6.2, and one H100 80 GB GPU. Install the CUDA build appropriate for your machine, then install the pinned main environment:

~~~bash
python -m venv .venv
# Activate the environment using your shell's instructions.
python -m pip install --upgrade pip
python -m pip install torch==2.10.0 --index-url https://download.pytorch.org/whl/cu128
python -m pip install -r requirements.txt
~~~

The mechanism runs have separate dependency sets and recorded Transformers versions. Use a fresh environment for the relevant run rather than combining these pins:

~~~bash
python -m pip install -r requirements-l22h6.txt
python -m pip install -r requirements-l22h6-monolingual.txt
python -m pip install -r requirements-gemma-l11h3.txt
~~~

These files list direct dependencies; they are not complete lockfiles for every operating system or CUDA installation. The table-generation scripts use the Python standard library.

## Inputs

The committed prompts/prompts_european.csv contains the 2,500 FLORES-derived prompts used for the five-language continuation runs. To rebuild it from the upstream dataset:

~~~bash
python prompts/prompts.py
~~~

The extended set is built with:

~~~bash
python prompts/prompts.py --set extended
~~~

The LCB runner downloads test_sets.zip into lcb/ when the test-set directory is absent and downloads the fastText language identifier when needed. The LCB URL in the runner follows the upstream repository HEAD; the exact data revision used in a future rerun can therefore differ from the saved experiments. For a strictly version-matched rerun, record and pin the benchmark revision before generating outputs.

The model aliases used below are defined in experiments/sweep.py. Some model weights may require a Hugging Face account, license acceptance, or gated access. No model weights are stored in this repository.

## Example: Qwen2.5-1.5B discovery and follow-up

Run the 125-prompt screen, then evaluate the selected layer on the full prompt set and run the mean/scaling follow-up:

~~~bash
python experiments/sweep.py --model qwen-instruct --per-lang 25 --bs 125 --out out/qwen-instruct-screen
python experiments/sweep.py --model qwen-instruct --per-lang 500 --layers 22 --modes head --bs 125
python experiments/followup.py run --model qwen-instruct --heads L22H6
~~~

The full layer sweep writes to out/qwen-instruct, which is the default input expected by followup.py.

## Example: LCB and mechanism checks

Run the LCB evaluation and then the phase/attention diagnosis:

~~~bash
python experiments/lcb.py --model qwen-instruct --heads L22H6
python experiments/diagnose.py run --model qwen-instruct --head L22H6
~~~

The LCB runner uses the benchmark's monolingual and crosslingual test sets and line-level scoring. The mechanism script has a dedicated implementation and dependency file; see experiments/liha_l22h6_reproduce.py and requirements-l22h6.txt for the run matching the reported Qwen analysis.

## Example: activation steering

Steering consumes the earlier LCB and diagnosis outputs for the selected model. For the Gemma-3-1B case, first produce those inputs with the model-specific environment and then run:

~~~bash
python experiments/steer.py --model gemma3-1b-instruct --head L11H3 --add
python experiments/steer_quality.py --model gemma3-1b-instruct
~~~

## Rebuild the paper tables

From the repository root:

~~~bash
python tables/main_tables.py
python tables/tables.py
~~~

main_tables.py builds the cross-model and mechanism tables. tables.py regenerates the exploratory table set. Both read saved outputs in results/; they do not run model inference.

## Output and compute notes

- Model runners write generated files under out/; this directory is ignored by Git.
- Greedy FLORES continuations use 40 new tokens. LCB replies use up to 100 new tokens. Sampling runs and per-model exceptions are documented in EXPERIMENTS.md.
- The reported compute used one H100 80 GB GPU. Memory use depends on model, batch size, precision, and attention implementation; reduce batch size if adapting a run.
- Results include screening-only cases and full-set follow-ups. Treat these separately as indicated in the tables and experiment notes.
