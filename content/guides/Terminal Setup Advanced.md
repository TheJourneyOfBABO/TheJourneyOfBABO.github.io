---
title: "Terminal Setup: Advanced"
description: "tmux, herdr, VS Code, Neovim, Lazygit, and handy CLI tools for an Ubuntu development setup"
tags:
  - guide/linux
date: 2026-10-10
---
The next step after [[Terminal Setup Basics|Terminal Setup: Basics]]: tools for keeping long-running sessions alive, running coding agents, editing code, and working with Git.

## 1. tmux
tmux keeps terminal sessions alive after you close the window or lose an SSH connection, and splits one terminal into windows and panes.

### Installation

```bash
# Install packages
sudo apt install git xclip tmux

# Install TPM (Tmux Plugin Manager)
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

### Basic Configuration

Create `~/.tmux.conf`:

```bash
# Core Options
set -g mouse on
# set-option -g default-shell /bin/zsh # When using zsh shell
set -g default-terminal "tmux-256color"
set-option -sa terminal-overrides ",xterm*:Tc"

# Clipboard setup
set-option -g set-clipboard on
bind-key -T copy-mode-vi y send-keys -X copy-pipe-and-cancel "xclip -selection clipboard -in"

# Window and Pane Settings
set -g base-index 1
set -g pane-base-index 1
set-window-option -g pane-base-index 1
set-option -g renumber-windows on
set-window-option -g mode-keys vi

# Key Bindings
bind -n M-H previous-window
bind -n M-L next-window
bind-key -T copy-mode-vi v send-keys -X begin-selection
bind-key -T copy-mode-vi C-v send-keys -X rectangle-toggle
# bind-key -T copy-mode-vi y send-keys -X copy-selection-and-cancel
bind '"' split-window -v -c "#{pane_current_path}"
bind % split-window -h -c "#{pane_current_path}"

# Plugins
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin 'christoomey/vim-tmux-navigator'
# set -g @plugin 'catppuccin/tmux'
set -g @plugin 'dreamsofcode-io/catppuccin-tmux'

set -g automatic-rename on
set -g automatic-rename-format '#{b:pane_current_path}'

# Initialize TPM (keep this line at the very end)
run '~/.tmux/plugins/tpm/tpm'
```

Install plugins: Start tmux, then press `Ctrl + b` + `I`

### Essential Commands

- **Sessions:** `tmux new -s name`, `tmux attach -t name`
- **Windows:** `Ctrl + b + c` (new), `Alt + H/L` (navigate)
- **Panes:** `Ctrl + b + "` (horizontal), `Ctrl + b + %` (vertical)

## 2. herdr

