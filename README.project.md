# TrinityCore 3.3.5 project

A solo-maintained TrinityCore 3.3.5 server project with a controlled CI, immutable Release, local validation, and owner-authorized production deployment path.

## Source baseline

- Upstream: TrinityCore/TrinityCore, branch 3.3.5
- Production source pin: bc47ca8fe5b4a8f20272bbc0847217653cc95dc9
- Client target: WoW 3.3.5a, build 12340

Project workflows and release tooling are maintained in this repository and proposed through pull requests. The solo-owner review policy is in docs/project/solo-maintainer-policy.md.

## Windows, WSL2, and local release validation

Codex Desktop on Windows edits one canonical project worktree in Ubuntu WSL2 ext4. Linux CMake/Ninja builds and CTest run in the development Ubuntu WSL2 distro. Validate the exact immutable Release artifact in a clean Ubuntu 26.04.1 WSL2 local server runtime with the Windows build 12340 client. Record authentication, realm selection, world entry, gameplay, and persistence under docs/project/local-validation/.

There is no separate staging host or database tier in the current plan. Use local-only test databases; never use production data for tests. After local validation, require explicit owner authorization for the exact tag and package digest before deploying that same artifact to production. Do not synchronize source code directly to production.

## Solo-maintainer review

Pull requests are the change history and CI entry point. No second-person approval is required; the owner reviews and may merge after applicable checks pass. Classic branch protection is active on main and requires a pull request and passing checks. See the repository settings and current PR checks for live enforcement state.

## Production topology

- Authserver and MySQL/Auth: 192.168.1.100
- Worldserver and MySQL/Characters/World: 192.168.1.101
- Production rollout remains a separate owner-authorized operation. CI and the authorization workflow do not contact production.

## Data boundary

Do not commit production credentials, private keys, database dumps, live database state, or client-derived DBC/map/vmap/mmap data. Keep local test state outside source control and Release packages.

## Build and release state

Immutable Release r6 is tag trinitycore-3.3.5-project-snapshot-bc47ca8-r6-20261127, package SHA-256 51a0676c906d294bdba675e96527e38029a1bfcc553d2552874c834a5b2027ab. PR #4 merged to main as f6198bc75555706751985f0c1997fcaac687e0d0 after all seven PR checks passed. All three post-merge workflows (Linux, Windows, macOS) passed. The owner authorization gate now requires committed local WSL2/Windows validation evidence; that evidence has not yet been recorded for a specific Release digest. No production deployment occurred.
