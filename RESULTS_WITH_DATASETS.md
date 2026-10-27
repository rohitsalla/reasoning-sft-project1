# Results and datasets

Completed evaluations of four Qwen2.5-3B-Instruct variants. Training datasets and evaluation datasets are listed separately below.

## Training datasets

All project SFT datasets use original responses from [AceReason-1.1 100K](https://huggingface.co/datasets/redsgnaoh/acereason11_100k).

| Model | Project training dataset | Examples | Epochs | Training context | Dataset availability |
|---|---|---:|---:|---:|---|
| Qwen2.5-3B-Instruct (baseline) | No project fine-tuning; original Qwen instruction-tuned model | — | — | — | [Original model](https://huggingface.co/Qwen/Qwen2.5-3B-Instruct) |
| Random SFT | Random AceReason subset, seed 42 | 15,000 | 1 | 4,096 tokens | [random.json](https://huggingface.co/datasets/rohitspider/reasoning-sft-submission-data-15k/blob/course-submission-20261020-authors/random.json) |
| Advanced route SFT | Filtered AceReason subset with topic and reasoning-route coverage | 15,000 | 1 | 4,096 tokens | [advanced.json](https://huggingface.co/datasets/rohitspider/reasoning-sft-submission-data-15k/blob/course-submission-20261020-authors/advanced.json) |
| Assistance + coverage | Distinct AceReason subset selected using assistance utility and topic/length coverage | 15,000 | 3 | 16,384 tokens | Exact dataset is not published |

The advanced route model weights are [published on Hugging Face](https://huggingface.co/rohitspider/reasoning-sft-submission-full-15k). Random SFT and assistance-model weights are not published in the current release. See [dataset selection details](DATA_SELECTION.md).

## Overall results

| Model | Seven-task pass@1 | Seven-task pass@8 | Truncated answers / 15,792 |
|---|---:|---:|---:|
| Qwen2.5-3B-Instruct (baseline) | 23.5497% | 49.5474% | 141 |
| Random SFT | 21.1928% | 50.6262% | 2,494 |
| Advanced route SFT | 21.8526% | 51.5222% | 1,964 |
| Assistance + coverage | 22.9398% | 52.4773% | 2,324 |

The seven-task average is the unweighted mean of AMC, AIME 2025, CN Math 2024, Kaoyan, Minerva Math, OlympiadBench and GPQA-Diamond. It excludes AIME 2024 and MATH-500.

## Results by evaluation dataset

All scores in this table are **pass@8**: a question is solved when at least one of its eight sampled answers is correct.

| Evaluation dataset | Questions | Generated answers per model | Baseline | Random SFT | Advanced route SFT | Assistance + coverage |
|---|---:|---:|---:|---:|---:|---:|
| AMC | 40 | 320 | 77.50% | 77.50% | 70.00% | 70.00% |
| AIME 2025 | 30 | 240 | 13.33% | 13.33% | 20.00% | 26.67% |
| AIME 2024 | 30 | 240 | 23.33% | 20.00% | 16.67% | 20.00% |
| MATH-500 | 500 | 4,000 | 85.20% | 84.20% | 85.00% | 88.20% |
| CN Math 2024 | 30 | 240 | 23.33% | 30.00% | 33.33% | 33.33% |
| Kaoyan | 199 | 1,592 | 56.78% | 55.78% | 59.80% | 66.83% |
| Minerva Math | 272 | 2,176 | 49.63% | 46.32% | 48.90% | 47.06% |
| OlympiadBench | 675 | 5,400 | 49.48% | 49.63% | 49.33% | 52.74% |
| GPQA-Diamond | 198 | 1,584 | 76.77% | 81.82% | 79.29% | 70.71% |

Each model was evaluated on the same **1,974 questions × 8 answers = 15,792 generated answers**, using matched B200 hardware, questions, prompts, grader and decoding settings: temperature 0.6, top-p 0.95, seed 0 and a 32,768-token output limit. A truncated answer reaches that limit before finishing; truncation alone does not establish incorrectness.

The assistance comparison also changes training context, learning rate and epoch count, so its differences cannot be attributed solely to dataset selection. These are single-seed results.

Source records: [score summary](results/rubric-comparison/summary.json), [evaluation protocol](results/rubric-comparison/protocol.json), and [analysis and limitations](RESULTS.md).
