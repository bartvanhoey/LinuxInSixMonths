[![Docs checks](https://github.com/bartvanhoey/LinuxInSixMonths/actions/workflows/docs-checks.yml/badge.svg)](https://github.com/bartvanhoey/LinuxInSixMonths/actions/workflows/docs-checks.yml)

# Linux in 6 Months

A 26-week plan for the senior .NET developer: from daily WSL2 use to running, automating and monitoring production servers.

## Health Audit

**Last assessed:** 2026-11-18 | **Overall:** 3.8 — Excellent

Build A? · Quality A? · Architecture A? · Tests C?* · Dead Code A? · API A~? · Security A? · Docs A?

**Scope:** Docs-only repository; Fallback checks (markdownlint, link and structure checks).

> Full report: [reports/health-audit-2026-11-18.md](reports/health-audit-2026-11-18.md)

## How to use this plan

- **Time budget:** about 6-8 hours per week, for example four evenings of one hour plus one longer weekend session. Missing a week is fine; shift everything and keep going.
- **Learn by doing real work.** Every week ends with a deliverable you can show. If you can only read about it, you have not finished the week.
- **Keep one repo** (notes, scripts, Dockerfiles, playbooks). It becomes your portfolio and your cheat sheet.
- **Do it by hand first, then automate.** Phases 3-4 are manual on purpose; Ansible in Phase 7 makes the same work repeatable.
- **Setup:** WSL2 with Ubuntu LTS on your Windows machine, plus one small VPS or VM (a few euros per month) that you are allowed to break.

### A realistic expectation

Six months of steady work makes you a strong, confident practitioner: you can own Linux servers, containers and build agents in your own projects and at work. True expert level comes from years of production incidents, so the capstone and the post-mortems matter most; they compress that experience as far as practice can.

## Overview

| Phase | Focus | Weeks | What you get |
| --- | --- | --- | --- |
| [Phase 1](#phase-1-shell-fluency) | **Shell fluency** | Weeks 1-4 | Live in WSL2 daily; pipes, text tools, SSH, vim/tmux. |
| [Phase 2](#phase-2-linux-fundamentals) | **Linux fundamentals** | Weeks 5-8 | Permissions, processes, systemd, packages, storage. |
| [Phase 3](#phase-3-run-a-real-server) | **Run a real server** | Weeks 9-12 | VPS from scratch: hardening, Blazor on Kestrel, nginx, TLS, break and fix. |
| [Phase 4](#phase-4-networking-bash-troubleshooting) | **Networking, Bash, troubleshooting** | Weeks 13-16 | ss/tcpdump/dig, tunnels, solid scripts, a debugging method. |
| [Phase 5](#phase-5-containers) | **Containers** | Weeks 17-20 | Dockerfiles for .NET, compose, hardening, registries. |
| [Phase 6](#phase-6-cicd-on-linux) | **CI/CD on Linux** | Weeks 21-22 | Self-hosted Azure DevOps agent, pipeline to production. |
| [Phase 7](#phase-7-automation-monitoring-capstone) | **Automation, monitoring, capstone** | Weeks 23-26 | Ansible, Prometheus/Grafana, full rebuild from zero. |

## Weekly plan

### Phase 1: Shell fluency

*Weeks 1-4. Live in WSL2 daily; pipes, text tools, SSH, vim/tmux.*

- [Week 1 - WSL2 as your daily driver](weeks/week-01-wsl2-as-your-daily-driver.md): Make Linux the place where your normal dev work happens.
- [Week 2 - Filesystem and files](weeks/week-02-filesystem-and-files.md): Move around confidently and know where things live.
- [Week 3 - Pipes and text tools](weeks/week-03-pipes-and-text-tools.md): Use the shell as a data-processing tool.
- [Week 4 - Editors, tmux and SSH basics](weeks/week-04-editors-tmux-and-ssh-basics.md): Be productive on a remote machine with nothing but a terminal.

### Phase 2: Linux fundamentals

*Weeks 5-8. Permissions, processes, systemd, packages, storage.*

- [Week 5 - Users, groups and permissions](weeks/week-05-users-groups-and-permissions.md): Understand who may do what, and why 'permission denied' happens.
- [Week 6 - Processes and signals](weeks/week-06-processes-and-signals.md): See what is running, how it is limited, and how to stop it cleanly.
- [Week 7 - systemd and journald](weeks/week-07-systemd-and-journald.md): Run and supervise services the standard way.
- [Week 8 - Packages and storage](weeks/week-08-packages-and-storage.md): Install software properly and understand disks.

### Phase 3: Run a real server

*Weeks 9-12. VPS from scratch: hardening, Blazor on Kestrel, nginx, TLS, break and fix.*

- [Week 9 - Provision and harden a VPS](weeks/week-09-provision-and-harden-a-vps.md): Get a clean internet-facing server you fully control.
- [Week 10 - Deploy a Blazor app with Kestrel and systemd](weeks/week-10-deploy-a-blazor-app-with-kestrel-and-systemd.md): Run a real .NET app as a proper Linux service.
- [Week 11 - nginx reverse proxy and TLS](weeks/week-11-nginx-reverse-proxy-and-tls.md): Put the app behind nginx with HTTPS.
- [Week 12 - Break it on purpose](weeks/week-12-break-it-on-purpose.md): Learn to diagnose problems from evidence, not guesswork.

### Phase 4: Networking, Bash, troubleshooting

*Weeks 13-16. ss/tcpdump/dig, tunnels, solid scripts, a debugging method.*

- [Week 13 - Networking fundamentals](weeks/week-13-networking-fundamentals.md): Understand what happens between client and server.
- [Week 14 - Firewalls and SSH in depth](weeks/week-14-firewalls-and-ssh-in-depth.md): Control access and move traffic safely.
- [Week 15 - Bash scripting I](weeks/week-15-bash-scripting-i.md): Write scripts you can trust.
- [Week 16 - Bash II and a troubleshooting method](weeks/week-16-bash-ii-and-a-troubleshooting-method.md): Have a repeatable way to find out what is wrong.

### Phase 5: Containers

*Weeks 17-20. Dockerfiles for .NET, compose, hardening, registries.*

- [Week 17 - Docker fundamentals](weeks/week-17-docker-fundamentals.md): Understand images, containers and what runs underneath.
- [Week 18 - Dockerfiles for .NET](weeks/week-18-dockerfiles-for-net.md): Build small, fast and safe images for your apps.
- [Week 19 - docker compose](weeks/week-19-docker-compose.md): Run a multi-container stack the same way everywhere.
- [Week 20 - Containers deeper](weeks/week-20-containers-deeper.md): Production habits for container workloads.

### Phase 6: CI/CD on Linux

*Weeks 21-22. Self-hosted Azure DevOps agent, pipeline to production.*

- [Week 21 - Azure DevOps agent on Linux](weeks/week-21-azure-devops-agent-on-linux.md): Own a Linux build agent.
- [Week 22 - Pipeline to production](weeks/week-22-pipeline-to-production.md): Commit to production, automated.

### Phase 7: Automation, monitoring, capstone

*Weeks 23-26. Ansible, Prometheus/Grafana, full rebuild from zero.*

- [Week 23 - Ansible I](weeks/week-23-ansible-i.md): Make server setup repeatable.
- [Week 24 - Ansible II](weeks/week-24-ansible-ii.md): Structure automation like a professional.
- [Week 25 - Monitoring and upkeep](weeks/week-25-monitoring-and-upkeep.md): Know about problems before users do.
- [Week 26 - Capstone and review](weeks/week-26-capstone-and-review.md): Prove the whole chain works from nothing.

## Final self-check: what I can do after 26 weeks

- [ ] Work in a Linux shell all day and write one-liners with grep, sed, awk and pipes.
- [ ] Explain users, groups, permissions, sudo and fix 'permission denied' quickly.
- [ ] Write and debug systemd units and read journald logs fluently.
- [ ] Harden a fresh Ubuntu server (SSH keys, ufw, fail2ban, updates) from memory.
- [ ] Deploy a .NET app behind nginx with TLS, and operate it for weeks without drama.
- [ ] Diagnose CPU, memory, disk and network problems with standard tools.
- [ ] Explain TCP/IP, DNS, ports and firewalls and use ss, dig, curl and tcpdump.
- [ ] Write safe Bash scripts (set -euo pipefail, quoting, traps) and check them with shellcheck.
- [ ] Write small, non-root multi-stage Dockerfiles and run stacks with compose.
- [ ] Run a Linux Azure DevOps agent and a pipeline that deploys to a Linux server.
- [ ] Provision and configure servers with Ansible roles and Vault.
- [ ] Monitor a service with Prometheus and Grafana, alert on problems, and restore from backup.

## Resources

- The Linux Command Line by William Shotts (free online) - for Phases 1-2.
- OverTheWire: Bandit - shell skills through puzzles (Phase 1).
- Linux Journey (linuxjourney.com) - short lessons by topic.
- Microsoft docs: Host ASP.NET Core on Linux with Nginx - Phase 3 almost step by step.
- Docker: Get started, then the .NET container docs - Phase 5.
- Ansible documentation and the 'Ansible for DevOps' book by Jeff Geerling - Phase 7.
- ShellCheck (shellcheck.net), explainshell.com, man pages and tldr for fast help.
- Optional certification later: LFCS (Linux Foundation Certified System Administrator).

**Where to go next:** Kubernetes (k3s is a light entry), deeper Ansible and Terraform, the LFCS certification, or applying the skills to production work and on-call duties.
