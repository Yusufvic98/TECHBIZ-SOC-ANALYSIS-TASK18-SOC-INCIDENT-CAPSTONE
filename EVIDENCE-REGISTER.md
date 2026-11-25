# Task 18 Evidence Register

This register maps the evidence described and embedded in the two revised Task 18 capstone reports. It is an index; it does not mean that original standalone image files have already been uploaded to the repository.

## SOC Analysis report

| Evidence ID | Report location | What it supports |
|---|---|---|
| A1–A2 | SOC report, pages 6–7 | Original LetsDefend SOC342 alert overview and expanded technical details |
| B1–B2 | SOC report, page 8 | Kali environment verification, workspace and evidence/documentation directory |
| B3–B4 | SOC report, page 9 | Incident summary, methodology, IOC analysis and MITRE ATT&CK mapping |
| B5–B6 | SOC report, page 10 | Incident timeline, root-cause assessment and potential CIA impact |
| B7–B8 | SOC report, page 11 | Consolidated capstone report, verdict, response and limitations/conclusion |
| B9–B10 | SOC report, page 12 | Final report review and Kali content verification |
| C1–C2 | SOC report, pages 13–14 | Prior TryHackMe Investigating Windows and Juicy Details completion evidence |
| Supporting | SOC report, page 15 | Evidence register and submission notes; prior Introduction to SIEM evidence referenced from Task 16 |

## Ethical Hacking report

| Evidence ID | Report location | What it supports |
|---|---|---|
| Practical screenshots | Ethical Hacking report, page 8 | Kali interface, route/connectivity confirmation and beginning of full TCP scan |
| Figures 2–4 | Ethical Hacking report, page 9 | Anonymous SMB share listing, controlled temporary SMB write/upload/list/delete test, anonymous FTP login |
| Figure 5 | Ethical Hacking report, page 10 | Fiverr dashboard showing four active gigs |
| Figure 6 | Ethical Hacking report, page 11 | Freelancer.com profile and four published portfolio items |
| Figures 7–8 | Ethical Hacking report, page 11 | Freelancer Computer Security and Academic Writing exam pages showing US$5.00 purchase prices |
| THM-01 / THM-02 | Ethical Hacking report, page 13 | TryHackMe Vulnversity and Basic Pentesting 100% room-completion screenshots |

## Key evidence qualifications

- SOC verdict: True Positive for suspicious exploitation activity; successful compromise is not confirmed.
- SOC alert indicators are platform-derived; no independent source-IP reputation lookup or server-side telemetry is documented.
- Ethical Hacking: 30 open TCP ports were reported; anonymous SMB writable access and anonymous FTP login were demonstrated.
- The VSFTPD 2.3.4 exploit attempt did not create a session; no remote shell or command execution is claimed.
- The SMB test used a temporary file that was deleted. FTP write access was not tested.
- Freelancer skill tests each displayed US$5.00 pricing; neither test was purchased or completed.
- TryHackMe screenshots establish room-level completion only, not individual commands, answers, flags or exploit steps.

## Repository upload checklist

- [ ] Review both revised PDFs for personal data and sensitive details before making the repository public.
- [ ] Add the final revised SOC PDF under `reports/`.
- [ ] Add the final revised Ethical Hacking PDF under `reports/`.
- [ ] Add original screenshots as separate files under a structured evidence directory if available.
- [ ] Use descriptive filenames matching the evidence IDs above.
- [ ] Remove credentials, tokens, unrelated personal information and confidential production data.
- [ ] Confirm each image is legible and its caption accurately describes what it proves.
