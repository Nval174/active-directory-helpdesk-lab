# Operations and Next Session

Checkpoint: **October 18, 2026**. Keep live connection details, commands, and protected configuration in the private operational notes.

## Starting State

The last supplied checks showed working directory trust, synchronized time, active identity/proxy/application services, healthy direct/proxied database responses, and browser ticket persistence. Restore drills passed against isolated data.

The recovery host is prepared but not launched. A full rebuild and a later unattended backup with the repaired S3 hook remain pending. Detailed completed results are in the [recovery record](backup-and-recovery.md).

## Ending a Session

1. Save open work and check for failed Linux services.
2. Run the complete backup service. Inspect this run's result, exit status, and journal for fresh local-backup and both S3 verification messages plus a successful status record.
3. Preserve reviewed recovery preparation and private notes.
4. Close sessions and stop the client/application systems before the directory/DNS server.
5. Use normal cloud stop with graceful shutdown and confirm the intended instances reach the stopped state.

Retained storage and other allocated resources can remain billable. A stale backup success is not evidence of the current run.

## Resuming

1. Start the directory/DNS server and verify its services before dependent systems.
2. Read current connection details from the cloud console; public addresses may have changed.
3. Check private connectivity, DNS, time, identity lookup, and machine trust.
4. Test the backend, proxy, and Windows browser workflow.
5. Confirm existing tickets and inspect the backup timer, latest run, and cloud status.

A persistent timer can catch up after downtime but cannot run on a stopped host.

## Troubleshooting

| Symptom | Check |
|---|---|
| Backend health fails | Service logs, configuration loading, database access, and storage |
| Backend works, proxy fails | Proxy configuration and error logs |
| Local proxy works, browser fails | DNS, routing, cloud rules, host firewall, and client path |
| Directory identities fail | Directory availability, DNS, time, SSSD, and trust |
| Local backup works, cloud copy fails | Loaded upload hook, service journal, role permissions, network access, and fresh status |

Use bounded readiness retries after startup; investigate persistent failures. Collect diagnostics without secret-file contents or private keys.

## Next Recovery Drill

1. Recheck the saved preparation and launch the separate recovery host with its own identity.
2. Retrieve the pinned application/configuration and database versions; verify hashes and integrity.
3. Install fresh dependencies, generate a new secret, and rebuild Gunicorn/Nginx.
4. Keep the recovery host's backup timer and uploads disabled so test data cannot overwrite the live recovery copy.
5. Test health, ticket workflows, CSRF handling, browser access, and restart persistence; compare the original service/data before and after.
6. Record full rebuild time, review the result, then remove only the temporary recovery resources.

The configuration archive excludes the live secret, database, and machine credentials. It is a manual snapshot, separate from daily database backups. See [backup and recovery](backup-and-recovery.md) for retention rules and tested boundaries.

After the drill, follow the [roadmap](roadmap.md) for monitoring, access tests, sanitized source publication, certificates, intranet, and automation.
