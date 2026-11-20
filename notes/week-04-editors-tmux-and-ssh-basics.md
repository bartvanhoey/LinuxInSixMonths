[← Week 4 - Editors, tmux and SSH basics](../weeks/week-04-editors-tmux-and-ssh-basics.md)

# Week 4 - Editors, tmux and SSH basics: notes

**Status:** Not started

## Exercise 1

> Learn vim survival skills (modes, save/quit, search, replace, copy/paste) and nano as fallback.

_Commands, results, what surprised me._

## Exercise 2

> Install tmux: sessions, windows, panes, detach and reattach.

_Commands, results, what surprised me._

## Exercise 3

> Set up ~/.ssh/config with aliases; generate an ed25519 key pair and use ssh-agent.

_Commands, results, what surprised me._

### `git clone` fails with `Permission denied (publickey)`

A fresh WSL install has no SSH key, because it does not share `~/.ssh` with Windows. GitHub rejects the clone until you generate an ed25519 key and add its public half to your account. To use more than one GitHub account, give each account its own key and pick it with a host alias in `~/.ssh/config`.

Step by step: [Clone from GitHub over SSH in WSL (Permission denied (publickey))](github-ssh-keys-and-multiple-accounts-in-wsl.md).

## Exercise 4

> Create a dotfiles repo (.bashrc, aliases, .vimrc, .tmux.conf).

_Commands, results, what surprised me._

## Deliverable

> Dotfiles repo on GitHub. Phase 1 checkpoint: finish Bandit up to about level 20.

_Write-up._
