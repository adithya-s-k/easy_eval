# Indic Eval 

A lightweight evaluation suite tailored specifically for assessing Indic LLMs across a diverse range of tasks, aiding in performance assessment and comparison within the Indian language context

### Context

Indic Eval is a lightweight LLM evaluation suite built on top of [LightEval](https://github.com/huggingface/lighteval), which Hugging Face has been using internally. We at [cognitivelab.in](https://cognitivelab.in) utilize it for internal testing of LLMs on various tasks. 

While early in development, it offers a collaborative space for community-driven advancement in Indic language modeling. Please note that stability is a work in progress. Feel free to contribute or raise issues!

### What does it Offer on top of Light eval
- ✅ Intergration with [Indic LLM leaderboard](https://huggingface.co/spaces/Cognitive-Lab/indic_llm_leaderboard) 
- ✅ Support of the following translated dataset ARC, Hellaswag , Boolq, MMLU , Translate[IND 22] to indian languages
- ✅ [Skypilot](https://skypilot.readthedocs.io/) Integration to run Evals on 15+ CLoud providers with simple configuration
- ✅ Support for Language Base evaluation rather than task based


## News
- **April 01, 2024**: Release of `indic_eval`

## Installation

Clone the repo:

```bash
git clone https://github.com/adithya-s-k/indic_eval
cd indic_eval
```

Create a virtual environment using virtualenv or conda depending on your preferences. We require Python 3.10 or above:

```bash
conda create -n indic-eval-venv python=3.10 && conda activate indic-eval-venv
```

Install the dependencies. For the default installation, you just need:

```bash
pip install .
```

If you want to evaluate models with frameworks like `accelerate` or `peft`, you will need to specify the optional dependencies group that fits your use case (`accelerate`,`tgi`,`optimum`,`quantization`,`adapters`,`nanotron`):

```bash
pip install .[optional1,optional2]
```

The setup tested most is:

```bash
pip install .[accelerate,quantization,adapters]
```

If you want to push your results to the Hugging Face Hub, don't forget to add your access token to the environment variable `HUGGING_FACE_HUB_TOKEN`. You can do this by running:

```shell
huggingface-cli login
```
and pasting your access token.

## Run Indic LLM Leaderboard Eval
To conduct an evaluation for the Indic LLM Leaderboard, you can use the following commands:

```bash
accelerate launch run_indic_evals_accelerate.py \
    --model_args="pretrained=<>path to model on the hub" \
    --language <language you want to evaluate on> \
    --tasks indic_llm_leadeboard \
    --output_dir evals \
    --push_to_leaderboard <yourname@company.com> 
```

- `--model_args`: Specifies the arguments for the model. This includes parameters like `pretrained`, which is the path to the model on the hub that you want to evaluate. Example: `--model_args="pretrained=path_to_model"`
  
- `--language`: Specifies the language you want to evaluate on. This argument determines the language-specific evaluation tasks. Example: `--language english` Langauges supported are kannada, hindi, tamil, telugu, gujarati, marathi, malayalam , english

- `--tasks`: Specifies the tasks you want to evaluate the model on. In this case, it's `indic_llm_leaderboard`, indicating the evaluation tasks specific to the Indic LLM Leaderboard. which includes the following 

- `--output_dir`: Specifies the directory where the evaluation results will be saved. Example: `--output_dir evals`

- `--push_to_leaderboard`: Specifies the email address through which we can contact you to verify the scores if needed. Example: `--push_to_leaderboard yourname@company.com`

Some other parameters include:

- `--override_batch_size`: You can increase the batch size. If not specified, the library itself picks the most optimal batch size. It is recommended to leave this option out.

to see all the indic benchmarks available you can refer to [indic_tasks.txt](https://github.com/adithya-s-k/indic_eval/blob/main/tasks_examples/indic_tasks.txt)

### Single Benchmark Evaluation for a Single Language

For instance, to run the ARC-Easy benchmark for Kannada:

```bash
accelerate launch run_indic_evals_accelerate.py \
    --model_args="pretrained=<path to model on the hub>" \
    --language kannada \
    --tasks indiceval|ARC-Easy:kannada|5|0 \
    --output_dir output_dir \
```

### Multiple Benchmark Evaluation for a Single Language

If you want to evaluate multiple benchmarks for a single language, such as ARC-Easy, ARC-Challenge, and Hellaswag for Kannada:

```bash
accelerate launch run_indic_evals_accelerate.py \
    --model_args="pretrained=<path to model on the hub>" \
    --language kannada \
    --tasks indiceval|ARC-Easy:kannada|5|0,indiceval|ARC-Challenge:kannada|10|0 \
    --output_dir output_dir \
```

### Note on Evaluating Multiple Benchmarks for Multiple Languages

Currently, the framework doesn't support evaluating multiple benchmarks for multiple languages simultaneously. If you wish to evaluate for both Kannada and Hindi, you will need to run the evaluation iteratively for each language:

For Kannada:

```bash
accelerate launch run_indic_evals_accelerate.py \
    --model_args="pretrained=<path to model on the hub>" \
    --language kannada \
    --tasks indiceval|ARC-Easy:kannada|5|0,indiceval|ARC-Challenge:kannada|10|0 \
    --output_dir output_dir \
```

For Hindi:

```bash
accelerate launch run_indic_evals_accelerate.py \
    --model_args="pretrained=<path to model on the hub>" \
    --language hindi \
    --tasks indiceval|ARC-Easy:hindi|5|0,indiceval|ARC-Challenge:hindi|10|0 \
    --output_dir output_dir \
```

Please make sure to replace `<path to model on the hub>` with the actual path to your pre-trained model.


## ✈️ [SKYPILOT](https://skypilot.readthedocs.io/en/latest/docs/index.html)

SkyPilot is a framework for running LLMs, AI, and batch jobs on any cloud, offering maximum cost savings, highest GPU availability, and managed execution.

### Run Indic Eval through SkyPilot

```bash
pip install skypilot-nightly[all]
```
You can also download only for certain cloud providers. After that, authenticate the cloud provider.

```bash
sky launch -c indic-eval skypilot_indic_eval.yaml --idle-minutes-to-autostop 5
```
The cluster will automatically shut down after 5 minutes when it's idle.

Please refer to the [documentation](https://skypilot.readthedocs.io/en/latest/docs/index.html) for more information.


## To-DO
- [x] Proper Intergration with [Indic_LLM_Leaderboard](https://huggingface.co/spaces/Cognitive-Lab/indic_llm_leaderboard)
- [x] Test out ARC-Easy for all indic Languages and see consistency
- [x] Test out ARC-Challenge for all indic Languages and see consistency
- [x] Test out Hellaswag for all indic Languages and see consistency
- [ ] Test out Boolq for all indic Languages and see consistency
- [ ] Make Intergration with [Indic_LLM_Leaderboard](https://huggingface.co/spaces/Cognitive-Lab/indic_llm_leaderboard) more secure
- [ ] Test out MMLU for all indic Languages and see consistency
- [ ] Test out Translate for all indic Languages and see consistency
- [ ] Integrate VLLM for faster evaluation
- [ ] Test out Benchmark consistence

<details>
<summary><h3>Indepth Features of Indic_eval/Ligtheval </h3></summary>

- to load and push big models/datasets, your machine likely needs Git LFS. You can install it with `sudo apt-get install git-lfs`
- If you want to run bigbench evaluations, install bigbench `pip install "bigbench@https://storage.googleapis.com/public_research_data/bigbench/bigbench-0.0.1.tar.gz"`

Lastly, if you intend to push to the code base, you'll need to install the precommit hook for styling tests:

```bash
pip install .[dev]
pre-commit install
``` 

## Usage

We provide two main entry points to evaluate models:

* `run_evals_accelerate.py`: evaluate models on CPU or one or more GPUs using [🤗 Accelerate](https://github.com/huggingface/accelerate).
* `run_evals_nanotron.py`: evaluate models in distributed settings using [⚡️ Nanotron](https://github.com/huggingface/nanotron).

For most users, we recommend using the 🤗 Accelerate backend - see below for specific commands.

### Evaluate a model on one or more GPUs (recommended)

To evaluate a model on one or more GPUs, first create a `multi-gpu` config by running:

```shell
accelerate config
```

You can then evaluate a model using data parallelism as follows:

```shell
accelerate launch --multi_gpu --num_processes=<num_gpus> run_evals_accelerate.py \
    --model_args="pretrained=<path to model on the hub>" \
    --tasks <task parameters> \
    --output_dir output_dir
```

Here, `--tasks` refers to either a _comma-separated_ list of supported tasks from the [metadata table](src/lighteval/tasks/tasks_table.jsonl) in the format:

```
suite|task|num_few_shot|{0 or 1 to automatically reduce `num_few_shot` if prompt is too long}
```

or a file path like [`tasks_examples/recommended_set.txt`](./tasks_examples/recommended_set.txt) which specifies multiple task configurations. For example, to evaluate GPT-2 on the Truthful QA benchmark run:

```shell
accelerate launch --multi_gpu --num_processes=8 run_evals_accelerate.py \
    --model_args "pretrained=gpt2" \
    --tasks "lighteval|truthfulqa:mc|0|0" \
    --override_batch_size 1 \
    --output_dir="./evals/"
```

Here, `--override_batch_size` defines the _batch size per device_, so the effective batch size will be `override_batch_size x num_gpus`. To evaluate on multiple benchmarks, separate each task configuration with a comma, e.g.

```shell
accelerate launch --multi_gpu --num_processes=8 run_evals_accelerate.py \
    --model_args "pretrained=gpt2" \
    --tasks "leaderboard|truthfulqa:mc|0|0,leaderboard|gsm8k|0|0" \
    --override_batch_size 1 \
    --output_dir="./evals/"
```

See the [`tasks_examples/recommended_set.txt`](./tasks_examples/recommended_set.txt) file for a list of recommended task configurations.

### Evaluating a large model with pipeline parallelism

To evaluate models larger that ~40B parameters in 16-bit precision, you will need to shard the model across multiple GPUs to fit it in VRAM. You can do this by passing `model_parallel=True` and adapting `--num_processes` to be the number of processes to use for data parallel. For example, on a single node of 8 GPUs, you can run:

```shell
# PP=2, DP=4 - good for models < 70B params
accelerate launch --multi_gpu --num_processes=4 run_evals_accelerate.py \
    --model_args="pretrained=<path to model on the hub>" \
    --model_parallel \
    --tasks <task parameters> \
    --output_dir output_dir

# PP=4, DP=2 - good for huge models >= 70B params
accelerate launch --multi_gpu --num_processes=2 run_evals_accelerate.py \
    --model_args="pretrained=<path to model on the hub>" \
    --model_parallel \
    --tasks <task parameters> \
    --output_dir output_dir
```

### Evaluate a model on the Open LLM Leaderboard benchmarks

To evaluate a model on all the benchmarks of the [Open LLM Leaderboard](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard) using a single node of 8 GPUs, run:

```shell
accelerate launch --multi_gpu --num_processes=8 run_evals_accelerate.py \
    --model_args "pretrained=<model name>" \
    --tasks tasks_examples/open_llm_leaderboard_tasks.txt \
    --override_batch_size 1 \
    --output_dir="./evals/"
```

### Evaluate a model on CPU

You can also use `lighteval` to evaluate models on CPU, although note this will typically be very slow for large models. To do so, run:

```shell
python run_evals_accelerate.py \
    --model_args="pretrained=<path to model on the hub>"\
    --tasks <task parameters> \
    --output_dir output_dir
```

### Evaluate a model on extended, community, or custom tasks.

Independently of the default tasks provided in `lighteval` that you will find in the `tasks_table.jsonl` file, you can use `lighteval` to evaluate models on tasks that require special processing (or have been added by the community). These tasks have their own evaluation suites and are defined as follows:

* `extended`: tasks which have complex pre- or post-processing and are added by the `lighteval` maintainers. See the [`extended_tasks`](./src/lighteval/tasks/extended_tasks) folder for examples.
* `community`: tasks which have been added by the community. See the [`community_tasks`](./community_tasks) folder for examples.
* `custom`: tasks which are defined locally and not present in the core library. Use this suite if you want to experiment with designing a special metric or task.


For example, to run an extended task like ifeval, you can run:
```shell
python run_evals_accelerate.py \
    --model_args "pretrained=HuggingFaceH4/zephyr-7b-beta" \
    --use_chat_template \ # optional, if you want to run the evaluation with the chat template
    --tasks "extended|ifeval|0|0" \
    --output_dir "./evals"
```

To run a community or custom task, you can use (note the custom_tasks flag):

```shell
python run_evals_accelerate.py \
    --model_args="pretrained=<path to model on the hub>"\
    --tasks <task parameters> \
    --custom_tasks <path to your custom or community task> \
    --output_dir output_dir
```

For example, to launch `lighteval` on `arabic_mmlu:abstract_algebra` for `HuggingFaceH4/zephyr-7b-beta`, run:

```shell
python run_evals_accelerate.py \
    --model_args "pretrained=HuggingFaceH4/zephyr-7b-beta" \
    --use_chat_template \ # optional, if you want to run the evaluation with the chat template
    --tasks "community|arabic_mmlu:abstract_algebra|5|1" \
    --custom_tasks "community_tasks/arabic_evals" \
    --output_dir "./evals"
```

## Customisation
If your new task or metric has requirements, add a specific `requirements.txt` file with your evaluation.

### Adding a new task
To add a new task, first either open an issue, to determine whether it will be integrated in the core evaluations of indic_eval, in the extended tasks, or in the community tasks, and **add its dataset** on the hub.

- Core evaluations are evaluation which only require standard logic in their metrics and processing, and that we will add to our test suite to ensure non regression through time. They already see a high usage in the community.
- Extended evaluations are evaluations which require custom logic in their metrics (complex normalisation, an LLM as a judge, ...), that we added to facilitate the life of users. They already see a high usage in the community.
- Community evaluations are submissions by the community of new tasks.

A popular community evaluation can move to becoming an extended or core evaluation through time.

#### Core evaluations
Prompt function: **find a suitable prompt function** in `src.indic_eval.tasks.task_prompt_formatting.py`, or code your own. This function must output a `Doc` object, which should contain `query`, your prompt, and either `gold`, the gold output, or `choices` and `gold_index`, the list of choices and index or indices of correct answers. If your query contains an instruction which should not be repeated in a few shot setup, add it to an `instruction` field.

Summary: create a **line summary** of your evaluation, in `src/indic_eval/tasks/tasks_table.jsonl`. This summary should contain the following fields:
- `name` (str), your evaluation name
- `suite` (list), the suite(s) to which your evaluation should belong. This field allows us to compare different tasks implementation, and is used a task selection to differentiate the versions to launch. At the moment, you'll find the keywords ["indic_eval","helm", "bigbench", "original", "lighteval", "community", "custom"]; for indic evals, please choose `indic_eval` 
- `prompt_function` (str), the name of the prompt function you defined in the step above
- `hf_repo` (str), the path to your evaluation dataset on the hub
- `hf_subset` (str), the specific subset you want to use for your evaluation (note: when the dataset has no subset, fill this field with `"default"`, not with `None` or `""`)
- `hf_avail_splits` (list), all the splits available for your dataset (train, valid or validation, test, other...)
- `evaluation_splits` (list), the splits you want to use for evaluation
- `few_shots_split` (str, can be `null`), the specific split from which you want to select samples for your few-shot examples. It should be different from the sets included in `evaluation_splits`
- `few_shots_select` (str, can be `null`), the method that you will use to select items for your few-shot examples. Can be `null`, or one of:
    - `balanced` selects examples from the `few_shots_split` with balanced labels, to avoid skewing the few shot examples (hence the model generations) towards one specific label
    - `random` selects examples at random from the `few_shots_split`
    - `random_sampling` selects new examples at random from the `few_shots_split` for every new item, but if a sampled item is equal to the current one, it is removed from the available samples
    - `random_sampling_from_train` selects new examples at random from the `few_shots_split` for every new item, but if a sampled item is equal to the current one, it is kept! Only use this if you know what you are doing.
    - `sequential` selects the first `n` examples of the `few_shots_split`
- `generation_size` (int), the maximum number of tokens allowed for a generative evaluation. If your evaluation is a log likelihood evaluation (multi-choice), this value should be -1
- `stop_sequence` (list), a list of strings acting as end of sentence tokens for your generation
- `metric` (list), the metrics you want to use for your evaluation (see next section for a detailed explanation)
- `output_regex` (str), A regex string that will be used to filter your generation. (Genrative metrics will only select tokens that are between the first and the second sequence matched by the regex. For example, for a regex matching `\n` and a generation `\nModel generation output\nSome other text` the metric will only be fed with `Model generation output`)
- `frozen` (bool), for now is set to False, but we will steadily pass all stable tasks to True.
- `trust_dataset` (bool), set to True if you trust the dataset.

Make sure you can launch your model with your new task using `--tasks lighteval|yournewtask|2|0`.

#### Community evaluations
Copy the `community_tasks/_template.yml` to `community_tasks/yourevalname.py` and edit it to add your custom tasks (the parameters you can use are explained above). It contains an interesting mechanism if the dataset you are adding contains a lot of subsets.

Make sure you can launch your model with your new task using `--tasks community|yournewtask|2|0 --custom_tasks community_tasks/yourevalname.py`.

### Adding a new metric
First check if you can use one of the parametrized functions in `src.indic_eval.metrics.metrics_corpus` or `src.indic_eval.metrics.metrics_sample`.

If not, you can use the custom_task system to register your new metric:
- create a new python file which should contain the full logic of your metric.
- the file also needs to start with these imports
```python
from aenum import extend_enum
from indic_eval.metrics import Metrics

# And any other class you might need to redefine your specific metric, depending on whether it's a sample or corpus metric.
```

- and to end with the following, so that it adds your metric to our metrics list when loaded as a module.

```python
# Adds the metric to the metric list!
extend_enum(Metrics, "metric_name", metric_function)
if __name__ == "__main__":
    print("Imported metric")
```

You can then give your custom metric to indic_eval by using `--custom-tasks path_to_your_file` when launching it.

To see an example of a custom metric added along with a custom task, look at `tasks_examples/custom_tasks_with_custom_metrics/ifeval/ifeval.py`.


### Metrics for specific tasks
To keep compatibility with the Harness for some specific tasks, we ported their evaluations more or less as such. They include `drop` (for the DROP dataset) and `truthfulqa_mc_metrics` (for TruthfulQA). In general, except for tasks where the dataset has a very different formatting than usual (an other language, programming language, math, ...), we want to use standard implementations of the above metrics. It makes little sense to have 10 different versions of an exact match depending on the task. However, most of the above metrics are parametrizable so that you can change the normalization applied easily for experimental purposes.

### Not working yet
These metrics need both the generation and its logprob. They are not working at the moment, as this fn is not in the AI Harness.
- `prediction_perplexity` (HELM): Measure of the logprob of a given input.

## Examples of scripts to launch lighteval on the cluster
### Evaluate a whole suite on one node, 8 GPUs
1) Create a config file for accelerate

```yaml
compute_environment: LOCAL_MACHINE
distributed_type: MULTI_GPU
downcast_bf16: 'no'
gpu_ids: all
machine_rank: 0
main_training_function: main
mixed_precision: 'no'
num_machines: 1
num_processes: 8
rdzv_backend: static
same_network: true
tpu_env: []
tpu_use_cluster: false
tpu_use_sudo: false
use_cpu: false
```

2) Create a slurm file

```bash
#!/bin/bash
#SBATCH --job-name=kirby-one-node
#SBATCH --nodes=1
#SBATCH --exclusive
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=24
#SBATCH --gres=gpu:8
#SBATCH --mem-per-cpu=11G # This is essentially 1.1T / 96
#SBATCH --partition=production-cluster
#SBATCH --mail-type=ALL
#SBATCH --mail-user=clementine@huggingface.co

set -x -e
export TMPDIR=/scratch

echo "START TIME: $(date)"

# Activate your relevant virtualenv
source <path_to_your_venv>/activate #or conda activate yourenv

cd <path_to_your_lighteval>/lighteval

export CUDA_LAUNCH_BLOCKING=1
srun accelerate launch --multi_gpu --num_processes=8 run_evals_accelerate.py --model_args "pretrained=your model name" --tasks tasks_examples/open_llm_leaderboard_tasks.txt --override_batch_size 1 --save_details --output_dir=your output dir
```

## Releases

### Building the package
```bash
pip install build
python3 -m build .
```
</details>



## How to navigate this project
`indic_eval` is supposed to be used as a standalone evaluation library.
- To run the evaluations, you can use `run_indic_evals_accelerate`,`run_evals_accelerate.py` or `run_evals_nanotron.py`.
- [src/indic_eval](https://github.com/adithya-s-k/indic_eval/tree/main/src/indic_eval) contains the core of the lib itself
    - [indic_eval](https://github.com/adithya-s-k/indic_eval/tree/main/src/indic_eval) contains the core of the library, divided in the following section
        - [indic_accelerate.py](https://github.com/adithya-s-k/indic_eval/blob/main/src/indic_eval/main_accelerate.py) is the entry point to run indic languge benchmark
        - [main_accelerate.py](https://github.com/adithya-s-k/indic_eval/blob/main/src/indic_eval/main_accelerate.py) and [main_nanotron.py](https://github.com/adithya-s-k/indic_eval/blob/main/src/indic_eval/main_nanotron.py) are our entry points to run evaluation
        - [logging](https://github.com/adithya-s-k/indic_eval/tree/main/src/indic_eval/logging): Our loggers, to display experiment information and push it to the hub after a run
        - [metrics](https://github.com/adithya-s-k/indic_eval/tree/main/src/indic_eval/metrics): All the available metrics you can use. They are described in metrics, and divided between sample metrics (applied at the sample level, such as a prediction accuracy) and corpus metrics (applied over the whole corpus). You'll also find available normalisation functions.
        - [models](https://github.com/adithya-s-k/indic_eval/tree/main/src/indic_eval/models): Possible models to use. We cover transformers (base_model), with adapter or delta weights, as well as TGI models locally deployed (it's likely the code here is out of date though), and brrr/nanotron models.
        - [tasks](https://github.com/adithya-s-k/indic_eval/tree/main/src/indic_eval/tasks): Available tasks. The complete list is in `tasks_table.jsonl`, and you'll find all the prompts in `tasks_prompt_formatting.py`. Popular tasks requiring custom logic are exceptionally added in the [extended tasks](https://github.com/adithya-s-k/indic_eval/blob/main/src/indic_eval/tasks/extended).
- [tasks_examples](https://github.com/adithya-s-k/indic_eval/tree/main/tasks_examples) contains a list of available tasks you can launch. We advise using tasks in the `recommended_set`, as it's possible that some of the other tasks need double checking.
- [tests](https://github.com/adithya-s-k/indic_eval/tree/main/tests) contains our test suite, that we run at each PR to prevent regressions in metrics/prompts/tasks, for a subset of important tasks.



## Available metrics
### Metrics for multiple choice tasks
These metrics use log-likelihood of the different possible targets.
- `loglikelihood_acc` (Harness): Fraction of instances where the choice with the best logprob was correct - also exists in a faster version for tasks where the possible choices include only one token (`loglikelihood_acc_single_token`)
- `loglikelihood_acc_norm` (Harness): Fraction of instances where the choice with the best logprob, normalized by sequence length, was correct - also exists in a faster version for tasks where the possible choices include only one token (`loglikelihood_acc_norm_single_token`)
- `loglikelihood_acc_norm_nospace` (Harness): Fraction of instances where the choice with the best logprob, normalized by sequence length, was correct, with the first space ignored
- `loglikelihood_f1` (Harness): Corpus level F1 score of the multichoice selection - also exists in a faster version for tasks where the possible choices include only one token (`loglikelihood_f1_single_token`)
- `mcc` (Harness): Matthew's correlation coefficient (measure of agreement between statistical distributions),
- `recall_at_1` (Harness): Fraction of instances where the choice with the best logprob was correct - also exists in a faster version for tasks where the possible choices include only one token per choice (`recall_at_1_single_token`)
- `recall_at_2` (Harness): Fraction of instances where the choice with the 2nd best logprob or better was correct  - also exists in a faster version for tasks where the possible choices include only one token per choice (`recall_at_2_single_token`)
- `mrr` (Harness): Mean reciprocal rank, measure of the quality of a ranking of choices ordered by correctness/relevance  - also exists in a faster version for tasks where the possible choices include only one token (`mrr_single_token`)
- `target_perplexity` (Harness): Perplexity of the different choices available.
- `acc_golds_likelihood`: (Harness): A bit different, it actually checks if the average logprob of a single target is above or below 0.5
- `multi_f1_numeric`: Loglikelihood F1 score for multiple gold targets

All these metrics also exist in a "single token" version (`loglikelihood_acc_single_token`, `loglikelihood_acc_norm_single_token`, `loglikelihood_f1_single_token`, `mcc_single_token`, `recall@2_single_token` and `mrr_single_token`). When the multichoice option compare only one token (ex: "A" vs "B" vs "C" vs "D", or "yes" vs "no"), using these metrics in the single token version will divide the time spent by the number of choices. Single token evals also include:
- `multi_f1_numeric` (Harness, for CB): computes the f1 score of all possible choices and averages it.

### Metrics for perplexity and language modeling
These metrics use log-likelihood of prompt.
- `word_perplexity` (Harness): Perplexity (log probability of the input) weighted by the number of words of the sequence.
- `byte_perplexity` (Harness): Perplexity (log probability of the input) weighted by the number of bytes of the sequence.
- `bits_per_byte` (HELM): Average number of bits per byte according to model probabilities.
- `log_prob` (HELM): Predicted output's average log probability (input's log prob for language modeling).

### Metrics for generative tasks
These metrics need the model to generate an output. They are therefore slower.
- Base:
    - `perfect_exact_match` (Harness): Fraction of instances where the prediction matches the gold exactly.
    - `exact_match` (HELM): Fraction of instances where the prediction matches the gold at the exception of the border whitespaces (= after a `strip` has been applied to both).
    - `quasi_exact_match` (HELM): Fraction of instances where the normalized prediction matches the normalized gold (normalization done on whitespace, articles, capitalization, ...). Other variations exist, with other normalizers, such as `quasi_exact_match_triviaqa`, which only normalizes the predictions after applying a strip to all sentences.
    - `prefix_exact_match` (HELM): Fraction of instances where the beginning of the prediction matches the gold at the exception of the border whitespaces (= after a `strip` has been applied to both).
    - `prefix_quasi_exact_match` (HELM): Fraction of instances where the normalized beginning of the prediction matches the normalized gold (normalization done on whitespace, articles, capitalization, ...)
    - `exact_match_indicator`: Exact match with some preceding context (before an indicator) removed
    - `f1_score_quasi` (HELM): Average F1 score in terms of word overlap between the model output and gold, with both being normalized first
    - `f1_score`:  Average F1 score in terms of word overlap between the model output and gold without normalisation
    - `f1_score_macro`: Corpus level macro F1 score
    - `f1_score_macro`: Corpus level micro F1 score
- Summarization:
    - `rouge` (Harness): Average ROUGE score [(Lin, 2004)](https://aclanthology.org/W04-1013/)
    - `rouge1` (HELM): Average ROUGE score [(Lin, 2004)](https://aclanthology.org/W04-1013/) based on 1-gram overlap.
    - `rouge2` (HELM): Average ROUGE score [(Lin, 2004)](https://aclanthology.org/W04-1013/) based on 2-gram overlap.
    - `rougeL` (HELM): Average ROUGE score [(Lin, 2004)](https://aclanthology.org/W04-1013/) based on longest common subsequence overlap.
    - `rougeLsum` (HELM): Average ROUGE score [(Lin, 2004)](https://aclanthology.org/W04-1013/) based on longest common subsequence overlap.
    - `rouge_t5` (BigBench): Corpus level ROUGE score for all available ROUGE metrics
    - `faithfulness` (HELM): Faithfulness scores based on the SummaC method of [Laban et al. (2022)](https://aclanthology.org/2022.tacl-1.10/).
    - `extractiveness` (HELM): Reports, based on [(Grusky et al., 2018)](https://aclanthology.org/N18-1065/)
        - `summarization_coverage`: Extent to which the model-generated summaries are extractive fragments from the source document,
        - `summarization_density`: Extent to which the model-generated summaries are extractive summaries based on the source document,
        - `summarization_compression`: Extent to which the model-generated summaries are compressed relative to the source document.
    - `bert_score` (HELM): Reports the average BERTScore precision, recall, and f1 score [(Zhang et al., 2020)](https://openreview.net/pdf?id=SkeHuCVFDr) between model generation and gold summary.
- Translation
    - `bleu`: Corpus level BLEU score [(Papineni et al., 2002)](https://aclanthology.org/P02-1040/) - uses the sacrebleu implementation.
    - `bleu_1` (HELM): Average sample BLEU score [(Papineni et al., 2002)](https://aclanthology.org/P02-1040/) based on 1-gram overlap - uses the nltk implementation.
    - `bleu_4` (HELM): Average sample BLEU score [(Papineni et al., 2002)](https://aclanthology.org/P02-1040/) based on 4-gram overlap - uses the nltk implementation.
    - `chrf` (Harness): Character n-gram matches f-score.
    - `ter` (Harness): Translation edit/error rate.
- Copyright
    - `copyright` (HELM): Reports:
        - `longest_common_prefix_length`: average length of longest common prefix between model generation and reference,
        - `edit_distance`: average Levenshtein edit distance between model generation and reference,
        - `edit_similarity`: average Levenshtein edit similarity (normalized by length of longer sequence) between model generation and reference.
- Math:
    - `quasi_exact_match_math` (HELM): Fraction of instances where the normalized prediction matches the normalized gold (normalization done for math, where latex symbols, units, etc are removed)
    - `quasi_exact_match_gsm8k` (Harness): Fraction of instances where the normalized prediction matches the normalized gold (normalization done for gsm8k, where latex symbols, units, etc are removed)

## Acknowledgement

This evaluation library is built on top of [lighteval](https://github.com/huggingface/lighteval), which itself leverages the excellent [Eleuther AI Harness](https://github.com/EleutherAI/lm-evaluation-harness). The framework also powers the [Open LLM Leaderboard](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard).

## Contribute

### Testing and Feedback
We welcome contributions from the community to help test and improve this library. If you encounter any issues or have suggestions for enhancements, please feel free to open an issue on the GitHub repository [here](https://github.com/adithya-s-k/indic_eval).

### Star History
You can track the repository's star history and show your support by giving it a star on GitHub:

![Star History](https://api.star-history.com/svg?repos=adithya-s-k/indic_eval&type=Date)

Your stars help us to gauge interest and prioritize development efforts.

### Pull Requests
If you'd like to contribute directly to the codebase, please fork the repository, make your changes, and submit a pull request. We appreciate all contributions!

### Documentation
Improvements to documentation are always valuable. If you find areas where the documentation could be clearer or more comprehensive, please consider submitting a pull request with your proposed changes.

### Spread the Word
Help us spread the word about this evaluation library! Share it with your colleagues and networks who might find it useful. Your support is essential in growing the community around this project.