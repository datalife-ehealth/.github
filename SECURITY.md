# Security Policy

## Supported versions

DataLife e-Health is pre-release reference software. Until versioned releases are
published, only the latest commit on each active repository's `main` branch is in
scope. There is no production deployment, security-update service-level agreement,
or clinical-use support commitment.

Repository-specific `SECURITY.md` files may define additional platform, agent, or
data-privacy requirements. The more specific policy applies when both exist.

## Report a vulnerability or privacy failure

Do not open a public issue or Discussion for a suspected vulnerability. Email
`auroral.sunflower@gmail.com` with:

- the affected repository, commit, platform, and configuration;
- a concise description of impact and the boundary that may be crossed;
- reproducible steps or a minimal proof of concept using synthetic data only; and
- suggested containment or mitigations, if available.

You should receive an acknowledgement within five business days. Please allow time
for validation, remediation, and coordinated disclosure. The project does not
currently operate a bug-bounty program.

## Sensitive material

Never include real patient information, access grants, live credentials, signing
keys, private datasets, or data obtained from systems you are not authorized to test.
If a credential or signing artifact was exposed, rotate or revoke it immediately;
deleting a file or rewriting Git history does not invalidate the exposed secret.

If public project content accidentally contains sensitive data, do not download,
redistribute, quote, or attach it to a report. Identify its location privately so the
maintainer can contain it.

## Research and clinical boundary

A model-quality problem, inaccurate synthetic result, or unsupported medical claim
may be a normal issue when it contains no sensitive information and creates no direct
security or privacy exposure. Report it privately when it could enable unauthorized
access, identity disclosure, re-identification, unsafe tool use, or bypass of a human
review boundary.
