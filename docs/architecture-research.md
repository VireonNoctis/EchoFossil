# EchoFossil Architecture Research

## Design principle
Build a provenance ledger and candidate-generation pipeline before a large visual graph interface. Do not introduce a graph database until measured query patterns require it.

## Planned components
1. **Snapshot registry:** dataset identity, version, hashes, acquisition metadata, licence notes, and declared split.
2. **Readers:** stream supported JSONL, CSV, and Parquet through format-specific adapters.
3. **Normalizer:** create a comparison representation without replacing original bytes or identifiers.
4. **Fingerprint engine:** exact hashes first; optional similarity features only when configured and benchmarked.
5. **Provenance graph builder:** add evidence-backed entities and relationships.
6. **Investigation engine:** overlap, split leakage, repeated source, and lineage queries.
7. **Report writer:** versioned JSON/JSONL plus a human-readable summary.
8. **Optional review interface:** Svelte + TypeScript for clusters and analyst annotations.

## Intended stack

- Python 3.13+ for CLI, adapters, and orchestration.
- Polars and DuckDB for tabular processing and local SQL analysis.
- PyArrow/Parquet for typed columnar intermediates.
- Pydantic and JSON Schema for contracts.
- SQLite for local cases; PostgreSQL only if shared workflows justify it.
- Rust only for measured hot paths after profiling.
- pytest, property-based tests, and GitHub Actions for verification.

## Proposed data model
- **DatasetSnapshot:** ID, logical name, version, source, acquisition date, input hash, usage notes, and declared splits.
- **Record:** snapshot ID, local record ID, byte hash, optional normalized fingerprint, modality, and metadata reference.
- **Activity:** ingestion, normalization, transformation, split assignment, or export with versioned parameters.
- **Agent:** person, organization, pipeline, or tool named as responsible for an action.
- **Evidence:** artifact, manifest claim, computed match, or external source reference.
- **Relationship:** typed edge with evidence IDs, method/version, time, status, and confidence rationale.
- **Finding:** reviewable issue, candidate records, rationale, unresolved questions, and disposition.

Use PROV-O concepts where helpful, but keep the initial schema small and document deviations.

## Scalability
Stream data and calculate exact hashes before expensive similarity work. Use partitions, length filters, or locality-sensitive methods only after benchmarking. Avoid unconditional all-pairs comparisons. Version intermediates so resumed runs cannot mix different normalization algorithms.

## Security and privacy
Treat dataset files, filenames, metadata, and archive entries as untrusted. Apply size limits and explicit encoding errors. Never deserialize executable formats from unknown sources. Run parsers with least privilege. Prefer record IDs, counts, hashes, and permitted short excerpts over raw text in reports. A hash is not anonymization.

## Decisions to record
Document fingerprint algorithms, normalization rules, duplicate semantics, schema evolution, provenance trust levels, retention, and the meaning of “likely overlap”. Each algorithm needs positive fixtures and hard negatives.
