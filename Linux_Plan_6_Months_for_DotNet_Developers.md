# Linux in 6 Months

A 26-week plan for the senior .NET developer: from daily WSL2 use to running, automating and monitoring production servers.

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
|---|---|---|---|
| Phase 1 | **Shell fluency** | Weeks 1-4 | Live in WSL2 daily; pipes, text tools, SSH, vim/tmux. |
| Phase 2 | **Linux fundamentals** | Weeks 5-8 | Permissions, processes, systemd, packages, storage. |
| Phase 3 | **Run a real server** | Weeks 9-12 | VPS from scratch: hardening, Blazor on Kestrel, nginx, TLS, break and fix. |
| Phase 4 | **Networking, Bash, troubleshooting** | Weeks 13-16 | ss/tcpdump/dig, tunnels, solid scripts, a debugging method. |
| Phase 5 | **Containers** | Weeks 17-20 | Dockerfiles for .NET, compose, hardening, registries. |
| Phase 6 | **CI/CD on Linux** | Weeks 21-22 | Self-hosted Azure DevOps agent, pipeline to production. |
| Phase 7 | **Automation, monitoring, capstone** | Weeks 23-26 | Ansible, Prometheus/Grafana, full rebuild from zero. |

## Phase 1: Shell fluency (Weeks 1-4)

### Week 1 - WSL2 as your daily driver

- [ ] **Done**

**Goal:** Make Linux the place where your normal dev work happens.

**Exercises:**

- [ ] Install WSL2 with Ubuntu LTS and Windows Terminal; install git and the .NET SDK inside WSL.
- [ ] Keep repos in the Linux filesystem (~/src), not under /mnt/c. Notice the speed difference.
- [ ] Do one full working day of git, dotnet build and dotnet test only from the WSL shell.
- [ ] Read The Linux Command Line (Shotts) part 1; start OverTheWire Bandit, levels 0-5.

**Deliverable:** A working WSL dev setup and a short note of what felt different from Windows.

### Week 2 - Filesystem and files

- [ ] **Done**

**Goal:** Move around confidently and know where things live.

**Exercises:**

- [ ] Study the layout: /etc, /var, /usr, /home, /tmp, /proc. Explore each with ls and find.
- [ ] Practice find, less, head, tail -f, du, df, ln (hard and symbolic links), tree.
- [ ] Run a .NET app and follow its console output with tail -f while sending requests.
- [ ] Bandit levels 6-12.

**Deliverable:** A personal cheat sheet (Markdown) in a git repo; add to it all 6 months.

### Week 3 - Pipes and text tools

- [ ] **Done**

**Goal:** Use the shell as a data-processing tool.

**Exercises:**

- [ ] Master grep (with -r, -i, -E), sed, awk, sort, uniq, cut, tr, xargs, wc.
- [ ] Analyse an ASP.NET Core log file: top 10 error messages, requests per minute, slowest endpoints.
- [ ] Learn redirection and pipes (>, >>, 2>&1, |, tee) and process substitution.
- [ ] Bandit levels 13-20.

**Deliverable:** A file of 15 useful one-liners, each with a one-line explanation.

### Week 4 - Editors, tmux and SSH basics

- [ ] **Done**

**Goal:** Be productive on a remote machine with nothing but a terminal.

**Exercises:**

- [ ] Learn vim survival skills (modes, save/quit, search, replace, copy/paste) and nano as fallback.
- [ ] Install tmux: sessions, windows, panes, detach and reattach.
- [ ] Set up ~/.ssh/config with aliases; generate an ed25519 key pair and use ssh-agent.
- [ ] Create a dotfiles repo (.bashrc, aliases, .vimrc, .tmux.conf).

**Deliverable:** Dotfiles repo on GitHub. Phase 1 checkpoint: finish Bandit up to about level 20.

## Phase 2: Linux fundamentals (Weeks 5-8)

### Week 5 - Users, groups and permissions

- [ ] **Done**

**Goal:** Understand who may do what, and why 'permission denied' happens.

**Exercises:**

- [ ] Practice chmod (symbolic and octal), chown, chgrp, umask, sticky bit, setgid directories.
- [ ] Create users and groups, a shared project directory, and test access from different users.
- [ ] Understand sudo and /etc/sudoers (visudo); add a limited sudo rule for one command.
- [ ] Look at ACLs with getfacl and setfacl.

