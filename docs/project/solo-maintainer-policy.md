# Solo-maintainer repository policy

**Repository:** `AJorge95/TrinityCore-3.3.5`  
**Default branch:** `main`  
**Maintainer model:** one owner; no external collaborator or reviewer dependency.

## Windows, WSL2, and production flow

- Codex runs on the Windows host as the authoring and control surface.
- Keep one dedicated project working tree in Ubuntu WSL2's ext4 filesystem, separate from the pinned upstream-only checkout. Open the WSL-backed project tree to the Windows editing workflow through WSL/VS Code integration; do not maintain a second divergent source copy under `D:\Eurybia`.
- Run Linux CMake/Ninja builds, CTest, and local server checks inside WSL2.
- The current `/home/ajorge/projects/trinitycore/source` checkout follows upstream TrinityCore 3.3.5 and remains the upstream baseline. Do not use it as the project fork working tree. A dedicated local working tree for the project PR has not yet been created.
- `D:\Eurybia` stores project design, documentation, tools, proofs, and downloaded release artifacts.
- Push source changes to GitHub through topic branches and pull requests. GitHub Actions builds the release artifact. After isolated staging and explicit owner authorization, deploy the same verified artifact digest to the split Authserver and Worldserver hosts. Production receives the verified artifact and approved configuration/schema operations, never source-editor synchronization.

## Pull request workflow

Use a short-lived topic branch and open a pull request for each project change. The pull request is the audit record and CI entry point, even when its author is also the only maintainer. The owner reviews the diff, checks the evidence and may merge their own pull request. Required approving reviews: **0**.

For applicable source/build changes, wait for all configured PR build/test lanes (Linux/CMake/CTest, Windows x64, and macOS arm64) to pass. Check the actual run's required status names before configuring branch protection; do not guess status contexts. A green build is not staging, gameplay, or production acceptance.

## Intended `main` protection

- Require a pull request before merging.
- Require all selected CI checks to pass.
- Require zero approving reviews; permit owner self-review and self-merge.
- Block force pushes and branch deletion.
- Keep release creation separate from production promotion.
- Do not configure auto-deployment from pull requests or ordinary build runs.

## Enforcement status

The repository is private. On 27 November 2026, a read-only GitHub ruleset request returned HTTP 403 with the message that rulesets require GitHub Pro or a public repository. The policy above is therefore **documented but not enforced** at present. Recheck the repository plan and visibility before claiming branch protection is active. If the repo remains private on an ineligible plan, preserve the PR/check discipline manually; do not add an impossible second-review gate.

## Solo production approval

A single owner can authorize a promotion after staging evidence is complete. No second-person approval is part of this project. Production credentials remain outside GitHub; the production hosts remain separate from CI runners. A self-approval environment gate may be used only if the account/repository plan supports it and self-review prevention is disabled. Until then, promotion is an explicit owner-run operation with a recorded change, artifact digest, backup, and rollback point.

## Data boundary

Never add credentials, private keys, database dumps, live auth/characters/world state, or client-derived DBC/map/vmap/mmap data to source control or Release assets. Keep staging proof, production health, gameplay acceptance, and deployment history as distinct evidence.
