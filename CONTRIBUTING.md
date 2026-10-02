# Contributing to DataLife e-Health

Thank you for contributing to DataLife e-Health. This guide applies across the
organization. A repository's own `CONTRIBUTING.md`, `SECURITY.md`, and README may add
more specific requirements.

Maintainer: [Luchang Jiang (@FinalSunFlower)](https://github.com/FinalSunFlower).

## Start here

1. Read the [organization profile](profile/README.md) and the target repository's
   README to understand its maturity and system boundary.
2. Search existing issues and the current milestone before proposing work.
3. Comment on a scoped issue with your intended approach. A comment is not a claim;
   wait for maintainer assignment before substantial implementation.
4. Fork the repository, branch from `main`, and keep the pull request focused on one
   agreed outcome.

Good first issues are deliberately narrow. An issue's acceptance criteria define a
minimum safe result, not the only implementation or the limit of future work.

## RFC-first changes

Open an RFC-style issue and align on the design before writing substantial code when
a change affects any of the following:

- architecture, frameworks, public schemas, or API contracts;
- privacy, authorization, cryptography, key custody, recovery, or trust boundaries;
- a new external service, network destination, telemetry path, or paid dependency;
- agent tools, prompts with operational effects, clinical workflows, or human-review
  boundaries;
- analytics data contracts, cohort definitions, model claims, or bulk exports; or
- compatibility across more than one ecosystem repository.

A useful RFC explains the user need, current reality, proposed outcome, non-goals,
alternatives, privacy and security impact, migration or compatibility concerns, and
testable acceptance criteria. Diagrams, schemas, threat models, and wireframes are
welcome; a large code submission is not required to start the discussion.

Small documentation corrections and well-reproduced bug fixes usually need a focused
issue rather than a full RFC unless they change a boundary above.

## Branch names

Create branches from the latest `main`.

| Prefix | Use |
|---|---|
| `feat/` | New observable behavior or capability |
| `fix/` | A defect with a reproduction or failing test |
| `docs/` | Documentation-only work |
| `test/` | Test or evaluation coverage without behavior changes |
| `chore/` | Maintenance with no product behavior change |

Examples: `feat/otp-consent-review`, `fix/merkle-tamper-case`,
`docs/mobile-threat-model`.

## Project-wide boundaries

- Use synthetic, invented, or explicitly licensed de-identified data only. Never put
  real patient information in source, fixtures, issues, screenshots, recordings, or
  pull requests.
- Keep names, government identifiers, phone numbers, email addresses, emergency
  contacts, and identity mappings out of central services and external telemetry.
- Treat opaque subject identifiers, access grants, physician identifiers, audit
  reasons, and clinical content as sensitive.
- Do not add hidden network calls, mandatory paid services, proprietary cloud/EHR
  lock-in, or a second audit mechanism.
- Do not describe the chained Merkle audit log as a distributed blockchain.
- Do not present research software as medical advice, a clinical decision maker, a
  certified medical device, or an emergency-response service.
- Agent output is untrusted and cannot bypass typed validation, authorization, or
  required human review.
- Downstream analytics remain read-only and must not scrape transactional internals.

If a proposal cannot preserve these boundaries, the RFC must identify the conflict
explicitly. Do not silently weaken an invariant to make an implementation pass.

## Development and verification

Follow the target repository's README for its current toolchain. Several ecosystem
repositories are still scaffolds; do not introduce a framework before the relevant
RFC is accepted.

For `datalife-datalake-core`:

```bash
python -m pip install -e ".[test]"
python -m pytest -q
```

For documentation:

```bash
npx --yes markdownlint-cli2@0.18.1 "**/*.md" "#**/.git/**"
```

As implementations arrive, repository-specific checks take precedence. A pull
request should test the behavior it changes and include evidence proportional to its
risk: unit and contract tests, adversarial evaluation, accessibility checks,
cross-platform verification, leakage tests, or reproducible statistical analysis.

## Pull requests

- Link the assigned issue or accepted RFC.
- Explain what users or contributors can observe after the change.
- State what is deliberately out of scope.
- List exact verification commands and relevant environments or platforms.
- Document privacy, security, clinical-safety, accessibility, API, schema, and
  migration impact as applicable.
- Keep generated artifacts, dependencies, credentials, model weights, private data,
  and build output out of Git.
- Resolve review conversations and update documentation when behavior or contracts
  change.

Protected branches require maintainer/CODEOWNER approval. The maintainer reviews for
correctness, scope, maintainability, test evidence, and preservation of project
boundaries.

## Security and privacy reports

Do not disclose a suspected vulnerability, PII leak, exposed credential, unsafe agent
action, or re-identification risk in a public issue or Discussion. Follow the
[organization security policy](SECURITY.md) or the repository-specific policy.

## Communication

- Use repository Issues for scoped work, bugs, and RFC decisions.
- Use [organization Discussions](https://github.com/datalife-ehealth/.github/discussions)
  for introductions and cross-project questions.
- Read and follow the [Code of Conduct](CODE_OF_CONDUCT.md).
- General contact: `auroral.sunflower@gmail.com`.
