<h1>Adversary Simulation & SOC Detection Lab </h1>

<h2>Description </h2>
This lab demonstrates a hands-on adversary simulation and security operations center (SOC) detection workflow using VMware virtual machines, Windows 10, Ubuntu Linux, Sysmon, LimaCharlie, and Sliver C2 framework. It covers setting up attacker and victim VMs, disabling Windows Defender protections, deploying monitoring tools, executing a Command & Control (C2) payload, and crafting detection & response rules in LimaCharlie.
<br />


<h2>Languages & Utilities Used </h2>

- <b>PowerShell (Windows)</b>
- <b>Bash (Linux)</b>
- <b>Sysmon (System Monitor)</b>
- <b>LimaCharlie Agent</b>
- <b>Silver C2 Framework</b>

<h2>Environments Used </h2>

- <b>Windows 10 Home x64 ("Victim" VM)</b>
- <b>Ubuntu 22.04.5 LTS Server ("Adversary" VM)</b>
- <b>VMware Workstation Pro 17 (Virtualization platform)</b>

<h2>Lab Walkthrough </h2>

### 1. Setting up VMs and Initial Configuration:

- Installed VMware Workstation Pro 17.
- Downloaded and installed Ubuntu 22.04.5 LTS Server ISO as “User1” (Attacker VM). Booted in terminal mode.
- Created Windows 10 Home x64 VM using Media Creation Tool and named it “User2” (Victim VM).
- Disabled Windows Defender protections on the Victim VM by:
  - Disabling Real-time Protection, Cloud-delivered Protection, Automatic Sample Submission, and Tamper Protection in Windows Security settings.
  - Editing registry keys (Start DWORD values set to 4) for WinDefend and related services.
  - Running PowerShell commands as Administrator to disable Windows Defender tasks.
  - Restarted the Windows VM to apply changes.

##### Windows Security settings showing Defender disabled on PowerShell:
![Windows Security settings showing Defender disabled on PowerShell 1](https://i.imgur.com/wJ9IN8c.png)
![Windows Security settings showing Defender disabled on PowerShell 2](https://i.imgur.com/iZtQbON.png)

##### Registry editor with modified key of WinDefend:
![Registry editor with modified key of WinDefend](https://i.imgur.com/giey59e.png)

---

### 2. Installing Sysmon on Windows VM:

- Used PowerShell with administrative privileges to install, unzip, and verify Sysmon.
- [Brief explanation on why Sysmon is used: for advanced system monitoring and logging of suspicious activities.]

##### PowerShell command executing Sysmon:
![PowerShell command executing Sysmon](https://i.imgur.com/OBqo3OZ.png)

---

### 3. Installing LimaCharlie Agent on Windows VM:

- Logged into LimaCharlie.io portal.
- Created a new organization and installation key named “Windows VM Lab”.
- Downloaded and installed LimaCharlie sensor on Windows VM using the provided executable.
- Created Artifact Collection Rules in LimaCharlie to capture relevant data.

##### LimaCharlie agent installation confirmation in Command Prompt (cmd):
![LimaCharlie agent installation confirmation in Command Prompt](https://i.imgur.com/7TUZ3JE.png)

##### Configuring Sysmon with the Artifact Collection Rule:
![Configuring Sysmon with the Artifact Collection Rule](https://i.imgur.com/r6M2a2t.png)

---

### 4. Configuring Network on Ubuntu VM:

- Logged in to Ubuntu VM using `sudo su`.
- Ran `ip a` to retrieve Ubuntu VM IP address and saved it.
- Edited netplan config (`/etc/netplan/00-installer-config.yaml`) to configure static IP based on DHCP assignment.
- Applied configuration with `sudo netplan try` and `sudo netplan apply`.
- Tested connectivity by pinging Google DNS.
- Established SSH session to Ubuntu VM via IP address.

**Image Placeholder:**  
##### N/A:
![N/A](N/A)

---

### 5. Installing and Launching Sliver C2 on Ubuntu VM:

- Downloaded Sliver server binary from GitHub using `wget`.
- Set executable permissions and installed required dependencies (`mingw-w64`).
- Created working directory `/opt/sliver`.
- Launched Sliver server.
- Generated C2 payload with Ubuntu VM IP address; noted payload name.

**Image Placeholder:**  
##### N/A:
![N/A](N/A)

---

### 6. Delivering and Executing Payload on Windows VM:

- Started a temporary Python web server on Ubuntu VM to host payload.
- Downloaded payload from Ubuntu VM to Windows VM via browser.
- Took a snapshot of Windows VM before running the payload.
- Terminated Python web server.
- Relaunched Sliver server on Ubuntu VM.
- Executed payload on Windows VM and observed new C2 session in Sliver.

**Image Placeholder:**  
##### N/A:
![N/A](N/A)

---

### 7. Interaction with C2 Session in Sliver:

- Used Sliver shell commands (`info`, `whoami`, `getprivs`, `pwd`, `netstat`) to gather information about the compromised session.
- Observed Sliver highlights implant process in green for easy identification.

**Image Placeholder:**  
##### N/A:
![N/A](N/A)

---

### 8. Monitoring Activity in LimaCharlie:

- Viewed process details and network connections related to the implant in LimaCharlie.
- Checked file system tab for implant directory.
- Used VirusTotal scan on the implant executable hash; no matches found (expected for freshly generated payload).
- Explored timeline tab for recent event logs involving the implant.

**Image Placeholder:**  
##### N/A:
![N/A](N/A)

---

### 9. Red Team Exercise: Credential Dumping and Detection:

- Used Sliver shell to dump `lsass.exe` process memory from Windows VM to simulate credential theft.
- Observed detection event logged in LimaCharlie timeline under `SENSITIVE_PROCESS_ACCESS`.
- Created a Detection & Response (D&R) rule in LimaCharlie named “LSASS Access” with specific detect and respond commands.
- Tested detection rule by triggering test events and confirming “Match” notifications.
- Executed procdump command in Sliver to validate detection success.
- Confirmed detection alert appeared in LimaCharlie under “LSASS Access”.

**Image Placeholder:**  
##### N/A:
![N/A](N/A)

---

## Summary

This lab provided hands-on experience in setting up a virtual SOC environment, performing adversary simulation with a C2 framework, and crafting detection and response rules. It demonstrates proficiency in Windows and Linux system configuration, endpoint monitoring deployment, red team operations, and SOC alerting workflows using modern cybersecurity tools.

## Credit & References

- Microsoft Sysmon: https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon  
- MITRE ATT&CK - Credential Dumping via LSASS: https://attack.mitre.org/techniques/T1003/001/  
- BishopFox Sliver Framework: https://github.com/BishopFox/sliver  
- LimaCharlie: https://limacharlie.io/
- *TBA*
