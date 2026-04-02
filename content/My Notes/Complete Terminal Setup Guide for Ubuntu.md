---
title: Complete Terminal Setup Guide for Ubuntu
draft: false
tags:
  - "#linux"
  - "#ubuntu"
  - "#setup"
aliases:
  - ubuntu basic setup
date: 2026-04-02
---
I've noticed that many friends who are starting to use Linux (particularly Ubuntu) struggle with terminal usage. Based on my experience and terminal setup, I'd like to share some tips that can significantly improve your command-line experience.

This guide covers four essential tools:

- **Bash Enhancement** - Case-insensitive completion and fuzzy history search
- **Neovim** - Modern text editor setup
- **Tmux** - Terminal multiplexer for session management
- **Lazygit** - Visual Git interface

> ZSH + Oh My Zsh 환경을 원한다면 [[ZSH Setup Guide]] 참고. (ROS/ROS2 환경에서는 bash 권장)

You can also find this setup on my GitHub: [Ubuntu Basic Setup](https://github.com/JisangYun00/Ubuntu-Basic-Setup)

---

## 1. Bash Enhancement

### 대소문자 무시 자동완성

`~/.inputrc` 파일을 설정하면 자동완성 시 대소문자를 구분하지 않게 만들 수 있음.

#### 방법 1: 직접 편집
```bash
vim ~/.inputrc
```

아래 내용 추가:
```
set completion-ignore-case on
set show-all-if-ambiguous on
set colored-stats on
```

#### 방법 2: 명령어로 생성

```bash
echo 'set completion-ignore-case on' > ~/.inputrc
echo 'set show-all-if-ambiguous on' >> ~/.inputrc
echo 'set colored-stats on' >> ~/.inputrc
```

#### 설정 적용

```bash
bind -f ~/.inputrc
```

---

### fzf (Fuzzy Finder)
명령어 히스토리를 더 편하게 검색할 수 있는 도구.

#### 설치
```bash
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```

#### 사용
- `Ctrl + R` → 히스토리 fuzzy 검색

---

### 적용 확인

```bash
source ~/.bashrc
```

---

## 2. Neovim Setup
[Download newest Neovim on Ubuntu](https://github.com/neovim/neovim/blob/master/BUILD.md)
### Installation

```bash
# Install dependencies (optional)
sudo apt install ninja-build gettext cmake unzip curl
```

```
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

---

## 3. Tmux Setup

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

---

## 4. Lazygit - Visual Git Interface

### What is Lazygit?
[Lazygit github](https://github.com/jesseduffield/lazygit)
Lazygit provides a simple terminal UI for Git commands, making Git operations visual and intuitive without memorizing complex commands.

### Instalation
```
LAZYGIT_VERSION=$(curl -s "https://api.github.com/repos/jesseduffield/lazygit/releases/latest" | \grep -Po '"tag_name": *"v\K[^"]*')
curl -Lo lazygit.tar.gz "https://github.com/jesseduffield/lazygit/releases/download/v${LAZYGIT_VERSION}/lazygit_${LAZYGIT_VERSION}_Linux_x86_64.tar.gz"
tar xf lazygit.tar.gz lazygit
sudo install lazygit -D -t /usr/local/bin/
```

### Install for Jetson Orin Nano
```
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

---

## Benefits of This Setup

### Productivity Gains

- **Faster tab completion** with case-insensitive matching
- **Session persistence** with tmux for long-running tasks
- **Simplified Git workflow** with lazygit's visual interface

### Development Workflow

- **Fuzzy history search** speeds up repeated commands
- **Multiple terminal sessions** without losing context
- **Efficient editing** with modern Neovim configuration
- **Intuitive Git operations** reduce command-line errors

This setup transforms your terminal into a powerful development environment that significantly improves productivity and makes the command-line experience more enjoyable.
