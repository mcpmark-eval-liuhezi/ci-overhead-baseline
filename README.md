# ci-overhead-baseline

A throwaway repository created to measure the **compute overhead of a GitHub Actions evaluation harness** that does no real work.

## What this measures

The workflow at `.github/workflows/overhead.yml` contains a single job on `ubuntu-latest` with a single `echo` step. It performs no checkout, no dependency installation, no model calls, and no test execution.

Any compute time this run reports is therefore **pure harness overhead**: runner provisioning, agent/image setup, job wrapper time, and teardown.

## Why

Position note: tool-use-agent papers routinely evaluate their systems on GitHub Actions-based harnesses, yet almost never report the compute overhead of the harness itself. This repository exists to provide a concrete, reproducible baseline number for that overhead.

## Reproducing

1. Push any commit to `main` (the workflow triggers on `push`).
2. Wait for the run to complete (expected: well under one minute).
3. Read the run's usage metrics via the GitHub API:

```
GET /repos/mcpmark-eval-liuhezi/ci-overhead-baseline/actions/runs/{run_id}/timing
```

The `billable` map reports milliseconds per platform, and `run_duration_ms` reports total run duration.
