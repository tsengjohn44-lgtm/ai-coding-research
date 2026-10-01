# Local LLM Coding Research

A beginner research notebook exploring coding evaluation with Qwen3.5-4B on a MacBook, using MLX LM. The goal is to learn how to run models, preserve experiment records, inspect generated code, and eventually evaluate and fine-tune a model.

## Current status

- Completed two informal generation runs on September 28, 2026.
- Recorded the Python environment for run 002.
- Reviewed generated functions by inspection; automated correctness tests have not yet been executed.
- Run 003 is planned as a repeat of run 002.
- HumanEval evaluation and local fine-tuning have not yet been performed.

These initial prompts are custom exercises, not HumanEval problems. No HumanEval score is claimed.

## Setup

| Component | Recorded value |
|---|---|
| Computer | MacBook Pro M3 MAX 48GB|
| macOS | 26.5.2 |
| Python | 3.11.16 |
| MLX LM | 0.31.3 |
| MLX / MLX Metal | 0.32.2 / 0.32.2 |
| Model | `mlx-community/Qwen3.5-4B-4bit` |
| Model revision | Not yet recorded |
| Quantization | 4-bit community conversion |

Versions above come from the environment recorded after run 002. See `runs/002-largest-loop/environment.txt` for the complete package list. A model identifier alone does not pin an immutable model revision.

## Initial observations

| Run | Task | Output budget | Generated tokens | Generation tokens/s | Reported peak memory | Status |
|---|---|---:|---:|---:|---:|---|
| 001 | Largest number; no algorithm restriction | 512 | 456 | 118.476 | 2.551 GB | Generated function uses `max()`; not execution-tested |
| 002 | Largest integer using a loop; no `max()` or `sorted()` | 1024 | 180 | 119.216 | 2.623 GB | Appears correct by inspection; not execution-tested |
| 003 | Repeat run 002 with the same explicit settings | 1024 | Pending | Pending | Pending | Not yet run |

Both observed responses included a reasoning section followed by a final answer. Run 002 initialized its running maximum with the first list element, which supports all-negative inputs. Its final answer used Markdown code fences; the complete raw output was not directly executable Python.

The prompts and output budgets differ between runs 001 and 002, so their differences cannot be attributed to a single changed variable. These are individual measurements, not reliable estimates of general accuracy or typical speed. Reported peak memory is a runtime statistic, not total system memory.

## Experiment records

The root `README.md` summarizes the project. Each `runs/<run-name>/` folder holds one experiment:

| File | Purpose |
|---|---|
| `command.txt` | Exact generation command |
| `output.txt` | Unmodified captured output |
| `environment.txt` | Python and package versions |
| `notes.md` | Observations, test status, and limitations |

The output and command records for earlier runs are saved manually from Terminal history. Keep original observations separate from later reruns. Save future runs to fresh folders. Do not upload model weights, virtual environments, or credentials.

## Third-run protocol

Repeat the exact run 002 prompt and 1024-token budget. Preserve the command, raw output, and current environment. Record sampling settings as defaults not explicitly captured; do not invent a temperature or seed.

Compare whether the final function still uses a loop, starts with the first element, handles negative integers by inspection, and finishes before the token limit. This is an informal repeatability check, not a controlled estimate of model accuracy. Separate response generation from execution testing.

## Benchmark card: HumanEval — Python function completion

**What information does the model receive?** A Python function signature and a docstring describing required behavior, usually with examples. Evaluation tests are withheld from the prompt.

**What must it produce or accomplish?** Complete the function with working Python code.

**How is success checked?** Generated code is executed against automated tests. Results use pass@k: the estimated probability that at least one of k candidates passes all required tests. With one candidate per problem, observed pass@1 is the fraction of problems solved. Evaluation needs an isolated execution environment.

**What ability does this measure?** Understanding a specification and implementing a short, correct function.

**What does it fail to measure?** Navigating large projects, modifying multiple files, collaborating with developers, and maintaining software. Passing the tests does not prove correctness on every possible input.

**What could make its score misleading?** Prior exposure to public problems, incomplete tests, and comparisons using different numbers of attempts, prompts, or execution settings.

Source: [Official HumanEval repository](https://github.com/openai/human-eval).

## Next milestones

- Finish run 003 and replace its pending results with actual observations.
- Test generated functions in an isolated environment and preserve test results.
- Validate a small HumanEval run before evaluating the full set.
- Study a SWE-bench Verified issue and compare an attempted repair with its reference patch.
- Compare reported model scores while recording benchmark versions and evaluation conditions.
- Establish a baseline before experimenting with LoRA fine-tuning.

## References

- [MLX LM](https://github.com/ml-explore/mlx-lm)
- [Model repository](https://huggingface.co/mlx-community/Qwen3.5-4B-4bit)
- [lm-evaluation-harness](https://github.com/EleutherAI/lm-evaluation-harness)
- [HumanEval](https://github.com/openai/human-eval)
