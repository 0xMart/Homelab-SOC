# 🔧 Installation Guide

Complete step-by-step guide to build your own SOC home lab.

---

## 📋 Table of Contents

- [Prerequisites](#prerequisites)
- [1. VMware Network Setup](#1-vmware-network-setup)
- [2. Splunk Server Setup](#2-splunk-server-setup)
- [3. Windows Forwarder Setup](#3-windows-forwarder-setup)
- [4. Linux Forwarder Setup](#4-linux-forwarder-setup)
- [5. Verification](#5-verification)
- [Troubleshooting](#troubleshooting)

---

## Prerequisites

### Hardware Requirements

**Host Machine (Physical PC):**

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| **RAM** | 16 GB | 32 GB |
| **Storage** | 250 GB free | 300 GB SSD |
| **CPU** | Quad-core  | Hexa-core+ |

**Individual VMs:**

| VM | Purpose | RAM | Storage | CPU |
|----|---------|-----|---------|-----|
| **Debian 13** | Splunk Server | 8 GB | 80 GB | 4 vCPU |
| **Windows 10** | Monitored Endpoint | 4 GB | 60 GB | 2 vCPU |
| **Kali Linux** | Attack Platform | 4 GB | 40 GB | 2 vCPU |
| **Ubuntu Desktop** | Monitored Endpoint | 4 GB | 40 GB | 2 vCPU |
| **Total** | - | ~20 GB | ~220 GB | 10 vCPU |

> 💡 **Tip:** Start with Debian + Ubuntu (12 GB RAM, 120 GB storage) to learn SIEM basics. Add Windows 10 VM and Kali later for attack simulations.

> ⚠️ **Important:** Do NOT install Universal Forwarder on your physical host machine. Use dedicated VMs as monitored endpoints to keep your main system safe from attack simulations.

---

### Software Requirements

**Virtualization:**
- VMware Workstation Pro 17 (or VMware Player/VirtualBox)

**Operating Systems:**
- Debian 13 ISO (for Splunk Server)
- Windows 10 ISO (for monitored endpoint)
- Kali Linux ISO (for penetration testing)
- Ubuntu 22.04/24.04 Desktop ISO (for monitored endpoint)

**Splunk Components:**
- Splunk Enterprise 10.0.1 (.deb for Linux)
- Universal Forwarder 10.0.1 (.msi for Windows VM)
- Universal Forwarder 10.0.1 (.deb for Linux VMs)

---

### Download Links

| Software | Link |
|----------|------|
| **VMware Workstation Pro** | [Download Trial](https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html) |
| **Splunk Enterprise** | [Download (.deb)](https://www.splunk.com/en_us/download/splunk-enterprise.html) |
| **Universal Forwarder** | [Download (all platforms)](https://www.splunk.com/en_us/download/universal-forwarder.html) |
| **Debian 13** | [Download ISO](https://www.debian.org/download) |
| **Kali Linux** | [Download Installer](https://www.kali.org/get-kali/#kali-installer-images) |
| **Ubuntu Desktop** | [Download ISO](https://ubuntu.com/download/desktop) |
| **Windows 10** | [Download ISO](https://www.microsoft.com/software-download/windows10) |

> ⚠️ **Note:** You'll need a free Splunk account to download Splunk software. License is free for up to 500 MB/day.

> 💡 **Windows 10:** Microsoft provides free evaluation ISOs (180 days trial). Perfect for lab testing without purchasing a license.

---

## 1. VMware Network Setup

Goal:

- One **NAT network** (for Splunk Web + internet on the Splunk server)
- One **Host-Only network** (for internal traffic + logs + attacks)
- The **host is NOT reachable** from the internal lab network

### 1.1 Create Host-Only Network

**Create an isolated network for the lab (no internet access).**

This network will be used for:

- Kali ↔ Windows ↔ Ubuntu ↔ Splunk (internal traffic)
- Universal Forwarders sending logs to Splunk

1. Open **VMware Workstation**
2. Go to **Edit → Virtual Network Editor**
3. Click **Change Settings** (Run as Administrator)
4. Select **VMnet1**
5. Configure VMnet1 as:

   - **Type:** Host-only
   - **Subnet IP:** `10.10.10.0`
   - **Subnet mask:** `255.255.255.0`
   - **Use local DHCP:** unchecked (we will use static IPs)
   - **Connect a host virtual adapter to this network:** checked

> The host adapter must stay enabled so the virtual switch remains active.  
> We will isolate the host in a later step by removing its IP.

Click **Apply**, then **OK**.

---

### 1.2 Verify Configuration

VMnet8 is the default NAT network. It will be used **only** for:

- Internet access on the Splunk server
- Accessing Splunk Web from the host

1. In **Virtual Network Editor**, select **VMnet8**
2. Make sure it is configured as:

   - **Type:** NAT
   - **DHCP:** enabled
   - **Subnet IP:** (example) `192.168.101.0`
   - **Subnet mask:** `255.255.255.0`
   - Click **NAT Settings…** and confirm:
     - **Gateway IP:** e.g. `192.168.101.2`

Click **Apply**, then **OK**.

---

### 1.3 Network Topology
### 1.3 Assign Networks to VMs

#### Splunk Server VM (Debian)

- **Network Adapter 1 (NAT):**
  - **Connection:** NAT or Custom → VMnet8

- **Network Adapter 2 (Internal Lab):**
  - **Connection:** Custom → VMnet1

#### Windows Endpoint VM

- Single adapter:
  - **Connection:** Custom → VMnet1

#### Ubuntu Endpoint VM

- Single adapter:
  - **Connection:** Custom → VMnet1

#### Kali Attacker VM

- Single adapter:
  - **Connection:** Custom → VMnet1

---

### 1.4 Assign Network to VMs
We’ll use this IP plan:

| Host            | Interface | Network | Address        |
|-----------------|-----------|---------|----------------|
| Splunk server   | NAT       | VMnet8  | 192.168.101.X  |
| Splunk server   | Internal  | VMnet1  | 10.10.10.10    |
| Windows endpoint| Internal  | VMnet1  | 10.10.10.13    |
| Ubuntu endpoint | Internal  | VMnet1  | 10.10.10.12    |
| Kali attacker   | Internal  | VMnet1  | 10.10.10.11    |


> ⚠️ **Security Note:**  
> - The physical host is used ONLY for VMware and accessing Splunk Web UI  
> - All monitored endpoints and attacks happen in isolated VMs  
> - This protects your main system from attack simulations
---
#### Splunk Server 

1. Boot the Debian Splunk server
2. Check interfaces:

   ```bash
   ip -br a
   ```
   You should see something like:

- **ens33 → NAT** (192.168.101.128 via DHCP)
- **ens37 → Internal** (no IP yet)

### Configure the internal NIC:

1. Bring the interface up:

   ```bash
   sudo ip link set ens37 up
   sudo ip addr add 10.10.10.10/24 dev ens37
   ```
You will later access Splunk Web using:

```
http://192.168.101.128:8000
```

---
## 2 splunk server setup

