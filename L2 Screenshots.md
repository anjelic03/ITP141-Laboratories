# ITP 141 - Lab 2: OS Maintenance, Updates, Service Management, and the Four-Step SOP

## Screenshot & Evidence Repository

This markdown file serves as the centralized repository for all required screenshots and visual evidence for **Lab 2**. Group members should insert their corresponding screenshots under each designated section below.

---

### Part 1: OS Patching Workflow

#### 1. Windows Server 2022 (`sconfig` & `Get-HotFix`)

* **Description:** Screenshots showing the `sconfig` quality update list before installation, during/after installation, and the verified hotfix list via PowerShell (`Get-HotFix | Sort-Object InstalledOn`).

> **[Insert Windows Update Before/After & Get-HotFix Screenshots Here]**
> ![1791436519133](image/L2/1791436519133.png)   ![1791436769099](image/L2/1791436769099.png) ![1791438722917](image/L2/1791438722917.png) ![1791438922451](image/L2/1791438922451.png)  ![1791439520378](image/L2/1791439520378.png) ![1791439238842](image/L2/1791439238842.png)

---

#### 2. Ubuntu Server 24.04 LTS (`apt` & `unattended-upgrades`)

* **Description:** Screenshots showing `sudo apt update`, exact upgradable count using `apt list-upgradable 2>/dev/null | tail -n +2 | wc -l`, successful upgrade execution, `unattended-upgrades` dry-run output (`sudo unattended-upgrade --dry-run --debug`), and active timers (`systemctl list-timers 'apt-daily*'`).

> **[Insert Ubuntu Patching & Unattended-Upgrades Screenshots Here]**
> ![1791450574586](image/L2/1791450574586.png)
> ![1791450852195](image/L2/1791450852195.png)
> ![1791450914808](image/L2/1791450914808.png)
> ![1791452886587](image/L2/1791452886587.png)
> ![1791451963002](image/L2/1791451963002.png)
> ![1791453273400](image/L2/1791453273400.png)
> ![1791453680742](image/L2/1791453680742.png)
> ![1791453773623](image/L2/1791453773623.png)
> ![1791454219844](image/L2/1791454219844.png)
> ![1791454935411](image/L2/1791454935411.png)
> ![1791454945046](image/L2/1791454945046.png)
> ![1791455202969](image/L2/1791455202969.png)
> ![1791455644728](image/L2/1791455644728.png)

### Part 2: Service Management & Failure Recovery

#### 3. Windows Service Management (`W32Time` / Spooler)

* **Description:** Screenshots showing initial service status query, stopping the service (`net stop`), verifying stopped status (`sc query`), starting the service (`net start`), and verifying running status.

> ## **First ten services reading RUNNING:**
>
> * ![1791440962422](image/L2/1791440962422.png)
> * ![1791440990131](image/L2/1791440990131.png)
> * ![1791441043618](image/L2/1791441043618.png)
> * ![1791441058141](image/L2/1791441058141.png)
> * ![1791441074734](image/L2/1791441074734.png)
> * ![1791441097264](image/L2/1791441097264.png)
> * ![1791441113848](image/L2/1791441113848.png)

> ## **States and timestamps**
>
> ![1791445458516](image/L2/1791445458516.png) ![1791445579957](image/L2/1791445579957.png)
> ![1791445617155](image/L2/1791445617155.png)
> ![1791445638325](image/L2/1791445638325.png)
> ![1791445666440](image/L2/1791445666440.png)
> ![1791445693548](image/L2/1791445693548.png)

---

#### 4. Ubuntu Service Management & Masking (`cron`)

* **Description:** Screenshots showing `cron` stop/start/enable transitions, demonstrating that a masked service cannot be started (`Unit cron.service is masked`), and finally unmasking and restarting successfully.

> **Ubuntu cron Service**
> ![1791455880433](image/L2/1791455880433.png)
> ![1791456528680](image/L2/1791456528680.png)
> ![1791456623097](image/L2/1791456623097.png)
> **Disable vs Mask**
> ![1791457213568](image/L2/1791457213568.png)
> ![1791457277692](image/L2/1791457277692.png)

---

#### 5. Ubuntu SSH Socket/Service Failure Simulation & Recovery

* **Description:** Screenshots showing the shutdown of `ssh.socket` and `ssh.service`, the connection refused error tested from the Windows guest (`ssh sysadmin@192.168.10.12`), and successful recovery after restarting the socket.

> **[Insert SSH Outage and Recovery Screenshots Here]**
> ![1791459547710](image/L2/1791459547710.png)
> **Recovery Time = Recovery Time - Failure Time
> Recovery Time = 16.33s - 13.82s = 2.51s**

---

### Part 3: Log Analysis

#### 6. Windows Event Viewer (System Logs)

* **Description:** Screenshots showing three distinct Event Viewer entries filtered for Error and Warning levels over the past 24 hours / 7 days (including Event ID, Source, Time, and Description).

> **[Insert Windows Event Viewer Screenshots Here]**
> ![1791459824295](image/L2/1791459824295.png)
> ![1791459857408](image/L2/1791459857408.png)
> ![1791459872794](image/L2/1791459872794.png)

---

#### 7. Ubuntu Log Analysis (`journalctl`)

* **Description:** Screenshots showing `journalctl -p err -n 20`, errors filtered by time and grep, and systemctl failed units.

> **[Insert journalctl Error Logs Screenshots Here]**
> ![1791460413692](image/L2/1791460413692.png)
> ![1791460505778](image/L2/1791460505778.png)
> ![1791460556791](image/L2/1791460556791.png)
