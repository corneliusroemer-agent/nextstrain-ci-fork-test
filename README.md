Test harness for [nextstrain/.github#178](https://github.com/nextstrain/.github/issues/178): `pathogen-repo-ci` fails on PRs from forks because its `workflow-context` job needs an OIDC token, which fork `pull_request` runs don't get.

- `ci-control` calls `nextstrain/.github` `pathogen-repo-ci.yaml@master`.
- `ci-fixed` calls the fix branch `corneliusroemer-agent/.github@job-context-instead-of-oidc`, which reads `job.workflow_repository` / `job.workflow_sha` instead.
- `build-fixed` (manual only) checks that `pathogen-repo-build` checks out its own repo/sha; its AWS step fails outside the nextstrain org.

Both CI callers grant `id-token: write` explicitly, as nextstrain repos effectively do; without it a caller here fails at startup.

What shows what:

- **A PR from a fork** is the real test: `ci-control` should fail in `workflow-context` with `Unable to get ACTIONS_ID_TOKEN_REQUEST_URL`, `ci-fixed` should pass.
- **Dependabot PRs** are not fork-like for this: they get an OIDC token when the caller grants one, so `ci-control` passes there too. A Dependabot run under a token with no `id-token` (#1, first attempt) only shows that `ci-fixed` needs no OIDC permission.
