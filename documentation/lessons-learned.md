# Lessons Learned

| Incident | What I learned |
|---|---|
| Domain discovery failed despite a successful hostname lookup | DNS resolution alone did not establish directory-service connectivity. Checking the layers separately exposed the firewall issue. |
| Share access failed after sign-in | Authentication and resource permissions required separate checks. Restoring the missing NTFS permission resolved the exercise. |
| SSSD was active but responder units failed | I needed to inspect child units and retest identities and trust after changing configuration. |
| A health check returned 502 immediately after restart | Process state did not prove application readiness. Subsequent checks recovered, but the precise cause remained unconfirmed. |
| The backup timer produced only a local copy | Testing the uploader alone missed the absent service hook. I had to test the complete backup job. |
| Restored-source tests passed in the existing environment | That established application/data recovery, while fresh dependency installation and a new-host rebuild remained separate tests. |

I also learned to keep Windows transfer commands in PowerShell and Linux commands in the SSH shell, and to remove temporary elevated access after troubleshooting.

The [Linux record](linux-operations.md), [recovery record](backup-and-recovery.md), and [tickets](../tickets/README.md) contain the investigation details.
