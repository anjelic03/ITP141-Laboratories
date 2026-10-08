# ITP 141 - Lab 2: OS Maintenance, Updates, Service Management, and the Four-Step SOP

## Screenshot & Evidence Repository

This markdown file serves as the centralized repository for all required screenshots and visual evidence for **Lab 2**. Group members should insert their corresponding screenshots under each designated section below.

---

### Evidence Recording Checklist

Before final submission, verify that each screenshot is readable and directly supports the stated task. For every activity, record the command used, the observed result, and any required timestamp/state transition. Do not replace actual VM results with sample values.

### Part 1: OS Patching Workflow

#### 1. Windows Server 2022 (`sconfig` & `Get-HotFix`)

* **Description:** Screenshots showing the complete Windows patching process.
* **Commands/Actions:** Open PowerShell as Administrator → run `sconfig` → select **6. Install updates** → select **1. All quality updates** → allow installation → reboot if prompted → run `Get-HotFix | Sort-Object InstalledOn`.
* **Evidence to record:** Number of available updates, KB numbers, installation result, reboot-required status, and final installed KB list.
* **Purpose:** Proves that Windows Server 2022 was patched through the required `sconfig` workflow and that the installed updates were independently verified with PowerShell.

> **[Insert Windows Update Before/After & Get-HotFix Screenshots Here]**
> ![1791436519133](image/L2/1791436519133.png)   ![1791436769099](image/L2/1791436769099.png) ![1791438722917](image/L2/1791438722917.png) ![1791438922451](image/L2/1791438922451.png)  ![1791439520378](image/L2/1791439520378.png) ![1791439238842](image/L2/1791439238842.png)

---

#### 2. Ubuntu Server 24.04 LTS (`apt` & `unattended-upgrades`)

* **Description:** Screenshots documenting Ubuntu patching and the controlled `unattended-upgrades` configuration.
* **Commands/Actions:** Run `sudo apt update`; count packages with `apt list --upgradable 2>/dev/null | tail -n +2 | wc -l`; run `sudo apt full-upgrade -y`; run `sudo apt autoremove -y`; check `/var/run/reboot-required`; install/configure `unattended-upgrades`; verify `/etc/apt/apt.conf.d/50unattended-upgrades`; run `sudo unattended-upgrade --dry-run --debug`; check `systemctl list-timers 'apt-daily*'`.
* **Evidence to record:** Exact upgradable count, packages upgraded, kernel changed (Y/N), reboot-required status, allowed origins, dry-run output, and timer schedule.
* **Purpose:** Proves that Ubuntu was patched and that automatic security updating was configured and tested without installing packages during the dry run.

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

* **Description:** Screenshots showing the Windows service state transition and recovery.
* **Commands/Actions:** Run `sc query state= all`; check `sc query W32Time`; if W32Time is not running, start it with `net start W32Time`; stop with `net stop W32Time`; verify with `sc query W32Time`; start again with `net start W32Time`; verify that it is RUNNING.
* **Evidence to record:** Initial state, stop timestamp, STOPPED state, start timestamp, RUNNING state, and recovery time.
* **Fallback:** If W32Time cannot be started on the build, use `Spooler` and document the substitution.
* **Purpose:** Demonstrates controlled service failure and recovery on Windows Server 2022.

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

* **Description:** Screenshots showing the complete Ubuntu `cron` service lifecycle, including the difference between disabling and masking.
* **Commands/Actions:** Run `systemctl list-units --type=service --state=running | head -15`; stop with `sudo systemctl stop cron`; verify with `systemctl status cron`; start and enable with `sudo systemctl start cron` and `sudo systemctl enable cron`; stop again; mask with `sudo systemctl mask cron`; attempt `sudo systemctl start cron` and capture the `Unit cron.service is masked` failure; unmask with `sudo systemctl unmask cron`; start and verify the service.
* **Evidence to record:** Initial state, inactive state, enabled/running state, masked state, failed start, unmasked state, and final running state.
* **Purpose:** Demonstrates that **disable** prevents automatic startup while **mask** prevents the unit from being started until it is unmasked.

> **Ubuntu cron Service**
> ![1791455880433](image/L2/1791455880433.png)
> ![1791456528680](image/L2/1791456528680.png)
> ![1791456623097](image/L2/1791456623097.png)
> **Disable vs Mask**
> ![1791457213568](image/L2/1791457213568.png)
> ![1791457277692](image/L2/1791457277692.png)

---

#### 5. Ubuntu SSH Socket/Service Failure Simulation & Recovery

* **Description:** Screenshots documenting an SSH outage and successful recovery.
* **Important:** Perform the shutdown from the **Ubuntu VirtualBox console**, not from an SSH session.
* **Commands/Actions:** Record the start time → run `sudo systemctl stop ssh.socket ssh.service` → from Windows run `ssh sysadmin@192.168.10.12` and capture **Connection refused** → record failure time → run `sudo systemctl start ssh.socket` → connect again from Windows → record recovery time.
* **Evidence to record:** Outage start time, failed connection, failure time, recovery time, successful connection, and elapsed recovery seconds.
* **Purpose:** Proves that the group can simulate, identify, and recover an SSH service outage on Ubuntu 24.04.

> **[Insert SSH Outage and Recovery Screenshots Here]**
> ![1791459547710](image/L2/1791459547710.png)
> **Recovery Time = Recovery Time - Failure Time
> Recovery Time = 16.33s - 13.82s = 2.51s**

---

### Part 3: Log Analysis

#### 6. Windows Event Viewer (System Logs)

* **Description:** Three distinct Windows Event Viewer entries from **Windows Logs → System**, filtered to **Error and Warning** levels for the last 24 hours; widen to 7 days only if fewer than three events are available.
* **Evidence to record for each event:** Event ID, Source, Time, Level, brief description, and recommended action.
* **Purpose:** Identifies actual Windows system issues that may require investigation or corrective action.
* **Important:** Use the actual events from the VM; do not invent Event IDs or descriptions.

> **[Insert Windows Event Viewer Screenshots Here]**
> ![1791459824295](image/L2/1791459824295.png)
> ![1791459857408](image/L2/1791459857408.png)
> ![1791459872794](image/L2/1791459872794.png)

---

#### 7. Ubuntu Log Analysis (`journalctl`)

* **Description:** Screenshots documenting Ubuntu errors and failed units.
* **Commands/Actions:** Run `sudo journalctl -p err -n 20`; run `sudo journalctl --since '1 hour ago' | grep -i 'err\|fail' | head -10`; run `systemctl --failed`.
* **Evidence to record:** Three relevant journal entries, their unit/service, error level, brief description, and recommended action. Also note any failed units reported by `systemctl --failed`.
* **Purpose:** Provides evidence for the Ubuntu log-analysis requirement and identifies failures that may need remediation.
* **Important:** Use the actual journal output from the VM; do not fabricate errors or service names.

> **[Insert journalctl Error Logs Screenshots Here]**
> ![1791460413692](image/L2/1791460413692.png)
> ![1791460505778](image/L2/1791460505778.png)
> ![1791460556791](image/L2/1791460556791.png)
