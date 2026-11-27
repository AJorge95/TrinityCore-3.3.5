# Solo-maintainer repository policy

**Repository:** AJorge95/TrinityCore-3.3.5  
**Default branch:** main  
**Maintainer model:** one owner; no external collaborator or reviewer dependency.

## Windows, WSL2, and production flow

- Codex Desktop runs on Windows, outside WSL, as the authoring and control surface.
- Keep one canonical project worktree in Ubuntu WSL2's ext4 filesystem, separate from the pinned upstream-only checkout. Windows WSL filesystem integration exposes that tree to Codex Desktop; do not maintain a second divergent source copy under the Eurybia docs folder.
- Run Linux CMake/Ninja builds, CTest, and local server checks inside WSL2.
- The canonical project worktree is /home/ajorge/projects/trinitycore/project, cloned from AJorge95/TrinityCore-3.3.5 at main merge 078c128b4cc423feb44d8a96bea84e20eb8cd7e9. Codex Desktop runs on Windows outside WSL and edits this WSL2 ext4 checkout through \\wsl.localhost\Ubuntu\home\ajorge\projects\trinitycore\project; run Linux builds and server checks inside Ubuntu WSL. Use short-lived topic branches for changes and merge through pull requests.
- The separate /home/ajorge/projects/trinitycore/source checkout follows upstream TrinityCore 3.3.5 and remains pinned to the upstream baseline. Do not use it as the project fork working tree.
- The Eurybia docs folder stores project design, documentation, tools, proofs, and downloaded release artifacts.
- Push source changes to GitHub through topic branches and pull requests. GitHub Actions builds the release artifact. After isolated staging and explicit owner authorization, deploy the same verified artifact digest to the split Authserver and Worldserver hosts. Production receives the verified artifact and approved configuration/schema operations, never source-editor synchronization.

## Pull request workflow

Use a short-lived topic branch and open a pull request for each project change. The pull request is the audit record and CI entry point, even when its author is also the only maintainer. The owner reviews the diff, checks the evidence and may merge their own pull request. Required approving reviews: **0**.

For applicable source/build changes, wait for all six configured PR checks to pass. The required context names are listed below and must stay synchronized with the main branch rule. A green build is not staging, gameplay, or production acceptance.

## Active main protection

- Require a pull request before merging.
- Require all six strict status checks to pass:
  - clang-17 / Debug / PCH=0
  - clang-17 / Debug / PCH=1
  - clang-17 / Release / PCH=1
  - gcc-13 / Debug / PCH=1
  - macOS arm64 build
  - Windows x64 build
- Require zero approving reviews so the sole owner can self-review and merge.
- Enforce the rule for administrators.
- Block force pushes and branch deletion.
- Keep release creation separate from production promotion.
- Do not configure auto-deployment from pull requests or ordinary build runs.

## Enforcement status

The repository is public for owner-authorized testing. Classic branch protection is active and was read back on main on 27 November 2026. It enforces the pull-request gate, the six strict checks listed above, zero required approvals, administrator enforcement, and force-push/deletion blocks.

The GitHub rule enforces the pull request and passing checks; it does not enforce the owner's diff review because required approvals are zero. The owner review is the project's operating policy and checklist. Check the live PR for current check results; all six required contexts must be green before merge.

## Solo production approval

A single owner can authorize a promotion after staging evidence is complete. No second-person approval is part of this project. Production credentials remain outside GitHub; the production hosts remain separate from CI runners. A self-approval environment gate may be used only if the account/repository plan supports it and self-review prevention is disabled. Until then, promotion is an explicit owner-run operation with a recorded change, artifact digest, backup, and rollback point.

## Data boundary

Never add credentials, private keys, database dumps, live auth/characters/world state, or client-derived DBC/map/vmap/mmap data to source control or Release assets. Keep staging proof, production health, gameplay acceptance, and deployment history as distinct evidence.
