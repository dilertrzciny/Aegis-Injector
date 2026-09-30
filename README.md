# Aegis Injector

<div align="center">

**Modern CS2 / CS:GO Library Injector for Linux**

[![Rust](https://img.shields.io/badge/Rust-1.70+-orange?logo=rust)]()
[![Platform](https://img.shields.io/badge/platform-Linux-lightgrey?logo=linux)]()
[![Status](https://img.shields.io/badge/status-Active-green.svg)]()

*.so injector with pkexec elevation*

</div>

---

## Overview

Aegis Injector is a **modern, GUI-based shared library injector** designed specifically for **Counter-Strike 2** and **CS:GO** on Linux. Built with Rust and the Iced framework, it provides a clean, dark-themed interface that handles the entire injection process — from PID detection to `dlopen()` execution via GDB.



---

## Features

- **Dark Modern GUI** — Built with [Iced](https://github.com/iced-rs/iced), clean card-based layout
- **Dual Game Support** — Inject into CS2 or CS:GO with one click
- **Smart PID Detection** — Automatically finds the game process
- **Secure Elevation** — Uses `pkexec` for privilege escalation (GUI password prompt)
- **Colored Logs** — Real-time injection logs with syntax highlighting
- **Verification** — Confirms injection success by checking `/proc/PID/maps`


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
3. Highest CPU usage PID is selected
4. `gdb -batch -p PID -ex "call dlopen(...)"` is executed with `pkexec`
5. Injection is verified by scanning `/proc/PID/maps`

---

## Requirements

| Tool | Purpose | Install |
|------|---------|---------|
| **gdb** | Execute `dlopen()` in target process | `sudo apt install gdb` |
| **pkexec** | Privilege escalation | `sudo apt install policykit-1` |
| **procps** | Process inspection (`ps`, `pgrep`) | Pre-installed on most distros |

---

## Installation

```bash
Just download the program from the release.
