# TrinityCore 3.3.5 project

A solo-maintained project repository for the TrinityCore 3.3.5 server build, CI, and controlled releases. Repository visibility is public for owner-authorized testing.

## Source baseline

- Upstream: [TrinityCore/TrinityCore](https://github.com/TrinityCore/TrinityCore), branch `3.3.5`
- Production source pin: `bc47ca8fe5b4a8f20272bbc0847217653cc95dc9`
- Client target: WoW 3.3.5a, build 12340

Project-specific workflows and release tooling are maintained in this repository and proposed through pull requests. The project policy and PR checklist are in [docs/project/solo-maintainer-policy.md](docs/project/solo-maintainer-policy.md) and [.github/PULL_REQUEST_TEMPLATE.md](.github/PULL_REQUEST_TEMPLATE.md).

## Windows and WSL2 development

Codex Desktop runs on Windows, outside WSL, as the authoring/control surface. The intended project checkout is one dedicated worktree in Ubuntu WSL2's ext4 filesystem, exposed to Windows through WSL filesystem integration. CMake/Ninja builds, CTest, and local server checks run in WSL2. Keep the upstream-only checkout separate. `D:\Eurybia` is the project design/docs/tools/proofs/artifacts folder, not a second source copy.

Source changes go through GitHub PRs. GitHub Actions creates the release artifact; after isolated staging and explicit owner authorization, deploy the same verified digest to the Authserver and Worldserver production hosts. Do not synchronize source code directly to production.

## Solo-maintainer review

Pull requests are retained for change history and CI. No second-person approval is required; the owner reviews and may merge their own PR after all applicable checks pass. Classic branch protection is active on `main`: it requires a PR, six strict Linux/Windows/macOS check contexts, zero approvals, administrator enforcement, and blocks force pushes and deletion. Rulesets are not used or required. The rule blocks merge until all six required status checks pass and the owner reviews the diff. Check PR #1 for live run state before merge.

## Data boundary

Do not commit production credentials, database dumps, live database state, or client-derived DBC/map/vmap/mmap data. These remain outside source control and release packages.

## Build and release state

Linux CI, Windows x64 CI, macOS arm64 CI, and the Ubuntu 26.04 Release publisher are active; all r5 tag checks passed. GitHub Release r5 is published with an archive, SHA-256 sidecar, and provenance, but GitHub currently reports it as mutable. Phase 5 still requires an immutable Release, a genuinely isolated staging environment, staging acceptance, and a separately authorized production promotion. No production deployment is triggered by a PR or ordinary build.
