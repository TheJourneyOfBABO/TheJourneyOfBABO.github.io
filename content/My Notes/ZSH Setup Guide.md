---
title: ZSH Setup Guide
draft: false
tags:
  - "#linux"
  - "#ubuntu"
  - "#setup"
  - "#zsh"
---
ZSH + Oh My Zsh 기반 셸 환경 설정 가이드. 자동완성, 문법 하이라이팅, ROS2 alias 설정을 포함함.

> 전체 터미널 셋업 (Neovim, Tmux, Lazygit 포함)은 [[Complete Terminal Setup Guide for Ubuntu]] 참고.

---

## 1. Zsh with Oh My Zsh Setup

### Quick Installation

```bash
# Install prerequisites
sudo apt install wget curl git fonts-powerline zsh

# Change default shell
chsh -s $(which zsh)

# Install Oh My Zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

# Install essential plugins
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

### Configuration

Edit `~/.zshrc`:

```bash
# Theme
ZSH_THEME="af-magic"

# Plugins
plugins=(git zsh-syntax-highlighting zsh-autosuggestions)

# ROS2 aliases
alias cbr="colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release"
alias cbrs="cbr --packages-skip"
alias cbp="colcon build --packages-select"
alias sor="source install/setup.zsh"
alias rlt="ros2 topic list"
alias rle="ros2 topic echo"
alias rli="ros2 topic info"
alias ril="ros2 interface list"
alias rill="ros2 interface list | grep"
alias rils="ros2 interface show"
alias rpe="ros2 pkg executables"

# System aliases
alias sr="source ~/.zshrc"
alias lz="lazygit"
```

Apply changes: `source ~/.zshrc`

**Little tips for ROS2**
```
# ROS2 environment setup
echo "source /opt/ros/humble/setup.zsh" >> ~/.zshrc
```

**ROS2 autocomplete**
```
pip3 install argcomplete
```
```
# ROS2 terminal autocomplete activate
echo "source /opt/ros/humble/share/ros2cli/environment/ros2-argcomplete.zsh" >> ~/.zshrc
```
