# Example Water Treatment Plant incident

ATT&CK data: Enterprise 2026-10-13, ICS 2026-10-13. Mapping method: phrase rules and text similarity, analyst reviewed.

## Attack steps

1. A plant engineer received a spearphishing email with a malicious Excel attachment and enabled macros, which launched an encoded PowerShell command on his corporate laptop.
   - **T0865 Spearphishing Attachment** (ICS, Initial Access, high confidence): phrase: “spearphish”, “malicious Excel”; text similarity 32.3
   - **T1566.001 Phishing: Spearphishing Attachment** (Enterprise, Initial Access, high confidence): phrase: “malicious Excel”; text similarity 45.2
   - **T1204.002 User Execution: Malicious File** (Enterprise, Execution, medium confidence): phrase: “macros”
   - **T1059.005 Command and Scripting Interpreter: Visual Basic** (Enterprise, Execution, medium confidence): phrase: “macros”
   - **T0863 User Execution** (ICS, Execution, medium confidence): phrase: “macros”
   - **T1059.001 Command and Scripting Interpreter: PowerShell** (Enterprise, Execution, medium confidence): phrase: “PowerShell”
2. The attacker dumped credentials from LSASS with Mimikatz and found a stored vendor VPN password.
   - **T1003.001 OS Credential Dumping: LSASS Memory** (Enterprise, Credential Access, high confidence): phrase: “dumped credential”; text similarity 26.6; used by a named threat in this incident
   - **T1552.001 Unsecured Credentials: Credentials In Files** (Enterprise, Credential Access, high confidence): phrase: “stored vendor VPN password”; text similarity 18.6
   - **T1555 Credentials from Password Stores** (Enterprise, Credential Access, high confidence): phrase: “stored vendor VPN password”; used by a named threat in this incident
3. Using the stolen VPN credentials, they logged in remotely and connected to the HMI over TightVNC, which had no MFA.
   - **T0822 External Remote Services** (ICS, Initial Access, high confidence): phrase: “VPN credentials, they logged in remotely”, “VNC”
   - **T1133 External Remote Services** (Enterprise, Persistence/Initial Access, medium confidence): phrase: “VPN credentials, they logged in remotely”
   - **T1078 Valid Accounts** (Enterprise, Stealth/Persistence/Privilege Escalation/Initial Access, medium confidence): phrase: “stolen VPN credentials”
   - **T0859 Valid Accounts** (ICS, Persistence/Lateral Movement, medium confidence): phrase: “stolen VPN credentials”
   - **T1219 Remote Access Tools** (Enterprise, Command and Control, medium confidence): phrase: “VNC”
   - **T0886 Remote Services** (ICS, Initial Access/Lateral Movement, medium confidence): phrase: “VNC”
4. From the HMI they scanned the OT network and identified the Modbus gateway, the ControlLogix PLC and the DNP3 RTU.
   - **T0846 Remote System Discovery** (ICS, Discovery, high confidence): phrase: “scanned the OT network”, “identified the Modbus gateway”
   - **T1046 Network Service Discovery** (Enterprise, Discovery, medium confidence): phrase: “scanned the OT network”
   - **T0846.001 Remote System Discovery: Port Scan** (ICS, Discovery, medium confidence): phrase: “scanned the OT network”
   - **T1018 Remote System Discovery** (Enterprise, Discovery, medium confidence): phrase: “identified the Modbus gateway”
5. They logged in to the Modbus gateway with its default password over telnet.
   - **T1694.001 Insecure Credentials: Default Credentials** (ICS, Persistence/Lateral Movement, high confidence): phrase: “default password”; text similarity 9.3
   - **T1078.001 Valid Accounts: Default Accounts** (Enterprise, Stealth/Persistence/Privilege Escalation/Initial Access, medium confidence): phrase: “default password”
   - **T1021 Remote Services** (Enterprise, Lateral Movement, medium confidence): phrase: “telnet”
   - **T0886 Remote Services** (ICS, Initial Access/Lateral Movement, medium confidence): phrase: “telnet”
