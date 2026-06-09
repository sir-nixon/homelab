<p align="center">
  <img src="Images/homelab-v2.png" alt="Homelab Diagram" width="700"/>
</p>

<h1 align="center">HomelabHQ</h1>
<p align="center">
  A self-hosted, Proxmox-powered homelab built on a decade-old workstation. Documenting infrastructure setup, VM migration, VPN configuration, surveillance integration and continuous expansion.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Proxmox-VE-E57000?style=flat-square&logo=proxmox&logoColor=white"/>
  <img src="https://img.shields.io/badge/WireGuard-VPN-88171A?style=flat-square&logo=wireguard&logoColor=white"/>
  <img src="https://img.shields.io/badge/Windows_11-VM-0078D4?style=flat-square&logo=windows&logoColor=white"/>
  <img src="https://img.shields.io/badge/Ubuntu-Client-E95420?style=flat-square&logo=ubuntu&logoColor=white"/>
  <img src="https://img.shields.io/badge/Status-Active-brightgreen?style=flat-square"/>
</p>

---

## Overview

This project documents the build-out of a personal homelab running on a **Dell Precision T1700 SFF** repurposed as a Proxmox hypervisor. The goal is a fully self-managed home infrastructure covering virtualisation, remote access, network segmentation, surveillance and any future projects.

Everything is tracked, named, and versioned. Missions follow a structured execution model, each one scoped, named and logged with key learnings.

---

## Hardware

| Component | Spec |
|---|---|
| **Host** | Dell Precision T1700 SFF |
| **CPU** | Intel Xeon E3-1371 v3 @ 3.60GHz (4C/8T) |
| **RAM** | 16GB DDR3 (4× 4GB, 1600 MT/s, Non-ECC UDIMM) |
| **Storage** | 1TB HDD |
| **GPU** | NVIDIA Quadro K620 (2GB VRAM) |
| **Hypervisor** | Proxmox VE |
| **Network** | Mercusys MS110CMP PoE Switch |
| **Cameras** | 2× TP-Link VIGI C440I (PoE) |
| **Client** | HP Laptop (Ubuntu) |

---

## Device Register

All devices follow the `NIX-[LOC]-[TYPE]-[SEQ]` naming convention.

| Device_ID | Name | Model | OS | Status | Notes |
|---|---|---|---|---|---|
| NIX-HLB-SVR-01 | Homelab | Dell Precision T1700 SFF | Proxmox VE | ✅ Active | Xeon E3-1371 v3, 16GB DDR3, 1TB HDD, Quadro K620 |
| NIX-HLB-VPC-01 | Windows VM | VM | Windows 11 | ✅ Active | Hosted on NIX-HLB-SVR-01 |
| NIX-HLB-CAM-01 | Camera #1 | TP-Link VIGI C440I | - | 🔄 In Progress | Connected via Mercusys PoE switch |
| NIX-HLB-CAM-02 | Camera #2 | TP-Link VIGI C440I | - | 🔄 In Progress | Connected via Mercusys PoE switch |
| NIX-CLT-NB-02 | Error404 | HP Laptop | Ubuntu | ✅ Active | Primary client device |

---

## Missions

Missions are named using **Star Wars canon** - the name must thematically mirror the technical action, not loosely reference it.

### ✅ Mission 1 - Carbonite Imprisonment
> *Han Solo was frozen in carbonite - preserved, contained, retrievable on demand. The Windows 11 VHD was migrated into Proxmox, locked into a VM, alive but fully under control.*

**Objective:** Migrate a physical Windows 11 VHD to a Proxmox VM.

**What was done:**
- Transferred `.vhd` to Proxmox host
- Imported disk directly to `local-lvm` via `qm importdisk` to avoid storage exhaustion
- Configured VM with VirtIO disk, CPU host passthrough, memory ballooning disabled
- Pre-injected VirtIO drivers (`viostor.inf`, `vioscsi.inf`) via `pnputil` to prevent boot-time BSODs

**Key learnings:**
- VirtIO drivers **must be injected before** switching disk bus type - switching without them causes `INACCESSIBLE_BOOT_DEVICE` BSODs
- Import directly to `local-lvm`, not `local` dir storage, to avoid space exhaustion during large disk conversions

---

### ✅ Mission 2 - Fulcrum Network
> *Fulcrum was Ahsoka Tano's codename as a rebel intelligence asset - operating remotely, securely, with trusted access across hostile networks. The WireGuard VPN connects the homelab to the outside world on the same terms.*

**Objective:** Establish a WireGuard VPN tunnel between the Proxmox host and the Ubuntu laptop for secure remote access.

**What was done:**
- WireGuard installed and configured on both server (NIX-HLB-SVR-01) and client (NIX-CLT-NB-02)
- Assigned tunnel IPs: server `10.0.0.1`, client `10.0.0.2`
- Key pairs generated, peer configs exchanged
- IP forwarding enabled on the server
- WireGuard service enabled via `systemd`
- Router configured for UDP port 51820 forwarding

**Status:** Configuration complete - tunnel operational.

---

### 📋 Mission 3 - ISB Watchtower
> *The ISB maintained surveillance across the Empire - monitoring, recording, reporting. The PoE cameras feed into a centralised NVR on Proxmox with the same mandate.*

**Objective:** Integrate 2× TP-Link VIGI C440I PoE cameras with an NVR/surveillance VM on Proxmox.

**Status:** Queued.

---

## Stack

- **Hypervisor:** Proxmox VE
- **VPN:** WireGuard
- **Virtualisation:** KVM/QEMU via Proxmox
- **Driver injection:** virtio-win ISO
- **Hardware verification:** dmidecode
