# Staging acceptance record template

Copy this file to a date- or run-specific Markdown file in this directory only after the isolated staging acceptance run is complete. Replace all placeholders with observed evidence and remove criteria that were not exercised. Do not write PASS before all required staging checks succeed.

Required exact fields for the approval workflow:

    STAGING_ACCEPTANCE: PENDING
    Release tag: <tag>
    Package SHA-256: <digest>

Include:

- Staging VM identity, operating system, and host-key fingerprint.
- Isolated bridge/VLAN and confirmation that no route or credentials reach production.
- Staging Authserver, Worldserver, and separated Auth/Characters/World database topology.
- Test date and operator.
- Build artifact tag and digest actually installed on staging.
- Auth login, realm advertisement, world connection, gameplay, logout/relogin, and persistence results.
- Database backup and restore evidence, service logs, and relevant proof links.
- Acceptance criteria and any exceptions.

The approval workflow requires the status line to be changed exactly to STAGING_ACCEPTANCE: PASS and verifies that the tag and digest in this record match the selected immutable release.
