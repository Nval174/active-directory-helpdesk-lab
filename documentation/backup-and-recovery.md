# Backup and Recovery

Session: **October 17–18, 2026**. I restored database and application files and prepared a replacement host. This record summarizes the results I reported during the session; raw reports and backup packages remain private.

## Restore results

| Check | Recorded result |
|---|---|
| Database comparison | Integrity `ok`; one ticket in each copy; zero missing or extra rows |
| S3 retrieval | Downloaded bytes matched the source checksum; database integrity and ticket count passed |
| Application/configuration archive | 14 application files; 36 archived files total restored and matched their hashes |
| Recovered application | 12 recovery checks with CSRF enabled, plus four original regression tests |
| Live service | Database contents unchanged; later application/proxy health checks passed |
| Replacement preparation | Selected image/network checks, restricted-role simulation, pinned backup versions, and launch dry run passed; no host launched |

The recovered application used disposable data and a fresh test secret. Its test database grew from one ticket to two. Recovered files stayed read-only and matched their archive hashes.

I reused the existing Python environment after comparing it with the dependency inventory. These results establish recovery in that environment; fresh dependency installation, a complete replacement deployment, and rebuild time remain untested.

## Local backup policy

The job uses SQLite's backup mechanism, validates a staged copy, and publishes it atomically. A lock prevents overlapping runs. It retains the newest **14 completed copies**, rather than a fixed number of days.

Observed server runs were below the retention limit; deletion behavior was checked with isolated fixtures. The daily timer is enabled and can catch up after downtime, but cannot run while the instance is stopped.

## S3 policy and verification

The job uses temporary instance-role credentials to upload a dated snapshot and a latest-recovery copy. I checked size, SHA-256, encryption, version metadata, and downloaded bytes. Public-access blocking was enabled; an anonymous request to the tested object was denied.

| Object | Current-version expiration | Noncurrent-version expiration |
|---|---|---|
| Daily database snapshot | 30 days | Seven noncurrent days |
| Latest recovery database | None | 30 noncurrent days |
| Disposable validation copy | Seven days | Seven noncurrent days |
| Manual application/configuration package | No current-version expiration under these rules | Separate from the database policy |

These are configured lifecycle rules, not observed deletion results. Expiration is asynchronous.

### Upload-hook repair

The first observed timer run completed a local backup but skipped S3 upload. The uploader worked separately; its hook was missing from the loaded backup service.

I restored the hook and inspected three complete manual service runs. Each verified both S3 copies, a successful service exit, and a fresh success record. A later unattended activation with the repaired hook has not been observed.

## Application/configuration package

I archived the deployed source, selected service/proxy configuration, dependency inventory, and recovery metadata. The package excludes the live secret, database, and machine credentials. The database has its own backup.

Private S3 readback and per-file hash comparisons passed. This is a manual snapshot, separate from the daily database job; it records dependencies without bundling fresh installation packages.

## Replacement-host preparation

I pinned two backup versions and prepared a separate recovery role. IAM simulation allowed the selected reads and denied writes, deletes, and unrelated reads. The role has not been exercised from a replacement host.

The image validator initially rejected additional non-EBS mappings. I adjusted it to identify the root EBS mapping and retested the preparation. The launch permission dry run passed.

The [runbook](operations-runbook.md#next-recovery-drill) contains the next drill. [Evidence notes](evidence-notes.md) describe the published material.
