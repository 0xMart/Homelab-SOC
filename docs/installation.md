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

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| **RAM** | 16 GB | 32 GB |
| **Storage** | 200 GB free | 300 GB SSD |
| **CPU** | 4 cores | 8 cores |

**Individual VMs :**

| VM | RAM | Storage | CPU |
|----|-----|---------|-----|
| **Splunk Server (Debian)** | 8 GB | 80 GB | 4 vCPU |
| **Kali Linux** | 4 GB | 40 GB | 2 vCPU |
| **Ubuntu Desktop** | 4 GB | 40 GB | 2 vCPU |
| **Total VMs** | ~16 GB | ~160 GB | 8 vCPU |
> **Note:** Windows host doesn't need a separate VM if using the physical machine as an endpoint.
### Software Requirements

- **Hypervisor:** VMware Workstation Pro 17
- **OS Images:**
  - Debian 13 ISO
  - Windows 10/11 ISO (or use host machine)
  - Kali Linux ISO (for attacks)
- **Splunk:**
  - Splunk Enterprise 10.0.1 (.deb)
  - Universal Forwarder 10.0.1 (Windows .msi)
  - Universal Forwarder 10.0.1 (Linux .deb)

### Download Links

- [VMware Workstation Pro Trial](https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html)
- [Splunk Enterprise](https://www.splunk.com/en_us/download/splunk-enterprise.html)
- [Universal Forwarder](https://www.splunk.com/en_us/download/universal-forwarder.html)
- [Debian ISO](https://www.debian.org/download)
- [Kali Linux ISO](https://www.kali.org/get-kali/#kali-installer-images)

---
