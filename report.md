# Phishing Email Investigation Report

## Case Summary

| Field | Details |
|---|---|
| Case | Bradesco Livelo suspicious email |
| Analyst | Tushar Tyagi |
| Email date | 19 September 2023 |
| Sender | banco.bradesco@atendimento.com.br |
| Subject | CLIENTE PRIME - BRADESCO LIVELO: Seu cartão tem 92.990 pontos LIVELO expirando hoje! |
| Source IP | 137.184.34.4 |
| Assessment | Suspected phishing email |

## 1. Executive Summary

A suspicious email impersonating Banco do Bradesco / Livelo claims that 92,990 reward points are expiring that day and urges the recipient to redeem them through an external URL. Header review identified a mismatch between the visible sender and Return-Path, an unrelated sending hostname, and email-authentication anomalies. The body uses urgency and a reward incentive to encourage the recipient to click.

Based on the supplied evidence, the message is assessed as a suspected phishing attempt. The URL's final behavior and any credential collection were not verified in this investigation.

## 2. Header Analysis

- From: `banco.bradesco@atendimento.com.br`
- Return-Path: `root@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06`
- Sending host: `ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06`
- Source IP: `137.184.34.4`
- Message-ID: `<20230919183549.39DEA3F725@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06>`

The Return-Path and sending host do not align with the visible Bradesco/Livelo sender identity. This is a suspicious inconsistency.

## 3. Email Authentication

| Check | Observed result |
|---|---|
| SPF | TempError — DNS timeout |
| DKIM | None — message not signed |
| DMARC | TempError in original Authentication-Results |
| Composite authentication | Fail, reason=001 |

MXToolbox also reported no DKIM-Signature header and no aligned DKIM signature. Its separate DMARC lookup showed no DMARC record for `atendimento.com.br` on 9 November 2026. This later lookup should not be treated as proof of the domain's DNS state in September 2023.

## 4. URL Analysis

Embedded URL:

`hxxps://blog1seguimentmydomaine2bra[.]me/`

The link appears under “Clique aqui” and “Resgatar Agora”. The domain does not visibly correspond to the claimed Bradesco/Livelo brand. The URL was not opened during this investigation and its landing-page behavior was not confirmed.

## 5. Threat Intelligence — Source IP

VirusTotal screenshot result for `137.184.34.4`:
- Detection ratio displayed: 0/92.
- Gridinsoft: Suspicious.
- Other visible vendors: Clean.
- ASN: AS14061 (DigitalOcean, LLC).
- Country displayed: United States.

Interpretation: The screenshot shows mixed reputation information. A 0/92 result does not establish that an IP is safe, and one vendor's Suspicious label is not conclusive proof of malicious activity.

## 6. Social-Engineering Indicators

- Urgency: the email says the points expire “today”.
- Reward incentive: it advertises redeemable points, airline miles and discounts.
- Brand impersonation: it presents itself as Banco do Bradesco / Livelo.
- Call to action: it repeatedly encourages the recipient to redeem points through an external link.

## 7. Indicators of Compromise

See `iocs.txt` for the extracted sender, domain, sending host, source IP, URL, subject, Message-ID and authentication results.

## 8. MITRE ATT&CK Mapping

**T1566.002 — Spearphishing Link**

The email contains a link and uses a persuasive message to encourage the recipient to visit it. This is a plausible mapping based on the delivery method. The destination's final behavior was not independently established.

## 9. Conclusion

The email is classified as a **suspected phishing email** based on the sender and Return-Path mismatch, unrelated sending host, authentication anomalies, external URL and urgency-based reward messaging.

The VirusTotal screenshot does not show broad vendor detection of the source IP, so the IP should not be described as confirmed malicious based on that result alone. Further URL analysis and organizational mail-log correlation would be needed for a stronger conclusion.

## 10. Recommended Actions

1. Do not click the URL or provide credentials.
2. Report and preserve the message for the security team.
3. Search mail logs for matching sender, subject, URL and source IP.
4. Investigate the destination using approved, isolated security tools.
5. If credentials were submitted, follow the organization's password-reset and session-revocation procedures.

## Evidence

Screenshots in the `screenshots/` directory document the MXToolbox header analysis, authentication/DNS findings, VirusTotal IP lookup and EML preview containing the embedded links.
