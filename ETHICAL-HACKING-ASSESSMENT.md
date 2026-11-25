# Ethical Hacking — Task 18 Full Penetration Test Capstone

## Scope and environment

This was an authorised training-lab assessment against one intentionally vulnerable Metasploitable 2 virtual machine. It was not a commercial client engagement or production assessment.

| Component | Observed configuration |
|---|---|
| Attacker workstation | Kali Linux VM, 192.168.56.101/24 |
| Target | Metasploitable 2 VM, 192.168.56.103 |
| Network | VirtualBox host-only, 192.168.56.0/24 |
| Connectivity | ICMP: 4 transmitted, 4 received, 0% packet loss |
| Tools evidenced | Nmap 7.99.1, smbclient, enum4linux, FTP client, Metasploit Framework, netcat/connection check |
| Assessment date | 24 November 2026 |

No public IP address, unrelated local device or production environment was scanned or exploited.

## Methodology and commands

The workflow covered reconnaissance, scanning, enumeration, controlled access testing, exploitation validation and reporting.

```bash
ip -br addr
ip route get 192.168.56.103
ping -c 4 192.168.56.103
nmap -sC -sV -O -p- 192.168.56.103 -oN task18_full_scan.txt
smbclient -L //192.168.56.103 -N
nmap -p139,445 --script smb-os-discovery,smb-enum-shares,smb-enum-users,smb-security-mode 192.168.56.103
enum4linux -a 192.168.56.103
smbclient //192.168.56.103/tmp -N
ftp 192.168.56.103
nmap -sV -p21,139,445,6200 192.168.56.103
```

The report notes that these commands are a concise transcription of the sequence in the technical evidence report; screenshots remain the primary proof of execution.

## TCP service discovery

The full TCP scan reported the host up, 65,505 closed TCP ports and **30 open TCP ports**:

| Port | Service | Detected version / note |
|---:|---|---|
| 21 | FTP | vsftpd 2.3.4 |
| 22 | SSH | OpenSSH 4.7p1 Debian 8ubuntu1 |
| 23 | Telnet | Linux telnetd |
| 25 | SMTP | Postfix smtpd |
| 53 | DNS | ISC BIND 9.4.2 |
| 80 | HTTP | Apache httpd 2.2.8 (Ubuntu), DAV/2 |
| 111 | RPC | rpcbind |
| 139 | NetBIOS/SMB | Samba smbd 3.X–4.X |
| 445 | SMB | Samba 3.0.20-Debian |
| 512 | rexec | netkit-rsh rexecd |
| 513 | rlogin | login service |
| 514 | rsh | Netkit rshd |
| 1099 | Java RMI | GNU Classpath grmiregistry |
| 1524 | Bindshell | Metasploitable root shell banner |
| 2049 | NFS | NFS v2–4 |
| 2121 | FTP | ProFTPD 1.3.1 |
| 3306 | MySQL | MySQL 5.0.51a-3ubuntu5 |
| 3632 | distccd | distccd v1 / GCC 4.2.4 |
| 5432 | PostgreSQL | PostgreSQL DB 8.3.0–8.3.7 |
| 5900 | VNC | VNC protocol 3.3; authentication |
| 6000 | X11 | Access denied in probe |
| 6667 | IRC | UnrealIRCd |
| 6697 | IRC | UnrealIRCd |
| 8009 | AJP13 | Apache JServ Protocol 1.3 |
| 8180 | HTTP | Apache Tomcat/Coyote; Tomcat/5.5 |
| 8787 | DRb/RMI | Ruby DRb RMI (Ruby 1.8) |
| 36865 | RPC | nlockmgr |
| 44459 | RPC | mountd |
| 44862 | RPC | status |
| 47322 | Java RMI | GNU Classpath grmiregistry |

Service banners are identification clues, not independent proof that each component is exploitable.

## Verified findings

### F-01 — VSFTPD 2.3.4 backdoor indicator (TCP/21)

**Priority:** High (potential) | **Confidence:** Medium

Nmap identified vsftpd 2.3.4. The Metasploit check reported that the target appeared vulnerable. The exploit output reported a spawned backdoor but explicitly stated that no session was created. A follow-up scan showed TCP/6200 closed.

**Assessment:** Unauthenticated command execution would be severe if confirmed, but it was not confirmed in this test. No remote shell or command execution is claimed.

