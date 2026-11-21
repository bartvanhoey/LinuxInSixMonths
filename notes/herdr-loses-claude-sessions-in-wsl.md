[← Week 1 - WSL2 as your daily driver notes](week-01-wsl2-as-your-daily-driver.md)

# herdr "loses" Claude Code sessions in WSL

When you open the terminal again, herdr shows your workspaces and tabs, but the `claude` tab is an empty shell. If you type `claude`, a new conversation starts and the old one seems gone. This note explains why and how to get the old conversation back.

## Problem

- Yesterday: herdr runs with Claude Code in a tab. You close the terminal or shut down Windows.
- Today: you start herdr. The workspaces, tab names and folders are back, but no `claude` process is running in the tab.
- Typing `claude` starts a fresh conversation.

## Cause

herdr has a background **server** (`herdr server`) that keeps panes alive when you close the terminal window. That only works while the Linux machine keeps running.

On WSL the whole Linux VM stops when:

- you shut down or reboot Windows,
- you run `wsl --shutdown`,
- WSL is idle for a while with no open terminal (the default `vmIdleTimeout`).

When the VM stops, every process dies with it, including the herdr server and every `claude` inside it. tmux and zellij behave the same way.

You can see it in the logs:

```bash
grep -E 'Hangup|app.shutdown|persist.restore' ~/.config/herdr/herdr-server.log | tail
uptime
```

```text
... pane child exited ... signal: Some("Hangup")     # all panes killed
... herdr exiting event="app.shutdown" subsystem="server"
... session restore evaluated event="persist.restore" ... workspaces=4
```

A short `uptime` (a few minutes) confirms that the VM booted fresh.

herdr restores only the **layout** from `~/.config/herdr/session.json`: workspaces, tabs and the working folder of each pane. It does not restart the programs that ran in them.

## Fix: resume the conversation

Claude Code saves every conversation to disk, per project folder:

```bash
ls ~/.claude/projects/
ls ~/.claude/projects/-home-<user>-src-<repo>/*.jsonl
```

So nothing is lost. In the restored `claude` tab (which is already in the right folder) run:

| Command           | Result                                                    |
| ----------------- | --------------------------------------------------------- |
| `claude --continue` (`-c`) | Reopen the most recent conversation in this folder.|
| `claude --resume` (`-r`)   | Pick one of the earlier conversations from a list.  |
| `/resume`         | Same picker, from inside a running Claude Code.           |

### Shortcut alias

Put the alias in `~/.bash_aliases` (the default Ubuntu `~/.bashrc` loads that file):

```bash
echo "alias clc='claude --continue'" >> ~/.bash_aliases
source ~/.bashrc
```

Now `clc` brings back the last conversation.

> **Note:** don't use `cc` as the alias name. `cc` is the C compiler (`/usr/bin/cc`, from `build-essential`), and the alias would hide it at the prompt. Check that a name is free with `type <name>` before you use it.

## Optional: keep WSL running when idle

To stop WSL from shutting down after the last terminal closes, create `C:\Users\<you>\.wslconfig` on Windows:

```ini
[wsl2]
vmIdleTimeout=-1
```

Then run `wsl --shutdown` once from PowerShell to apply it. This does **not** help across a Windows reboot, so `claude --continue` is still the real fix.
