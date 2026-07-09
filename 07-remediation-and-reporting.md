# 07 - Remediation & Reporting

## Finding Structure (per vuln)

| Field       | Description                     | Example                                       |
| ----------- | ------------------------------- | --------------------------------------------- |
| Title       | Short, descriptive              | SQL Injection in Login Endpoint               |
| Severity    | CVSS/OWASP rating               | Critical / High / Medium / Low / Info         |
| CVE/CWE     | Reference if applicable         | CWE-89: SQL Injection                         |
| Affected    | URL, IP, service, param         | `http://target/login` — username param        |
| Description | What the vuln is                | User input passed to SQL without sanitization |
| Impact      | Business/technical consequence  | Attacker can dump the database, bypass auth   |
| Evidence    | Screenshot / payload / response | Burp request + response                       |
| Remediation | How to fix                      | Parameterized queries / prepared statements   |

## Severity Ratings (CVSS 3.x, approximate)

| Rating   | CVSS     | Examples                                    |
| -------- | -------- | ------------------------------------------- |
| Critical | 9.0–10.0 | Unauthenticated RCE, internet-facing        |
| High     | 7.0–8.9  | Privesc, SQLi with data access, auth bypass |
| Medium   | 4.0–6.9  | XSS, IDOR, information disclosure           |
| Low      | 0.1–3.9  | Missing headers, minor info leak            |
| Info     | 0.0      | Best-practice notes, no direct impact       |

## Report Checklist

* [ ] Executive summary (non-technical, business risk)
* [ ] Scope and methodology
* [ ] All findings structured per the table above
* [ ] Attack chain / kill chain diagram where applicable
* [ ] Annotated screenshot for every finding
* [ ] Credentials redacted in the report body; full detail in an NDA'd appendix
* [ ] Remediation prioritized by severity
* [ ] Retest confirmation section (placeholder)

## Obsidian Note Template

```yaml
---
tags: [htb, active, pentest]
categories: engagement
target: TARGET_NAME
ip: 10.10.11.X
os: Linux
difficulty: Medium
status: in-progress
---
## Summary
## Recon
## Foothold
## Lateral Movement
## Privilege Escalation
## Flags
## Lessons Learned
```

This mirrors the phase file (`00`–`06`) index at the top of each Quartz writeup — this template is the raw capture note during the engagement; the Quartz writeup is the polished writeup afterward.
