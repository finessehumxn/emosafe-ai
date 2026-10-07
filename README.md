# EmoSafe AI

A small Streamlit probe that sends emotionally loaded prompts to an LLM and flags the response with simple heuristics: refusal, very short answer, absolute language.

This was an early experiment (April 2026). It is 53 lines, three prompts and one model. I am keeping it public because it is where the question started: what does a model do when the input carries emotional weight that nobody labelled?

## What it does

- Sends three prompts, from explicit distress to a neutral coping question, to `gpt-4o-mini` with a plain system prompt.
- Flags each response with keyword rules: `refusal detected`, `low confidence / short response`, `overconfidence risk` (the words "always" or "never"), or `standard response`.
- Shows prompt, response and flag as JSON in a Streamlit page.

## Run it

```bash
pip install -r requirements.txt
mkdir -p .streamlit && echo 'OPENAI_API_KEY = "sk-..."' > .streamlit/secrets.toml
streamlit run app.py
```

## What it taught me

Keyword flags on the *output* are the wrong layer. By the time you are grading a response, the unsafe text already exists. The better design is to classify the *input* first and stop the pipeline before generation on the dangerous paths, and to fail closed when the classifier is unavailable.

That is what I built next:

- **[failclosed-guardrail](https://github.com/finessehumxn/failclosed-guardrail)**: a LangGraph guardrail that routes before generation, fails closed, and gates CI on a labelled eval set.
- **[ai-failure-analysis](https://github.com/finessehumxn/ai-failure-analysis)**: the companion experiment on input classification.
- **[Case study: a safety pipeline that runs before generation](https://github.com/finessehumxn/ai-engineering-portfolio/blob/main/case-studies/01-safety-first-langgraph.md)**

## Limits

Three prompts is a demo, not an evaluation. The heuristics are crude by design. No results are claimed here.

L.Finesse Humxn · [finessehumxn.com/work](https://finessehumxn.com/work)