**Recommendation:** Remove/replace the suspect legacy build with a trusted supported package, review host integrity/logs, restrict FTP exposure and prefer secure file transfer.

### F-02 — Anonymous SMB access and writable tmp share

**Priority:** Medium | **Confidence:** High

An anonymous smbclient session succeeded and the tmp share was listed. Anonymous read/write access was reported. A temporary test file was uploaded, listed and deleted.

**Assessment:** Unauthenticated file placement or modification may enable staging or tampering, depending on how the share is consumed.

**Recommendation:** Disable guest access and anonymous write permissions; enforce named-group ACLs and least privilege; audit file operations.

### F-03 — SMB account/share information disclosure

**Priority:** Low–Medium | **Confidence:** High

enum4linux and Nmap SMB scripts exposed WORKGROUP/host information, shares and account names including root, msfadmin, postgres and www-data.

**Assessment:** Account names and share metadata can help attackers focus password guessing and host mapping; enumeration does not prove account authentication.

**Recommendation:** Restrict SMB enumeration to authorised users, disable guest sessions, remove unused accounts and monitor access.

### F-04 — Anonymous FTP authentication

**Priority:** Low | **Confidence:** High

FTP accepted the anonymous username and returned “230 Login successful.” The directory-listing request completed; captured output showed no files. No FTP write test was performed.

**Assessment:** Anonymous access may expose content if present; sensitive-data exposure and write permission were not established.

**Recommendation:** Disable anonymous FTP or confine it to an isolated read-only directory; monitor access.

### F-05 — Broad exposure of legacy/remote services

**Priority:** High exposure priority | **Confidence:** High for open ports

Nmap identified Telnet, rexec, rlogin, rsh, multiple FTP/SMB services, databases, NFS, VNC, Java RMI, AJP, Tomcat and a bindshell banner.

**Assessment:** Unnecessary services expand attack surface; open-port results alone do not prove successful access or exploitability.

**Recommendation:** Disable unused services; replace insecure remote protocols with SSH; restrict management/database access; patch supported software and monitor exposure.

## Remediation priorities

| Priority | Recommended action | Verification |
|---|---|---|
| Immediate | Replace suspect VSFTPD 2.3.4 build; verify package provenance and host integrity | Confirm supported version, review integrity/logs and repeat an authorised vulnerability check |
| Immediate | Remove anonymous SMB write access; disable guest access and apply least-privilege ACLs | Repeat SMB enumeration and confirm guest users cannot write |
| High | Disable Telnet, rexec, rlogin, rsh and unnecessary bind/remote services | Rescan and confirm unnecessary ports are closed or filtered |
| High | Restrict databases, NFS, VNC, RMI and AJP to approved hosts/networks | Review firewall rules and test reachability from approved management systems |
| Medium | Reduce SMB information disclosure; remove unused accounts/shares; disable anonymous FTP or isolate read-only | Repeat enumeration, verify access controls and review logs |

## Limitations

- Metasploitable 2 is deliberately vulnerable; findings are not representative of a production environment without context.
- Nmap version detection relies on probes and banners and may require manual validation.
- VSFTPD exploitation did not create a session; no remote shell or command execution is claimed.
- SMB write access was tested only with a temporary file, which was deleted; persistence or code execution was not tested.
- FTP anonymous login was confirmed, but no FTP write test was performed and no files were visible in the captured listing.
- No exhaustive UDP scan, authenticated configuration audit, full database audit, privilege escalation or post-exploitation activity is claimed.

## Conclusion

The capstone demonstrates an end-to-end practical workflow from network discovery through service scanning, enumeration, controlled access testing, exploitation validation and professional reporting. The strongest verified finding was anonymous writable SMB access; anonymous FTP login was also confirmed. The VSFTPD backdoor check warranted attention, but the exploit attempt did not establish a session. The highest priorities are to remove anonymous SMB write access, replace the suspect FTP service and reduce the exposed legacy service footprint.

## Source evidence locations

The revised Ethical Hacking report includes:
- Pages 1–3: cover, executive summary, objectives, scope and rules of engagement.
- Pages 4–5: lab methodology, commands and 30-port service inventory.
- Pages 6–7: findings, remediation, limitations and conclusion.
- Pages 8–9: TryHackMe learning references and Kali/SMB/FTP technical screenshots.
- Pages 10–12: Fiverr dashboard, Freelancer.com portfolio and paid exam evidence.
- Pages 12–13: submission summary, evidence register and TryHackMe completion screenshots.
