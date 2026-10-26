# EchoFossil Mitigation Strategy

EchoFossil makes dataset-risk investigation systematic. It does not guarantee contamination prevention; it helps discover, review, and document candidate problems.

## Risk-to-control map

| Risk | Control | Validation | Residual limitation |
|---|---|---|---|
| Synthetic data reused recursively | Capture source and generator metadata where available; link dataset versions and transformations | Known-lineage fixture with expected edges | Unlabelled generated data may be impossible to identify |
| Train/test leakage | Compare fingerprints across splits and historical snapshots | Known overlap and non-overlap fixtures | Common text can resemble leakage |
| Exact duplicate inflation | Hash original bytes and explicitly normalized forms separately | Tests for whitespace, encoding, newline differences | Normalization may erase meaningful distinctions |
| Near-duplicate false positives | Multi-stage candidates, configurable thresholds, reason codes, human queue | Labelled positives and difficult negatives | Similarity does not prove copying or origin |
| False AI-generated verdict | No binary origin verdict based on style alone; allow unknown | Weak evidence must not trigger a definitive origin label | Users may misread scores |
| Unverified metadata | Record claimant/source, time, verification method, and artifact hash | Tampered manifest and hash mismatch tests | A signed claim can still be false |
| Excessive compute cost | Hash-first filtering, partitions, bounded comparisons, profiling | Increasing corpus-size and memory tests | Very large investigations may need external infrastructure |
| Privacy or copyright exposure | Read-only inputs, minimal retention, local-first execution, explicit export | Ensure default output omits raw text | Hashes can sometimes reveal known-content membership |
| Destructive curation | Report only; never silently delete or rewrite source records | Verify input fingerprints remain unchanged | Human follow-up may still be mistaken |

## Operating safeguards

- Source datasets are read-only. Fingerprint inputs before analysis and never mutate them in place.
- Keep original-byte hashes distinct from normalized fingerprints.
- Version every normalization and similarity algorithm.
- Keep evidence, inference, and analyst annotations separate.
- Unknown origin is not proof of human authorship.
- Do not upload corpora or snippets to a third party by default.
- Make report contents and export scope visible.
- Never automatically purge records based solely on a similarity score.

## Release gates
Exact-match behaviour must be deterministic; every report must trace back to input snapshots; near-duplicate metrics must be measured on labelled data; and privacy tests must show that default reports do not copy source content accidentally.
