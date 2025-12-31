# CNPG Study Cases (Hands-On)

Each case mirrors a real issue. Follow the steps to diagnose and fix it.

Study case 1: PVC stuck in Pending
Goal: Understand storage class and binding issues.
Symptoms
- Pods stay Pending.
- PVC shows `Pending` with no bound volume.
Steps
1) Check PVCs: `kubectl -n postgres get pvc`
2) Describe the PVC and look for events about storage class.
3) List storage classes: `kubectl get sc`
4) Update `storageClass` in the cluster spec and re-apply.
Expected outcome
- PVC binds and pods start.

Study case 2: Primary pod deleted, failover does not happen
Goal: Learn how CNPG promotes replicas.
Symptoms
- Primary pod deleted; cluster stuck without a new primary.
Steps
1) Check cluster status: `kubectl -n postgres get cluster app-db -o yaml`
2) Check CNPG controller logs in `cnpg-system`.
3) Verify replicas are healthy and have WAL replay.
4) Check if `primaryUpdateStrategy` is `supervised`.
Expected outcome
- Replica promoted; `-rw` Service points to new primary.

Study case 3: Read replicas are slow
Goal: Learn to detect replication lag.
Symptoms
- Reads from `-ro` are stale or slow.
Steps
1) Check metrics or logs for replication lag.
2) Inspect primary load and write rate.
3) Increase resources or reduce write load.
4) Consider synchronous replication if needed.
Expected outcome
- Replicas catch up and read lag drops.

Study case 4: "Too many connections"
Goal: Understand connection limits and pooling.
Symptoms
- App errors with too many connections.
Steps
1) Check current connections inside the primary.
2) Reduce app connection pool size.
3) Consider adding PgBouncer in front of `-rw` and `-ro`.
Expected outcome
- Connection count stabilizes and errors stop.

Study case 5: Backups not running
Goal: Learn backup configuration and validation.
Symptoms
- Backup jobs failing or no backups in the bucket.
Steps
1) Check CNPG backup status and events.
2) Verify secret credentials and bucket permissions.
3) Validate the endpoint (S3/MinIO) reachability.
4) Re-run a backup and check the bucket.
Expected outcome
- Backups succeed and objects appear in storage.
