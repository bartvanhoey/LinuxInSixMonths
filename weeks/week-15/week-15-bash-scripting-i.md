[← Week 14](../week-14/week-14-firewalls-and-ssh-in-depth.md) | [Overview](../../README.md) | [Week 16 →](../week-16/week-16-bash-ii-and-a-troubleshooting-method.md)

# Week 15 - Bash scripting I

*Phase 4: Networking, Bash, troubleshooting (Weeks 13-16)*

- [ ] **Done**

**Goal:** Write scripts you can trust.

**Reading:** Ch. 24-29 (first script through `while`/`until` loops) and Ch. 31-36 (`case`, positional parameters, `for`, strings and numbers, arrays, exotica with `trap`).

**Exercises:**

- [ ] Variables, quoting, conditionals, loops, functions, arrays, exit codes, trap, getopts.
- [ ] Always start with set -euo pipefail; run shellcheck on everything you write.
- [ ] Write deploy.sh: publish, rsync, restart service, wait for a health endpoint, roll back on failure.
- [ ] Learn when to stop using Bash and reach for Python or a dotnet tool.

**Deliverable:** A tested deploy.sh that passes shellcheck without warnings.

**My notes:** [week-15-notes.md](week-15-notes.md)

---

[← Week 14](../week-14/week-14-firewalls-and-ssh-in-depth.md) | [Overview](../../README.md) | [Week 16 →](../week-16/week-16-bash-ii-and-a-troubleshooting-method.md)
