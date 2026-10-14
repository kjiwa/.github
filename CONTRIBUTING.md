# Contributing

This file is the default contributing guide for kjiwa's public repositories
that do not define their own. See each repo's own `README.md` or
`CONTRIBUTING.md` if it has one; it names the checks that repo actually runs
and takes precedence over this one.

## General expectations

- For anything beyond a trivial fix, open an issue first to discuss the
  change before writing code.
- CI must pass on the pull request. Each repo's checks are its source of
  truth for what "passing" means; there is no account-wide test suite.
- Keep changes scoped to the repo and files the issue or PR is about.

## This repo specifically

Changes here are default community files and the shared workflows other
repos call. A change to `.github/workflows/privacy.yml` or `dependabot-automerge.yml` affects every repo
that references it at `@main`; treat it with the same care as a
security-relevant change.
