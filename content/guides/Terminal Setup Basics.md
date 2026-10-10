---
title: "Terminal Setup: Basics"
description: "First-day terminal setup for Ubuntu: essential commands, Terminator, case-insensitive Tab completion, and fzf"
tags:
  - guide/linux
aliases:
  - ubuntu basic setup
date: 2025-06-21
---
I've noticed that many friends who are starting to use Linux (particularly Ubuntu) struggle with terminal usage. This guide covers the basics you can set up on day one, in about 10 minutes. Once you are comfortable, continue with [[Terminal Setup Advanced|Terminal Setup: Advanced]].

This guide uses Bash, the default shell on Ubuntu. If you prefer Zsh, see [[ZSH Setup]]. You can also find this setup on my GitHub: [Ubuntu Basic Setup](https://github.com/JisangYun00/Ubuntu-Basic-Setup)

## 1. Essential Commands

Open a terminal with `Ctrl + Alt + T`. These are the commands you will use every day:

| Command | What it does | Example |
| --- | --- | --- |
| `pwd` | Show the current directory | `pwd` |
| `ls` | List files (`-a` hidden files, `-l` details) | `ls -al` |
| `cd` | Change directory (`..` up, `~` home) | `cd ~/Downloads` |
| `mkdir` | Create a directory (`-p` for nested paths) | `mkdir -p ros2_ws/src` |
| `cp` | Copy files (`-r` for directories) | `cp -r src backup` |
| `mv` | Move or rename | `mv old.txt new.txt` |
| `rm` | Delete (`-r` for directories) | `rm -r build` |
| `cat` / `less` | Print a file / scroll through it | `less log.txt` |
| `sudo` | Run a command as administrator | `sudo apt update` |
| `apt` | Install and remove software | `sudo apt install git` |
| `man` / `--help` | Show the manual for a command | `man ls` |

> [!warning]
> `rm` does not move files to the trash. Deleted files are gone, so double-check the path, especially with `rm -r`.

> [!tip] Want to understand how it all works?
> [[Linux Terminal 101]] walks through the shell, the file system, permissions, pipes, and APT step by step.

## 2. Terminator

Terminator lets you split one window into several terminals, which is handy when you run multiple ROS 2 nodes at once.

```bash
sudo apt install terminator
```

To open Terminator with `Ctrl + Alt + T`, make it the default terminal and select `terminator` from the list:

```bash
sudo update-alternatives --config x-terminal-emulator
```

| Action | Shortcut |
| --- | --- |
| Split side by side | `Ctrl + Shift + E` |
| Split top and bottom | `Ctrl + Shift + O` |
| Move between splits | `Alt + Arrow keys` |
| Maximize / restore a split | `Ctrl + Shift + X` |
| New tab | `Ctrl + Shift + T` |
| Close the current split | `Ctrl + Shift + W` |

## 3. Case-Insensitive Tab Completion
Configure `~/.inputrc` to make tab completion case-insensitive.

### Option 1: Edit directly
```bash
vim ~/.inputrc
```

Add the following:
```bash
set completion-ignore-case on
set show-all-if-ambiguous on
set colored-stats on
```

### Option 2: Generate via commands

```bash
echo 'set completion-ignore-case on' > ~/.inputrc
echo 'set show-all-if-ambiguous on' >> ~/.inputrc
echo 'set colored-stats on' >> ~/.inputrc
```

### Apply settings

```bash
bind -f ~/.inputrc
```

## 4. fzf (Fuzzy Finder)
A fuzzy finder for your command history, files, and directories.

### Installation
```bash
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```

### Usage
- `Ctrl + R` → Fuzzy search through command history
- `Ctrl + T` → Find a file and paste its path into the command line
- `Alt + C` → Find a directory and `cd` into it

## 5. Keyboard Shortcuts

These work in any Bash terminal without extra setup:

| Shortcut | What it does |
| --- | --- |
| `Tab` / `Tab Tab` | Complete a command or path / show all options |
| `↑` / `↓` | Previous / next command in history |
| `Ctrl + A` / `Ctrl + E` | Jump to the start / end of the line |
| `Ctrl + U` / `Ctrl + K` | Delete to the start / end of the line |
| `Ctrl + W` | Delete the previous word |
| `Ctrl + L` | Clear the screen |
| `Ctrl + C` | Stop the running command |
| `!!` | Run the previous command again (e.g. `sudo !!`) |

## Apply the Settings

Reload your shell configuration, or simply open a new terminal:

```bash
source ~/.bashrc
```

## See also
- [[Terminal Setup Advanced|Terminal Setup: Advanced]] — tmux, herdr, VS Code, Neovim, and Lazygit
- [[Linux Terminal 101]] — How the terminal works, from the ground up
- [[ZSH Setup]] — Switch from Bash to Zsh
