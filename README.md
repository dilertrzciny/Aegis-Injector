# Aegis Injector

<div align="center">

**Modern CS2 / CS:GO Library Injector for Linux**

[![Rust](https://img.shields.io/badge/Rust-1.70+-orange?logo=rust)]()
[![License](https://img.shields.io/badge/license-MIT-blue.svg)]()
[![Platform](https://img.shields.io/badge/platform-Linux-lightgrey?logo=linux)]()
[![Status](https://img.shields.io/badge/status-Active-green.svg)]()

*Clean GUI-based .so injector with pkexec elevation*

</div>

---

## Overview

Aegis Injector is a **modern, GUI-based shared library injector** designed specifically for **Counter-Strike 2** and **CS:GO** on Linux. Built with Rust and the Iced framework, it provides a clean, dark-themed interface that handles the entire injection process — from PID detection to `dlopen()` execution via GDB.

**No manual terminal commands required.** Just select your library, pick your target game, and click inject.

---

## Features

- **Dark Modern GUI** — Built with [Iced](https://github.com/iced-rs/iced), clean card-based layout
- **Dual Game Support** — Inject into CS2 or CS:GO with one click
- **Smart PID Detection** — Automatically finds the real game process (skips bash wrappers and zombie processes)
- **File Validation** — Only accepts `.so` shared library files
- **Secure Elevation** — Uses `pkexec` for privilege escalation (GUI password prompt)
- **Colored Logs** — Real-time injection logs with syntax highlighting
- **Verification** — Confirms injection success by checking `/proc/PID/maps`

---

## Screenshots

> *Screenshot placeholder — will be added in v3.2*

---

## How It Works
┌─────────────┐     ┌──────────────┐     ┌─────────────────┐
│  User picks │────>│  App finds   │────>│  Calls gdb with │
│   .so file  │     │   game PID   │     │   pkexec root   │
└─────────────┘     └──────────────┘     └────────┬────────┘
                                                  │
                                                  ▼
                                           ┌─────────────┐
                                           │ call dlopen │
                                           │  in target  │
                                           └─────────────┘


Under the hood:
1. `pgrep -f` locates the game binary (CS2: `linuxsteamrt64/cs2`, CS:GO: `csgo_linux64`)
2. Zombie processes are filtered out via `/proc/PID/status`
3. Highest CPU usage PID is selected (actual game, not launcher)
4. `gdb -batch -p PID -ex "call dlopen(...)"` is executed with `pkexec`
5. Injection is verified by scanning `/proc/PID/maps`

---

## Requirements

| Tool | Purpose | Install |
|------|---------|---------|
| **Rust** | Build the injector | [rustup.rs](https://rustup.rs) |
| **gdb** | Execute `dlopen()` in target process | `sudo apt install gdb` |
| **pkexec** | Privilege escalation | `sudo apt install policykit-1` |
| **procps** | Process inspection (`ps`, `pgrep`) | Pre-installed on most distros |

---

## Installation

```bash
# Clone the repository
git clone https://github.com/dilertrzciny/aegis-injector.git
cd aegis-injector

# Build release binary
cargo build --release

# Run (no sudo needed — pkexec handles elevation)
./target/release/aegis_injector
Usage

1.    Launch aegis_injector
2.  Select target game — CS2 or CS:GO
3.  Click Browse — select your .so library file
4.  Click INJECT — enter your password in the pkexec prompt
5.  Check logs — green [+] means success, red [-] means failure

Roadmap
v3.1 (Current)

    CS2 injection
    CS:GO injection
    Dark theme GUI
    .so file validation
    Colored log output
    pkexec elevation

v3.2 (Planned)

    Auto-detect installed games
    Injection history (save last used libraries)
    Custom themes support
    Drag & drop file support
    Pre-injection hooks (anti-detection options)

v4.0 (Future)

    Multi-process injection (inject to all threads)
    DLL proxy support
    Built-in library compiler
    Plugin system
    Windows support (Wine/Proton)
    Troubleshooting
Problem
	
Solution
pkexec: command not found
	
Install polkit: sudo apt install policykit-1
ptrace: Operation not permitted
	
Set echo 0 > /proc/sys/kernel/yama/ptrace_scope
gdb: process is zombie
	
Game crashed — restart it and try again
CS2/CSGO not found
	
Make sure the game is running before injecting
Library not found in maps
	
Check if your .so was compiled for the right architecture (64-bit)
Author
diler_trzciny

    First release: v3.1
    Platform: Linux (tested on Arch Linux / Ubuntu)
    Status: Actively maintained

<div align="center">

If this project helped you, consider starring it!
Made with Rust and Iced
</div>
