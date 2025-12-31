# CloudNativePG Learning Plan (SDET to SRE)

Audience
- For someone new to SRE who wants a practical, CNPG-first path.

Goals
- Understand how CNPG runs and manages Postgres on Kubernetes.
- Run a small HA cluster on k3s and see failover in action.
- Learn WAL, backups, and recovery at a hands-on level.
- Get comfortable operating, upgrading, and troubleshooting CNPG.

Checklist
- [ ] I can explain the CNPG operator's role and the basic cluster resources.
- [ ] I can deploy the dev overlay and access the database.
- [ ] I know the difference between `-rw` and `-ro` Services and when to use each.
- [ ] I have tested a primary failover and seen a replica promoted.
- [ ] I can describe WAL in my own words and where it shows up in CNPG.
- [ ] I can configure backups (S3/MinIO) and restore into a new cluster.
- [ ] I can find replication lag and basic health signals in metrics/logs.
- [ ] I can explain the difference between `supervised` and `unsupervised`.
- [ ] I have a small runbook for "primary down" and "restore from backup".

Use cases
- Homelab HA Postgres for small apps.
- Learning operator patterns, stateful workloads, and DR basics.
- Building SRE instincts: reliability, observability, incident response.

Prerequisites (minimal)
- Kubernetes basics: Pods, Services, StatefulSets, PVCs.
- Postgres basics: primary/replica, WAL, connections.
- CLI basics: kubectl, kustomize.

Learning path (progressive)
1) Baseline setup
   - Apply `cloudnativepg/overlays/dev` and verify pods and Services.
   - Learn the Services: `<cluster>-rw` and `<cluster>-ro`.
   - Validate a simple connection from a test pod.

2) HA behavior
   - Scale to 3 instances in the prod overlay.
   - Simulate failure: delete the primary pod and watch promotion.
   - Watch cluster status and events during the failover.

3) Storage and scheduling
   - Set a `storageClass` and see how PVCs bind.
   - Add node affinity to spread replicas across workers.
   - Compare local-path storage vs networked storage.

4) Postgres config
   - Add `postgresql.parameters` for safe basics (connections, logging).
   - Understand where config lives and when reloads happen.

5) Backups and WAL archiving
   - Add S3/MinIO backup configuration.
   - Restore into a new cluster.
   - Walk through a PITR (point-in-time recovery) flow.

6) Observability
   - Enable metrics and a PodMonitor (Prometheus).
   - Track replication lag and primary/replica health.
   - Define a minimal alert set.

7) Upgrades and maintenance
   - Learn CNPG update strategy (supervised vs unsupervised).
   - Practice a minor version upgrade.
   - Write a simple rollback plan.

8) Incident response drills
   - Simulate disk full on a replica.
   - Simulate a network partition.
   - Restore from backup and validate data.

Milestones
- M1: Dev cluster up, able to connect and query.
- M2: Failover tested and understood.
- M3: Backups + restore to a new cluster verified.
- M4: Basic metrics and alerts wired.

Suggested exercises
- Write a small script that sends writes to `-rw` and reads to `-ro`.
- Create a runbook for "primary pod down".
- Measure recovery time (RTO) during failover.

References to explore
- CNPG docs: operators, clusters, and backup configuration.
- PostgreSQL docs: WAL and streaming replication.
- Kubernetes docs: StatefulSets, PVCs, storage classes.
