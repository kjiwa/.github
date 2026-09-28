# .github

Default community health files and shared GitHub Actions workflows for
[kjiwa](https://github.com/kjiwa)'s public repositories.

## What this provides

- Default community profile files (`SECURITY.md`, `CONTRIBUTING.md`, issue and
  pull request templates) that any repo without its own copy inherits
  automatically. A repo's own `.github/ISSUE_TEMPLATE` still overrides this
  one's.
- `.github/workflows/privacy.yml`, a reusable (`workflow_call`) workflow that
  checks out a repo's history-free working tree and fails the run if it finds
  a machine-local home directory path (`/Users/<name>/...`,
  `/home/<name>/...`, or a Windows `C:\Users\<name>\...`), the same class of
  leak pentimento's own privacy tests guard against.

## Using the privacy workflow from another repo

```yaml
jobs:
  privacy:
    uses: kjiwa/.github/.github/workflows/privacy.yml@main
```

This pins to `@main` rather than a SHA. Every other reusable or third-party
action referenced from this account's repos is pinned to a full commit SHA;
this is the one deliberate exception, because the workflow lives in a repo I
own, under the same account and trust boundary as the repo calling it. A bad
commit to `.github`'s `main` could affect every repo that calls it in the
same run it lands, which is the trade made in exchange for never having to
bump a SHA here across every dependent repo. Third-party actions get no such
exception: a compromised upstream tag or branch is not something I control.
