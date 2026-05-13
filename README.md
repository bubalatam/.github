# bubalatam/.github

Org-level repo. Hosts shared GitHub Actions reusable workflows and org-wide
defaults (issue templates, profile README, etc.) for the [@bubalatam](https://github.com/bubalatam)
org.

## Why this repo exists

GitHub special-cases a repo named `.github` at the org level: anything in
`.github/workflows/` here is callable as a **reusable workflow** from any other
repo in the org, no per-repo Actions access toggles required. That's the only
reason we have it — to avoid duplicating CI logic across `bubalatam/api`,
`bubalatam/web`, and friends.

## Reusable workflows

| Workflow | Purpose | Called from |
|---|---|---|
| [`deploy-service-staging.yml`](.github/workflows/deploy-service-staging.yml) | Build → push to ECR → SSM deploy to staging EC2 → health check → optional Slack notify | `bubalatam/api`, `bubalatam/web` |

### `deploy-service-staging.yml`

Single reusable pipeline for the staging stack. Caller picks `service`
(`api` or `web`) and the dockerfile path; everything else (ECR repo,
compose service, container name, env-var, cache scope) is derived from
the convention `buba/<service>` → `staging-<service>` → `buba_staging_<service>`
→ `<SERVICE>_IMAGE`.

**Caller requirements:**

```yaml
on:
  push:
    branches: [Staging]
  workflow_dispatch:

permissions:
  id-token: write     # OIDC for AWS assume-role
  contents: read

jobs:
  staging:
    uses: bubalatam/.github/.github/workflows/deploy-service-staging.yml@main
    with:
      service: api                       # or web
      dockerfile: src/api/Dockerfile.staging
      build-args: |
        BUILD_ENVIRONMENT=Staging
      # Optional: AWS Secrets Manager secret with runtime app secrets.
      # If set, fetched on every deploy and rendered as
      # /opt/buba/.env.staging.secrets (root:root 600), then loaded
      # by compose via --env-file. JSON keys must match the .NET
      # config keys verbatim (e.g. `Stripe__SecretKey`).
      secrets-manager-arn: buba/staging/api
    secrets:
      AWS_ROLE_ARN:      ${{ secrets.AWS_ROLE_ARN }}
      SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}   # optional
```

**Required repo-level config (in the caller repo, Settings → Secrets and variables → Actions):**

| Kind   | Name                       | Notes |
|--------|----------------------------|-------|
| Secret | `AWS_ROLE_ARN`             | ARN of `GitHubActionsDeployRole` (per [`bubalatam/infra`](https://github.com/bubalatam/infra/blob/main/aws/iam-policies/github-actions-trust-policy.json)) |
| Secret | `SLACK_WEBHOOK_URL`        | Optional. If unset, notify step is skipped. |
| Var    | `EC2_INSTANCE_ID_STAGING`  | EC2 instance ID of the staging host (`i-…`). |
| Var    | `DEPLOY_BUCKET`            | Optional. If set, deploy metadata gets mirrored to `s3://<bucket>/deploy-status/`. |
| Var    | `AWS_REGION`               | Optional override. Defaults to `us-east-2`. |

**Pinning:** callers reference `@main`. Once stable, tag releases (`v1`, `v1.1`)
and pin callers to a tag.

## Related

- AWS IAM policies (trust + permissions): [`bubalatam/infra`](https://github.com/bubalatam/infra/tree/main/aws/iam-policies)
- Staging compose stack & nginx config: [`bubalatam/infra`](https://github.com/bubalatam/infra/tree/main/docker/staging)
