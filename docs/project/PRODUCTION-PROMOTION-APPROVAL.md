# Owner-controlled production promotion authorization

This procedure records the solo maintainer’s explicit authorization for one immutable package that has passed isolated staging. It does not deploy to Authserver or Worldserver and does not read production credentials.

## Repository and authorization model

The workflow is designed for a single owner and works with either a public or private repository. It does not require a GitHub Environment, a second reviewer, a paid plan, or a particular branch-protection feature. The owner authorizes one exact release by starting `workflow_dispatch` from `main` as `AJorge95` and typing the release tag and package SHA-256. The workflow then verifies the release and staging evidence before writing a receipt. The owner must still review the change and confirm CI is green before dispatching.

GitHub branch protection and deployment environments may add server-side safeguards when available, but they are optional to this solo workflow. Keep repository access limited to the owner if relying on owner-only dispatch as the approval boundary.

## Acceptance evidence required

Before requesting authorization, merge a staging acceptance record into `main` under `docs/project/staging-acceptance/`. Start from `TEMPLATE.md` and replace every placeholder. The record must contain these exact lines:

    STAGING_ACCEPTANCE: PASS
    Release tag: <exact immutable release tag>
    Package SHA-256: <exact package digest>

The record must also include the staging host identities, isolated network, database topology, test date, login/realm/gameplay/persistence outcomes, and links to the full staging proof. It must identify the same release tag and package digest entered in the workflow.

## Request the authorization receipt

From the GitHub Actions tab, run **Production Promotion Approval** on the `main` branch. Enter the immutable release tag, the full lowercase package SHA-256, the committed staging acceptance path, and this exact confirmation:

    AUTHORIZE <release tag> <package SHA-256>

The workflow fails closed unless the caller is `AJorge95`, the workflow ref is `main`, the typed confirmation matches, the release is immutable, the GitHub asset digest and downloaded bytes match the entered digest, the checksum sidecar and provenance match the source tag/commit, and the committed staging record marks the same digest as accepted.

A successful run writes a JSON approval receipt to the Actions run artifact and records the run URL in the summary. The receipt captures the owner, exact confirmation, tag, source commit, package digest, staging proof hash, and run ID. No separate environment review is used.

This is an authorization record only. The later promotion workflow must consume the receipt and revalidate the same tag and digest before deploying. It must remain disabled until p5t8 staging acceptance is complete. Keep application and database secrets out of this repository and workflow.

## Current status

The private-compatible owner-dispatch workflow and runbook are committed locally as `1561146` on the feature branch and await publication and hosted CI. No staging acceptance record exists yet, so the workflow intentionally fails closed until staging has passed and its proof is committed.
