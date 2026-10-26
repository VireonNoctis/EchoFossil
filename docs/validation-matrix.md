# EchoFossil Validation Matrix

## Test data policy
Use synthetic or openly licensed fixtures. Do not commit private or restricted datasets. Every labelled fixture should explain why each pair is considered a match or non-match.

## Test groups

| Group | Cases | Required property |
|---|---|---|
| Snapshot integrity | Identical file, changed file, changed manifest | Same bytes yield stable hashes; changes are detected |
| Readers | Valid rows, malformed row, missing field, unsupported encoding | Errors are explicit and traceable |
| Exact duplicates | Byte equality, newline and encoding differences | Raw and normalized hashes remain distinct |
| Near duplicates | Paraphrase, quote, template, unrelated lookalike | Scores are reproducible and not treated as authorship proof |
| Split leakage | Same record in train/test; transformed duplicate; no overlap | Candidate overlap shows source split |
| Provenance | Verified, declared, inferred, and contradictory edges | Evidence class and source remain attached |
| Scale | Large records, skewed sizes, repeated IDs, increasing corpus size | Resource use is bounded or fails clearly |
| Privacy | Full-text and sensitive metadata fixtures | Default reports avoid raw source text |
| Export | Schema versions and interrupted export | Output validates and indicates incomplete runs |
| Security | Malicious filenames, huge lines, control characters, executable serializations | No code execution and safe failure |

## Evaluation protocol
For near-duplicate detection, record domain, size, label process, threshold, precision, recall, false-positive examples, runtime, and peak memory. Keep tuning data separate from the final evaluation set. Include hard negatives such as standard clauses, quotations, prompts, code snippets, and templated records.

Evaluate direct, declared, and inferred provenance edges separately. A good aggregate score can hide poor performance on the evidence class that matters most.

## Acceptance criteria
- Identical snapshots produce reproducible fingerprints.
- Input contents remain unchanged.
- Unknown origin is preserved when evidence is missing.
- Every match has a reason code, algorithm version, and path to source records.
- No origin verdict is based solely on writing style.
- Partial results are explicitly marked incomplete.
- Default export minimizes source content and uses no remote service.

## Regression policy
Every confirmed defect becomes a fixture. Changes to normalization or similarity must state whether historical results will change. Every report must include algorithm versions so older investigations remain interpretable.
