[← Week 6](../week-06/week-06-processes-and-signals.md) | [Overview](../../README.md) | [Week 8 →](../week-08/week-08-packages-and-storage.md)

# Week 7 - systemd and journald

*Phase 2: Linux fundamentals (Weeks 5-8)*

- [ ] **Done**

**Goal:** Run and supervise services the standard way.

**Exercises:**

- [ ] Enable systemd in WSL (/etc/wsl.conf). Learn systemctl status/start/stop/enable and journalctl -u -f.
- [ ] Write a unit file for a small .NET console or worker app: Restart=, User=, EnvironmentFile=.
- [ ] Replace a cron job with a systemd timer; compare both.
- [ ] Read journal filters: --since, -p err, -b, and disk usage of the journal.

**Deliverable:** A working unit and timer pair in your repo, plus a systemd cheat sheet.

**My notes:** [week-07-notes.md](week-07-notes.md)

---

[← Week 6](../week-06/week-06-processes-and-signals.md) | [Overview](../../README.md) | [Week 8 →](../week-08/week-08-packages-and-storage.md)
