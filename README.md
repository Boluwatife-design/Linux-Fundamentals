# Linux-Fundamentals
A hands-on writeup covering the history of Linux, distributions, editors, virtualization, package management, shells, and core server roles — plus the basic commands practiced along the way.



## Table of Contents

1. [How Linux Came to Exist](#1-how-linux-came-to-exist)
2. [Linux Distributions](#2-linux-distributions)
3. [Text Editors on Linux](#3-text-editors-on-linux)
4. [Running Linux in a Virtual Machine](#4-running-linux-in-a-virtual-machine)
5. [The Three Categories of Linux Software](#5-the-three-categories-of-linux-software)
6. [Interpreted vs Compiled Languages](#6-interpreted-vs-compiled-languages)
7. [Package Managers, RPM, and YUM](#7-package-managers-rpm-and-yum)
8. [Linux Shells](#8-linux-shells)
9. [Server Roles: Web, Database, Email, File Sharing](#9-server-roles-web-database-email-file-sharing)
10. [Basic Commands Cheat Sheet](#10-basic-commands-cheat-sheet)
11. [Challenges & Key Takeaways](#11-challenges--key-takeaways)

---

## 1. How Linux Came to Exist

Linux traces back to **Unix**, an operating system built at Bell Labs (AT&T) in 1969 by Ken Thompson and Dennis Ritchie. Unix introduced ideas that are still core to Linux today: a hierarchical filesystem, "everything is a file," small composable tools, and multi-user support.

In the 1980s, **Richard Stallman** started the **GNU Project** ("GNU's Not Unix") to build a completely free, Unix-compatible operating system. By the late 1980s GNU had produced most of the pieces of an OS — a compiler (GCC), a shell (Bash), core utilities — but it was missing one critical piece: a **kernel**.

In 1991, a Finnish student named **Linus Torvalds** began writing his own kernel as a personal project and announced it on a newsgroup. That kernel became **Linux**. Combined with the GNU tools, it formed a complete, free operating system often called **GNU/Linux**.

Key concepts learnt here:
- The **kernel** is the core of the OS — it manages the CPU, memory, devices, and processes. Linux (the kernel) itself is not a full OS; it needs surrounding tools (shell, utilities, package manager) to be usable.
- Linux is **open source**: its source code is publicly available under the GPL license, so anyone can view, modify, and redistribute it.
- This openness is *why* so many different "flavors" of Linux (distributions) exist — different groups packaged the same kernel with different tools and philosophies.

<!-- screenshot: e.g. `uname -a` output showing kernel version -->

---

## 2. Linux Distributions

A **distribution ("distro")** is the Linux kernel bundled with a package manager, default software, and configuration choices, put together by a company or community.

| Distro | Family | Package Format | Typical Use |
|---|---|---|---|
| Debian | Debian | `.deb` | Stability-focused, server/desktop |
| Ubuntu | Debian | `.deb` | Beginner-friendly desktop & cloud |
| Linux Mint | Debian | `.deb` | Desktop, Windows-like UX |
| Fedora | Red Hat | `.rpm` | Cutting-edge desktop, upstream for RHEL |
| RHEL (Red Hat Enterprise Linux) | Red Hat | `.rpm` | Enterprise servers, paid support |
| CentOS Stream / AlmaLinux / Rocky Linux | Red Hat | `.rpm` | Free RHEL-compatible servers |
| openSUSE | SUSE | `.rpm` | Desktop & server, YaST tool |
| Arch Linux | Independent | pacman | Rolling release, full manual control |
| Kali Linux | Debian | `.deb` | Security testing / pentesting |

**Lessons learnt:** distros mostly differ in three things —
1. **Package format & manager** (how software is installed — `.deb`/APT vs `.rpm`/YUM-DNF vs pacman, etc.)
2. **Release model** (fixed releases like Ubuntu LTS vs rolling releases like Arch)
3. **Target audience** (Ubuntu/Mint for beginners, RHEL/CentOS for enterprise servers, Kali for security work)

---

## 3. Text Editors on Linux

Editing config files and code is a daily task, so I looked at the common options:

- **Nano** — beginner-friendly, keyboard shortcuts shown on screen. Good for quick edits.
- **Vim / Vi** — modal editor (Normal, Insert, Command modes), pre-installed on almost every Linux/Unix system, steep learning curve but very fast once learned.
- **Emacs** — highly extensible editor, almost a full development environment on its own.
- **Gedit / Kate** — GUI text editors for desktop distros.
- **VS Code** — modern, extension-rich code editor, widely used for actual development work, runs on Linux natively.

**Key takeaway:** Vim is worth learning even minimally because it's *always* available on remote servers where you may have no GUI — you can't always install VS Code on a production server.

```bash
nano filename.txt     # open/create file in Nano
vim filename.txt      # open/create file in Vim (press i to insert, Esc then :wq to save & quit)
```


## 4. Running Linux in a Virtual Machine

Virtualization lets you run a full Linux OS *inside* your existing OS (Windows/macOS), without dual-booting or dedicating a physical machine.

**Tools used/explored:**
- **VirtualBox** (Oracle, free) — most common for learning/testing.
- **VMware Workstation/Player** — another popular hypervisor.
- **Hyper-V** — built into Windows Pro/Enterprise.

**Basic steps to set up a Linux VM:**
1. Download a distro's `.iso` file (e.g. Ubuntu Server or Ubuntu Desktop).
2. Install a hypervisor (e.g. VirtualBox).
3. Create a new VM: allocate CPU cores, RAM, and virtual disk space.
4. Mount the `.iso` as the VM's virtual optical drive.
5. Boot the VM and run through the OS installer.
6. Install **Guest Additions** (VirtualBox) for better screen resolution, shared clipboard, and shared folders.

**Concept learned:** A hypervisor creates a virtual machine that behaves like a real computer — virtual CPU, virtual RAM, virtual disk, virtual network card — so the guest OS "believes" it's running on real hardware. This is what makes it safe to experiment (break the VM, snapshot/restore, delete and start over) without touching your host system.
---

## 5. The Three Categories of Linux Software

Linux software generally falls into three broad categories:

1. **Server Applications** — run in the background, provide a service over the network. Examples: Apache/Nginx (web), MySQL/PostgreSQL (database), Postfix (email), Samba (file sharing). Usually no GUI, managed via config files and `systemctl`.
2. **Desktop Applications** — have a GUI, meant for direct interactive use. Examples: Firefox, LibreOffice, GIMP, file managers, desktop environments (GNOME, KDE, XFCE).
3. **Tools / Utilities** — small, focused command-line programs that do one job well, often chained together. Examples: `grep`, `awk`, `sed`, `tar`, `curl`, `find`.

**Key takeaway:** This mirrors the Unix philosophy — "do one thing well" — and explains why a headless server distro only needs categories 1 and 3, while a desktop distro adds category 2 on top.

---

## 6. Interpreted vs Compiled Languages

| | Compiled | Interpreted |
|---|---|---|
| **How it runs** | Source code is translated *entirely* into machine code (a binary/executable) *before* running, by a compiler (e.g. GCC, `javac` + JVM is a hybrid). | Source code is read and executed *line by line* at runtime by an interpreter — no separate machine-code binary is produced. |
| **Speed** | Generally faster at runtime since it's already machine code. | Generally slower, since translation happens every time it runs. |
| **Examples** | C, C++, Go, Rust | Python, Bash, PHP, Ruby, JavaScript |
| **Portability** | Compiled binary is tied to the target OS/architecture (needs recompiling elsewhere). | Same script can run anywhere the interpreter is installed. |
| **Error discovery** | Errors mostly caught at compile time. | Errors often only show up when that line actually executes. |

**Why this matters on Linux:** most core system tools (the kernel, `ls`, `cat`, `bash` itself) are compiled C programs for speed, while a lot of automation/scripting (shell scripts, Python system scripts) is interpreted for flexibility and quick editing without recompiling.

```bash
gcc hello.c -o hello   # compile C source into an executable binary
./hello                 # run the compiled binary

python3 hello.py        # interpreter reads & runs the script directly, no separate build step
```

---

## 7. Package Managers, RPM, and YUM

**What is a package manager?**
A package manager is a tool that automates installing, updating, configuring, and removing software, along with resolving **dependencies** (other software packages a program needs to run). Without it, you'd have to manually download, compile, and link every piece of software and its dependencies.

**RPM (Red Hat Package Manager / RPM Package Manager)**
- A packaging **format** (`.rpm` files) and low-level tool used by Red Hat–family distros (RHEL, Fedora, CentOS, AlmaLinux, Rocky Linux, openSUSE).
- `rpm` itself installs a single package but does **not** automatically resolve/download dependencies from the internet — that's what YUM/DNF add on top.

```bash
rpm -ivh package.rpm     # install a local .rpm package (i=install, v=verbose, h=hash progress)
rpm -qa                  # list all installed rpm packages
rpm -e package_name      # remove (erase) a package
```

**YUM (Yellowdog Updater, Modified)**
- A **higher-level** package manager built on top of RPM, used on older RHEL/CentOS systems (7 and earlier). It connects to software **repositories** (repos), resolves dependencies automatically, and downloads everything needed.
- Modern Red Hat–family systems (RHEL 8+, Fedora) use **DNF**, the successor to YUM, with the same basic command syntax.

```bash
sudo yum install httpd        # install the Apache web server package + its dependencies
sudo yum update                # update all installed packages
sudo yum remove httpd          # uninstall a package
sudo yum search "database"     # search repos for matching packages
sudo yum list installed        # list installed packages
```

**Debian-family equivalent (for comparison):**
```bash
sudo apt install nginx    # APT is Debian/Ubuntu's higher-level package manager
sudo apt update && sudo apt upgrade
```


## 8. Linux Shells

A **shell** is the command-line program that interprets the commands you type and asks the kernel to carry them out. It's the interface between the user and the OS.

Common shells:
- **sh (Bourne Shell)** — the original Unix shell, minimal.
- **bash (Bourne Again Shell)** — the default on most Linux distros; adds scripting features, history, tab completion.
- **zsh (Z Shell)** — bash-compatible but with extra features (better autocompletion, themes via Oh My Zsh); default on modern macOS.
- **csh/tcsh (C Shell)** — syntax closer to C, less common today.
- **fish** — very beginner-friendly, syntax highlighting and suggestions out of the box, not fully POSIX-compatible.

```bash
echo $SHELL         # show my current default shell
chsh -s /bin/zsh     # change my default shell to zsh
cat /etc/shells      # list shells installed/available on the system
```

**Key takeaway:** the shell is also a full scripting language — a `.sh` file full of shell commands can automate multi-step tasks, which is why shell scripting shows up everywhere in server administration.

---

## 9. Server Roles: Web, Database, Email, File Sharing

Linux dominates server infrastructure because these core services are mature, free, and stable on it:

**Web Servers** — serve web pages/apps over HTTP/HTTPS.
- **Apache (httpd)** — very configurable, module-based (`.htaccess` support).
- **Nginx** — event-driven, excellent at handling many concurrent connections, also used as a reverse proxy/load balancer.

```bash
sudo yum install httpd
sudo systemctl start httpd
sudo systemctl enable httpd    # auto-start on boot
```

**Database Servers** — store and manage structured data.
- **MySQL / MariaDB** — the most common open-source relational databases.
- **PostgreSQL** — advanced open-source relational database, strong on standards compliance.

```bash
sudo yum install mariadb-server
sudo systemctl start mariadb
```

**Email Servers** — handle sending/receiving mail.
- **Postfix / Sendmail** — handle **SMTP** (sending mail between servers).
- **Dovecot** — handles **IMAP/POP3** (letting mail clients retrieve mail from the mailbox).

**File Sharing Servers** — let systems share files/folders over a network.
- **Samba** — implements the SMB/CIFS protocol, lets Linux share files with Windows machines (and vice versa).
- **NFS (Network File System)** — native Unix/Linux file sharing protocol, common between Linux servers.
- **vsftpd** — a lightweight FTP server for simple file transfer.

**Key takeaway:** All of these follow the same pattern — install the package, start/enable the `systemd` service, then edit a config file (usually in `/etc/`) to configure it. Learning `systemctl` and reading logs in `/var/log/` covers 80% of "how do I manage this service" questions regardless of which service it is.

---

## 10. Basic Commands Cheat Sheet

```bash
# Navigation
pwd                     # print current working directory
cd /path/to/dir         # change directory
cd ..                   # go up one directory
ls                       # list files in current directory
ls -la                   # list all files (incl. hidden), long format with permissions

# File & directory management
mkdir new_folder         # create a new directory
touch file.txt           # create an empty file
cp file.txt backup.txt   # copy a file
mv file.txt newname.txt  # move/rename a file
rm file.txt               # delete a file
rm -r folder_name         # delete a folder and its contents

# Viewing files
cat file.txt              # print whole file to screen
less file.txt              # view file page-by-page (q to quit)
head -n 10 file.txt        # first 10 lines
tail -n 10 file.txt        # last 10 lines
tail -f /var/log/messages  # "follow" a log file live

# Permissions & ownership
sudo <command>              # run a command as another user (default: root/admin)
chmod +x script.sh          # make a file executable
chown user:group file.txt   # change file owner/group

# Package management (Red Hat family)
sudo yum install <package>
sudo yum update
sudo yum remove <package>

# Package management (Debian family)
sudo apt install <package>
sudo apt update && sudo apt upgrade

# Process & service management
ps aux                       # list running processes
top                            # live view of running processes & resource use
sudo systemctl start <service>
sudo systemctl status <service>
sudo systemctl enable <service>   # start automatically on boot

# System info
uname -a               # kernel and system info
df -h                   # disk space usage, human-readable
free -h                 # memory usage, human-readable
```


## 11. Challenges & Key Takeaways

**Challenges I ran into:**
- Getting used to the difference between `yum`/`dnf` (Red Hat family) and `apt` (Debian family) — same job, different syntax and package formats.
- Understanding that `rpm` alone doesn't fetch dependencies — I had errors until I realized YUM/DNF is the layer that actually talks to repositories.
- Vim's modal editing (`i` to insert, `Esc` then `:wq` to save/quit) tripped me up the first few times — easy to get "stuck" without knowing the escape sequence.
- Remembering that most server management follows the same repeatable pattern (`install → systemctl start → systemctl enable → edit config in /etc/ → check logs`) made new services feel a lot less intimidating once I saw the pattern.

**Biggest takeaways:**
- Linux isn't one thing — it's a kernel plus whatever tools a distro chooses to bundle with it, which is why the ecosystem looks so varied.
- Package managers exist specifically to solve dependency management — that's the core problem they're solving.
- A shell is both an interactive tool *and* a scripting language, which is why shell fluency compounds in usefulness over time.
- Server administration on Linux is very pattern-based once you've set up one service (web, database, email, or file sharing) — the same install/enable/configure/log-check loop applies broadly.

---
