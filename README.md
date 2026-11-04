# ATTCBench artifact

Reproducibility materials accompanying:

**[Experiment, Analysis, and Benchmark] ATTCBench: Stress-Testing Data Systems under Agentic and Test-Time-Compute Query Workloads**

[Download `ATTCBench_artifact_review_20261105.zip`](ATTCBench_artifact_review_20261105.zip)

## Included scope

The archive contains deterministic agentic/test-time-compute workload traces, a trace generator and schema, replay and characterization scripts, result aggregation tools, and existing raw logs and summaries.

- **Dry-run validation:** checks trace generation, replay, reuse accounting, and metric aggregation. Its simulated latencies are **not DBMS performance measurements**.
- **DuckDB native subset:** executes relational trace records against deterministic in-memory `fact` and `dim` tables.
- **FAISS native subset:** executes vector trace records against a seeded synthetic corpus using `IndexFlatL2`.

These are native subtrace evaluations, not a complete multi-system experimental campaign. PostgreSQL, Spark SQL, Milvus, pgvector, Flink, and Materialize appear as target systems in the manifest but have no native adapter in this archive. The package makes no promise to reproduce results for those systems.

## Requirements

Use **Python 3.9–3.14**; Python 3.11 is a suitable choice. The dry-run entry point uses the Python standard library. The Makefile also requires `make` and `zip`.

The optional native subset uses the supplied `artifact9/requirements-real.txt`:

```text
duckdb==1.4.4
faiss-cpu==1.13.0
numpy>=1.25,<3
```

## Extract and run

Download the ZIP, then extract it into an empty directory:

```bash
mkdir attcbench-release
unzip ATTCBench_artifact_review_20261105.zip -d attcbench-release
cd attcbench-release
```

The extracted root contains **`Makefile` beside `artifact9/`**. Run commands from this root, not from inside `artifact9/`.

Generate and characterize the seeded trace, run dry-run validation, and aggregate the results:

```bash
make artifact
```

Plain `make` selects the same `artifact` target. This target also writes a generated `artifact9.zip`; that file is distinct from the downloaded, checksummed release archive.

Install the optional dependencies and run the DuckDB/FAISS native subset:

```bash
python3 -m pip install -r artifact9/requirements-real.txt
make real-subset
```

`make real-subset` also invokes the existing fanout sweep. To invoke that sweep separately after installing the dependencies:

```bash
make fanout-sweep
```

Existing traces are in `artifact9/traces/`; raw outputs are in `artifact9/results/raw/`, with summaries and characterization under `artifact9/results/`. Running the commands may overwrite generated traces or outputs. Native wall-clock timings depend on the execution environment.

The inherited `paper`, `clean`, and explicit `all` targets concern manuscript building, require files omitted from this artifact-only release, and are not artifact entry points. See `RELEASE_README.md` and `artifact9/README.md` inside the archive for packaging provenance and further details. The artifact scripts were not run during release packaging or the static publication check.

## Archive integrity

File: `ATTCBench_artifact_review_20261105.zip`

SHA256:

```text
bb2f23e7d42f844767165a029c37c9e0d03cc9ef6745df8b6feb556e164c060a
```

Verify the downloaded file with either of the following commands:

```bash
sha256sum ATTCBench_artifact_review_20261105.zip
# macOS alternative:
shasum -a 256 ATTCBench_artifact_review_20261105.zip
```

The archive includes `MANIFEST.json` and `SHA256SUMS` with per-file provenance and integrity information. The release ZIP is preserved unchanged; the root Makefile adds only `.DEFAULT_GOAL := artifact` to the original Makefile, whose original copy is included under `provenance/`.
