[← Week 8](week-08-packages-and-storage.md) | [Overview](../README.md) | [Week 10 →](week-10-deploy-a-blazor-app-with-kestrel-and-systemd.md)

# Week 9 - Provision and harden a VPS

*Phase 3: Run a real server (Weeks 9-12)*

- [ ] **Done**

**Goal:** Get a clean internet-facing server you fully control.

**Exercises:**

- [ ] Rent a small Ubuntu LTS VPS (for example at Hetzner) or use a VM on your own hypervisor.
- [ ] Create a non-root admin user, SSH key login, disable password and root login.
- [ ] Configure ufw (allow 22, 80, 443 only), fail2ban and unattended-upgrades.
- [ ] Check logins with last, lastb and /var/log/auth.log.

**Deliverable:** A hardening checklist you could hand to a colleague, with every command listed.

**My notes:** [notes/week-09-provision-and-harden-a-vps.md](../notes/week-09-provision-and-harden-a-vps.md)

---

[← Week 8](week-08-packages-and-storage.md) | [Overview](../README.md) | [Week 10 →](week-10-deploy-a-blazor-app-with-kestrel-and-systemd.md)
