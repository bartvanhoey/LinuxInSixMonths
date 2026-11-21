[← Week 15 - Bash scripting I](week-15-bash-scripting-i.md)

# Week 15 - Bash scripting I: notes

**Status:** Not started

## Exercise 1

> Variables, quoting, conditionals, loops, functions, arrays, exit codes, trap, getopts.

_Commands, results, what surprised me._

## Exercise 2

> Always start with set -euo pipefail; run shellcheck on everything you write.

_Commands, results, what surprised me._

## Exercise 3

> Write deploy.sh: publish, rsync, restart service, wait for a health endpoint, roll back on failure.

_Commands, results, what surprised me._

## Exercise 4

> Learn when to stop using Bash and reach for Python or a dotnet tool.

_Commands, results, what surprised me._

## Deliverable

> A tested deploy.sh that passes shellcheck without warnings.

_Write-up._

## Extra: pre-push markdownlint hook

This repo lints its Markdown in CI. A `pre-push` git hook runs the same check locally, so lint errors are caught before the push instead of after a failed CI run.

The hook lives in `.githooks/pre-push`. Git ignores that folder until you point it there, once per clone:

```bash
git config core.hooksPath .githooks
```

A hook is just an executable script that git runs at a fixed moment. This one is a short shell script that stops the push when the lint command fails (`|| exit 1`), the same exit-code idea as in Exercise 1. Run it by hand with `.githooks/pre-push`. See the README section "Lint Markdown before pushing".
