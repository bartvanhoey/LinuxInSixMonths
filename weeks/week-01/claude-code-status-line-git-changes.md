[← Week 1 - WSL2 as your daily driver notes](week-01-notes.md)

# See git changes while working in Claude Code

I wanted to see how many files are changed in the current repo while I work in Claude Code inside herdr. The herdr spaces menu can't show that, but the Claude Code status line can.

## What herdr's `git_status` shows

The herdr spaces menu can show two git items per space:

```toml
# ~/.config/herdr/config.toml
[ui.sidebar.spaces]
rows = [
  ["state_icon", "workspace"],
  ["branch", {token = "git_status", fg = "#f38ba8"}],  # fg = red
]
```

- `branch` is the current branch.
- `git_status` is **not** the number of changed files. It shows how many commits the branch is **ahead of or behind** its remote (it runs `git rev-list --left-right --count`). In sync with GitHub means an empty marker, even with uncommitted changes.

To see it, make a commit and don't push it yet. After `git push` the marker is empty again.

herdr 0.9.5 has no built-in sidebar item for uncommitted changes.

## Claude Code status line

The status line is a bar under the Claude Code input box. Claude Code runs a script you choose and shows the first line it prints. The script gets information about the session as JSON on stdin, including the folder Claude works in (`workspace.current_dir`).

Result:

```text
LinuxInSixMonths  main  ✎ 1 changed  +0 staged  ? 2 new
DevCheatSheets  gh-pages  ✓ clean
```

| Part          | Meaning                                         |
| ------------- | ----------------------------------------------- |
| `✓ clean`     | Nothing changed.                                |
| `✎ N changed` | Files changed but not staged (`git add`).       |
| `+N staged`   | Files staged for the next commit.               |
| `? N new`     | Untracked files (git doesn't know them yet).    |

### 1 - Create the script

Save this as `~/.claude/statusline.sh`:

```bash
#!/usr/bin/env bash
# Claude Code status line: folder, git branch and number of changed files.
# Claude Code sends session info as JSON on stdin; we only need the current folder.

input=$(cat)
dir=$(printf '%s' "$input" | python3 -c 'import json,sys
try:
    d = json.load(sys.stdin)
    print(d.get("workspace", {}).get("current_dir") or d.get("cwd") or "")
except Exception:
    print("")')
[ -n "$dir" ] || dir=$PWD

cyan=$'\e[36m'; yellow=$'\e[33m'; green=$'\e[32m'; red=$'\e[31m'; dim=$'\e[2m'; reset=$'\e[0m'

line="${cyan}$(basename "$dir")${reset}"

if git -C "$dir" rev-parse --is-inside-work-tree >/dev/null 2>&1; then
  branch=$(git -C "$dir" branch --show-current 2>/dev/null)
  [ -n "$branch" ] || branch=$(git -C "$dir" rev-parse --short HEAD 2>/dev/null)
  line+="  ${yellow}${branch}${reset}"

  # --no-optional-locks: don't take git's index lock while Claude or you run git.
  status=$(git -C "$dir" --no-optional-locks status --porcelain 2>/dev/null)
  if [ -z "$status" ]; then
    line+="  ${green}✓ clean${reset}"
  else
    staged=$(printf '%s\n' "$status" | grep -c '^[MADRC]')
    changed=$(printf '%s\n' "$status" | grep -c '^.[MD]')
    untracked=$(printf '%s\n' "$status" | grep -c '^??')
    line+="  ${red}✎ ${changed} changed${reset}"
    line+="  ${green}+${staged} staged${reset}"
    [ "$untracked" -gt 0 ] && line+="  ${dim}? ${untracked} new${reset}"
  fi
fi

printf '%s' "$line"
```

Make it executable:

```bash
chmod +x ~/.claude/statusline.sh
```

How it works:

- `python3` reads the folder from the JSON. (`jq` is the usual tool for this, but it isn't installed by default on Ubuntu.)
- `git status --porcelain` prints one line per file in a fixed format: the first column is the staged state, the second the unstaged state, `??` means untracked. `grep -c` counts the matching lines.
- `$'\e[31m'` and friends are ANSI color codes; `\e[0m` resets the color.

### 2 - Turn it on

Add this to `~/.claude/settings.json` (your personal settings, so it works in every project):

```json
{
  "statusLine": {
    "type": "command",
    "command": "~/.claude/statusline.sh"
  }
}
```

Keep the other settings that are already in the file; `statusLine` is just one more key.

### 3 - Test it

Test the script without Claude Code by sending it fake JSON:

```bash
echo '{"workspace":{"current_dir":"'"$PWD"'"}}' | ~/.claude/statusline.sh; echo
```

Then restart Claude Code (`/exit`, then `clc`). The status line appears under the input box and updates while Claude edits files.
