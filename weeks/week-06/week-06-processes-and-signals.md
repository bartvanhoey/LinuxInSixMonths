[← Week 5](../week-05/week-05-users-groups-and-permissions.md) | [Overview](../../README.md) | [Week 7 →](../week-07/week-07-systemd-and-journald.md)

# Week 6 - Processes and signals

*Phase 2: Linux fundamentals (Weeks 5-8)*

- [ ] **Done**

**Goal:** See what is running, how it is limited, and how to stop it cleanly.

**Reading:** Ch. 10 Processes (`ps`, `top`, `jobs`, `bg`, `fg`, `kill`, signals).

**Exercises:**

- [ ] Use ps, top/htop, pgrep, kill, nice, jobs, bg/fg, lsof and explore `/proc/<pid>`.
- [ ] Start a .NET app, find its PID, inspect its open files, threads and memory in /proc.
- [ ] Send SIGTERM versus SIGKILL and observe graceful shutdown via IHostApplicationLifetime.
- [ ] Learn ulimit and what an out-of-memory kill looks like (dmesg).

**Deliverable:** A .NET worker that logs a clean shutdown on SIGTERM, with notes on what you observed.

**My notes:** [week-06-notes.md](week-06-notes.md)

---

[← Week 5](../week-05/week-05-users-groups-and-permissions.md) | [Overview](../../README.md) | [Week 7 →](../week-07/week-07-systemd-and-journald.md)
