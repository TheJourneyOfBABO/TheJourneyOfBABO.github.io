---
title: Linux Terminal 101
description: "A beginner-friendly tour of the Linux terminal: how the shell works, the file system, files, permissions, pipes, and installing software"
tags:
  - guide/linux
date: 2025-03-14
---
The terminal looks intimidating at first: a black window, a blinking cursor, and no buttons. But it follows a few simple ideas, and once they click, everything else is just vocabulary. This guide walks through those ideas in order. Open a terminal with `Ctrl + Alt + T` and type along as you read.

> [!tip] You can't break much
> Everything in sections 1–6 only looks around or touches files in your home directory. Commands that can cause real damage are marked with a warning.

If you just want a working setup first, see [[Terminal Setup Basics|Terminal Setup: Basics]], then come back here.

## Quick Reference

The commands you will use from day one. Keep this table handy; the sections below explain how each one works.

| Task | Command |
| --- | --- |
| Where am I? / What's here? | `pwd` / `ls -la` |
| Go somewhere / go home / go back | `cd <dir>` / `cd ~` / `cd -` |
| Create a directory / file | `mkdir -p <dir>` / `touch <file>` |
| Read a file | `cat`, `less`, `head`, `tail -f` |
| Copy / move or rename / delete | `cp -r` / `mv` / `rm -r` |
| Find files | `find . -name "<pattern>"` |
| Search inside files | `grep -r "<text>" .` |
| Edit a file | `nano <file>` or `vim <file>` |
| Make a script executable | `chmod +x <file>` |
| Run as administrator | `sudo <command>` |
| Save output to a file | `cmd > file` / `cmd >> file` |
| Chain commands | `cmd1 \| cmd2` |
| Stop / find / kill a program | `Ctrl + C` / `ps aux \| grep <name>` / `kill <PID>` |
| Install software | `sudo apt update && sudo apt install <package>` |
| Get help | `<command> --help` / `man <command>` |

### Handy Combinations

Once the basics feel natural, small commands can be combined. Don't worry if the symbols look cryptic: `|` sends one command's output into the next (section 8), `&&` runs the next command only if the first one succeeded, and `>>` appends to a file.

| Task | Command |
| --- | --- |
| Find files with "launch" in the name | `ls \| grep launch` |
| List only the directories here | `ls -l \| grep "^d"` |
| Find a command you ran before | `history \| grep apt` |
| Find running ROS 2 processes | `ps aux \| grep ros` |
| Search for text in all files, with line numbers | `grep -rn "TODO" .` |
| Show only the error lines of a log | `grep -i error log.txt` |
| Count the Python files in a project | `find . -name "*.py" \| wc -l` |
| See what takes up space here, largest last | `du -sh * \| sort -h` |
| Check free disk space | `df -h` |
| Create a workspace and move into it | `mkdir -p ros2_ws/src && cd ros2_ws` |
| Update the system in one line | `sudo apt update && sudo apt upgrade` |
| Add an alias permanently | `echo "alias ll='ls -alF'" >> ~/.bashrc` |

## 1. What Is the Terminal?

When you type a command, it passes through a few layers:

```text
You  →  Terminal  →  Shell  →  Kernel  →  Hardware
        (window)     (bash)    (Linux)
```

- The **terminal** is just the window that shows text and takes your keystrokes.
- The **shell** is the program running inside it. It reads what you type, figures out what you mean, and runs it.
- The **kernel** is the core of the operating system that actually talks to the hardware.

So "using the terminal" really means talking to the shell. There are several shells:

| Shell | Notes |
| --- | --- |
| `sh` | The original Bourne shell |
| `bash` | An improved `sh`, and the default on Ubuntu |
| `zsh` | A modern shell with rich customization (see [[ZSH Setup]]) |

Why bother? Many tools, including ROS 2, Git, and compilers, are built for the command line first. Commands can be repeated, combined, saved as scripts, and run on a robot over SSH where there is no screen at all.

## 2. Reading the Prompt and Running Commands

### The prompt

Before you type anything, the shell shows a prompt:

```text
jisang@ubuntu:~$
```

| Part | Meaning |
| --- | --- |
| `jisang` | Your user name |
| `ubuntu` | The computer's name (hostname) |
| `~` | The directory you are in (`~` means your home directory) |
| `$` | You are a normal user. It becomes `#` when you are the administrator (root) |

### The shape of a command

Almost every command follows the same pattern:

```text
command  [options]  [arguments]
ls       -l         /home
```

