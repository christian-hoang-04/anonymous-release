# Third-party data and software notices

This artifact contains derived prompt text, research outputs, and scripts that depend on external datasets and software. This notice identifies the known upstream sources; it does not replace their license files or terms.

## FLORES-200

The committed prompt manifests contain text derived from the FLORES-200 devtest split. The upstream Hugging Face dataset card identifies the dataset as CC BY-SA 4.0: <https://huggingface.co/datasets/facebook/flores>. The MTEB mirror used by prompts/prompts.py is <https://huggingface.co/datasets/mteb/flores>. Attribute the dataset to its upstream authors and preserve the applicable share-alike terms when redistributing derived prompt text.

## Language Confusion Benchmark

LCB test sets are downloaded by experiments/lcb.py from <https://github.com/for-ai/language-confusion>. The benchmark paper is Kelly Marchisio, Wei-Yin Ko, Alexandre Bérard, Théo Dehaze, and Sebastian Ruder, “Understanding and Mitigating Language Confusion in LLMs” (2024), <https://arxiv.org/abs/2406.20052>. The scoring implementation cites the benchmark's compute_metrics.py and its Apache-2.0 license. The LCB test-set sources may have their own terms; consult the upstream repository before redistributing those files.

## Models and language identification

Model weights are fetched from the Hugging Face model IDs listed in experiments/sweep.py; weights are not included here. Follow each model card's access and license terms. The fastText language-identification model is downloaded from Meta's supervised model distribution URL in the scripts; consult its upstream terms before redistribution.

## Project code license

No project-wide license was present in the source repository. This release does not add one or make a license claim on code or result files. A project owner should select the code license separately from the terms governing FLORES-derived prompts, LCB material, model weights, and generated outputs.
