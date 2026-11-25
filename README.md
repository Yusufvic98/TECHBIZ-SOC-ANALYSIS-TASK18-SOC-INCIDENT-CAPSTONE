# TechBiz Academy — Task 18 Capstone Portfolio

**Portfolio owner:** Oyewumi Yusuf Olayemi  
**Programme:** TechBiz Academy — B-503 | SOC-2026-03  
**Project:** Task 18 — End-to-End SOC Investigation and Ethical Hacking Capstone  
**Status:** Practical assessments and report documentation completed; platform-paid skill tests not completed.

This repository brings together the two Task 18 learning tracks and supporting career-development evidence. The SOC and Ethical Hacking exercises were separate activities and are documented separately below. Findings are based on the supplied revised capstone reports and embedded evidence.

## Project contents

| Area | Work performed | Evidence |
|---|---|---|
| SOC Analysis | LetsDefend SOC342 / Event ID 320 investigation of a Critical SharePoint ToolShell web-attack alert associated with CVE-2025-53770 | Original alert screenshots, IOC assessment, timeline, MITRE ATT&CK mapping, impact analysis and response recommendations |
| Ethical Hacking | Authorised network and service assessment of Metasploitable 2 from Kali Linux on an isolated VirtualBox host-only network | Nmap scan, SMB/FTP enumeration, controlled SMB write test, VSFTPD validation attempt and remediation plan |
| Career portfolio | Four active Fiverr cybersecurity gigs and four Freelancer.com portfolio items | Fiverr dashboard and Freelancer.com portfolio screenshots |
| Learning evidence | Previously completed TryHackMe rooms | Completion screenshots included in the reports |

## 1. SOC Analysis — SharePoint ToolShell alert

- **Platform:** LetsDefend
- **Case:** SOC342 — Event ID 320
- **Alert:** CVE-2025-53770 SharePoint ToolShell Auth Bypass and RCE
- **Severity/type:** Critical / Web Attack
- **Affected host:** SharePoint01
- **Recorded source → destination:** `107.191.58.76 → 172.16.20.17`
- **HTTP method:** POST
- **Target URI:** `/_layouts/15/ToolPane.aspx?DisplayMode=Edit&a=/ToolPane.aspx`
- **Recorded request content length:** 7,699 bytes
- **Device action:** Allowed

**Verdict: TRUE POSITIVE for suspicious exploitation activity. Successful remote code execution or system compromise was not confirmed.**

The investigation extracted alert-derived indicators, assessed event metadata, built a timeline from the single supplied event timestamp (22 July 2025, 13:07:10 UTC+03:00), mapped the activity to **MITRE ATT&CK T1190 — Exploit Public-Facing Application** (Initial Access), assessed potential CIA impact, and recommended evidence preservation and L2 Incident Response escalation.

The alert's “Allowed” action is not proof that the payload executed. No independent IIS/SharePoint logs, packet capture, endpoint telemetry, forensic image, malware sample or independent IOC reputation results were supplied. The IP addresses are treated as alert-derived indicators, not independently validated malicious infrastructure.

See [SOC investigation details](docs/SOC-INVESTIGATION.md).

## 2. Ethical Hacking — Metasploitable 2 assessment

**Scope:** One intentionally vulnerable Metasploitable 2 VM (`192.168.56.103`) assessed from Kali Linux (`192.168.56.101`) over the isolated VirtualBox host-only network `192.168.56.0/24`. Assessment date: 24 November 2026.

The practical workflow included:
1. Interface, route and ICMP reachability checks.
2. Full TCP port scan and service/version identification with Nmap.
3. SMB share/user enumeration with smbclient, Nmap scripts and enum4linux.
4. Anonymous SMB access and a controlled write test to the `tmp` share; a temporary file was uploaded, listed and deleted.
5. Anonymous FTP login validation.
6. VSFTPD 2.3.4 backdoor validation attempt and follow-up TCP/6200 check.

**Key results:** 30 open TCP ports were identified. Anonymous SMB access and anonymous write access to the `tmp` share were demonstrated. Anonymous FTP login was accepted. The VSFTPD 2.3.4 module check indicated that the target appeared vulnerable, but the exploit attempt did not create a session and the follow-up scan found TCP/6200 closed. **No remote shell or command execution is claimed.**

See [Ethical Hacking assessment details](docs/ETHICAL-HACKING-ASSESSMENT.md).

## 3. Career and freelancing portfolio evidence

- **Fiverr:** Four Active cybersecurity gigs were shown:
  - Analyze security logs and write SOC incident reports.
  - Assess your network security and recommend hardening steps.
  - Write professional cybersecurity and vulnerability assessment reports.
  - Perform a web application security assessment.
- **Freelancer.com:** Profile and four portfolio items were shown, covering AWS IAM cloud security, SOC analysis/security monitoring, web application security assessment, and Metasploitable 2 network security assessment.
- **Paid skill-test limitation:** Freelancer Computer Security Level 1 and Academic Writing Level 1 exam pages each displayed a US$5.00 purchase price. The exams were not purchased or completed. This is a platform-cost limitation, not a completed certification or test.

See [Career portfolio evidence](docs/CAREER-PORTFOLIO.md).

## 4. Learning evidence

The Ethical Hacking report includes 100% TryHackMe completion screenshots for **Vulnversity** and **Basic Pentesting**. The SOC report includes prior learning completion evidence for **Investigating Windows** and **Juicy Details**, with prior Introduction to SIEM evidence referenced from Task 16. These are prior learning records, not newly completed Task 18 exercises. The reports do not claim unsupported task-by-task flags or procedures.

## Repository structure

```text
.
├── README.md
├── EVIDENCE-REGISTER.md
├── docs/
│   ├── SOC-INVESTIGATION.md
│   ├── ETHICAL-HACKING-ASSESSMENT.md
│   └── CAREER-PORTFOLIO.md
└── reports/
    ├── TECHBIZ_SOC_B503_Task18_REVISED_CAPSTONE_INCIDENT_REPORT.pdf
    └── TechBiz_B503_Ethical_Hacking_Task18_Capstone_Report_REVISED.pdf
```

## Responsible-use and scope statement

The Ethical Hacking assessment was limited to an intentionally vulnerable training VM on an isolated host-only network. It was not a production penetration test or a client engagement. The SOC case is an evidence-based training investigation using the supplied LetsDefend alert. Neither activity is a certification, commercial security audit or guarantee of security.

## Author

**Oyewumi Yusuf Olayemi**  
TechBiz Academy — B-503 | SOC-2026-03
