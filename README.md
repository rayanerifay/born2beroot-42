*This project has been created as part of the 42 curriculum by kriad.*

# Born2beRoot

## Description
**Born2beRoot** is a system administration project whose goal is to set up a **secure Linux server** inside a **Virtual Machine (VM)**.  
This project helps you learn the fundamentals of:
- virtualization and installing a minimal Linux system
- user and group management
- `sudo` configuration (rules and logs)
- SSH configuration and usage (secure remote access)
- firewall setup (**UFW** on Debian / **firewalld** on Rocky)
- password policy enforcement using **PAM**
- system monitoring through a script automatically executed with **cron**

The project is evaluated during a live defense where the student must explain and demonstrate the full setup.

---

## Technical choices and configuration

### Operating system choice: Debian
I chose **Debian** because:
✅ it is very stable and reliable for server usage  
✅ it is lightweight (perfect for minimal VMs)  
✅ it has excellent documentation and a large community

**Pros**
- Great stability
- Widely used in server environments
- Simple and efficient package management (APT)

**Cons**
- Packages can be older compared to more “recent” distributions
- Some advanced configurations require manual tuning

---

### Main design choices

#### Partitioning
- **LVM (Logical Volume Manager)** is used to manage storage flexibly.
- Several mount points are separated for better stability and security:
  - `/` (root)
  - `/home`
  - `/var`
  - `/srv`
  - `/tmp`
  - `/var/log`
  - `swap`
- A dedicated `/boot` partition is used for boot files.

#### Security
- **SSH** is configured to use port `4242`
- SSH login as **root is disabled**
- Password policy is enforced using **PAM**
- `sudo` is configured with strict rules and logging in `/var/log/sudo/`

#### User management
- A main user (42 login) exists on the VM
- This user belongs to the groups:
  - `sudo`
  - `user42`

#### Installed services
- SSH server (remote administration)
- UFW firewall (network protection)
- Cron (automatic monitoring script execution)

---
## Instructions

### Requirements
- A virtualization tool (e.g., VirtualBox)
- A Debian (or Rocky Linux) ISO image
- Basic Linux command line knowledge

### Useful checks

Check OS and hostname:
```bash
hostnamectl
```
Check partitions:
```bash
lsblk
```
Check SSH and UFW:
```bash
sudo service ssh status
sudo ufw status
```
SSH connection (port 4242):
```bash
ssh <user>@localhost -p 4242
```
## Resources

### Classic references:
- Debian documentation
- OpenSSH: `man ssh`, `man sshd_config`
- UFW: `man ufw`
- PAM (password policy): `/etc/pam.d/`, `login.defs`
- LVM / partitions: `man lsblk`, `pvs`, `vgs`, `lvs`

### AI usage

AI was used mainly for explanations of Linux concepts.
explain key concepts (VM, LVM, SSH, UFW, sudo, PAM)
suggest useful commands for checks and defense preparation
All configurations and tests were performed manually on the VM.

---

## Comparisons

### Debian vs Rocky Linux
| Debian | Rocky Linux |
|------|-------------|
| `.deb` packages | `.rpm` packages |
| Package manager: `apt` | Package manager: `dnf` |
| MAC security: **AppArmor** | MAC security: **SELinux** |
| Common and stable for servers | Enterprise-focused (RHEL ecosystem) |

---

### AppArmor vs SELinux
**AppArmor (Debian)**
- Profile-based application security
- Easier to configure
- Good balance between security and simplicity

**SELinux (Rocky / RHEL)**
- Label-based access control
- Stricter and more powerful
- More complex to configure and manage

---

### UFW vs firewalld
**UFW (Debian)**
- Very easy to use
- Perfect to quickly manage essential firewall rules (allow/deny ports)

**firewalld (Rocky)**
- Zone-based firewall system
- More flexible for complex network setups
- Commonly used in enterprise environments

---

### VirtualBox vs UTM
**VirtualBox**
- Available on Linux / Windows / macOS
- Commonly used for 42 projects
- Easy VM management and snapshots

**UTM**
- Mostly used on macOS (especially Apple Silicon)
- Modern interface and simple VM setup
- A great alternative for Mac users

---