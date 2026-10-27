---
task_categories:
- text-generation
language:
- en
---
# Reasoning SFT Datasets

**Rohit Kumar Salla · Manoj Saravanan · Haissam Adam**

Two 15,000-example datasets derived from [AceReason-1.1 100K](https://huggingface.co/datasets/redsgnaoh/acereason11_100k), retaining original training responses:

| File | Examples | Selection | Completed model evaluation |
|---|---:|---|---|
| `random.json` | 15,000 | Random sampling with seed 42 | All nine benchmarks; seven-task pass@8 50.6262% |
| `advanced.json` | 15,000 | Quality filtering, topic coverage and reasoning-route diversity | All nine benchmarks; seven-task pass@8 51.5222% |

Each record contains `instruction`, `input`, `output` and `system` fields. The [published advanced model](https://huggingface.co/rohitspider/reasoning-sft-submission-full-15k) was trained on `advanced.json`. The completed random model uses `random.json`, with matching one-epoch full-parameter training settings. Random-model weights are not published.

The assistance-and-coverage model uses a **different** 15,000-example AceReason subset. Neither that exact dataset nor its model weights are included in this release. Do not substitute `advanced.json` and describe it as the assistance dataset.

## Results

| Model | Seven-task pass@1 | Seven-task pass@8 | Truncated / 15,792 |
|---|---:|---:|---:|
| Qwen2.5-3B-Instruct (baseline) | 23.5497% | 49.5474% | 141 |
| Random SFT | 21.1928% | 50.6262% | 2,494 |
| Advanced route SFT (weights in this release) | 21.8526% | 51.5222% | 1,964 |
| Assistance + coverage | 22.9398% | 52.4773% | 2,324 |

The seven-task mean averages AMC, AIME 2025, CN Math 2024, Kaoyan, Minerva Math, OlympiadBench and GPQA-Diamond; it excludes AIME 2024 and MATH-500. All four models completed the same nine-benchmark evaluation: 1,974 questions, eight answers each, matched B200 hardware, prompts, grader and decoding settings. Truncation counts answers that reached the output limit before finishing.

See [results with training and evaluation datasets](https://github.com/rohitsalla/reasoning-sft-project1/blob/main/RESULTS_WITH_DATASETS.md) and [all nine scores and limitations](https://github.com/rohitsalla/reasoning-sft-project1/blob/main/RESULTS.md). These are single-seed results. Assistance + coverage changes context, learning rate and epoch count as well as selection; its differences cannot be attributed solely to selection.

## Load

```python
from datasets import load_dataset

release = "course-submission-20261020-authors"
base_url = (
    "https://huggingface.co/datasets/"
    "rohitspider/reasoning-sft-submission-data-15k/resolve/" + release
)
data = load_dataset(
    "json",
    data_files={
        "random": base_url + "/random.json",
        "advanced": base_url + "/advanced.json",
    },
)
```

The readable release tag preserves the original dataset exports. See [selection method](https://github.com/rohitsalla/reasoning-sft-project1/blob/main/DATA_SELECTION.md) and [reproduction commands](https://github.com/rohitsalla/reasoning-sft-project1/blob/main/REPRODUCE.md) for filtering, training and limitations.

[Published advanced model](https://huggingface.co/rohitspider/reasoning-sft-submission-full-15k) · [Project repository](https://github.com/rohitsalla/reasoning-sft-project1) · [Report PDF](https://github.com/rohitsalla/reasoning-sft-project1/blob/main/REPORT.pdf)

Upstream AceReason dataset terms apply.
