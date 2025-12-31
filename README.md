# k3s-setup

This repo is my learning workspace as I move from SDET to SRE. I want to get comfortable running real services on k3s and documenting what I learn.

Why this repo
- Learn how Kubernetes operators manage stateful systems.
- Practice reliability basics: failover, backups, and recovery.
- Build hands-on SRE habits: observability, runbooks, and incident drills.

Scope
- `postgres-ha/`: HA Postgres setup and experiments.
- `argocd/`: Argo CD install (single environment).

Notes
- This is a learning repo, not a production template.
- I keep changes small and document what I learn along the way.

Roadmap
- Add more stateful services:
  - Redis (cache)
  - RabbitMQ (queue)
  - MinIO (object storage)
  - Kafka (streaming)
- Learn service-to-service networking and policies.
- Improve observability and alerting.
