# Initial Setup

### apt

> What it is: APT (Advanced Package Tool) manages software installation
> on Debian-based systems. It sits on top of dpkg and handles dependency
> resolution, repo syncing, and downloads.

## Update all packages 
> should run these commands every week

### apt update
Syncs your local package index with remote repositories. Downloads
Packages.gz from each source in sources.list. Does NOT install anything.

```bash
sudo apt update
```

**What happens under the hood:** Reads /etc/apt/sources.list → makes
HTTP requests to each mirror → verifies GPG signature → writes index
to /var/lib/apt/lists/

### apt upgrade
Installs newer versions of all currently installed packages.

```bash
sudo apt upgrade -y
```

**Safe because:** Never removes existing packages or installs new ones
as dependencies. 

### apt autoremove -y
Delete all leftover dependencies

```bash
sudo apt autoremove -y
```


### apt autoclean 
Installs newer versions of all currently installed packages.

```bash
sudo apt autoclean
```

> `sudo` means Superuser Do , use to get administrative privilege
> `-y` pre answering promt for yes
---




## Things I broke and fixed

