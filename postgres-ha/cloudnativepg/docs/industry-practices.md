# CloudNativePG in the Real World

This is a practical list of common patterns, pitfalls, and incidents you will see when running Postgres with CNPG.

Common industry practices
- Separate read and write traffic using `-rw` and `-ro` Services.
- Put backups on object storage (S3/MinIO) and test restores regularly.
- Set resource requests/limits and monitor for memory pressure.
- Spread replicas across nodes with affinity/topology rules.
- Track replication lag and alert when it grows.
- Use supervised primary updates for controlled maintenance windows.
- Keep Postgres parameters versioned alongside your manifests.

Typical use cases
- App database for internal tools with HA failover.
- Read scaling for reporting workloads using `-ro`.
- Staging environments that mirror prod at smaller scale.
- DR practice: restore to a new cluster, then cut over.

Issues you will actually hit
- PVC stuck Pending due to wrong or missing `storageClass`.
- Pods Pending because resources are too low or nodes are full.
- Connection storms after deploys causing "too many connections".
- Read replicas lagging under heavy write load.
- Failover confusion when apps hard-code the primary pod.
- Backups not running because credentials or bucket policy is wrong.
- Slow queries causing a perceived outage (not a CNPG issue).

Operational habits that prevent pain
- Always connect apps via `-rw` or `-ro` Services, not pod IPs.
- Keep a small runbook for: primary down, restore, and storage failure.
- Test failover and restore quarterly, even in a homelab.
- Keep one place where DB connection strings live and are audited.
- Keep alerts simple: primary down, replica lag, backup stale.

What SREs watch
- Replication lag and WAL retention.
- Backup freshness and restore success.
- Disk usage and growth trends.
- Connection counts and query latency.
- Restart loops or repeated failovers.

Good first alerts
- Primary is not Ready for >5 minutes.
- Replica lag exceeds a threshold (e.g., 30s).
- Backups missing for >24 hours.
- PVC usage above 80%.
