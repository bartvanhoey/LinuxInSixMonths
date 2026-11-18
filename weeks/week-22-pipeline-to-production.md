[← Week 21](week-21-azure-devops-agent-on-linux.md) | [Overview](../README.md) | [Week 23 →](week-23-ansible-i.md)

# Week 22 - Pipeline to production

*Phase 6: CI/CD on Linux (Weeks 21-22)*

- [ ] **Done**

**Goal:** Commit to production, automated.

**Exercises:**

- [ ] Pipeline stages: restore, build, test, publish, build and push image.
- [ ] Deploy over SSH (service connection) or by pulling the image on the server; health check after deploy.
- [ ] Add a rollback step and keep the last 3 images; use secure files or variables for secrets.
- [ ] Keep pipeline scripts in Bash with set -euo pipefail and proper quoting.

**Deliverable:** One push triggers a build, test and zero-touch deploy to your VPS.

**My notes:** [notes/week-22-pipeline-to-production.md](../notes/week-22-pipeline-to-production.md)

---

[← Week 21](week-21-azure-devops-agent-on-linux.md) | [Overview](../README.md) | [Week 23 →](week-23-ansible-i.md)