[herdr](https://herdr.dev/) is a terminal multiplexer built for AI coding agents such as Claude Code and Codex. Like tmux, it keeps your terminals running in a background server when you close the window or lose an SSH connection. On top of that, it marks every agent as `working`, `blocked`, or `idle`, so you can see at a glance which one needs your answer.

### Installation

```bash
curl -fsSL https://herdr.dev/install.sh | sh
```

### Basic Usage

Start it in your project directory, then run your agent inside a pane:

```bash
herdr
```

herdr works with the mouse: click to focus panes and tabs, drag borders to resize, and right-click for menus. For the keyboard, press the prefix `Ctrl + b`, then an action key:

| Action | Key |
| --- | --- |
| Split right / down | `prefix + v` / `prefix + -` |
| Move between panes | `prefix + h/j/k/l` |
| New tab / next / previous | `prefix + c` / `prefix + n` / `prefix + p` |
| New workspace | `prefix + Shift + n` |
| Workspace navigation | `prefix + w` |
| Show all key bindings | `prefix + ?` |
| Detach (everything keeps running) | `prefix + q` |

Run `herdr` again to reattach. To stop all panes, run `herdr server stop`.

> [!warning] Same prefix as tmux
> tmux and herdr both use `Ctrl + b` as the prefix. Avoid running one inside the other, or change one of the prefixes. See the [herdr keyboard docs](https://herdr.dev/docs/keyboard/).

### tmux or herdr?

| Use case | Tool |
| --- | --- |
| Servers, builds, ROS 2 nodes, SSH sessions | tmux |
| Several coding agents running in parallel | herdr |

## 3. VS Code

Download the `.deb` package from [code.visualstudio.com](https://code.visualstudio.com/download) and install it. This also adds Microsoft's apt repository, so VS Code updates with `apt`:

```bash
sudo apt install ./code_*_amd64.deb
```

Open the current directory from the terminal:

```bash
code .
```

### Recommended Extensions

| Extension | ID | Use |
| --- | --- | --- |
| C/C++ Extension Pack | `ms-vscode.cpptools-extension-pack` | C++ IntelliSense and debugging |
| Python | `ms-python.python` | Python development |
| rust-analyzer | `rust-lang.rust-analyzer` | Rust development |
| ROS 2 | `Ranch-Hand-Robotics.rde-ros-2` | ROS 2 build, launch, and debugging |
| Remote - SSH | `ms-vscode-remote.remote-ssh` | Develop on a robot or server over SSH |
| Dev Containers | `ms-vscode-remote.remote-containers` | Develop inside Docker containers |

Install them from the terminal:

```bash
code --install-extension ms-vscode.cpptools-extension-pack
code --install-extension rust-lang.rust-analyzer
```

## 4. Neovim
[Download newest Neovim on Ubuntu](https://github.com/neovim/neovim/blob/master/BUILD.md)
### Installation

```bash
# Install dependencies (optional)
sudo apt install ninja-build gettext cmake unzip curl
```

```bash
# Build from source
git clone https://github.com/neovim/neovim
cd neovim && git checkout stable
make CMAKE_BUILD_TYPE=RelWithDebInfo
sudo make install
```

### Quick Theme Setup

**NvChad (Recommended):**

```bash
git clone https://github.com/NvChad/starter ~/.config/nvim && nvim
```

**Custom Configuration (Made by me. Focus on Rust):**

```bash
git clone https://github.com/JisangYun00/clean-nvim.git ~/.config/nvim && nvim
```

## 5. Lazygit
### What is Lazygit?
[Lazygit github](https://github.com/jesseduffield/lazygit)
Lazygit provides a simple terminal UI for Git commands, making Git operations visual and intuitive without memorizing complex commands.

### Installation
```bash
LAZYGIT_VERSION=$(curl -s "https://api.github.com/repos/jesseduffield/lazygit/releases/latest" | \grep -Po '"tag_name": *"v\K[^"]*')
curl -Lo lazygit.tar.gz "https://github.com/jesseduffield/lazygit/releases/download/v${LAZYGIT_VERSION}/lazygit_${LAZYGIT_VERSION}_Linux_x86_64.tar.gz"
tar xf lazygit.tar.gz lazygit
sudo install lazygit -D -t /usr/local/bin/
```

### Install for Jetson Orin Nano
```bash
# Get the latest version of lazygit
LAZYGIT_VERSION=$(curl -s "https://api.github.com/repos/jesseduffield/lazygit/releases/latest" | \grep -Po '"tag_name": *"v\K[^"]*')

# Download the ARM64 version for Jetson Orin Nano
curl -Lo lazygit.tar.gz "https://github.com/jesseduffield/lazygit/releases/download/v${LAZYGIT_VERSION}/lazygit_${LAZYGIT_VERSION}_Linux_arm64.tar.gz"

# Extract and install
tar xf lazygit.tar.gz lazygit
sudo install lazygit -D -t /usr/local/bin/
```

**Key Features:**
- Visual file staging and committing
- Easy branch switching and merging
- Interactive rebase and conflict resolution
- Built-in diff viewer

## 6. More CLI Tools

Small tools that make everyday terminal work faster:

```bash
sudo apt install ripgrep fd-find bat zoxide btop tldr
```

| Tool | Command | What it does |
| --- | --- | --- |
| ripgrep | `rg` | Fast text search across files |
| fd | `fdfind` | Fast, simple alternative to `find` |
| bat | `batcat` | `cat` with syntax highlighting |
| zoxide | `z` | Jump to frequently used directories |
| btop | `btop` | System monitor for CPU, memory, and processes |
| tldr | `tldr` | Short, practical examples for commands |

On Ubuntu, `fd` and `bat` are installed as `fdfind` and `batcat`. Add these lines to `~/.bashrc` to use the usual names and enable zoxide:

```bash
alias fd=fdfind
alias bat=batcat
eval "$(zoxide init bash)"
```

## See also
- [[Terminal Setup Basics|Terminal Setup: Basics]] — Start here if you are new to the terminal
- [[ZSH Setup]] — Zsh and Oh My Zsh with ROS 2 aliases
- [[GitHub Token Setup]] — Authenticate Git over HTTPS
