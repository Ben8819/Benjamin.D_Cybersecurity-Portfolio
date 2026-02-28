# 📑 Windows Logs & Sysmon Event ID 
Practical SOC Reference for Log Investigation

---

# How to Think About Windows Logs

Windows logs are not isolated events.  
They tell a **story**.

When investigating, always ask:

- Who performed the action?
- What happened?
- When did it happen?
- From where?
- What happened next?

Think in terms of:

- Process tree
- User context
- Network activity
- Privilege escalation
- Persistence
- Defense evasion

Logs become powerful when correlated.

---

# Process Tree & Parent / Child Relationship

Every process has:

- `ProcessId`
- `ParentProcessId`
- `Image`
- `CommandLine`
- `User`

## Pivoting Logic

To go backward in the chain:

```
Search for ProcessId = ParentProcessId
```

To go forward in the chain:

```
Search for ParentProcessId = ProcessId
```

### Example Suspicious Chain

```
winword.exe → powershell.exe → cmd.exe → curl.exe
```

That is typically malicious behavior.

---

# Windows Log Categories Overview

| Log Type     | Purpose                                        |
|--------------|------------------------------------------------|
| Security     | Authentication, privilege use, account changes |
| System       | OS and service issues                          |
| Application  | Application-specific logs                      |
| PowerShell   | Script execution logging                       |
| Sysmon       | Advanced process, network, registry telemetry  |

---

# Windows Security Log – Critical Event IDs


## Authentication Events

| Event ID | Description                     | Why It Matters                     |
|----------|---------------------------------|------------------------------------| 
| 4624     | Successful Logon                | Track who logged in                |
| 4625     | Failed Logon                    | Detect brute-force attempts        |
| 4634     | Logoff                          | Session termination                | 
| 4648     | Logon with Explicit Credentials | RunAs / lateral movement           |
| 4672     | Special Privileges Assigned     | Admin login                        |  
| 4768     | Kerberos TGT Requested          | Domain authentication              |
| 4769     | Kerberos Service Ticket         | Service access / lateral movement  |
| 4776     | NTLM Authentication             | NTLM usage tracking                |

### Important Logon Types (4624)

- 2 → Interactive (local console)
- 3 → Network (SMB, remote share)
- 10 → RemoteInteractive (RDP)

Multiple 4625 followed by 4624 = possible brute-force success.

---

## Account Management Events

| Event ID | Description                  |
|----------|------------------------------|
| 4720     | User Account Created         |
| 4722     | Account Enabled              |
| 4723     | Password Change Attempt      |
| 4724     | Password Reset               |
| 4725     | Account Disabled             |
| 4726     | Account Deleted              |
| 4732     | User Added to Security Group |
| 4738     | User Account Modified        |

Watch for:
- New administrator accounts
- Privileged group membership changes

---

## Policy & Privilege Changes

| Event ID | Description               |
|----------|---------------------------|
| 4719     | Audit Policy Changed      |
| 4670     | Permissions Changed       |
| 4907     | Auditing Settings Changed |

These may indicate defense evasion.

---

## Log Clearing

| Event ID  | Description          |
|-----------|----------------------|
| 1102      | Security Log Cleared |

High severity. Often post-compromise activity.

---

# PowerShell Logging

If enabled:

| Event ID | Description          |
|----------|----------------------|
| 4103     | Module Logging       |
| 4104     | Script Block Logging |

4104 is extremely valuable because it logs:
- Decoded commands
- Obfuscated payloads
- Full script content

---

# Sysmon – Core Event IDs

Sysmon provides enhanced telemetry beyond native Windows logs.

## Essential Sysmon Events

| Event ID | Description                     | Investigation Use               |
|----------|---------------------------------|---------------------------------|
| 1        | Process Creation                | Process tree building           |
| 2        | File Creation Time Changed      | Timestomping detection          |
| 3        | Network Connection              | C2 detection                    |
| 5        | Process Terminated              | Process lifecycle               |
| 6        | Driver Loaded                   | Rootkit detection               |
| 7        | Image Loaded                    | DLL injection                   |
| 8        | CreateRemoteThread              | Code injection                  |
| 10       | Process Access                  | Credential dumping detection    |
| 11       | File Created                    | Payload drop detection          |
| 12       | Registry Object Created/Deleted | Persistence                     |
| 13       | Registry Value Set              | Persistence detection           |
| 15       | File Stream Created             | Alternate Data Stream detection |
| 22       | DNS Query                       | Domain resolution tracking      |

---

# Important Sysmon Events for SOC Analysts

If limited time, focus on:

## Event ID 1 – Process Creation

Key fields:
- Image
- ParentImage
- CommandLine
- User
- Hash

Look for:
- Office spawning PowerShell
- Browser spawning cmd
- Suspicious encoded commands

---

## Event ID 3 – Network Connection

Key fields:
- Destination IP
- Destination Port
- Image

Detect:
- Suspicious outbound traffic
- Rare external connections

---

## Event ID 10 – Process Access

Important for:
- LSASS access
- Credential dumping tools

---

## Event ID 22 – DNS Query

Correlate with:
- Network connections
- Suspicious domains

---

# Common Attack Patterns in Logs

## Brute Force Attack

Security:
- Multiple 4625
- Followed by 4624

Sysmon:
- Event 3 from suspicious source IP

---

## Malicious Macro Execution

Sysmon:
- winword.exe → powershell.exe (Event 1)
- PowerShell network connection (Event 3)

---

## Credential Dumping

Sysmon:
- Event 10 targeting lsass.exe

Security:
- 4672 (Admin privileges)
- 4624 (Successful admin logon)

---

## Registry Persistence

Sysmon:
- Event 13 (Registry Value Set)

Common key:

```
HKCU\Software\Microsoft\Windows\CurrentVersion\Run
```

---

# Practical Investigation Workflow

1. Start from alert (IP, process, user)
2. Pull process creation logs
3. Pivot via ParentProcessId
4. Check network activity
5. Check authentication events
6. Check registry and persistence
7. Check for log clearing (1102)

---
