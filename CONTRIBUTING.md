# Contributing to DataLife e-Health

Maintainer: Luchang Jiang (@FinalSunFlower).

These rules apply to every repository under the `datalife-ehealth` organization.

## Before you write code

1. Search open issues. If the work is not described, open one.
2. For a feature or a new repository, use the contribution-task template and wait until it is assigned to you.
3. A comment that says "I am taking this" is not a claim. The issue must be assigned.

## Branch names

Create the branch from `main`.

| Prefix | Use |
|---|---|
| `feat/` | A new behavior or endpoint |
| `fix/` | A defect with a failing test or a clear reproduction |
| `docs/` | Documentation only |

Example: `feat/otp-single-use`.

## Pull requests

- One concern per pull request.
- Describe the behavior a reviewer can observe. Link the issue.
- Include tests for `datalife-datalake-core`. Documentation-only changes say so in the test plan.
- Do not add paid APIs, hidden network calls, or a second audit mechanism.
- Keep personal identifiers out of fixtures. Use synthetic ids.
- The ledger is a cryptographic tamper-evident log with chained Merkle trees. Do not describe it as a distributed blockchain.

A maintainer reviews for correctness, privacy boundaries, and test evidence. Approval from Luchang Jiang is required before merge.

## Local checks

```bash
pip install -e ".[test]"
pytest -q
```

Run that command inside `datalife-datalake-core` when the change touches Python.

## Contact

Open an issue, or write to Luchang Jiang at auroral.sunflower@gmail.com.