- The **command** is the program to run.
- **Options** change how it behaves. Short options use one dash (`-l`) and can be combined (`-la` = `-l -a`). Long options use two dashes (`--all`).
- **Arguments** are what the command works on, such as a file or directory.

### Getting help

You never need to memorize options. Ask the command itself:

- `ls --help`: a short summary of the options
- `man ls`: the full manual. Scroll with the arrow keys, search with `/`, quit with `q`
- `which ls`: show where the program is installed

> [!tip]
> Press `Tab` to complete commands and paths, and `↑` to bring back previous commands. You will use these constantly.

## 3. The File System: Everything Starts at `/`

Windows has drives like `C:\`. Linux has a single tree that starts at the **root**, written as `/`. Every file and every disk lives somewhere under it.

```text
/
├── home/
│   └── jisang/        ← your home directory (~)
│       ├── Documents/
│       └── Downloads/
├── etc/               ← system settings
├── usr/
│   └── bin/           ← most installed programs
├── opt/               ← add-on software (e.g. /opt/ros)
└── tmp/               ← temporary files
```

| Directory | What lives there |
| --- | --- |
| `/home` | One directory per user. Your own files go in `/home/<you>` |
| `/usr` | Installed programs, libraries, and header files |
| `/etc` | System configuration files |
| `/var` | Data that changes, such as logs |
| `/tmp` | Temporary files, cleared on reboot |
| `/boot` | The boot loader and kernel |
| `/dev` | Devices, such as disks and USB serial ports |
| `/opt` | Optional software installed outside the package manager |

Two more ideas help a lot:

- **Hidden files start with a dot.** Files like `.bashrc` hold settings and don't show up in a normal `ls`.
- **"Everything is a file."** Even hardware appears as a file. A USB-serial adapter for a robot shows up as something like `/dev/ttyUSB0`.

## 4. Moving Around

Three commands get you anywhere:

| Command | What it does |
| --- | --- |
| `pwd` | **P**rint **w**orking **d**irectory: where am I? |
| `ls` | **L**i**s**t what's here (`-l` details, `-a` hidden files, `-h` human-readable sizes, `-F` mark directories with `/`) |
| `cd <dir>` | **C**hange **d**irectory: go somewhere |

### Absolute and relative paths

There are two ways to describe a location:

- An **absolute path** starts from the root and always means the same place: `/usr/bin/python3`
- A **relative path** starts from where you are now: `Documents/notes`

Relative paths use a few shortcuts:

| Shortcut | Meaning |
| --- | --- |
| `.` | The current directory |
| `..` | The parent directory (one level up) |
| `~` | Your home directory |
| `-` | The previous directory (`cd -` jumps back) |

### Try it

```bash
cd ~            # go home
pwd             # /home/jisang
cd Documents    # relative path: into Documents
cd ..           # back up to /home/jisang
cd /usr/bin     # absolute path: works from anywhere
cd -            # jump back to where you were
ls -la          # list everything here, including hidden files
```

## 5. Working with Files

### Creating

- `mkdir <dir>`: create a directory. Add `-p` to create nested directories in one go: `mkdir -p ros2_ws/src`
- `touch <file>`: create an empty file (or update the timestamp of an existing one)

### Reading

- `cat <file>`: print the whole file (`-n` adds line numbers)
- `less <file>`: scroll through a long file. `Space` for the next page, `/` to search, `q` to quit
- `head <file>` / `tail <file>`: show the first / last 10 lines (`-n 5` for 5 lines)
- `tail -f <file>`: keep showing new lines as they are added, which is handy for logs

### Copying, moving, and deleting

| Command | What it does |
| --- | --- |
| `cp <src> <dst>` | Copy a file (`-r` to copy a directory) |
| `mv <src> <dst>` | Move a file, or rename it if `<dst>` is a new name in the same place |
| `rm <file>` | Delete a file (`-r` for a directory, `-f` to skip confirmations) |
| `rmdir <dir>` | Delete an empty directory |

> [!warning] There is no trash bin
> `rm` deletes immediately and permanently. Be especially careful with `rm -rf`: it deletes a whole directory without asking.

### Wildcards

The shell can match many files at once:

- `*` matches anything: `ls *.py` lists all Python files
- `?` matches exactly one character: `ls log?.txt` matches `log1.txt` and `log2.txt`

Run `ls` with a pattern before using it with `rm`, so you can see exactly what will be deleted.

### Finding files

```bash
find . -name "notes.md"           # search the current directory and below
find . -name "*.launch.py" -type f   # all launch files (-type f = files only)
```

The type can be `f` (file), `d` (directory), or `l` (symbolic link).

### Links

A link is a second name for a file:

- `ln -s <target> <link>` creates a **symbolic link**, like a shortcut on Windows. It can point to directories and to other disks. This is the one you will use most.
- `ln <target> <link>` creates a **hard link**: another name for the exact same data on disk. It cannot point to directories or cross to another disk.

## 6. Editing Text Files

Sooner or later you'll need to edit a config file in the terminal.

### nano: the easy one

`nano <file>` opens a simple editor with its shortcuts listed at the bottom (`^` means `Ctrl`):

- Type to edit
- `Ctrl + O`, then `Enter` to save
- `Ctrl + X` to exit

### Vim: the powerful one

`vim <file>` is installed almost everywhere, so it's worth knowing how to survive in it. Vim has **modes**:

- **Normal mode** (the default): keys are commands, not text
- **Insert mode**: press `i` to start typing, `Esc` to go back to normal mode

From normal mode:

| Keys | What it does |
| --- | --- |
| `:w` | Save |
| `:q` | Quit |
| `:wq` or `:x` | Save and quit |
| `:q!` | Quit without saving |

> [!tip] Stuck in Vim?
> Press `Esc`, then type `:q!` and `Enter`. You're out, and nothing was changed.

## 7. Users, Permissions, and sudo

Linux was designed for many users sharing one machine, so every file has an **owner** and **permissions** that say who can do what with it.

### Reading `ls -l`

```text
-rwxr-xr-x  1  jisang  jisang  4096  Mar 14 10:00  run.sh
```

| Part | Example | Meaning |
| --- | --- | --- |
| Type | `-` | `-` file, `d` directory, `l` link |
| Owner permissions | `rwx` | What the owner can do |
| Group permissions | `r-x` | What members of the file's group can do |
| Others permissions | `r-x` | What everyone else can do |
| Links | `1` | Number of hard links (safe to ignore) |
| Owner | `jisang` | The user who owns the file |
| Group | `jisang` | The group that owns the file |
| Size | `4096` | Size in bytes (`ls -lh` shows `4.0K`) |
| Modified | `Mar 14 10:00` | Last time the file changed |
| Name | `run.sh` | The file name |

Each permission group is three letters, always in the same order. A `-` means that permission is missing:

| Letter | On a file | On a directory |
| --- | --- | --- |
| `r` (read) | Read the contents | List the files inside |
| `w` (write) | Change the contents | Create or delete files inside |
| `x` (execute) | Run it as a program | Enter it with `cd` |

### Changing permissions with `chmod`

**Symbolic mode** says who (`u` owner, `g` group, `o` others) and what to add (`+`), remove (`-`), or set (`=`):

```bash
chmod u+w notes.txt   # let the owner write
chmod g-x run.sh      # remove execute from the group
chmod o=r notes.txt   # others can only read
chmod +x run.sh       # make a script executable for everyone
```

**Numeric mode** adds up read (4) + write (2) + execute (1) for owner, group, and others:

| Number | Permissions | Typical use |
| --- | --- | --- |
| `755` | `rwxr-xr-x` | Scripts and programs |
| `644` | `rw-r--r--` | Normal files |

After `chmod +x run.sh`, run the script with `./run.sh`. The `./` is needed because the shell does not look for programs in the current directory by default (more on that in section 10).

### Ownership

`chown` changes who owns a file. For example, to take ownership of everything in the current directory:

```bash
sudo chown -R $USER:$USER .
```

### sudo: acting as the administrator

Some actions, like installing software or editing files in `/etc`, need administrator rights. The administrator account is called **root**.

- `sudo <command>`: run one command as root. It asks for your password.
- `su`: switch to a root shell entirely (rarely needed)
- `sudo usermod -aG sudo <user>`: give another user sudo rights
- `sudo visudo`: safely edit the sudo configuration

> [!warning]
> `sudo` skips every safety check. Only use it when a command needs it, and never paste a `sudo` command from the internet that you don't understand.

## 8. Connecting Commands: Redirection and Pipes

This is where the terminal becomes more powerful than any GUI.

Every program has three standard streams:

| Stream | Number | Default |
| --- | --- | --- |
| Standard input (stdin) | 0 | The keyboard |
| Standard output (stdout) | 1 | The screen |
| Standard error (stderr) | 2 | The screen |

**Redirection** sends a stream to a file instead:

| Syntax | What it does |
| --- | --- |
| `cmd > file` | Write the output to a file (overwrites it) |
| `cmd >> file` | Append the output to a file |
| `cmd 2> file` | Write only the errors to a file |
| `cmd &> file` | Write both output and errors to a file |
| `cmd < file` | Read input from a file instead of the keyboard |

A **pipe** (`|`) sends the output of one command straight into the next one. Small tools chained together can do surprisingly complex jobs:

```bash
ls /usr/bin | wc -l          # how many programs are installed?
history | grep apt           # which apt commands did I run before?
ros2 topic list | grep scan  # find the LiDAR topic
```

To run commands one after another, join them with `&&`: the second command runs only if the first one succeeded, as in `sudo apt update && sudo apt upgrade`.

`grep` keeps only the lines that contain a word. You can also search inside files with it: `grep -r "TODO" .` finds every `TODO` in the current directory.

## 9. Running and Stopping Programs

A running program is called a **process**.

| Action | How |
| --- | --- |
| Stop the program in the foreground | `Ctrl + C` |
| Run a program in the background | Add `&`: `./long_task.sh &` |
| Pause it / resume it in the background / bring it back | `Ctrl + Z` / `bg` / `fg` |
| See what's running | `ps aux` or `top` (press `q` to quit) |
| Stop a process by its ID (PID) | `kill <PID>` (`kill -9 <PID>` only if it refuses) |

For example, to find and stop a stuck ROS 2 node:

```bash
ps aux | grep my_node    # the second column is the PID
kill 12345
```

## 10. Environment Variables and Your Shell Config

### Environment variables

The shell keeps settings in **environment variables**:

```bash
echo $HOME      # /home/jisang
echo $USER      # jisang
printenv        # show all of them
export MY_VAR=hello   # set one for this terminal session
```

### How the shell finds programs: `$PATH`

When you type `ls`, how does the shell know where the program is? It checks each directory listed in `$PATH`, in order:

```bash
echo $PATH      # /usr/local/bin:/usr/bin:/bin:...
which ls        # /usr/bin/ls
```

That's also why `./run.sh` needs the `./`: the current directory is not in `$PATH`.

### `~/.bashrc` and `source`

Variables set with `export` disappear when you close the terminal. To keep a setting, put it in `~/.bashrc`, a script that runs every time you open a new terminal.

`source <file>` runs a file in your current shell, so its settings apply right away. That's exactly what the classic ROS 2 setup line does:

```bash
source /opt/ros/humble/setup.bash   # load ROS 2 into this terminal
source ~/.bashrc                    # reload your own settings after editing them
```

| File | When it runs |
| --- | --- |
| `/etc/profile` | At login, for every user |
| `~/.profile` | At login, for you |
| `~/.bashrc` | Every time you open a terminal |

### Aliases

An alias is a shortcut for a longer command:

```bash
alias ll='ls -alF'
```

Typed in the terminal, it lasts until you close it. Add the line to `~/.bashrc` to keep it.

### Customizing the prompt

The prompt is controlled by the `PS1` variable. Check it with `echo $PS1`, or try `PS1='bash> '`. Useful placeholders:

| Code | Shows |
| --- | --- |
| `\u` | User name |
| `\h` | Hostname |
| `\w` / `\W` | Full path / only the current directory name |
| `\d` / `\t` | Date / time |
| `\$` | `#` for root, `$` for normal users |

