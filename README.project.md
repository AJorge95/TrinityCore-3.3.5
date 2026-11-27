# TrinityCore 3.3.5 project

A solo-maintained project repository for the TrinityCore 3.3.5 server build, CI, and controlled releases. Repository visibility is currently private.

## Source baseline

- Upstream: [TrinityCore/TrinityCore](https://github.com/TrinityCore/TrinityCore), branch `3.3.5`
- Production source pin: `bc47ca8fe5b4a8f20272bbc0847217653cc95dc9`
- Client target: WoW 3.3.5a, build 12340

Project-specific workflows and release tooling are maintained in this repository and proposed through pull requests. The project policy and PR checklist are in [docs/project/solo-maintainer-policy.md](docs/project/solo-maintainer-policy.md) and [.github/PULL_REQUEST_TEMPLATE.md](.github/PULL_REQUEST_TEMPLATE.md).

## Solo-maintainer review

Pull requests are retained for change history and CI. No second-person approval is required; the owner reviews and may merge their own PR after all applicable checks pass. Branch protection is documented but is not currently enforced because GitHub rejected the ruleset read for this private repository/plan combination. Do not claim `main` is protected until the saved settings can be read back and verified.

## Data boundary

Do not commit production credentials, database dumps, live database state, or client-derived DBC/map/vmap/mmap data. These remain outside source control and release packages.

## Build and release state

Linux CI, Windows x64 CI, macOS arm64 CI, and the Ubuntu 26.04 Release publisher are active; all r5 tag checks passed. GitHub Release r5 is published with an archive, SHA-256 sidecar, and provenance, but GitHub currently reports it as mutable. Phase 5 still requires an immutable Release, a genuinely isolated staging environment, staging acceptance, and a separately authorized production promotion. No production deployment is triggered by a PR or ordinary build.