**Deliverable:** A shared directory setup that works for two users, documented with the exact commands.

### Week 6 - Processes and signals

- [ ] **Done**

**Goal:** See what is running, how it is limited, and how to stop it cleanly.

**Exercises:**

- [ ] Use ps, top/htop, pgrep, kill, nice, jobs, bg/fg, lsof and explore /proc/<pid>.
- [ ] Start a .NET app, find its PID, inspect its open files, threads and memory in /proc.
- [ ] Send SIGTERM versus SIGKILL and observe graceful shutdown via IHostApplicationLifetime.
- [ ] Learn ulimit and what an out-of-memory kill looks like (dmesg).

**Deliverable:** A .NET worker that logs a clean shutdown on SIGTERM, with notes on what you observed.

### Week 7 - systemd and journald

- [ ] **Done**

**Goal:** Run and supervise services the standard way.

**Exercises:**

- [ ] Enable systemd in WSL (/etc/wsl.conf). Learn systemctl status/start/stop/enable and journalctl -u -f.
- [ ] Write a unit file for a small .NET console or worker app: Restart=, User=, EnvironmentFile=.
- [ ] Replace a cron job with a systemd timer; compare both.
- [ ] Read journal filters: --since, -p err, -b, and disk usage of the journal.

**Deliverable:** A working unit and timer pair in your repo, plus a systemd cheat sheet.

### Week 8 - Packages and storage

- [ ] **Done**

**Goal:** Install software properly and understand disks.

**Exercises:**

- [ ] apt: update, upgrade, search, show, policy, autoremove; dpkg -L and dpkg -S.
- [ ] Add a third-party repository the modern way (signed-by keyring), e.g. Microsoft packages.
- [ ] lsblk, df, du, mount, /etc/fstab; add a virtual disk in a VM and format, mount, persist it.
- [ ] Overview of LVM (pvcreate, vgcreate, lvcreate) and ext4 versus XFS.

**Deliverable:** Phase 2 checkpoint: a self-test where you explain each topic from memory without notes.

## Phase 3: Run a real server (Weeks 9-12)

### Week 9 - Provision and harden a VPS

- [ ] **Done**

**Goal:** Get a clean internet-facing server you fully control.

**Exercises:**

- [ ] Rent a small Ubuntu LTS VPS (for example at Hetzner) or use a VM on your own hypervisor.
- [ ] Create a non-root admin user, SSH key login, disable password and root login.
- [ ] Configure ufw (allow 22, 80, 443 only), fail2ban and unattended-upgrades.
- [ ] Check logins with last, lastb and /var/log/auth.log.

**Deliverable:** A hardening checklist you could hand to a colleague, with every command listed.

### Week 10 - Deploy a Blazor app with Kestrel and systemd

- [ ] **Done**

**Goal:** Run a real .NET app as a proper Linux service.

**Exercises:**

- [ ] Install the ASP.NET Core runtime on the server. dotnet publish locally, then rsync or scp.
- [ ] Run it under a dedicated system user with a systemd unit; keep secrets in an EnvironmentFile.
- [ ] Read logs with journalctl -u yourapp -f; restart behavior after crash and reboot.
- [ ] Follow Microsoft's guide 'Host ASP.NET Core on Linux with Nginx' alongside.

**Deliverable:** Your app running on the VPS, surviving a reboot, reachable on its Kestrel port.

### Week 11 - nginx reverse proxy and TLS

- [ ] **Done**

**Goal:** Put the app behind nginx with HTTPS.

**Exercises:**