6. At 02:13 on Saturday they sent unauthorized Modbus write commands that changed the chlorine pump setpoints and suppressed the low-chlorine alarms.
   - **T1692.001 Unauthorized Message: Command Message** (ICS, Evasion/Impair Process Control, high confidence): phrase: “unauthorized Modbus”; text similarity 14.3
   - **T0831 Manipulation of Control** (ICS, Impact, high confidence): phrase: “unauthorized Modbus”; text similarity 11.5
   - **T0878 Alarm Suppression** (ICS, Inhibit Response Function, high confidence): phrase: “suppressed the low-chlorine alarms”; text similarity 9.8
   - **T0836 Modify Parameter** (ICS, Impair Process Control, medium confidence): phrase: “changed the chlorine pump setpoints”
7. They also sent a DNP3 cold restart to the RTU at the north booster station.
   - **T0816 Device Restart/Shutdown** (ICS, Inhibit Response Function, high confidence): phrase: “restart to the RTU”, “cold restart”; text similarity 8.6
   - **T1529 System Shutdown/Reboot** (Enterprise, Impact, medium confidence): phrase: “restart to the RTU”
8. Operators could not see the correct chlorine readings for about forty minutes and had to switch to manual control.
   - **T0829 Loss of View** (ICS, Impact, high confidence): phrase: “Operators could not see”; text similarity 11.6
   - **T0815 Denial of View** (ICS, Impact, medium confidence): phrase: “Operators could not see”
9. The attacker deleted Windows event logs on the HMI before disconnecting.
   - **T1685.005 Disable or Modify Tools: Clear Windows Event Logs** (Enterprise, Defense Impairment, high confidence): phrase: “deleted Windows event logs”; text similarity 52.7
   - **T1070 Indicator Removal** (Enterprise, Stealth, medium confidence): phrase: “deleted Windows event logs”
   - **T0872 Indicator Removal on Host** (ICS, Evasion, medium confidence): phrase: “deleted Windows event logs”
   - **T1685.001 Disable or Modify Tools: Disable or Modify Windows Event Log** (Enterprise, Defense Impairment, medium confidence): text similarity 35.2

## By tactic

### Enterprise

| Tactic | Techniques |
|---|---|
| Reconnaissance | not observed |
| Resource Development | not observed |
| Initial Access | T1566.001 Phishing: Spearphishing Attachment; T1133 External Remote Services; T1078 Valid Accounts; T1078.001 Valid Accounts: Default Accounts |
| Execution | T1204.002 User Execution: Malicious File; T1059.005 Command and Scripting Interpreter: Visual Basic; T1059.001 Command and Scripting Interpreter: PowerShell |
| Persistence | T1133 External Remote Services; T1078 Valid Accounts; T1078.001 Valid Accounts: Default Accounts |
| Privilege Escalation | T1078 Valid Accounts; T1078.001 Valid Accounts: Default Accounts |
| Stealth | T1078 Valid Accounts; T1078.001 Valid Accounts: Default Accounts; T1070 Indicator Removal |
| Defense Impairment | T1685.005 Disable or Modify Tools: Clear Windows Event Logs; T1685.001 Disable or Modify Tools: Disable or Modify Windows Event Log |
| Credential Access | T1003.001 OS Credential Dumping: LSASS Memory; T1552.001 Unsecured Credentials: Credentials In Files; T1555 Credentials from Password Stores |
| Discovery | T1046 Network Service Discovery; T1018 Remote System Discovery |
| Lateral Movement | T1021 Remote Services |
| Collection | not observed |
| Command and Control | T1219 Remote Access Tools |
| Exfiltration | not observed |
| Impact | T1529 System Shutdown/Reboot |

### ICS

| Tactic | Techniques |
|---|---|
| Initial Access | T0865 Spearphishing Attachment; T0822 External Remote Services; T0886 Remote Services |
| Execution | T0863 User Execution |
| Persistence | T0859 Valid Accounts; T1694.001 Insecure Credentials: Default Credentials |
| Privilege Escalation | not observed |
| Evasion | T1692.001 Unauthorized Message: Command Message; T0872 Indicator Removal on Host |
| Discovery | T0846 Remote System Discovery; T0846.001 Remote System Discovery: Port Scan |
| Lateral Movement | T0859 Valid Accounts; T0886 Remote Services; T1694.001 Insecure Credentials: Default Credentials |
| Collection | not observed |
| Command and Control | not observed |
| Inhibit Response Function | T0878 Alarm Suppression; T0816 Device Restart/Shutdown |
| Impair Process Control | T1692.001 Unauthorized Message: Command Message; T0836 Modify Parameter |
| Impact | T0831 Manipulation of Control; T0829 Loss of View; T0815 Denial of View |

