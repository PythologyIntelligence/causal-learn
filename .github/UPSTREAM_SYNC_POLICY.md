# Pythology Upstream Sync Policy

This fork does **not** accept upstream changes directly into `main`.

## Trust boundary

The upstream repository `py-why/causal-learn` is treated as an external software supplier. Popularity, previous clean reviews, or a clean prior sync do not make future commits trusted automatically.

## Sync path

1. `.github/workflows/upstream-sync.yml` fetches `py-why/causal-learn:main`.
2. New upstream commits are merged only into `automation/upstream-sync`.
3. The workflow opens or refreshes a pull request into `main`.
4. The pull request must pass all required CI checks and human review.
5. Only then may the change be merged into `main`.

The sync workflow must never enable auto-merge and must never push upstream changes directly to `main`.

## High-risk review areas

Review these especially carefully on every upstream sync:

- `.github/workflows/**` — workflow permissions, third-party actions, secret access and artifact handling.
- dependency manifests and lock files — new dependencies, version changes and supply-chain risk.
- build, packaging and release configuration.
- executable scripts, downloaded binaries, network installers and remote code-loading paths.
- authentication, credential, filesystem, subprocess and network-facing code.
- native extensions, compiled components, serialization/deserialization and model-loading code where present.

## Merge requirements for main

Configure the repository's `main` ruleset / branch protection to require:

- pull requests before merging;
- at least one approving review;
- dismissal of stale approvals when new commits are pushed;
- Code Owner review;
- all review conversations resolved;
- required CI status checks;
- branch up to date before merge where GitHub supports it;
- no force pushes;
- no branch deletion;
- restrictions applying to administrators as well, except emergency recovery where deliberately configured.

Do not configure upstream-sync PRs for automatic merging.

## Failure mode

A merge conflict with upstream is a stop condition. The automation must fail closed. Resolve the conflict manually on a review branch, inspect the resulting diff, run CI, and obtain approval before merge.

## Emergency rule

If a security incident requires bypassing normal review, document the reason and exact commit SHA in the PR or incident record, then restore normal protection immediately after the emergency change.
