# GSM8K with Qwen2.5-0.5B-Instruct: prompting methods and a calculator agent

Agentic AI assignment, İbrahim Halil Demircioğlu. The full write-up is in `Report_Ibrahim_Halil_Demircioglu.pdf`.

| File | What it does |
|---|---|
| `q1.ipynb` | Question 1: zero-shot, four-shot, self-consistency (4 samples, majority vote) and reflection (initial solution + one review call). Reports accuracy, generated tokens and model calls. |
| `q2.ipynb` | Question 2: an agent that writes `CALC: <expression>`, calls a restricted calculator (AST-based, no `eval`/`exec`, at most 3 calls per question) and answers. Compared with the strongest Q1 method. |
| `Report_Ibrahim_Halil_Demircioglu.pdf` | Report: approach, experiments, testing, improvements and final results. |

## Setup

- Model: `Qwen/Qwen2.5-0.5B-Instruct`, frozen weights, Hugging Face `transformers`. No paid APIs.
- Data: GSM8K (`openai/gsm8k`, `main`). Dev = train examples 1–10, four-shot demos = train 11–14, test = test examples 1–30.
- Hardware used for the reported results: free Google Colab, T4 GPU (fp16). The notebooks also run on Apple Silicon (MPS) or CPU, but much more slowly.

## How to run

1. Open `q1.ipynb` in Google Colab (File → Upload notebook) and choose **Runtime → Change runtime type → T4 GPU**.
2. **Runtime → Run all.** The first cell installs the packages, and the model and dataset download automatically. It runs every method on dev, then on test, prints the final table and saves `q1_results.json` (about 10–15 minutes).
3. In the **same Colab runtime**, open `q2.ipynb` and **Run all** (about 5 minutes). It reads `q1_results.json` to compare the agent with the strongest Q1 method. If that file is missing, it prints a warning and uses the four-shot numbers typed in the notebook.

To look at any question in detail without re-running the model, use `show("four-shot")` (or another method name) in `q1.ipynb` and `show_failures()` in `q2.ipynb`.

## Final test results (test 1–30)

| Method | Accuracy | Generated tokens | Model calls | Calculator calls |
|---|---|---|---|---|
| Zero-shot | 10/30 (33%) | 7,681 | 30 | – |
| Four-shot | 10/30 (33%) | 3,446 | 30 | – |
| Self-consistency | 11/30 (37%) | 15,784 | 120 | – |
| Reflection | 10/30 (33%) | 9,687 | 60 | – |
| Calculator agent | 10/30 (33%) | 4,028 | 50 | 20 |

Sampling uses a fixed seed (`torch.manual_seed(0)`), but GPU type and library versions can change individual outputs slightly.
