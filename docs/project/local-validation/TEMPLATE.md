# Local WSL2 and Windows release validation record

Copy this template to a date- or release-specific Markdown file in this directory after testing the exact immutable release artifact locally. Do not write PASS until each required check has been observed. Do not include account passwords, database credentials, private keys, live database dumps, or client-derived data.

Required exact fields for the production authorization workflow:

    LOCAL_VALIDATION: PENDING
    Release tag: <exact immutable release tag>
    Package SHA-256: <exact package digest>
    Validation environment: Ubuntu 26.04.1 WSL2 + Windows
    Windows client build: 12340
    WSL server artifact: PENDING
    Authserver login: PENDING
    Realm selection: PENDING
    Worldserver session: PENDING
    Gameplay smoke: PENDING
    Logout/relogin persistence: PENDING

Include the test date/operator, WSL distribution/version, release install path, command/log references, and concise observed outcomes for each check. Prove the same package digest was installed in WSL and selected for production. Keep logs sanitized and omit secrets or client-derived data.

Change each PENDING result to PASS only after observation. The top-level status must be exactly LOCAL_VALIDATION: PASS before production authorization can be requested. Local acceptance is not production acceptance and does not deploy.
