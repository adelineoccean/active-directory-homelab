# Setup notes

## Environment

- Hypervisor: UTM 4.7.5
- Host: Apple Silicon Mac, macOS Tahoe
- DC01 OS: Ubuntu Server 24.04.3 LTS ARM64
- CLIENT01 OS: Windows 11 ARM64

## Phase 1 \'97 Ubuntu Server install

### Issue: VM rebooted back into installer
After installation completed, UTM booted the ISO again instead of
the installed disk. Fix: power off the VM, confirm the ISO was
not ejected automatically, eject it manually via UTM's drive icon,
then power back on. The install had succeeded \'97; it was purely a
boot order issue.

## Phase 2 \'97 Samba AD DC provisioning

### Static IP configuration
Set via /etc/netplan/00-installer-config.yaml:
- IP: 192.168.64.10/24
- Gateway: 192.168.64.1
- DNS: 127.0.0.1 (itself, post-Samba)

### Issue: DNS failing after Samba provision
Commands `host -t SRV _kerberos._udp.lab.local` and
`kinit administrator@LAB.LOCAL` both failed after provisioning.
Cause: Ubuntu 24.04 runs systemd-resolved by default, which
occupies port 53 and blocks Samba's internal DNS.

Fix:
1. Stopped and disabled systemd-resolved
2. Removed symlinked /etc/resolv.conf
3. Created static /etc/resolv.conf pointing to 127.0.0.1
4. Made it immutable with chattr +i to prevent Ubuntu overwriting it
5. Restarted samba-ad-dc

All three verification checks passed after fix.

## Phase 3 \'97 OUs and users

### OUs created
- OU=IT
- OU=Sales
- OU=HR
- OU=Management

### Issue: samba-tool user create failing with OU path
Using full DN path in --userou flag caused DC=lab,DC=local to be
duplicated in the final path. 

Fix: pass only the OU component to --userou, not the full DN.
Samba appends the base domain automatically.

Correct usage:
  sudo samba-tool user create jsmith 'Password' --userou="OU=IT"

Incorrect usage (causes duplication):
  sudo samba-tool user create jsmith 'Password' \
    --userou="OU=IT,DC=lab,DC=local"}
