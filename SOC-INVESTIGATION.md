# SOC Analysis — Task 18 Incident Investigation

## Case summary

| Field | Alert-derived value |
|---|---|
| Platform | LetsDefend |
| Alert/rule | SOC342 — CVE-2025-53770 SharePoint ToolShell Auth Bypass and RCE |
| Event ID | 320 |
| Severity / type | Critical / Web Attack |
| Event timestamp | 22 July 2025, 13:07:10 (UTC+03:00) |
| Affected host | SharePoint01 |
| Source → destination | 107.191.58.76 → 172.16.20.17 |
| HTTP method | POST |
| Requested URI | `/_layouts/15/ToolPane.aspx?DisplayMode=Edit&a=/ToolPane.aspx` |
| Referer | `/_layouts/SignOut.aspx` (flagged by the alert as suspicious/potentially spoofed) |
| User-Agent | Firefox 120 on Windows 10 (as recorded by the alert; not independently validated) |
| Content length | 7,699 bytes |
| Device action | Allowed |

## Investigation workflow

1. Reviewed the expanded SOC342 alert and recorded severity, event identity, timestamp, host and request metadata.
2. Extracted alert-derived indicators and documented their evidential limits.
3. Built a timeline from the single event timestamp; related observations are not presented as independently timed attacker actions.
4. Assessed the alert verdict and mapped the activity to MITRE ATT&CK.
5. Evaluated potential impact across confidentiality, integrity and availability.
6. Prepared evidence-preservation, validation and escalation recommendations.

The supplied Kali screenshots document the analyst's report-writing workspace and content. They are not independent SharePoint server telemetry.

## IOC and evidence assessment

- **107.191.58.76:** source address recorded by the alert. Reputation and historical activity were not independently checked.
- **172.16.20.17:** destination address recorded for SharePoint01.
- **SharePoint01:** affected asset named in the alert.
- **ToolPane.aspx URI and HTTP POST:** suspicious request metadata recorded by LetsDefend.
- **7,699-byte request:** content length reported by the alert.
- **Allowed:** device action recorded by the alert; it does not prove server-side execution.

The supplied evidence does not include independent packet capture, IIS/SharePoint log exports, endpoint telemetry, forensic image, malware sample or independent reputation lookup.

## Incident timeline

The platform supplies one recorded event timestamp: **22 July 2025, 13:07:10 (UTC+03:00)**. At that event, LetsDefend recorded the Critical SOC342 alert against SharePoint01 and the suspicious unauthenticated POST request to the ToolPane.aspx endpoint. The alert associated the activity with potential CVE-2025-53770 exploitation and recorded the device action as Allowed. Subsequent L1 review, indicator extraction, verdict assessment and L2 escalation recommendation are analyst actions; no separate timestamps were supplied.

## MITRE ATT&CK mapping

**T1190 — Exploit Public-Facing Application**  
**Tactic:** Initial Access

This is the primary mapping because the alert describes suspicious unauthenticated HTTP activity targeting a SharePoint web-application endpoint. No additional techniques are asserted as confirmed because the supplied evidence does not establish code execution, persistence, lateral movement or exfiltration.

## Root-cause and CIA impact assessment

The alert associates the request with potential exploitation of CVE-2025-53770. SharePoint patch level, vulnerability exposure, exploit success and the precise underlying configuration weakness were not independently verified. The vulnerability association is therefore alert-reported, not a forensically confirmed root cause.

If exploitation succeeded:
- **Confidentiality:** unauthorised access to SharePoint information could occur.
- **Integrity:** files or configuration could be modified.
- **Availability:** server compromise or service disruption could occur.

Confirmed: a Critical suspicious exploitation alert was recorded, request metadata and the affected host were displayed, and the device action was marked Allowed. Not confirmed: successful RCE, unauthorised data access or exfiltration, persistence, lateral movement, webshell presence, service disruption or actual business impact.

## Verdict

**TRUE POSITIVE — suspicious exploitation attempt detected.**

Confidence is high that the platform recorded the suspicious activity described in the alert. This validates the suspicious detection, not successful compromise. The Allowed device action increases the need for follow-up investigation but is not proof that the payload executed.

## Response and escalation recommendations

1. Preserve the original alert and associated security, network, IIS, SharePoint and endpoint logs with timestamps.
2. Escalate the Critical case to L2 Incident Response for server-side and endpoint validation.
3. Correlate IIS and SharePoint logs around 22 July 2025 13:07:10 (+03:00), including related ToolPane requests and activity from the recorded source address.
4. Inspect SharePoint01 for unexpected processes, command execution, webshell indicators, unauthorised file changes, persistence and unusual outbound connections.
5. Verify the server product/version, exposure to CVE-2025-53770 and vendor patch status using approved asset and vulnerability-management records.
6. Assess isolation or access restriction under incident-response policy; block the source only after validation and authorisation.
7. If compromise is confirmed, eradicate malicious artefacts, apply vendor-approved updates, rotate affected credentials where warranted, restore from trusted sources and monitor for recurrence.

## Limitations and quality controls

- No independent IIS/SharePoint log exports, packet captures, endpoint telemetry, forensic image or malware sample were supplied.
- Source-IP reputation was not independently checked.
- The single supplied timestamp does not support a multi-stage attack timeline.
- No attribution, successful exploitation, confirmed data loss or confirmed root cause is claimed.
- The report distinguishes platform-reported facts from analyst assessment and potential impact.

## Source evidence locations

The revised SOC report includes:
- Pages 1–2: cover, executive summary, incident details and methodology.
- Page 3: IOC assessment, timeline and MITRE ATT&CK mapping.
- Pages 4–5: impact, verdict, recommendations, limitations and conclusion.
- Pages 6–7: original LetsDefend alert screenshots (Evidence A1–A2).
- Pages 8–12: Task 18 Kali documentation screenshots (Evidence B1–B10).
- Pages 13–14: prior TryHackMe completion evidence (Evidence C1–C2).
- Page 15: evidence register and recommended portal summary.
