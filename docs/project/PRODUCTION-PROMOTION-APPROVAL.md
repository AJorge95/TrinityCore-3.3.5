# Owner-controlled production promotion authorization

This procedure records the solo maintainer’s explicit authorization for one immutable package that passed local WSL2 and Windows client validation. It does not deploy to Authserver or Worldserver and does not read production credentials. There is no separate staging host or staging database tier in the current plan.

## Repository and authorization model

The workflow is designed for a single owner and works with either a public or private repository. It does not require a GitHub Environment, a second reviewer, a paid plan, or a particular branch-protection feature. The owner authorizes one exact release by starting workflow_dispatch from main as AJorge95 and typing the release tag and package SHA-256. The workflow then verifies the immutable release and committed local-validation record before writing an authorization receipt. The owner reviews the code and local test evidence and confirms CI is green before dispatching.

Keep production credentials and live database state out of the repository and CI. Use local test databases in WSL2; do not point the Windows client or WSL runtime at production databases.

## Local validation evidence required

Before requesting authorization, merge a completed test record into main under docs/project/local-validation/. Start from TEMPLATE.md, replace all placeholders with observed evidence, and use the same release tag and package digest entered in the workflow.

The record must contain these exact lines:

    LOCAL_VALIDATION: PASS
    Release tag: <exact immutable release tag>
    Package SHA-256: <exact package digest>
    Validation environment: Ubuntu 26.04.1 WSL2 + Windows
    Windows client build: 12340
    WSL server artifact: PASS
    Authserver login: PASS
    Realm selection: PASS
    Worldserver session: PASS
    Gameplay smoke: PASS
    Logout/relogin persistence: PASS

Test the exact Release artifact in the local Ubuntu 26.04.1 WSL2 server runtime and connect from the Windows 3.3.5a build 12340 client. Record evidence for server startup, authentication, realm selection, entering the world, representative gameplay, and persistence after logout/relogin. Keep logs sanitized and omit secrets or client-derived data.

## Request the authorization receipt

From GitHub Actions, run Production Promotion Approval on main. Enter the immutable release tag, full lowercase package SHA-256, committed local-validation proof path, and exact confirmation:

    AUTHORIZE <release tag> <package SHA-256>

The workflow fails closed unless the caller is AJorge95, the ref is main, the typed confirmation matches, the release is immutable, the GitHub asset digest and downloaded bytes match the entered digest, the checksum sidecar and provenance match the source tag/commit, and the committed local-validation record marks the same digest as passed.

A successful run writes a JSON authorization receipt to the Actions run artifact and records the run URL in the summary. The receipt captures the owner, exact confirmation, tag, source commit, package digest, local-validation proof hash, and run ID.

This is an authorization record only. A later production deployment operation must consume the receipt and revalidate the same tag and digest before making changes. It must never deploy automatically from CI or from the owner-authorization workflow. Keep application and database secrets out of this repository and workflow.

## Current status

The workflow and runbook now require local WSL2/Windows proof. The local servers are available, but there is not yet a committed acceptance record tied to an immutable release digest, so the workflow must fail closed until that evidence is created.
