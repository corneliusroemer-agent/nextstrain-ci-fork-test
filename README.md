Test harness for nextstrain/.github#167: does pathogen-repo-ci work on fork-like PRs when it uses
`job.workflow_*` instead of an OIDC token? Dependabot PRs run "as if from a fork" (read-only token,
no secrets), so a Dependabot PR here exercises both callers.
