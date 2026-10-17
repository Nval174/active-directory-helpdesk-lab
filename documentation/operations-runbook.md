# Operations and Next Session

Public checkpoint: **October 16–17, 2026**. Live connection details, paths, service identifiers, and access settings are intentionally omitted; retain the private configuration separately.

## Completed Checkpoint

- Directory membership, machine trust, and identity resolution verified
- Intended role checks and elevated-command test passed
- Key-based administration retained after hardening
- Internal portal workflow working
- Sample ticket preserved across service restart
- Direct and proxied health checks successful
- Manual SQLite backup created and integrity-checked
- Backup timer enabled and active

A complete instance stop/start recovery exercise and database restoration have not yet been tested.

## Ending a Session

1. Check for unexpected failed Linux services.
2. Close administration sessions cleanly.
3. Stop the dependent client and application system before the directory/DNS dependency.
4. Use the cloud stop operation, not termination, when retaining the build.
5. Remember that storage and other retained resources may remain billable.

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

Current copies protect only the application database and remain local to its instance storage. They do not capture every host setting, the application secret, or the directory server.

There is no configured retention deletion yet. An integrity pass proves structural consistency at that point, not successful recovery of useful records.

## Ordered Next Steps

- [ ] Restore a copy into an isolated file and compare records without overwriting the live database.
- [ ] Validate a full cloud stop/start cycle and record recovery behavior.
- [ ] Define retention and off-instance backup storage.
- [ ] Document the configuration required for rebuilding the service.
- [ ] Add health/backup-failure monitoring and log-review practice.
- [ ] Add application authentication/authorization and encrypted web transport before widening use.
- [ ] Perform negative access tests and review older broad connectivity allowances.
- [ ] Upload sanitized [milestone screenshots](evidence-notes.md#linux-milestone-placeholders).
- [ ] Separately review existing repository images and earlier history for sensitive information.

Only fictional records belong in the prototype. Network reachability is not a substitute for application permission controls.
