# Owner-controlled production promotion approval

This procedure creates an auditable approval record for one immutable package that has passed isolated staging checks. It does not deploy to Authserver or Worldserver and does not read production credentials.

## GitHub environment setup

A repository owner must configure this once in GitHub:

1. Open AJorge95/TrinityCore-3.3.5, then Settings → Environments.
2. Create the environment named production-approval.
3. Add AJorge95 as the required reviewer.
4. Leave Prevent self-review unchecked. The same solo owner requests and approves the deployment review.
5. Restrict deployments to protected branches. The workflow verifies this policy and requires main to remain protected.
6. Disable administrator bypass for this environment.
7. Do not add production SSH keys, database secrets, or server credentials to this environment.

GitHub provides required reviewers and deployment protection rules for public repositories on its current plans. Required reviewers may be configured for up to six users or teams, with one reviewer sufficient. Prevent self-review is optional; when enabled, the workflow initiator cannot approve the job. See the official [environment protection documentation](https://docs.github.com/en/actions/reference/workflows-and-actions/deployments-and-environments) and [environment setup guide](https://docs.github.com/en/actions/how-tos/deploy/configure-and-manage-deployments/manage-environments).

## Acceptance evidence required

Before requesting approval, merge a staging acceptance record into main under docs/project/staging-acceptance/. Start from TEMPLATE.md and replace every placeholder. The record must contain these exact lines:

    STAGING_ACCEPTANCE: PASS
    Release tag: <exact immutable release tag>
    Package SHA-256: <exact package digest>

The record must also include the stage host identities, isolated network, database topology, test date, login/realm/gameplay/persistence outcomes, and links to the full staging proof. It must identify the same release tag and package digest entered in the workflow.

## Request the approval

From the GitHub Actions tab, run Production Promotion Approval on the main branch. Enter the immutable release tag, the full lowercase package SHA-256, the committed staging acceptance path, and the exact confirmation:

    AUTHORIZE <release tag> <package SHA-256>

The preflight stops unless the caller is AJorge95, the workflow ref is main, the release is immutable, the API asset digest and downloaded bytes match the entered digest, the checksum sidecar and provenance match the source tag/commit, the production-approval environment has AJorge95 as its required reviewer with owner self-review allowed for the solo-owner model, protected-branch deployment policy is enabled, and the committed staging record marks the same digest as accepted.

Review the preflight summary, then approve the production-approval environment job. The completed job writes a JSON approval receipt to the Actions run artifact and records the run URL in the summary. The environment approval is visible in GitHub's deployment history.

This is an approval gate only. The later promotion workflow must consume the approved run and revalidate the same tag and digest before deploying. It must remain disabled until p5t8 staging acceptance is complete. Keep application and database secrets out of this repository and workflow.

## Current status

The environment configuration requires an owner action in GitHub. No staging acceptance record exists yet. The workflow fails closed until both are configured.
