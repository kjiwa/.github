# .github

Default community health files and shared GitHub Actions workflows for
[kjiwa](https://github.com/kjiwa)'s public repositories.

## What this provides

- Default community profile files (`SECURITY.md`, `CONTRIBUTING.md`, issue and
  pull request templates) that any repo without its own copy inherits
  automatically. A repo's own `.github/ISSUE_TEMPLATE` still overrides this
  one's.
- `.github/workflows/privacy.yml`, a reusable (`workflow_call`) workflow that
  fails the run if the working tree holds a machine-local home directory path
  (`/Users/<name>/...`, `/home/<name>/...`, or a Windows `C:\Users\<name>\...`),
  a home-relative path (`~/` or `$HOME/`) that contains the repo owner's handle,
  or a `com.<owner>.` identifier, the same class of leak pentimento's own
  privacy tests guard against. On pull requests opened by the repo owner it
  also fails if any commit's author or committer email is not a GitHub noreply
  address.
- `.github/workflows/dependabot-automerge.yml`, a reusable workflow that
  enables squash auto-merge on a Dependabot pull request when its update is a
  patch or minor bump. The caller's required checks gate the merge; a major
  bump is left for manual review.

## Using a shared workflow from another repo

```yaml
jobs:
  privacy:
    uses: kjiwa/.github/.github/workflows/privacy.yml@main
```

The auto-merge caller is a `pull_request` workflow whose job needs
`contents: write` and `pull-requests: write`, and the repo needs
`allow_auto_merge` on (managed in terraform-github).

This pins to `@main` rather than a SHA. Every other reusable or third-party
action referenced from this account's repos is pinned to a full commit SHA;
this is the one deliberate exception, because the workflow lives in a repo I
own, under the same account and trust boundary as the repo calling it. A bad
commit to `.github`'s `main` could affect every repo that calls it in the
same run it lands, which is the trade made in exchange for never having to
bump a SHA here across every dependent repo. Third-party actions get no such
exception: a compromised upstream tag or branch is not something I control.
