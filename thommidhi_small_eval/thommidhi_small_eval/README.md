# Thommidhi Oli — Small Tool-Use Evaluation

## What this is
A small, inspectable evaluation starter for the Task 5 brief. It contains 20 realistic creator requests, expected tool calls, and a Python script that scores an assistant transcript against a selected case.

## Files
- `cases.json` — 20 creator requests, expected tool names, and the behaviour each case checks.
- `eval.py` — rule-based tool-call precision/recall/F1 plus conservative flags for manual safety review.
- `demo_transcript.json` — **synthetic demonstration only**, not a real Oli/Task 1 transcript.
- `eval_result_demo.json` — example output from running the evaluator on the synthetic demo.

## Run
Requires Python 3.9+; no third-party packages required.

```bash
python eval.py --transcript demo_transcript.json --out eval_result_demo.json
```

To evaluate a real Task 1 transcript, export/copy the actual observed tool calls and assistant text into a JSON file using this structure:

```json
{
  "case_id": "C05",
  "tool_calls": [
    {"name": "actual_tool_name_seen_in_transcript", "arguments": {}}
  ],
  "assistant_text": "Paste the actual assistant response here"
}
```

Then run:

```bash
python eval.py --transcript task1_real_transcript.json --out task1_eval_result.json
```

**Important:** Do not label a synthetic example as a real transcript. If Oli does not expose tool calls, record that limitation and use the visible assistant interaction as qualitative evidence; do not invent hidden tool calls. Map actual tool names to the expected names only if you document the mapping.

## Scoring
For a transcript with a `case_id`, the evaluator compares the actual tool names with that case's expected tools. It reports tool precision, recall, and F1. It also raises limited keyword-based flags for manual review. This is a baseline, not a semantic evaluation: it cannot establish whether a tool call was justified, whether private data was leaked, or whether the final state was correct.

## Suggested next improvements
1. Add exact assertions for grounded values and status transitions.
2. Add a human-reviewed rubric for prompt-injection resistance and factual grounding.
3. Run every case against the assistant and preserve anonymized transcripts.
4. Track latency and errors separately from tool-selection accuracy.
