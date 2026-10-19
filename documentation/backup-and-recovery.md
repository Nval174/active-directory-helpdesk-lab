# Backup and Recovery Session Record

Session: **October 17–18, 2026**. I verified database and application-file recovery, then prepared a separate replacement host. This public record describes the results without including live addresses, cloud resource IDs, bucket names, object version IDs, private paths, secrets, or raw diagnostic reports.

## What I Completed

| Milestone | Verified result |
|---|---|
| Local database restore | SQLite integrity passed; source and restored copies each held one ticket; full-row comparisons found no missing or extra rows |
| Local retention | Updated backup job installed and run successfully; newest 14 recognized completed copies retained by policy |
| Private cloud backup | S3 encryption, versioning, upload checksum, exact downloaded bytes, database integrity, matching ticket count, and anonymous-access denial passed |
| Cloud retention | Lifecycle rules inspected for daily snapshots, the latest recovery copy, and disposable validation objects |
| Complete backup job | Missing upload hook repaired; three subsequent manual service runs verified both S3 database copies |
| Application/configuration archive | 14 application files preserved; all 36 archived files restored and verified after private S3 readback |
| Recovered application | Actual recovered source passed 12 recovery checks and four original regression tests using isolated data |
| Live service preservation | Live database logical contents unchanged; later proxy/application state and database health checks passed |
| Replacement-host preparation | Image/network checks, restricted IAM policy simulation, and launch permission dry run passed; two backup versions selected; no instance launched |

The sample tickets used for these checks are fictional.

## Local Backups and Retention

I use SQLite's online backup mechanism to create consistent copies of the active database. The updated job stages a copy, checks integrity and the expected ticket table, then publishes it atomically with restrictive permissions. A lock prevents overlapping runs.

The policy keeps the newest 14 recognized completed copies, not 14 days of backups. Before pruning, it validates the copies selected for retention. Unexpected time ordering and failed validation stop pruning. The first inspected server run found three copies, kept three, and removed none. Later supplied runs remained below the expiration threshold; deletion behavior was checked with isolated fixtures.

The daily timer was already enabled. It cannot execute while the instance is stopped; persistent catch-up behavior is separate from continuous operation.

## Private S3 Storage

I installed the AWS CLI after signature verification and used an instance role for temporary credentials. The application host does not need permanent AWS access keys for this workflow.

Each complete database backup job uploads a dated snapshot and a separate latest recovery object. The uploader checks size, SHA-256, encryption, and version metadata before reporting success. A downloaded recovery copy matched its source bytes, passed SQLite integrity checks, and contained the expected ticket. Anonymous access to the tested object returned an access-denied response, and public access blocking was verified.

I also configured and inspected lifecycle rules:

| Object category | Current version | Noncurrent versions |
|---|---|---|
| Daily database snapshots | Expiration after 30 days | Expiration after seven noncurrent days |
| Latest recovery database | No date-based expiration | Expiration after 30 noncurrent days |
| Disposable validation copies | Expiration after seven days | Expiration after seven noncurrent days |
| Manual application/configuration package | No current-object date-based expiration under these rules | Reviewed separately from the database policy |

S3 expiration is asynchronous. With versioning, current-version expiration and noncurrent-version cleanup are different operations; this is not a strict maximum-age deletion guarantee. Unrelated lifecycle rules were preserved, and encryption/versioning/public access settings were rechecked.

### Missing upload hook

The first observed timer activation completed only the local backup. The uploader had worked manually, but its service drop-in was absent from the loaded backup unit. I restored the hook, verified the effective unit configuration, and inspected three subsequent complete manual service executions. Each confirmed the dated snapshot and latest recovery object, a successful service result, a zero exit status, and a fresh successful status record.

A later timer-triggered run with the repaired hook loaded has not yet been observed. I do not treat manual service execution as proof of that future unattended run.

## Application and Configuration Recovery

I inventoried the deployed sources, selected active deployment configuration, environment variable names, and installed Python dependencies. The reviewed package preserved 14 application files and 36 archived files total, including configuration and recovery metadata.

The live environment secret, database, and machine credentials were excluded. Database recovery uses its separately verified copy. A rebuild will generate a fresh application secret; existing browser sessions and CSRF tokens will not carry over.

The package was uploaded to a unique private S3 object. Encryption, versioning, and checksum checks passed. Downloaded bytes matched, and every restored archived file matched its recorded hash. This is a manual configuration snapshot, separate from the daily database timer. It records dependency versions rather than bundling operating-system packages or Python wheels.

## Testing the Actual Recovered Application

I first tested the application against an isolated restored database. I then repeated the meaningful checks using the actual recovered source tree, templates, and schema rather than importing the original application directory.

The existing Python environment matched the recovered dependency inventory. The drill used a fresh test secret, disposable data, restricted execution, and recovered sources held read-only. The 12 recovery checks ran with CSRF protection enabled; the four original regression tests were also run with their original test settings.

The isolated database started with one ticket and ended with two after the test workflow. The live database's logical contents stayed unchanged, and the recovered files stayed read-only and matched their archive hashes. A subsequent live check showed the proxy and application active and the database reachable.

The test runtime is not a server-rebuild recovery-time measurement. Fresh dependency installation, a new host, a complete proxy/backend deployment, and browser access to that replacement are separate pending checks.

## Replacement-Host Preparation

I prepared a separate recovery role limited to two selected backup objects and their versions. IAM policy simulation allowed the intended reads and denied writes, deletes, and unrelated object reads. This is a policy-simulation result; the replacement host has not yet exercised the role.

The preparation checked the AWS account, exact Ubuntu image identity, SSH key name, subnet, effective route, and ingress rules. It also pinned the selected application package and database object versions and passed an EC2 launch permission dry run. The proposed host uses encrypted gp3 storage, IMDSv2, and Standard CPU credits. No instance was launched.

The initial image check rejected additional non-EBS block-device mappings. I corrected it to find the expected root EBS mapping by device name, accept recognized non-EBS entries, and reject unexpected additional EBS disks. The revised check passed against the reviewed image.

## Evidence Boundaries and Next Steps

These results come from supplied terminal output and browser confirmations. I have not added new recovery screenshots, raw backup archives, databases, private preparation files, or cloud configuration exports to this repository. The existing [screenshot index](../pictures/README.md) remains evidence for the earlier build milestones.

Next, I will:

1. Launch the prepared replacement host with a distinct identity and retrieve the selected backup versions.
2. Verify hashes and database integrity, install fresh dependencies, and generate a fresh application secret.
3. Rebuild the backend and proxy, keeping the recovery host's backup timer and cloud uploads disabled.
4. Verify health, ticket workflows, CSRF handling, browser access, and restart persistence while checking that the original service and data stay unchanged.
5. Record the complete rebuild time, review the result, and remove only the temporary recovery resources after the drill.
6. Observe an unattended backup with the repaired cloud hook, then complete monitoring, access tests, and repeatable deployment work.

The configuration package does not provide directory-server or certificate-authority recovery. Certificates, the company homepage, and domain-based web sign-in remain planned extensions in the [roadmap](roadmap.md). The private cybersecurity study planner is a separate application.

See the [Linux build record](linux-operations.md), [operations runbook](operations-runbook.md), and [evidence notes](evidence-notes.md).
