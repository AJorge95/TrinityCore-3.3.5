# TrinityCore 3.3.5

Private project repository for the TrinityCore 3.3.5 server build and controlled CI/release workflow.

## Source baseline

- Upstream: [TrinityCore/TrinityCore](https://github.com/TrinityCore/TrinityCore), branch `3.3.5`
- Production source pin: `bc47ca8fe5b4a8f20272bbc0847217653cc95dc9`
- Client target: WoW 3.3.5a, build 12340

The upstream branch is being mirrored into this repository. Project-specific workflow changes will be proposed through pull requests after the source branch is present.

## Data boundary

Do not commit production credentials, database dumps, live database state, or client-derived DBC/map/vmap/mmap data. Those remain outside source control and release packages.

## Build and release state

Linux build profiles and the Ubuntu 26.04 Release package have passed local validation. Hosted Actions, protected branch rules, immutable artifact publication, isolated staging, and protected production promotion are still being brought online.
