# Lab 1C — Member 1 Screenshot Checklist

## Windows Server 2022 Verification

Use this file to organize and label all screenshots for **Member 1** of Lab 1C.

The lab requires Member 1 to verify the Windows Server 2022 guest from Lab 1B before the other members proceed.

---

## Screenshot 01 — Windows Server Hostname

**Purpose:** Verify that the Windows Server hostname is correct.

**Command used:**

```cmd
hostname
```

**Expected result:**

```text
WINSRV-DORSU-[GroupID]
```

**Screenshot:**

![1790498750581](image/L1p3/1790498750581.png)

---

## Screenshot 02 — Windows Server IP Configuration

**Purpose:** Verify the static IP configuration of the Windows Server.

**Command used:**

```cmd
ipconfig /all
```

**Required values:**

| Setting         | Expected Value    |
| --------------- | ----------------- |
| IPv4 Address    | `192.168.10.11` |
| Subnet Mask     | `255.255.255.0` |
| Default Gateway | `192.168.10.1`  |
| Adapter         | Bridged           |

**Screenshot:**

![1790498770261](image/L1p3/1790498770261.png)

---

## Screenshot 03 — Remote Desktop

**Purpose:** Verify that Remote Desktop is enabled.

**Location:**

```text
Server Manager → Local Server → Remote Desktop
```

**Expected result:**

```text
Enabled
```

**Screenshot:**

![1790498992112](image/L1p3/1790498992112.png)

---

## Screenshot 04 — RSAT

**Purpose:** Verify that RSAT is installed/present.

**Server Manager verification:**

```text
Server Manager → Tools
```

**Alternative verification:**

```powershell
Get-WindowsFeature *RSAT*
```

If RSAT is installed, the relevant feature should show:

```text
Install State : Installed
```

**Screenshot:**

![1790500077381](image/L1p3/1790500077381.png) ![1790500145777](image/L1p3/1790500145777.png)

**Note:** If RSAT is installed but does not appear under the Server Manager Tools menu, keep the PowerShell verification screenshot as evidence and check the specific RSAT feature installed.

---

## Screenshot 05 — Windows Server VM Firewall ICMPv4 Rule

**Purpose:** Verify that Windows allows inbound ICMPv4 Echo Requests.

**Open:**

```text
Win + R → wf.msc
```

Then:

```text
Inbound Rules
→ File and Printer Sharing (Echo Request - ICMPv4-In)
```

**Expected result:**

```text
Enabled
```

**Screenshot:**

![1790500381398](image/L1p3/1790500381398.png)

---

# Member 2 — Ubuntu Server Installer

### Screenshot 06 — VirtualBox VM Settings

**Purpose:** Show the configuration of the Ubuntu Server guest.

**Expected settings:**

| Setting   | Required Value                  |
| --------- | ------------------------------- |
| VM Name   | `UBSRV-DORSU-[GroupID]`       |
| Type      | Linux                           |
| Version   | Ubuntu (64-bit)                 |
| RAM       | 2048 MB                         |
| CPU       | 2                               |
| Disk      | 30 GB VDI                       |
| Disk Type | Dynamic                         |
| Adapter 1 | Bridged Adapter                 |
| Interface | Host's wired Ethernet interface |

**Screenshot:**

![1790514248413](image/L1p3/1790514248413.png)

---

### Screenshot 07 — VirtualBox Network Adapter

**Purpose:** Show that Adapter 1 is configured as Bridged.

**Expected:**

```text
Adapter 1
Attached to: Bridged Adapter
Name: [Host's wired Ethernet interface]
```

**Screenshot:**

 ![1790514269607](image/L1p3/1790514269607.png)

---

### Screenshot 08 — Ubuntu Server Installation Login

**Purpose:** Show that Ubuntu Server 24.04 LTS was successfully installed.

**Expected:**

```text
Ubuntu Server 24.04 LTS
Username: sysadmin
Server name: ubsrv-dorsu-[GroupID]
```

**Screenshot:**

 ![1790514914917](image/L1p3/1790514914917.png) ![1790515068496](image/L1p3/1790515068496.png) ![1790515122420](image/L1p3/1790515122420.png)

---

### Screenshot 09 — System Update

**Purpose:** Show that the Ubuntu Server packages were updated.

**Command:**

```bash
sudo apt update && sudo apt full-upgrade -y
```

**Screenshot:**

![1790515244688](image/L1p3/1790515244688.png)

---

### Screenshot 10 — Required Packages Installed

**Purpose:** Show that the required packages were installed.

**Command:**

```bash
sudo apt install net-tools curl htop tree -y
```

**Screenshot:**

![1790515291574](image/L1p3/1790515291574.png)

---

### Screenshot 11 — SSH Socket Status

**Purpose:** Verify that OpenSSH is listening through `ssh.socket`.

**Commands:**

```bash
systemctl status ssh.socket
```

and:

```bash
systemctl is-enabled ssh.socket
```

**Expected:**

```text
active (listening)
```

and:

```text
enabled
```

**Screenshot:**

 ![1790515549013](image/L1p3/1790515549013.png)

---

### Screenshot 12 — Ubuntu Server VM Hostname and Timezone

**Purpose:** Verify the Ubuntu hostname and timezone.

**Commands:**

```bash
hostnamectl
```

and:

```bash
timedatectl
```

**Expected hostname:**

```text
ubsrv-dorsu-[GroupID]
```

**Expected timezone:**

```text
Asia/Manila
```

**Screenshot:**

![1790515735996](image/L1p3/1790515735996.png)

---

# Member 3 — Network Configurator

### Screenshot 13 — Ubuntu Server VM Network Interface

**Purpose:** Identify the actual bridged network interface before editing Netplan.

**Command:**

