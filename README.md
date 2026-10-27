# Generative AI Response Evaluation Framework

A beginner-friendly Python project for evaluating AI-generated responses against reference answers using transparent, rule-based checks. It demonstrates QA skills relevant to AI evaluation: test-case design, response scoring, failure categorization, regression checks, and reporting.

> **Important limitation:** This starter project does not call an LLM API and does not prove that a response is factually true. It uses small, explicit rule-based checks and reference-answer overlap to flag responses for human review. A real AI evaluation workflow needs carefully designed datasets, stronger semantic evaluation, and human validation.

## Features

- Load prompt/expected-answer/response cases from CSV.
- Score responses across:
  - **Answer presence** — whether a response is provided.
  - **Reference overlap** — a simple word-overlap heuristic against the reference answer.
  - **Instruction check** — basic checks for empty responses and simple expected keywords.
- Flag likely issues for human review, including empty responses, low overlap, and missing expected keywords.
- Export per-case results to CSV.
- Calculate a basic pass rate and issue summary.
- Unit tests for the evaluation logic.
- Repeatable regression testing using the same dataset.

## Project structure

```text
generative-ai-response-evaluation/
├── README.md
├── requirements.txt
├── data/
│   └── evaluation_test_cases.csv
├── src/
│   └── evaluate_responses.py
├── tests/
│   └── test_evaluation.py
└── results/
    └── sample_evaluation_report.csv
```

## Requirements

- Python 3.10 or newer recommended
- No external Python packages required for the evaluator or tests

## Run the project

From the project root:

```bash
python src/evaluate_responses.py
```

To use a different input or output file:

```bash
python src/evaluate_responses.py --input data/evaluation_test_cases.csv --output results/evaluation_report.csv
```

Run unit tests:

```bash
python -m unittest discover -s tests -v
```

## Dataset format

The CSV contains these columns:

- `test_id`: unique case ID
- `category`: factual, reasoning, instruction_following, or negative
- `prompt`: the prompt given to the AI system
- `reference_answer`: a reference answer prepared for this test
- `ai_response`: a sample response to evaluate
- `expected_keywords`: pipe-separated terms expected in a satisfactory response
- `expected_behavior`: short description of the expected behavior

The included responses are **illustrative examples** designed to exercise pass/fail paths. They are not outputs from a live AI model.

## Scoring approach

The evaluator calculates a heuristic score out of 100:

- Answer presence: 20 points
- Reference word overlap: up to 50 points
- Expected-keyword coverage: up to 30 points

A case passes when:
- a response is present,
- the heuristic score is at least 60, and
- no required keyword is missing.

The scoring is intentionally simple and explainable for a starter project. It may incorrectly score paraphrases, synonyms, long answers, or answers that are fluent but wrong. Do not interpret the score as a reliable factuality probability.

## Issue flags

- `EMPTY_RESPONSE`
- `LOW_REFERENCE_OVERLAP`
- `MISSING_EXPECTED_KEYWORDS`

A flag means a case needs review; it is not a definitive proof of a hallucination. The project labels outputs as `PASS` or `REVIEW` rather than claiming automatic truth detection.

## Example output

The report includes:
- `test_id`
- `category`
- `score`
- `status`
- `issue_flags`
- `reference_overlap`
- `keyword_coverage`
- `response_length`
- `review_note`

Run the script to generate the actual report for the bundled dataset.

## Suggested improvements

1. Add a human-review column with reviewer name, decision, and notes.
2. Expand the dataset to 50–100 cases across factuality, reasoning, safety, and instruction following.
3. Add regression comparison between two model versions.
4. Integrate a real LLM API only after securely storing API keys in environment variables.
5. Add semantic similarity or rubric-based evaluation, and validate it against human labels.
6. Add CI tests with GitHub Actions.

