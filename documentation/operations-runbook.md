# Operations Runbook

Recorded checkpoint: **October 18, 2026**. I keep connection details and recovery preparation in private notes. Completed results are in [backup and recovery](backup-and-recovery.md).

## Resume

1. Start the directory/DNS server and check its services before starting dependent systems.
2. Read current connection details from AWS.
3. Check connectivity, DNS, time, SSSD identities, and machine trust.
4. Test backend health, proxied health, and the Windows browser workflow.
5. Confirm existing tickets and inspect the backup timer and latest local/cloud results.

## Finish

1. Save work and check for failed services.
2. Run the complete backup service. Check this run's exit status, journal, both S3 verification messages, and fresh success record.
3. Save recovery preparation and close sessions.
4. Gracefully stop the client/application systems before the directory server; confirm their stopped state.

Retained storage and other allocated resources can remain billable.

## Troubleshoot

| Symptom | Check |
|---|---|
| Backend health fails | Service logs, runtime configuration, database permissions, storage |
| Backend works; proxy fails | Nginx configuration and error logs |
| Local proxy works; browser fails | DNS, routing, cloud rules, host firewall, client path |
| Directory lookup fails | Directory availability, DNS, time, SSSD, machine trust |
| Local backup works; S3 copy fails | Loaded upload hook, current service journal, role permissions, connectivity |

Retry readiness checks for a bounded period after startup and investigate persistent failures. Exclude secrets from shared diagnostics.

## Next Recovery Drill

1. Recheck the saved preparation and launch a separate host with its own identity.
2. Retrieve the pinned application/configuration and database versions; verify hashes and database integrity.
3. Install fresh dependencies, generate a new secret, and rebuild Gunicorn/Nginx.
4. Keep the recovery host's backup timer/uploads disabled so test data cannot replace live backups.
5. Test health, ticket workflow, CSRF handling, browser access, and restart persistence. Compare the original service/data before and after.
6. Record full rebuild time and remove only the temporary resources after reviewing the result.

Then observe an unattended backup with the repaired S3 hook. Remaining work is in the [roadmap](roadmap.md).
