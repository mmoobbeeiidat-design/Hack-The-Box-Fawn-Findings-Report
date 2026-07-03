---
tags: [pentnote, report, htb, fawn]
engagement: HTB_Fawn
date: 2026-07-03
author: Mohammad Obeidat
tool: PentNote v1.2.0
---

# PentNote Report - HTB_Fawn

## Executive Summary

During the security assessment of the target host `10.129.76.197` (HTB_Fawn) conducted on July 3, 2026, the system was found to contain a single, highly exploitable vulnerability within its FTP infrastructure. The `vsftpd` service was discovered to be misconfigured, permitting unrestricted anonymous login without credentials. This oversight immediately exposes the system's file repository to the public, allowing unauthenticated attackers to read, enumerate, and extract sensitive data. The assessment team successfully validated this vulnerability by accessing and retrieving the `flag.txt` file, confirming the tangible business risk of unauthorized data disclosure.
While the identified issue carries a **High severity** risk score of 7.5, the remediation path is straightforward and low-effort. Disabling anonymous access in the FTP configuration will effectively neutralize this threat. This report maps the vulnerability to the MITRE ATT&CK framework (T1078, T1213) and aligns remediation strategies with the NSA D3FEND matrix (D3-FUAC) to provide a clear, actionable roadmap for securing the asset against similar adversarial tactics.

| **Client**            | Hack The Box                    |
|-----------------------|---------------------------------|
| **Engagement type**   | Full Scope / Machine Assessment |
| **Scope**             | 10.129.76.197                   |
| **Operator**          | Mohammad Obeidat                |
| **Start date**        | 2026-07-03                      |

| Severity | Count |
|----------|------:|
| Critical | 0     |
| High     | 1     |
| Medium   | 0     |
| Low      | 0     |
| Info     | 0     |

---

## Attack Chains Detected

- **Initial Access via FTP** ➔ **Flag Extraction**  [Completed]

---

## Top 5 Risks

| # | Finding                      | Severity | Risk Score | Exploitability                     |
|---|------------------------------|----------|------------|------------------------------------|
| 1 | Anonymous FTP Login Allowed  | High     | 7.5        | Easy (No Credentials Required)     |

---

## Remediation Roadmap

| Priority | Finding                      | Severity | Effort | D3FEND  | Recommendation                         |
|----------|------------------------------|----------|--------|---------|----------------------------------------|
| 1        | Anonymous FTP Login Allowed  | High     | Low    | D3-FUAC | Disable anonymous access in vsftpd.conf|

---

## Affected Assets

| Asset          | Finding Count |
|----------------|--------------:|
| 10.129.76.197  | 1             |

---

## Target Group Findings

### HTB_Fawn Findings (10.129.76.197/32)

| Severity | Finding                      | Host          |
|----------|------------------------------|---------------|
| High     | Anonymous FTP Login Allowed  | 10.129.76.197 |

---

## Findings

### 1. Anonymous FTP Login Allowed (High)

- **Description:**  
  The FTP service (vsftpd 3.0.3) running on port 21 is configured to allow anonymous users to authenticate using the username `anonymous` or `ftp` with an empty/arbitrary password. This grants unauthorized read access to the shared files.

- **Impact:**  
  An attacker can access the system's files exposed via FTP, which in this case included the sensitive file `flag.txt`.

- **Evidence:**

	The following sequence demonstrates the complete exploitation chain for the anonymous FTP vulnerability. First, a standard FTP connection was established to the target IP `10.129.76.197` using the username `anonymous` with an empty password. The server responded with `230 Login successful.`, confirming that unauthenticated access is permitted. Subsequently, a directory listing was performed to identify accessible files, revealing the presence of `flag.txt`. The file was successfully downloaded to the local system using the `get` command.


![FTP Login Successful](Pasted%20image%2020260704005232.png)

	Finally, the downloaded file was read locally using the `cat` command, confirming the extraction of the sensitive flag. This validates that the anonymous FTP misconfiguration directly leads to unauthorized data disclosure.

![Flag Extraction](Pasted%20image%2020260704005406.png)