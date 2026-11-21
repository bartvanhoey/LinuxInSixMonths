[← Week 9](../week-09/week-09-provision-and-harden-a-vps.md) | [Overview](../../README.md) | [Week 11 →](../week-11/week-11-nginx-reverse-proxy-and-tls.md)

# Week 10 - Deploy a Blazor app with Kestrel and systemd

*Phase 3: Run a real server (Weeks 9-12)*

- [ ] **Done**

**Goal:** Run a real .NET app as a proper Linux service.

**Exercises:**

- [ ] Install the ASP.NET Core runtime on the server. dotnet publish locally, then rsync or scp.
- [ ] Run it under a dedicated system user with a systemd unit; keep secrets in an EnvironmentFile.
- [ ] Read logs with journalctl -u yourapp -f; restart behavior after crash and reboot.
- [ ] Follow Microsoft's guide 'Host ASP.NET Core on Linux with Nginx' alongside.

**Deliverable:** Your app running on the VPS, surviving a reboot, reachable on its Kestrel port.

**My notes:** [week-10-notes.md](week-10-notes.md)

---

[← Week 9](../week-09/week-09-provision-and-harden-a-vps.md) | [Overview](../../README.md) | [Week 11 →](../week-11/week-11-nginx-reverse-proxy-and-tls.md)
