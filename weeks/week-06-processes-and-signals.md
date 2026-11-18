[← Week 5](week-05-users-groups-and-permissions.md) | [Overview](../README.md) | [Week 7 →](week-07-systemd-and-journald.md)

# Week 6 - Processes and signals

*Phase 2: Linux fundamentals (Weeks 5-8)*

- [ ] **Done**

**Goal:** See what is running, how it is limited, and how to stop it cleanly.

**Exercises:**

- [ ] Use ps, top/htop, pgrep, kill, nice, jobs, bg/fg, lsof and explore `/proc/<pid>`.
- [ ] Start a .NET app, find its PID, inspect its open files, threads and memory in /proc.
- [ ] Send SIGTERM versus SIGKILL and observe graceful shutdown via IHostApplicationLifetime.
- [ ] Learn ulimit and what an out-of-memory kill looks like (dmesg).

**Deliverable:** A .NET worker that logs a clean shutdown on SIGTERM, with notes on what you observed.

---

[← Week 5](week-05-users-groups-and-permissions.md) | [Overview](../README.md) | [Week 7 →](week-07-systemd-and-journald.md)
