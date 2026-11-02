# Fine Tuning Lab: Zero-Shot, Few-Shot, and LoRA/Q-LoRA

This repo is my version of the Fine Tuning lab from the [Northeastern MLOps course](https://github.com/raminmohammadi/MLOps/tree/main/Labs/Model_Development/Fine%20Tuning). It covers three ways to adapt a language model to a task:

| Notebook | What it shows |
|---|---|
| `Fine Tuning with Zero Shot Learning.ipynb` | Classifying restaurant reviews from instructions alone |
| `Fine Tuning with Few Shot Learning.ipynb` | Improving that classifier by adding labeled examples to the prompt |
| `Fine Tuning with LoRA and Q-LoRA.ipynb` | A from-scratch TensorFlow implementation of LoRA and 4-bit Q-LoRA |
| `Assignment.ipynb` | A few-shot customer service response generator, checked by `tests/test_methods.py` |

## My modifications

**1. New model: `gpt-3.5-turbo` → `gpt-6-luna`.** The original lab called `gpt-3.5-turbo`, which OpenAI shut down on October 23, 2026, so the notebooks no longer ran. All three notebooks that call the API now use `gpt-6-luna`, set once in a `MODEL_NAME` variable so it's easy to swap again.

**2. Updated API parameters for the new model.** Newer OpenAI models reject `temperature` and `max_tokens` in Chat Completions. The calls now use `reasoning_effort="none"` (fastest and cheapest for short answers) and `max_completion_tokens`.

**3. Safer API key handling.** The assignment originally had you paste your API key into the notebook, which would leak it once the repo is public. It now loads the key from a `.env` file that is excluded by `.gitignore`.

**4. Expanded customer service bot (`Assignment.ipynb`).**
- Two new tones: `reassuring` and `concise`
- Two new categories with examples: `billing` and `order_status`
- Per-category response guidelines added to the prompt (for example, refunds start with an apology, and technical answers use numbered steps)
- Input validation that raises `ValueError` for empty messages, unknown categories, and unknown tones
- Two new demo scenarios that exercise the new categories and tones

The LoRA/Q-LoRA notebook is unchanged. It defines the LoRA classes but doesn't load a pretrained model or call an API.

## How to run

```bash
pip install -r requirements.txt
echo "OPENAI_API_KEY=your-key-here" > .env
jupyter notebook
```

Run `Assignment.ipynb` from the repo root so it can import `tests/test_methods.py`. The last cell prints the test results.

Running the notebooks requires an OpenAI API key with billing enabled.
