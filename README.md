# Homelab Infrastructure

Personal homelab built to develop and demonstrate real-world systems administration skills — virtualization, networking, automation, and monitoring — as I work toward a sysadmin role.

## Overview

This repo documents the infrastructure I've built, the problems I ran into, and how I solved them. It's meant to be a transparent look at the actual process, not just a polished end result.


**Hypervisor:** Oracle VirtualBox

## Current Infrastructure

| VM Name | OS | Purpose | IP/Access |
|---|---|---|---|
| sysadmin-vm-01 | Ubuntu Server 24.04 LTS | Primary practice server | NAT, SSH via `127.0.0.1:2222` |

## What's Been Set Up

- [x] VirtualBox installed, first VM provisioned (Ubuntu Server, headless)
- [x] SSH access configured (openssh-server installed, port forwarding rule added)
- [ ] User/permission management practice
- [ ] Ansible automation
- [ ] Monitoring stack (Prometheus/Grafana)
- [ ] Additional VMs for multi-server practice

## Setup Steps

1. Installed VirtualBox on Windows host
2. Downloaded Ubuntu Server 24.04 LTS ISO
3. Created VM (`sysadmin-vm-01`) — 2GB RAM, 20GB disk
4. Installed Ubuntu Server, configured basic user account
5. Installed and enabled `openssh-server` for remote access
6. Configured NAT + port forwarding (host `2222` → guest `22`) for SSH access from host machine

## Troubleshooting Log

Real problems I hit and how I solved them — this is the part that actually matters for interviews.

### Bridged networking wouldn't get a DHCP lease
- **Problem:** VM showed `UP` on its network adapter but never received an IPv4 address via Bridged Adapter mode.
- **Diagnosis:** Verified Netplan config was correct (`dhcp4: true`), confirmed the right physical adapter was selected in VirtualBox, ruled out promiscuous mode. Root cause was likely a Windows networking conflict.
- **Solution:** Switched to NAT mode with an explicit port forwarding rule (`2222 → 22`) instead of fighting Bridged Adapter — a common, valid approach for laptop-based labs.

### SSH connection refused
- **Problem:** After setting up port forwarding, `ssh` from Windows returned "Connection refused."
- **Diagnosis:** Ran `systemctl status ssh` inside the VM — found the SSH service didn't exist at all.
- **Solution:** Ubuntu Server doesn't install `openssh-server` by default in all cases. Installed it manually (`sudo apt install openssh-server`) and enabled the service.

## Screenshots

See [`/screenshots`](./screenshots) for setup process images, including:
- VirtualBox VM configuration
- First successful boot
- First SSH session

## Next Steps

- Practice core Linux administration (users, permissions, services)
- Automate VM provisioning with Ansible
- Add a monitoring stack
- Expand to multiple VMs for realistic multi-server scenarios

---

*This is a living document, updated as the lab grows.*