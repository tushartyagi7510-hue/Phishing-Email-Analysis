# Phishing Email Analysis — Bradesco Livelo

**Analyst:** Tushar Tyagi  
**Project:** Phishing Email Analysis  
**Tools:** MXToolbox Email Header Analyzer, VirusTotal, CyberHunter EML Analyzer  
**Sample date:** 19 September 2023

## Overview

This project documents a defensive analysis of a suspicious email impersonating Banco do Bradesco / Livelo. The message claims that 92,990 Livelo points are expiring that day and encourages the recipient to redeem them through a link.

The investigation focuses on the email sender, delivery infrastructure, authentication results, embedded URL, IP reputation and social-engineering indicators.

## Email Summary

- **Display name:** BANCO DO BRADESCO LIVELO
- **From:** banco.bradesco@atendimento.com.br
- **Subject:** CLIENTE PRIME - BRADESCO LIVELO: Seu cartão tem 92.990 pontos LIVELO expirando hoje!
- **Date:** 19 September 2023, 18:35:49 UTC
- **Sending IP:** 137.184.34.4
- **Embedded URL:** `hxxps://blog1seguimentmydomaine2bra[.]me/`

## Key Findings

- The visible sender claims to represent Banco do Bradesco / Livelo, while the message was submitted from a host named `ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06`.
- The Return-Path is `root@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06`, which does not align with the visible From identity.
- The message's Authentication-Results records `spf=temperror`, `dkim=none`, and `dmarc=temperror`. The SPF temporary error is explicitly associated with a DNS timeout; it should not be rewritten as SPF fail.
- MXToolbox reported no DMARC record for `atendimento.com.br` in the lookup shown in the supplied screenshot. This is a lookup performed on 9 November 2026, not necessarily the DNS state on the message date.
- VirusTotal's screenshot for `137.184.34.4` shows **0/92** vendor detections, while Gridinsoft labels it **Suspicious**. This is mixed reputation evidence, not proof that the IP is clean or malicious.
- The email body uses urgency and a points-expiry incentive to encourage the recipient to click the embedded link.

## Investigation Workflow

1. Review the `.eml` sample and extract relevant headers.
2. Inspect the email body and identify embedded links.
3. Analyze headers with MXToolbox Email Header Analyzer.
4. Review the sending IP on VirusTotal.
5. Record observed indicators in `iocs.txt`.
6. Document analyst observations in `analyst_notes.md`.
7. Summarize evidence, limitations and recommendations in `report.md`.

## Tools Used

- [MXToolbox Email Header Analyzer](https://mxtoolbox.com/EmailHeaders.aspx)
- [VirusTotal](https://www.virustotal.com/)
- [CyberHunter EML Analyzer](https://cyberhunter.fr/tools/eml-analyzer)

## Repository Contents

```text
Bradesco-Livelo-Phishing-Email-Analysis/
├── README.md
├── email_body.txt
├── email_headers.txt
├── iocs.txt
├── analyst_notes.md
├── report.md
└── screenshots/
    ├── 01-mxtoolbox-header-findings.png
    ├── 02-mxtoolbox-authentication-dns.png
    ├── 03-virustotal-source-ip.png
    └── 04-eml-analyzer-email-links.png
```

## MITRE ATT&CK

- **T1566.002 — Spearphishing Link:** The email contains a link intended to entice the recipient to visit an external website. This is a plausible technique mapping based on the observed delivery method; the destination's final behavior was not independently established in this evidence set.

## Conclusion

The message is assessed as a **suspected phishing email** based on the impersonated brand identity, mismatched Return-Path, authentication anomalies, external link and urgency-based content. The available screenshots provide supporting evidence, but the VirusTotal result is mixed and the URL was not independently reputation-checked in the submitted evidence.

## Evidence and Safety Notes

- The screenshots were supplied from the analyst's investigation session.
- The URL is defanged in the text files to reduce accidental navigation.
- The screenshots may visibly contain the original URL; do not click it.
- The original `.eml` is not included in this public-ready package. It contains active HTML and external links. Keep the original sample offline and use it only in a controlled analysis environment.
- The email sample is from the public beginner SOC challenge linked in the supplied lab notes: [30-Days-SOC-Challenge-Beginner](https://github.com/0xrajneesh/30-Days-SOC-Challenge-Beginner).
- This repository documents a training investigation. It does not claim that a victim account was compromised or that the URL's current state is malicious.

## Author

**Tushar Tyagi**  
Aspiring SOC Analyst | Cybersecurity
