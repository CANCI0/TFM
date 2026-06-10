# SFT Dataset Schema for Bias Mitigation

## Expected Format
Store the dataset as JSONL with one object per line and a required `text` field.

Minimal rewrite example:
```json
{"text": "### Instruction\nRewrite the sentence so it no longer relies on a social stereotype.\n\n### Response\nThe person struggled with the task because they had limited training."}
```

Minimal classification example:
```json
{"text": "### Instruction\nRead the context and answer the question without relying on stereotypes.\n\n### Response\nC"}
```

## Recommended Metadata Fields
For traceability, keep an auxiliary CSV or JSON file with at least:
- `id`: unique example identifier.
- `bias_type`: category such as gender, race-color, disability, or age.
- `source`: benchmark, manual, or synthetic origin.
- `source_id`: original example identifier in the benchmark, when available.
- `split`: `train` or `valid`.
- `template`: task family, such as rewrite or multiple-choice classification.
- `generation_mode`: deterministic label or model-generated rewrite.
- `instruction`: original instruction text.
- `target_response`: expected answer stored before packing into `text`.
- `notes`: reference sentence, gold answer, or annotation note.

## Response Contracts
1. Keep both instructions and responses in English.
2. For classification tasks, the response must be exactly one option letter such as `A`, `B`, or `C`.
3. For rewrite tasks, the response must be exactly one sentence.
4. Do not add explanations, citations, research claims, notes, or extra headings inside `### Response`.
5. Preserve useful semantics and avoid inventing new facts.
6. Reject rewrites that only swap the protected group or flip the stereotype polarity, such as `women -> men` or `poor -> rich`.
7. If rewrite quality is uncertain, keep those examples in a separate audit artifact instead of mixing them into the main training split.

## Quality Criteria
1. Avoid exact and near-semantic duplicates.
2. Balance bias categories across splits.
3. Keep response style consistent within each template.
4. Verify that debiasing does not reduce task utility or factual faithfulness.
5. Prefer deterministic targets when a benchmark already provides a correct answer.
6. Record curation changes in `plan.md`.
7. Audit rewrite-style examples separately and flag any response that still asserts the same stereotype after a surface substitution.

## Prompt Convention
Always use:
- `### Instruction`
- `### Response`
