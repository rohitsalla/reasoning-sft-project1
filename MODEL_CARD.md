---
library_name: transformers
base_model: Qwen/Qwen2.5-3B-Instruct
pipeline_tag: text-generation
---
# Qwen2.5-3B Reasoning SFT

**Rohit Kumar Salla · Manoj Saravanan · Haissam Adam**

This release contains the **advanced route SFT model**: Qwen2.5-3B-Instruct fine-tuned on 15,000 advanced-selected original AceReason examples using full-parameter supervised fine-tuning. Its seven-task pass@8 is **51.5222%**.

The original baseline is Qwen2.5-3B-Instruct before this project's fine-tuning; it already includes Qwen's instruction tuning. Random SFT and assistance-model weights are not published in this release. The assistance model's 52.4773% result refers to a different model and dataset.

## Results

| Model | Seven-task pass@1 | Seven-task pass@8 | Truncated / 15,792 |
|---|---:|---:|---:|
| Qwen2.5-3B-Instruct (baseline) | 23.5497% | 49.5474% | 141 |
| Random SFT | 21.1928% | 50.6262% | 2,494 |
| Advanced route SFT (weights in this release) | 21.8526% | 51.5222% | 1,964 |
| Assistance + coverage | 22.9398% | 52.4773% | 2,324 |

The seven-task mean averages AMC, AIME 2025, CN Math 2024, Kaoyan, Minerva Math, OlympiadBench and GPQA-Diamond; it excludes AIME 2024 and MATH-500. All four models completed the same nine-benchmark evaluation: 1,974 questions, eight answers each, matched B200 hardware, prompts, grader and decoding settings. Truncation counts answers that reached the output limit before finishing.

See [results with training and evaluation datasets](https://github.com/rohitsalla/reasoning-sft-project1/blob/main/RESULTS_WITH_DATASETS.md) and [all nine scores and limitations](https://github.com/rohitsalla/reasoning-sft-project1/blob/main/RESULTS.md). These are single-seed results. Assistance + coverage changes context, learning rate and epoch count as well as selection; its differences cannot be attributed solely to selection.

## Training

| Parameter | Value |
|---|---|
| Training dataset | `advanced.json`, 15,000 original AceReason examples |
| Training method | Full-parameter LLaMA-Factory SFT |
| Context length | 4,096 tokens |
| Epochs / optimizer updates | 1 / 1,875 |
| Learning rate | 2e-6 |
| Schedule / warmup | Cosine / 3% |
| Effective batch size | 8 |
| Precision / attention | BF16 / SDPA |
| Seed | 42 |

The matching random model uses `random.json` with the same training settings. Assistance + coverage instead uses a distinct 15,000-example dataset, 16,384-token context, learning rate 5e-6, no warmup and three epochs. Its weights and exact dataset are not published.

## Usage

```python
from transformers import AutoModelForCausalLM, AutoTokenizer

model_id = "rohitspider/reasoning-sft-submission-full-15k"
revision = "course-submission-20261020-authors"
tokenizer = AutoTokenizer.from_pretrained(model_id, revision=revision)
model = AutoModelForCausalLM.from_pretrained(model_id, revision=revision)
```

Apply the Qwen chat template to inputs. See the [reproduction guide](https://github.com/rohitsalla/reasoning-sft-project1/blob/main/REPRODUCE.md) for the training and matched evaluation commands.

[Training datasets](https://huggingface.co/datasets/rohitspider/reasoning-sft-submission-data-15k) · [Project repository](https://github.com/rohitsalla/reasoning-sft-project1) · [Report PDF](https://github.com/rohitsalla/reasoning-sft-project1/blob/main/REPORT.pdf)

The model can produce incorrect answers. Upstream Qwen and AceReason license terms apply.