- [ ] Install nginx; write a server block that proxies to Kestrel; test with nginx -t.
- [ ] Configure forwarded headers in ASP.NET Core and WebSocket/SignalR support for Blazor Server.
- [ ] Get a certificate with certbot (Let's Encrypt), check auto-renewal, redirect HTTP to HTTPS.
- [ ] Add basic security headers and gzip; look at the access and error logs.

**Deliverable:** Your app live on a domain with valid TLS and an A grade on an online TLS test.

### Week 12 - Break it on purpose

- [ ] **Done**

**Goal:** Learn to diagnose problems from evidence, not guesswork.

**Exercises:**

- [ ] Cause and fix: wrong file permissions, a port already in use, a full disk, a bad nginx config, an OOM kill.
- [ ] Install PostgreSQL (or SQL Server on Linux), connect the app, schedule a pg_dump backup with a timer.
- [ ] Restore the backup on a second machine to prove it works.
- [ ] Write a short post-mortem for each failure: symptom, evidence, cause, fix.

**Deliverable:** Five post-mortems. Phase 3 checkpoint: rebuild the whole server from your notes in under 2 hours.

## Phase 4: Networking, Bash, troubleshooting (Weeks 13-16)

### Week 13 - Networking fundamentals

- [ ] **Done**

**Goal:** Understand what happens between client and server.

**Exercises:**

- [ ] ip addr/route, ss -tulpn, ping, traceroute/mtr, dig/nslookup, curl -v, /etc/hosts, /etc/resolv.conf.
- [ ] Use tcpdump on the VPS to watch one HTTP request and one TLS handshake; open it in Wireshark.
- [ ] Review TCP/IP, DNS record types, DHCP and subnetting (CIDR) with 10 practice problems.
- [ ] Find out why a name resolves differently from two machines.

**Deliverable:** A one-page 'what happens when I open my site' write-up with commands for each step.

### Week 14 - Firewalls and SSH in depth

- [ ] **Done**

**Goal:** Control access and move traffic safely.

**Exercises:**

- [ ] Learn nftables basics under ufw; inspect rules with nft list ruleset.
- [ ] SSH tunnels: -L, -R and -D; ProxyJump; reach a private database through a bastion.
- [ ] rsync flags you will actually use (-avz, --delete, --dry-run); scp versus rsync.
- [ ] Restrict SSH further: AllowUsers, key-only, a non-default port debate (pros and cons).

**Deliverable:** A documented tunnel setup that exposes the database only to your laptop.

### Week 15 - Bash scripting I

- [ ] **Done**

**Goal:** Write scripts you can trust.

**Exercises:**

- [ ] Variables, quoting, conditionals, loops, functions, arrays, exit codes, trap, getopts.
- [ ] Always start with set -euo pipefail; run shellcheck on everything you write.
- [ ] Write deploy.sh: publish, rsync, restart service, wait for a health endpoint, roll back on failure.
- [ ] Learn when to stop using Bash and reach for Python or a dotnet tool.

**Deliverable:** A tested deploy.sh that passes shellcheck without warnings.

### Week 16 - Bash II and a troubleshooting method

- [ ] **Done**

**Goal:** Have a repeatable way to find out what is wrong.

**Exercises:**

- [ ] logrotate, cron versus timers, a backup script with retention and a restore test.
- [ ] strace, lsof, vmstat, iostat, free, pidstat: find CPU, memory, disk and network bottlenecks.
- [ ] Intro to dotnet-counters, dotnet-trace and dotnet-dump on Linux.
- [ ] Build a runbook: 'site down' and 'site slow' with the exact commands in order.

**Deliverable:** Phase 4 checkpoint: a runbook another person could follow at 3 a.m.

## Phase 5: Containers (Weeks 17-20)

### Week 17 - Docker fundamentals

- [ ] **Done**

**Goal:** Understand images, containers and what runs underneath.

**Exercises:**

- [ ] Install Docker Engine (in WSL) and on the VPS. run, ps, exec, logs, stop, rm, images, prune.
- [ ] Volumes versus bind mounts; user-defined networks; port mapping.
- [ ] Run nginx and PostgreSQL containers; inspect them with docker inspect and from the host via /proc.
- [ ] Learn the concepts: namespaces, cgroups, layers and union filesystems.

**Deliverable:** A short explainer in your own words: 'what is a container, really'.

### Week 18 - Dockerfiles for .NET

- [ ] **Done**

**Goal:** Build small, fast and safe images for your apps.

**Exercises:**

- [ ] Write a multi-stage Dockerfile using the official mcr.microsoft.com/dotnet sdk and aspnet images.
- [ ] Optimise layer caching (copy csproj and restore first), add .dockerignore, run as the non-root app user.
- [ ] Add a HEALTHCHECK and an environment-based configuration.
- [ ] Compare image sizes: sdk base, aspnet, alpine, chiseled.

**Deliverable:** Dockerfile for your Blazor app with the image size before and after optimisation.

### Week 19 - docker compose

- [ ] **Done**

**Goal:** Run a multi-container stack the same way everywhere.

**Exercises:**

- [ ] Compose file with app, database and nginx; env files, secrets, named volumes, restart policies.
- [ ] Use healthchecks and depends_on conditions so the app waits for the database.
- [ ] Deploy the compose stack to the VPS and switch DNS from the systemd setup to it.
- [ ] Practice docker compose logs, exec, down -v and rebuilds.

**Deliverable:** The full stack running from one compose file on the VPS.

### Week 20 - Containers deeper

- [ ] **Done**

**Goal:** Production habits for container workloads.

**Exercises:**

- [ ] Push images to a registry (GitHub Container Registry or Azure Container Registry).
- [ ] Scan an image with Trivy and fix a finding; set CPU and memory limits and watch an OOM kill.
- [ ] Try chiseled Ubuntu .NET images; find what the app really needs at runtime.
- [ ] Debug a running container: docker exec, docker cp, logs, nsenter, and why there is no shell in chiseled images.

**Deliverable:** Phase 5 checkpoint: a hardened, scanned, small image pushed to a registry.

## Phase 6: CI/CD on Linux (Weeks 21-22)

### Week 21 - Azure DevOps agent on Linux

- [ ] **Done**

**Goal:** Own a Linux build agent.

**Exercises:**

- [ ] Create a Linux VM (or use a spare machine); install and configure the agent (config.sh, svc.sh install).
- [ ] Create a PAT with minimal scope, an agent pool, and run a pipeline on your own agent.
- [ ] Install the .NET SDK and Docker on the agent; check permissions and disk cleanup.
- [ ] Monitor the agent as a systemd service and update it.

**Deliverable:** A self-hosted Linux agent that builds your app.

### Week 22 - Pipeline to production

- [ ] **Done**

**Goal:** Commit to production, automated.

**Exercises:**

- [ ] Pipeline stages: restore, build, test, publish, build and push image.
- [ ] Deploy over SSH (service connection) or by pulling the image on the server; health check after deploy.
- [ ] Add a rollback step and keep the last 3 images; use secure files or variables for secrets.
- [ ] Keep pipeline scripts in Bash with set -euo pipefail and proper quoting.

**Deliverable:** One push triggers a build, test and zero-touch deploy to your VPS.

## Phase 7: Automation, monitoring, capstone (Weeks 23-26)

### Week 23 - Ansible I

- [ ] **Done**

**Goal:** Make server setup repeatable.

**Exercises:**

- [ ] Install Ansible; inventory, ad-hoc commands, playbooks, modules, variables, handlers.
- [ ] Idempotency: run the same playbook twice and read the changed counts.
- [ ] Jinja2 templates for nginx and the systemd unit.
- [ ] First playbook: users, SSH hardening, ufw and unattended-upgrades.

**Deliverable:** A base-hardening playbook that turns a fresh VM into your standard server.

### Week 24 - Ansible II

- [ ] **Done**

**Goal:** Structure automation like a professional.

**Exercises:**

- [ ] Roles, group_vars, host_vars, tags, check mode and diff mode.
- [ ] Ansible Vault for secrets.
- [ ] Roles for nginx + certbot, Docker, and the app deployment.
- [ ] Rebuild your Phase 3 server from zero with one command.

**Deliverable:** A repo with roles that fully provisions the VPS stack.

### Week 25 - Monitoring and upkeep

- [ ] **Done**

**Goal:** Know about problems before users do.

**Exercises:**

- [ ] Persistent journald, logrotate; install node_exporter and cAdvisor.
- [ ] Run Prometheus and Grafana with compose; import a node dashboard; build one for your app.
- [ ] Set up alerts (Alertmanager or Uptime Kuma) for disk, memory and downtime.
- [ ] Run a Lynis audit, test a restore from backup, review pending updates. Optional weekend: k3s.

**Deliverable:** A dashboard and at least three working alerts; a successful restore test.

### Week 26 - Capstone and review

- [ ] **Done**

**Goal:** Prove the whole chain works from nothing.

**Exercises:**

- [ ] On a brand new VPS: Ansible provisions, the pipeline deploys, monitoring watches. Time the run.
- [ ] Break something at random (a colleague or a dice roll) and fix it with your runbook.
- [ ] Write a README for the repo and a short blog post or internal talk.
- [ ] Self-assess against the checklist at the end; decide the next step (LFCS, Kubernetes, deeper Ansible).

**Deliverable:** A clean-room rebuild in under one hour, documented.

## Final self-check: after 26 weeks I can...

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
