# EchoFossil Research Notes

## Research question
How can investigators discover synthetic-data reuse, duplication, and uncertain content origins from imperfect dataset records without pretending that text style reliably identifies the author or generator?

## Evidence-based framing

### Recursive synthetic data
Shumailov et al. describe how recursively training generative models on generated data can distort the learned distribution and remove low-frequency information over generations. This supports preserving original data and tracking lineage; it does not prove that every synthetic example is harmful.
Reference: https://doi.org/10.1038/s41586-024-07566-y

**Design implication:** measure known data reuse and document source lineage. Do not predict that every dataset containing generated material will cause model collapse.

### Structured provenance
The W3C PROV-O model represents provenance using entities, activities, and agents.
Reference: https://www.w3.org/TR/prov-o/

**Design implication:** represent datasets, records, transformations, source artifacts, and named agents separately. Relationships must include evidence, time, and verification method rather than only a descriptive label.

### Risk management and uncertainty
NIST's Generative AI Profile describes lifecycle risk management and documentation of limitations.
Reference: https://doi.org/10.6028/NIST.AI.600-1

**Design implication:** publish false-positive risks, data coverage, privacy expectations, and a review path. A heuristic score must not become a definitive verdict.

## Main failure modes

| Failure | Why naive methods fail | Research response |
|---|---|---|
| No generator metadata | Style classifiers vary across domains and models | Keep origin unknown unless supported by evidence |
| Hidden duplicates | Tokenization and normalization change representation | Layer raw hashes, normalized hashes, and candidate similarity |
| Match mistaken for contamination | Quotes, boilerplate, licensed reuse, and common sources can match | Add source context and human review |
| Benchmark leakage | Overlap may appear after chunking or transformation | Compare declared splits and corpus versions |
| Missing or forged provenance | A metadata claim is not automatically verified | Record source, verification method, and trust class |
| Quadratic comparison cost | All-pairs scans scale poorly | Filter and partition before expensive comparison |
| Sensitive data in reports | Raw text may be confidential or personal | Prefer IDs, counts, hashes, and minimal excerpts |
| Inference chains become “facts” | Inferred edges can propagate through a graph | Preserve evidence grade and prevent silent confidence upgrades |

## Evidence classes

- **Direct:** verifiable source artifact, fingerprint, or reproducible transformation.
- **Derived:** repeatable computation such as normalized equality or a specified similarity measure.
- **Declared:** supplied by a dataset publisher or pipeline but not independently verified.
- **Inferred:** plausible relationship supported indirectly, with rationale and uncertainty.
- **Unknown or disputed:** insufficient or conflicting evidence.

Every graph relationship should retain the source and method behind it.

## Open questions
1. Which modalities are in the first stable release?
2. Which normalizations are safe for each modality?
3. What precision/recall trade-offs are acceptable for candidate generation?
4. How can restricted or deleted source data be represented without retaining its contents?
5. How should very large corpora be indexed without losing lineage?
6. Which licence/privacy checks are automatable versus human review?

## Research exit criteria
Any detector needs labelled, permissioned data containing known duplicates, transformed copies, hard negatives, and unknown-origin records. Publish domain, sample size, labelling method, precision/recall, false-positive examples, and resource limits. Never publish a headline metric without the evaluation design.

**Conclusion:** provenance and duplicate analysis can provide useful evidence; origin remains uncertain when reliable lineage is absent.
