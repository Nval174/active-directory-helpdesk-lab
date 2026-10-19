# Operations and Next Session

Public checkpoint: **October 17–18, 2026**. Live connection details, paths, service identifiers, and access settings are intentionally omitted; retain the private configuration separately.

## Completed Checkpoint

- Directory membership, machine trust, and identity resolution verified
- Intended role checks and elevated-command test passed
- Key-based administration retained after hardening
- Internal portal workflow working
- Sample ticket preserved across service restart
- Direct and proxied health checks successful
- Manual SQLite backup created and integrity-checked
- Backup timer enabled and active
- Isolated database restore passed integrity and record comparisons: one ticket in each copy, no missing or extra rows
- All three instances brought back online; identity service, proxy, and application active
- Directory trust validated, time synchronized, direct/proxied database health successful, and no failed Linux units reported
- Existing ticket still visible from the Windows browser after the restart cycle
- Updated local backup job retains the newest 14 recognized completed copies and passed manual systemd runs
- Private S3 uploads verified encryption, versioning, checksums, retrieval, restored database integrity, and anonymous-access denial
- S3 lifecycle rules verified for daily snapshots, the latest recovery copy, and disposable validation copies
- Missing cloud upload hook restored in the loaded service configuration; three subsequent complete manual service runs verified both S3 copies
- Reviewed application/configuration package downloaded, restored, and verified: 14 application files and 36 archived files total
- Actual recovered application source passed 12 recovery checks and four original regression tests against isolated data; live database contents unchanged
- Separate recovery-host preparation passed image/network checks, IAM policy simulation, and an EC2 launch permission dry run; no replacement instance launched

The database and recovered-source drills used separate temporary data and did not replace the live database. The later live check showed the proxy and application active and database health successful. The application drill used the existing Python environment after matching it to the recovered dependency inventory; a fresh host, fresh dependency installation, and full web deployment remain untested.

The timer was observed starting the local backup before the missing S3 hook was repaired. Manual complete-job executions after that repair passed. I still need to observe a later unattended run with the repaired configuration loaded. Detailed results are in the [backup and recovery record](backup-and-recovery.md).

## Ending a Session

1. Save open work and check for unexpected failed Linux services.
2. Run the complete backup service and inspect this run's result, exit status, and journal. Require fresh local-backup and both S3 verification messages plus a successful status record; a stale earlier success is insufficient.
3. Save the reviewed recovery preparation and private operational notes separately from this public repository.
4. Close administration sessions cleanly and stop the dependent client and application system before the directory/DNS dependency.
5. Use the ordinary cloud stop operation with graceful OS shutdown enabled and confirm the intended instances reach the stopped state.
6. Remember that storage and other retained resources may remain billable.

There is no need to terminate instances to stop compute for the night.

## Resuming

1. Bring the directory/DNS dependency online first and verify its services.
2. Start the dependent systems.
3. Read current connection details from the authorized cloud console; public addresses may have changed.
4. Confirm private networking, DNS, and time synchronization.
5. Check identity resolution and machine trust.
6. Check the application and proxy separately before testing the browser workflow.
7. Confirm the sample data remains present and inspect backup timer/run state.

A persistent timer can catch up after missed schedules. It cannot back up a stopped host.

## Troubleshooting Approach

| Symptom | Investigation |
|---|---|
| Backend health fails | Check application service state, logs, configuration loading, and database access |
| Backend works, proxy fails | Validate proxy configuration and inspect proxy error logs |
| Local proxy works, browser fails | Check resolution, routing, cloud boundary, host firewall, and client path |
| Directory identities fail | Check dependency availability, DNS, time, identity service, and trust |
| Backup fails | Review job logs, storage space, source/destination availability, and permissions |

Process status alone is not an application-readiness test. Use bounded retries for transient startup delays and investigate failures that persist.

Never share secret-file contents or a private key while collecting diagnostics.

## Backup Boundaries

The local job creates consistent SQLite copies, validates them, and retains the newest 14 completed copies. The cloud uploader runs after the local job and verifies a dated snapshot and a separate latest recovery object. An instance role supplies temporary credentials; no permanent AWS keys were added to the host.

The configured S3 lifecycle policy expires current daily snapshots after 30 days and their noncurrent versions after seven noncurrent days. The current latest recovery object has no date-based expiration; its older versions expire after 30 noncurrent days. Disposable validation objects use seven-day current and noncurrent-version rules. Expiration is asynchronous and versioning means this is not a strict total-age deletion guarantee.

The separate application/configuration archive is a manual snapshot and has no current-object date-based expiration under these rules. It records the reviewed code, deployment configuration, and dependency inventory. It excludes the live environment secret, database, and machine credentials. Generate a fresh application secret during a rebuild; old browser sessions will not carry over.

These copies do not constitute a full machine image, directory-server recovery plan, CA recovery, or a proven fresh-host rebuild. Keep backup uploads and the backup timer disabled on an isolated recovery host so test data cannot replace the live recovery copy.

## Ordered Next Steps

- [x] Restore a copy into an isolated file and compare records without overwriting the live database.
- [x] Recheck all three instances after the cloud stop/start cycle, including browser data persistence.
- [x] Install local retention and verify private off-instance backup storage and database restoration.
- [x] Preserve and restore the reviewed application/configuration package and dependency inventory.
- [x] Test the actual recovered source against an isolated restored database.
- [x] Prepare the separate recovery host and pass its launch permission dry run.
- [ ] Launch that host and verify a full rebuild with fresh dependencies, a fresh secret, and browser access.
- [ ] Observe the next unattended backup with the repaired cloud hook loaded.
- [ ] Add health/backup-failure monitoring and log-review practice.
- [ ] Complete the planned [certificate, intranet, and domain sign-in phases](roadmap.md#planned-intranet-extension) before widening use.
- [ ] Perform negative access tests and review older broad connectivity allowances.
- [x] Review and redact the supplied screenshots and earlier repository history.
- [x] Add the available [milestone screenshots](evidence-notes.md#linux-milestones).
- [ ] Capture successful SSSD repair checks as `sssd-fix.png`.

The [roadmap](roadmap.md) keeps certificates and domain sign-in between recovery work and automation. No certificate authority, intranet gateway, or web single sign-on has been deployed as part of this planning update.

Only fictional records belong in the prototype. Network reachability is not a substitute for application permission controls.
