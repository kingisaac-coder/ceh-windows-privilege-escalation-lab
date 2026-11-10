# Windows Privilege Escalation Lab — CEH Module 06

A hands-on lab documenting the privilege escalation phase of the CEH (Certified Ethical Hacker) System Hacking module, performed entirely within an isolated virtual lab environment (Parrot Security OS attacker machine + Windows 10/11 victim VM on a private host-only network).

## Objective

Demonstrate the full attack chain from initial access to privilege escalation on a Windows target:

1. Generate a payload and gain initial remote access
2. Enumerate the compromised host for privilege escalation opportunities
3. Analyze findings and understand exploitation paths
4. Document the attack surface for defensive learning

## Environment

| Component | Details |
|---|---|
| Attacker | Parrot Security OS (VirtualBox VM) |
| Target | Windows 10 (Build 19045, 22H2) |
| Network | Host-only / NAT, isolated lab segment |
| Framework | Metasploit Framework |

## Tools Used

- **msfvenom** — payload generation (`windows/meterpreter/reverse_tcp`)
- **Metasploit (`multi/handler`)** — listener and session management
- **Meterpreter** — post-exploitation shell
- **winPEAS (winPEASx64.exe)** — automated Windows privilege escalation enumeration

## Methodology

### 1. Initial Access
A reverse TCP Meterpreter payload was generated with `msfvenom` and delivered to the target VM. A Metasploit multi/handler listener was configured to catch the callback, establishing an initial Meterpreter session on the target.

### 2. Post-Exploitation Enumeration
With a foothold established, `winPEASx64.exe` was uploaded to the target via Meterpreter's `upload` command and executed locally to perform automated enumeration across:

- System information & patch level (cross-referenced against known CVEs)
- User privileges and group memberships
- Installed services, permissions, and configurations
- Registry-based misconfigurations (e.g. AlwaysInstallElevated)
- Scheduled tasks
- AMSI / AntiVirus / AppLocker status

### 3. Analysis
The scan surfaced the target's exact patch level and flagged a number of CVE candidates matching the installed Windows build, along with configuration data used to identify potential escalation vectors for further manual review.

### 4. Lessons & Defensive Takeaways
- Unpatched systems with a long gap since the last update expose a wide CVE surface — regular patch cycles materially shrink this.
- Automated enumeration tools like winPEAS highlight exactly what a real attacker would check first; the same output is useful as a hardening checklist.
- Logging and endpoint monitoring (process creation, outbound connections on uncommon ports) would have flagged this activity in a production environment.

## Disclaimer

This lab was conducted entirely within a private, isolated virtual lab environment that I own and control, as part of structured CEH coursework. No unauthorized systems were accessed. This repository is for educational/portfolio purposes only.

## Screenshots

See `/screenshots` for walkthrough captures of each phase (payload generation, session establishment, winPEAS enumeration output).
