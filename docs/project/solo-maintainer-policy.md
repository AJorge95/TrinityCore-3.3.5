# Solo-maintainer repository and deployment policy

- Codex Desktop on Windows edits one canonical project worktree in Ubuntu WSL2 ext4; builds and server checks run inside WSL2.
- Keep the upstream-only checkout separate. Use topic branches and pull requests for project-source changes.
- Review the complete diff and wait for all hosted Linux, Windows, and macOS checks before merge. There is one maintainer; a second reviewer is not required.
- Keep production secrets, configuration, and database state out of the repository, CI runners, and local test configs.
- After GitHub publishes an immutable Release, download the exact artifact to the WSL2 test environment and record its digest.
- Test that exact package with local WSL2 Authserver/Worldserver and the Windows WoW 3.3.5a build 12340 client. Use local-only test databases.
- Record auth login, realm selection, Worldserver session, gameplay smoke, and logout/relogin persistence in docs/project/local-validation/. A green CI build is not local client validation or production acceptance.
- Require explicit owner authorization for the exact immutable tag and package digest after the local validation record is committed and all evidence is reviewed.
- Production remains split: Authserver and MySQL/Auth on .100; Worldserver and MySQL/Characters/World on .101. Push the same verified package digest to both hosts only through a separate owner-authorized deployment operation, never from an ordinary CI run.
- Before production changes, complete two-host preflight, verified database backups, schema/dependency checks, and coordinated rollback preparation. No production deployment occurs without explicit owner approval.
- Do not provision a separate staging tier in the current plan. Local WSL2/Windows validation is the preproduction gate; production hosts are never used as a test environment.

Never add credentials, private keys, database dumps, live auth/characters/world state, or client-derived DBC/map/vmap/mmap data to source control or Release assets. Keep local validation, production health, gameplay acceptance, and deployment history as distinct evidence.