## Mitigations that address the most mapped techniques

- M1042 Disable or Remove Feature or Program (6 techniques)
- M1027 Password Policies (6 techniques)
- M0930 Network Segmentation (6 techniques)
- M1017 User Training (5 techniques)
- M1018 User Account Management (4 techniques)
- M1047 Audit (4 techniques)
- M1038 Execution Prevention (4 techniques)
- M1026 Privileged Account Management (4 techniques)
- M1022 Restrict File and Directory Permissions (4 techniques)
- M0927 Password Policies (4 techniques)
- M1032 Multi-factor Authentication (4 techniques)
- M0801 Access Management (4 techniques)

## Data to collect for detection

| Log source | Techniques covered | Example events |
|---|---|---|
| WinEventLog:Sysmon | 25 (T0865, T1566.001, T1204.002, T1059.005, T0863, T1059.001, T1003.001, T1552.001, T1555, T1133, T1078, T0859, T1219, T0886, T0846, T1046, T0846.001, T1018, T1021, T0878, T1529, T1685.005, T1070, T0872, T1685.001) | EventCode=1 parent matches outlook.exe/winword.exe/excel.exe and image matches cmd.exe/pow…; EventCode=11 |
| auditd:SYSCALL | 19 (T0865, T1566.001, T1204.002, T1059.005, T0863, T1552.001, T1555, T1133, T1078, T0859, T1219, T0886, T0846, T1046, T0846.001, T1021, T0816, T1529, T1070) | Linux Audit SYSCALL/EXECVE/SOCKADDR records (configured audit rule): syscall=execve; shell…; execve: Execution of files saved in mail or download directories |
| WinEventLog:Security | 16 (T1003.001, T1552.001, T1555, T0822, T1133, T1078, T0859, T0886, T1694.001, T1078.001, T1021, T1529, T1685.005, T1070, T0872, T1685.001) | EventCode=4673; EventCode=4624, 4648 |
| macos:unifiedlog | 13 (T1566.001, T1204.002, T1059.005, T1552.001, T1555, T1133, T1078, T1219, T1046, T1018, T1021, T1529, T1070) | Inbound messages with attachments from suspicious domains; Execution of Terminal, osascript, or other interpreters originating from Mail or Preview |
| NSM:Content | 9 (T0865, T0859, T0846, T1694.001, T1692.001, T0878, T0836, T0816, T0872) | malformed or exploit-payload data via built-in communication interface from external sourc…; device authentication session from source Media Access Control (MAC)/Internet Protocol (IP… |
| networkdevice:syslog | 7 (T0859, T1018, T1694.001, T1078.001, T0878, T1529, T0872) | configuration change logged without corresponding change ticket reference following anomal…; syslog facility LOCAL7 or trap messages |
| macos:osquery | 6 (T1059.005, T1219, T1046, T1018, T1021, T1070) | process_events; CREATE/MODIFY: Creation of LaunchAgents/Daemons plists in user/system locations |
| linux:syslog | 5 (T1059.005, T1552.001, T1018, T1021, T1529) | /var/log/syslog; authentication success after file access |
| NSM:Connections | 5 (T0822, T1133, T1078, T0886, T0846.001) | Connection/session metadata: management-port session to field device originates through ex…; Failed password or accepted password for SSH users |
| AWS:CloudTrail | 4 (T1552.001, T1555, T1078.001, T1021) | command-line execution invoking credential enumeration; sudden role assumption after credential file access |

## Overlap with known activity (not attribution)

- 2015 Ukraine Electric Power Attack (campaign, C0028): 15 shared techniques
- FIN6 (group, G0037): 11 shared techniques
- Threat Group-3390 (group, G0027): 12 shared techniques
- Wizard Spider (group, G0102): 12 shared techniques
- Scattered Spider (group, G1015): 12 shared techniques
- OilRig (group, G0049): 12 shared techniques
