# Initial Setup

Everything I did to get from "no Linux environment" to "daily driver terminal I actually use."

---

## Choosing an environment

> I used to think "installing Linux" meant either a VM or WSL2 — something
> sitting inside Windows. Dual-booting is different: Linux Mint is installed
> on its own disk partition, running directly on the hardware, not virtualized
> inside anything. That means real driver access and real performance, but it
> also means the install step (partitioning, bootloader) can actually break
> the Windows side if done carelessly — there's no "just delete the VM" undo.

**What I'm using:** Linux Mint (Cinnamon), dual-booted alongside Windows on
my laptop.

### Making the installer USB
- Download the Linux Mint ISO (Cinnamon edition) from linuxmint.com
- Download Rufus (rufus.ie) to write the ISO to a USB drive from Windows
- In Rufus: select the USB drive, select the Mint ISO, partition scheme
  GPT (for UEFI — most laptops from the last several years), then Start

### Freeing up disk space for Mint
Before booting the installer, shrink the Windows partition to make room:
- Windows: Disk Management → right-click the main partition → Shrink Volume
- Leave the freed space **unallocated** — the Mint installer partitions it,
  don't pre-format it from Windows

**Dangerous because:** shrinking the wrong partition or getting the
partitioning step wrong during install can make Windows unbootable. I made
sure important files were backed up elsewhere before starting this step —
not something to attempt for the first time with only one copy of anything
that matters.

### Installing Mint
- Boot from the USB (usually F2/F10/F12/Del at startup to pick boot device,
  varies by laptop manufacturer — check before you're stuck)
- Choose "Install Linux Mint", select "Something else" at the partitioning
  step to control exactly which partition it installs into, rather than
  letting it auto-select and risk it touching the Windows partition
- Install GRUB (the bootloader) to the same drive Windows is on — this is
  what gives the OS selection menu on startup

### After install
```bash
sudo apt update && sudo apt upgrade -y
```
Run this first, before anything else — gets the freshly installed system's
packages current before installing more tools on top.

**What happens under the hood (dual boot generally):** GRUB replaces the
Windows Boot Manager as the first thing that runs at power-on. It reads a
config listing both OS entries and hands off control to whichever one is
picked — Windows' own bootloader is still there, just no longer first in
line.

---

## Git configuration

```bash
git config --global user.name "kamaleshpaldev"
git config --global user.email "your-github-email@example.com"
```
**What happens under the hood:** writes to `~/.gitconfig`, a plain text file Git
reads before every commit to stamp author info. Global scope means every repo on
this machine uses these values unless overridden locally with `git config user.name` (no `--global`) inside a specific repo.

---

## SSH keys for GitHub

> What it is: an SSH key pair lets me authenticate to GitHub without typing a
> password every push. The private key stays on my machine; the public key goes
> on GitHub.

```bash
ssh-keygen -t ed25519 -C "your-github-email@example.com"
```
**What happens under the hood:** generates a private key at `~/.ssh/id_ed25519`
and a public key at `~/.ssh/id_ed25519.pub`. `ed25519` is the key algorithm —
newer and faster than the older RSA default.

```bash
cat ~/.ssh/id_ed25519.pub
```
Copy the output, paste it into GitHub → Settings → SSH and GPG keys → New SSH key.

Test the connection:
```bash
ssh -T git@github.com
```
**Safe because:** the public key can't be used to derive the private key. Sharing
it is fine — that's the whole point of asymmetric auth.

> `-t` specifies the key type
> `-C` adds a comment (usually email) to identify the key later

---

## Shell config basics

`~/.bashrc` runs every time a new interactive shell opens. This is where I put
anything I want available in every terminal session — aliases, `PATH` additions,
prompt customization.

```bash
alias ll='ls -la'
alias gs='git status'
```
Add lines like this to the bottom of `~/.bashrc`, then reload without   restarting
the terminal:
```bash
source ~/.bashrc
```
**What happens under the hood:** `source` re-executes the file in the *current*
shell process instead of spawning a new one, so the changes apply immediately.

---

## Things I broke and fixed

See `things-i-broke/incident-log.md` for the running log — keeping it in one
place instead of scattered across every setup file.