## 11. Installing Software with APT

On Ubuntu you rarely download installers from websites. Instead, **APT** (the package manager) installs software from trusted repositories, along with everything it depends on. Think of it as an app store for the terminal.

```bash
sudo apt update            # refresh the list of available packages
sudo apt upgrade           # install newer versions of what you have
sudo apt install git       # install a package
sudo apt remove git        # remove a package
apt search lidar           # search for packages
apt show git               # details about a package
```

> [!note] update vs upgrade
> `update` only downloads the latest list of packages. `upgrade` actually installs the new versions. Run `update` first.

To look at what's already installed:

- `dpkg -l`: list every installed package (combine with `grep`: `dpkg -l | grep jpeg`)
- `dpkg -L <package>`: list the files a package installed

### Peeking under the hood

Packages are just files placed in the directories from section 3. For example, after installing a compiler and a library:

```bash
sudo apt install -y gcc libjpeg-dev
which gcc                                   # where the compiler lives
ls -alth /usr/bin/$(gcc --dumpmachine)*     # the toolchain programs
dpkg -L libjpeg-turbo8                      # where the library files went
readelf -a /usr/lib/x86_64-linux-gnu/libjpeg.so.8.2.2 | grep SONAME
```

`$(...)` runs the command inside first and pastes its output in place.

## See also
- [[Terminal Setup Basics|Terminal Setup: Basics]] — Terminator, Tab completion, and fzf
- [[Terminal Setup Advanced|Terminal Setup: Advanced]] — tmux, herdr, VS Code, and more
