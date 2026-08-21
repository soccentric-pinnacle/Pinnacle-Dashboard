# Pinnacle-Dashboard

## Project Summary

Pinnacle-Dashboard is the operator-facing web console for the Pinnacle fleet and test platform.

The `webui` package is a TypeScript application with its own package manifest, lockfile, and TypeScript configuration. Repository documentation describes a dashboard communicating with Pinnacle-HQ and the Server REST/SSE surface; `docs/WEB_FINDINGS.md` records the verified API inventory.

The UI domain covers:

- Device and fleet status.
- Telemetry, faults, and alarms.
- Logs, runs, and results.
- Configuration, updates, and operational controls.

`contract-tests` is a Python pytest package with HTTP contract tests, including settings round trips and configuration listing. `docs/req.md` records intended behavior and `docs/AUDIT.md` provides an independent review boundary.

Docker and Makefile helpers support local execution and validation. The `.claude` architecture skill documents frontend/backend communication patterns, but is not runtime code.

The source and API tests establish a real application surface, but this scan does not show a browser end-to-end suite or prove every screen is connected to a live backend.

Contract tests depend on service availability and credentials. The dashboard is therefore an implemented operator console with integration-dependent workflows, not a fully verified fleet product.

Run the web package checks and contract tests in the documented environment. Validate SSE updates, authentication, destructive actions, and responsive behavior against a running Pinnacle backend.

## Feature Status

| # | Name / Description | Implemented? | Test Coverage? | Manual Test |
|---:|---|---|---|---|
| 1 | TypeScript operator web application | Yes | Package tooling and source exist; browser behavior is not fully covered | [ ] Manual verification required |
| 2 | Fleet and device status views | Partial | API inventory supports the surface; no complete UI E2E suite identified | [ ] Manual verification required |
| 3 | Runs, results, telemetry, faults, and alarms | Partial | Backend contract evidence exists; live rendering needs verification | [ ] Manual verification required |
| 4 | Logs, configuration, and settings workflows | Yes | `contract-tests/tests/test_03_settings.py` covers settings/config API behavior | [ ] Manual verification required |
| 5 | REST and SSE backend communication | Yes | Selected endpoints are covered; live SSE lifecycle is unestablished | [ ] Manual verification required |
| 6 | OTA, provisioning, backup/restore, and factory operations | Partial | Requirements and API findings exist; live destructive testing is needed | [ ] Manual verification required |

## Tests and Commands

Use the repository Makefiles and package manifests. The primary automated boundary is pytest under `contract-tests`; `webui` supplies frontend build and test scripts.

## Limitations

- Tests require a compatible backend, authentication, and representative device data.
- No claim is made that all routes, screens, SSE states, or destructive operations have been manually exercised.
