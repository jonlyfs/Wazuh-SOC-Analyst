# 01 — Introduction and Lab Architecture

## Purpose
Describe the goal of this SOC lab and the security monitoring use cases you want to practice.

## Environment inventory

| Component | OS/version | Role | Notes |
|---|---|---|---|
| Wazuh server | TODO | SIEM/XDR manager and dashboard | |
| Endpoint 1 | TODO | Monitored endpoint | |
| Sysmon | TODO | Windows event enrichment | |

## Network design
Add a diagram or describe the isolated lab network here. Do not publish sensitive network details.

## Prerequisites
- Hardware and virtualization requirements: TODO
- Operating systems and versions: TODO
- Network connectivity and DNS requirements: TODO

## Success criteria
List how you will verify that the server, agents, and telemetry pipeline are working.

Here is a complete example you can copy and paste into `docs/01-introduction.md` (just adjust the OS versions, RAM/CPU specs, or IP subnets to match your exact home lab setup):

```markdown
# 01 — Introduction and Lab Architecture

## Purpose

The goal of this Security Operations Center (SOC) home lab is to deploy an enterprise-grade SIEM/XDR monitoring pipeline using Wazuh and Sysmon. Through this environment, I simulate realistic adversary behaviors and practice key SOC Tier 1/Tier 2 workflows, including:

* **Endpoint Telemetry Enrichment:** Ingesting granular process, network, registry, and DNS events via Sysmon.
* **Detection Engineering:** Writing and tuning custom Wazuh rules mapped to the MITRE ATT&CK framework.
* **File Integrity Monitoring (FIM):** Detecting unauthorized modifications to critical system files and startup directories in real time.
* **Active Response & Containment:** Automating host-level blocking against brute-force attacks and malicious binaries.
* **Incident Triage & Investigation:** Correlating alerts across dashboards to reconstruct attack timelines and document findings.

## Environment inventory

| Component | OS/version | Role | Notes |
| :--- | :--- | :--- | :--- |
| **Wazuh server** | Ubuntu Server 22.04 LTS (Wazuh v4.9.2) | SIEM/XDR manager, indexer, and dashboard | Single-node all-in-one deployment (`4 vCPU`, `8 GB RAM`, `80 GB SSD`) |
| **Endpoint 1** | Windows 11 Enterprise (23H2) | Monitored victim/target workstation | Runs Wazuh Agent v4.9.2; used for Atomic Red Team simulations |
| **Sysmon** | Sysmon v15.15 (Schema 4.90) | Windows event enrichment | Configured with a custom SOC XML profile (`sysmonconfig-soc-v1.xml`) |
| **Attacker VM** *(Optional)* | Kali Linux 2024.3 | Adversary simulation | Used for Nmap scans, Hydra brute-force tests, and payload delivery |

## Network design

All virtual machines are hosted in an isolated **Host-Only / Internal NAT Network** (`10.10.20.0/24`) within VirtualBox/VMware to ensure attack simulations remain completely isolated from the host network and external internet.

```text
+-------------------------------------------------------------------+
|                  Isolated Lab Subnet (10.10.20.0/24)              |
|                                                                   |
|  +-----------------------+              +----------------------+  |
|  |   Windows 11 Victim   |   TCP/1514   |     Wazuh Server     |  |
|  |     10.10.20.20       | -----------> |     10.10.20.10      |  |
|  | (Wazuh Agent + Sysmon)|  (Encrypted) | (Manager + Indexer + |  |
|  +-----------------------+              |      Dashboard)      |  |
|              ^                          +----------------------+  |
|              | Simulated Attacks                   ^              |
|              | (Brute Force / Execution)           | HTTPS/443    |
|  +-----------------------+                         | (Analyst UI) |
|  |   Kali Linux (Opt.)   |              +----------------------+  |
|  |     10.10.20.30       |              |   Host Workstation   |  |
|  +-----------------------+              +----------------------+  |
+-------------------------------------------------------------------+

```

* **Telemetry Flow:** The Windows endpoint forwards Windows Security logs, FIM alerts, and `Microsoft-Windows-Sysmon/Operational` events over encrypted **TCP port 1514** to the Wazuh Manager.
* **Agent Enrollment:** Initial agent registration occurs via **TCP port 1515**.
* **Analyst Access:** The Wazuh Web Dashboard is accessed from the host browser via **HTTPS (TCP port 443)**.

## Prerequisites

* **Hardware and virtualization requirements:** Host machine with at least 16 GB RAM and 4+ CPU cores running VirtualBox 7.x or VMware Workstation Pro; minimum 120 GB free disk space.
* **Operating systems and versions:** Ubuntu Server 22.04 LTS (64-bit) for the Wazuh Server; Windows 10/11 (64-bit) for monitored endpoints.
* **Network connectivity and DNS requirements:** Static IP reservations configured on the isolated virtual network adapter (`10.10.20.0/24`), with outbound internet access temporarily enabled during initial package installation and threat intelligence feed updates.

## Success criteria

* **Server Health:** `wazuh-manager`, `wazuh-indexer`, and `wazuh-dashboard` services report `active (running)` with zero startup errors, and the web UI is reachable at `https://10.10.20.10`.
* **Agent Connectivity:** Endpoint 1 appears as `Active` in the Wazuh Dashboard under **Agents** with successful heartbeat keepalives.
* **Telemetry Pipeline Verification:** Executing a test command (e.g., `whoami` or `powershell.exe`) on Endpoint 1 generates a Sysmon Event ID 1 log that is ingested, parsed, and searchable in the Wazuh Threat Hunting / Discover view within 10 seconds.

```

---

**Quick Tip on Your `docs/` Folder:**
Looking at your file tree on the left, you also have duplicate pairs in `docs/` (for example, `01-introduction.md` and `01-project-overview.md`, `02-wazuh-101.md` and `02-wazuh-installation.md`, etc.). As you fill each one out, you can delete the duplicate templates so your table of contents stays clean (`01` through `09`).

```