```bash
ip a
```

**Expected:**

Identify the interface connected to the bridged adapter.

Example:

```text
enp0s3
```

**Important:** Do not assume the interface is `enp0s3`.

**Screenshot:**

> Insert Screenshot 13 here. (WALA NA SCREENSHOT 💀 - NOT REQUIRED SIGURO HUEHUE)

---

### Screenshot 14 — Netplan Configuration File

**Purpose:** Show the static network configuration.

**Expected configuration should contain:**

```yaml
dhcp4: false
addresses:
  - 192.168.10.12/24
routes:
  - to: default
    via: 192.168.10.1
nameservers:
  addresses:
    - [DNS SERVER]
```

**Screenshot:**

![1790518444060](image/L1p3/1790518444060.png)

---

### Screenshot 15 — Netplan File Permissions

**Purpose:** Verify that the Netplan configuration file has the required permissions.

**Command:**

```bash
ls -l /etc/netplan/
```

The Netplan configuration file should have permissions equivalent to:

```text
-rw-------
```

which corresponds to:

```bash
chmod 600
```

**Screenshot:**

![1790518497168](image/L1p3/1790518497168.png)

---

### Screenshot 16 — Netplan Applied Successfully

**Purpose:** Show that the Netplan configuration was applied without warnings.

**Command:**

```bash
sudo netplan try
```

**Screenshot:**

![1790518583325](image/L1p3/1790518583325.png)

---

### Screenshot 17 — IP Address Verification

**Purpose:** Verify the Ubuntu Server VM static IP.

**Command:**

```bash
ip a
```

**Expected:**

```text
192.168.10.12/24
```

**Screenshot:**

![1790518601515](image/L1p3/1790518601515.png)

---

### Screenshot 18 — Routing Table

**Purpose:** Verify that Ubuntu Server VM has exactly one default route.

**Command:**

```bash
ip route
```

**Expected:**

```text
default via 192.168.10.1
```

There should be exactly **one** default route.

**Screenshot:**

![1790518633066](image/L1p3/1790518633066.png)

---

### Screenshot 19 — DNS Configuration

**Purpose:** Verify that the DNS server is configured.

**Command:**

```bash
resolvectl status
```

**Expected:**

The configured DNS server should be listed.

**Screenshot:**

![1790518667964](image/L1p3/1790518667964.png)

---

### Screenshot 20 — Ubuntu Server VM to Windows Server VM Ping

**Purpose:** Verify connectivity from Ubuntu Server to Windows Server.

**Command:**

```bash
ping 192.168.10.11 -c 4
```

**Expected:**

Four successful replies.

Example:

```text
64 bytes from 192.168.10.11
```

**Screenshot:**

![1790520183898](image/L1p3/1790520183898.png)

---

### Screenshot 21 — Windows Server VM to Ubuntu Server VM Ping

**Purpose:** Verify connectivity from Windows Server to Ubuntu Server.

**Command:**

```cmd
ping 192.168.10.12 -n 4
```

**Expected:**

Four successful replies.

**Screenshot:**

![1790520217685](image/L1p3/1790520217685.png)

---

### Screenshot 22 — SSH from Host to Ubuntu Server VM

**Purpose:** Verify SSH access to Ubuntu from the host computer.

**Command:**

```bash
ssh sysadmin@192.168.10.12
```

**Expected:**

Successful login to Ubuntu Server and a shell prompt.

**Screenshot:**

![1790522897180](image/L1p3/1790522897180.png)

---

### Screenshot 23 — SSH from Windows Server VM to Ubuntu Server VM

**Purpose:** Verify SSH access to Ubuntu from Windows Server.

**Command:**

```cmd
ssh sysadmin@192.168.10.12
```

Accept the fingerprint if prompted.

Then run:

```bash
hostname && uptime
```

**Expected:**

The hostname should show:

```text
ubsrv-dorsu-[GroupID]
```

followed by the system uptime.

**Screenshot:**

![1790520706323](image/L1p3/1790520706323.png)

**Status:** ☐ PASS ☐ NEEDS FIX

# Final Screenshot Checklist

## Member 1

- [ ] Screenshot 01 — Hostname
- [ ] Screenshot 02 — `ipconfig /all`
- [ ] Screenshot 03 — Remote Desktop
- [ ] Screenshot 04 — RSAT
- [ ] Screenshot 05 — ICMPv4 Firewall Rule

## Member 2

* [ ] Screenshot 06 — VirtualBox VM settings
* [ ] Screenshot 07 — Bridged network adapter
* [ ] Screenshot 08 — Ubuntu installation/login
* [ ] Screenshot 09 — `apt update` and `full-upgrade`
* [ ] Screenshot 10 — Required packages
* [ ] Screenshot 11 — `ssh.socket`
* [ ] Screenshot 12 — Hostname and timezone

## Member 3

* [ ] Screenshot 13 — `ip a`
* [ ] Screenshot 14 — Netplan configuration
* [ ] Screenshot 15 — Netplan permissions
* [ ] Screenshot 16 — `netplan try`
* [ ] Screenshot 17 — Ubuntu IP
* [ ] Screenshot 18 — `ip route`
* [ ] Screenshot 19 — `resolvectl status`
* [ ] Screenshot 20 — Ubuntu → Windows ping
* [ ] Screenshot 21 — Windows → Ubuntu ping
* [ ] Screenshot 22 — Host → Ubuntu SSH
* [ ] Screenshot 23 — Windows → Ubuntu SSH

## Member 4

* [ ] Screenshot 24 — Installation Verification Checklist
* [ ] Screenshot 25 — OS Comparison Table
* [ ] Screenshot 26 — Network Diagram
* [ ] Screenshot 27 — Final Group Documentation
