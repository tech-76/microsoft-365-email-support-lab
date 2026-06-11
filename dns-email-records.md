# DNS Email Records

## Overview

DNS email records help control how email is delivered, authenticated, and protected for a domain. Incorrect DNS records can cause mail delivery failures, spam filtering problems, spoofing risks, and authentication failures.

This document explains common DNS records used for email.

## Purpose

This document covers:

- MX records
- TXT records
- SPF
- DKIM
- DMARC
- CNAME records used for email services
- Autodiscover records
- Basic DNS troubleshooting commands

## Why DNS Matters for Email

Email systems use DNS to answer questions like:

- Which mail server receives email for this domain?
- Is this sending server allowed to send email for the domain?
- Is the message digitally signed?
- What should happen if authentication fails?
- How should email clients automatically find mailbox settings?

## Common DNS Email Records

| Record Type | Purpose |
|---|---|
| MX | Points incoming email to mail servers |
| TXT | Stores SPF, DKIM, DMARC, and verification values |
| CNAME | Creates aliases for services such as autodiscover |
| A | Maps hostnames to IPv4 addresses |
| AAAA | Maps hostnames to IPv6 addresses |

## MX Records

MX stands for Mail Exchange.

MX records tell the internet where to deliver email for a domain.

Example:

```text
example.com MX 10 mail.example.com
```

The lower priority number is preferred.

Example with multiple MX records:

```text
example.com MX 10 mail1.example.com
example.com MX 20 mail2.example.com
```

In this example, mail goes to `mail1.example.com` first. If unavailable, mail may try `mail2.example.com`.

## How to Check MX Records

Command Prompt:

```cmd
nslookup -type=mx example.com
```

Expected output may include:

```text
example.com MX preference = 10, mail exchanger = mail.example.com
```

## TXT Records

TXT records store text information in DNS.

Email-related TXT records often include:

- SPF
- DKIM
- DMARC
- Domain verification records

Check TXT records:

```cmd
nslookup -type=txt example.com
```

## SPF Records

SPF stands for Sender Policy Framework.

SPF identifies which mail servers are allowed to send email for a domain.

Example:

```text
v=spf1 include:spf.protection.outlook.com -all
```

A basic SPF record may include:

| Tag | Meaning |
|---|---|
| v=spf1 | Identifies SPF version |
| include | Includes another provider's SPF record |
| ip4 | Allows an IPv4 address |
| ip6 | Allows an IPv6 address |
| a | Allows the domain's A record |
| mx | Allows MX servers |
| -all | Hard fail for unauthorized senders |
| ~all | Soft fail for unauthorized senders |

## DKIM Records

DKIM stands for DomainKeys Identified Mail.

DKIM uses cryptographic signatures to help verify that an email was not changed in transit and that it was authorized by the domain.

DKIM records are often stored as TXT or CNAME records depending on the provider.

Example selector format:

```text
selector1._domainkey.example.com
selector2._domainkey.example.com
```

## DMARC Records

DMARC stands for Domain-based Message Authentication, Reporting, and Conformance.

DMARC tells receiving servers what to do when SPF or DKIM checks fail.

DMARC record location:

```text
_dmarc.example.com
```

Example:

```text
v=DMARC1; p=quarantine; rua=mailto:dmarc-reports@example.com
```

Common DMARC policies:

| Policy | Meaning |
|---|---|
| none | Monitor only |
| quarantine | Send suspicious mail to spam/quarantine |
| reject | Reject failing mail |

## Autodiscover Records

Autodiscover helps email clients automatically find mailbox settings.

Microsoft 365 commonly uses an Autodiscover CNAME record.

Example:

```text
autodiscover.example.com CNAME autodiscover.outlook.com
```

This helps Outlook configure the mailbox automatically.

## Common Email DNS Problems

| Issue | Possible Cause | Result |
|---|---|---|
| Missing MX record | No inbound mail route | Domain cannot receive email |
| Wrong MX record | Mail sent to wrong provider | Mail delivery failure |
| Missing SPF | Sender not authenticated | Messages may go to spam |
| SPF too strict | Legitimate sender not included | Valid messages may fail |
| Missing DKIM | No message signature | Lower trust with recipients |
| Missing DMARC | No domain-level policy | Weaker spoofing protection |
| Wrong autodiscover | Client setup fails | Outlook setup issues |

## Basic DNS Troubleshooting Steps

1. Confirm the affected domain.
2. Check MX records.
3. Check SPF TXT record.
4. Check DKIM records or selectors.
5. Check DMARC record.
6. Confirm Autodiscover records if Outlook setup fails.
7. Compare records with provider requirements.
8. Escalate before making DNS changes.

## Useful Commands

### Check MX

```cmd
nslookup -type=mx example.com
```

### Check TXT

```cmd
nslookup -type=txt example.com
```

### Check DMARC

```cmd
nslookup -type=txt _dmarc.example.com
```

### Check DKIM selector

```cmd
nslookup -type=txt selector1._domainkey.example.com
```

### Check Autodiscover

```cmd
nslookup autodiscover.example.com
```

## DNS Change Considerations

DNS changes may not apply immediately because of TTL, which stands for Time To Live.

Important notes:

- DNS changes can take time to propagate.
- Incorrect records can break mail flow.
- DNS changes should be documented.
- Always follow the email provider's required values.
- Avoid duplicate SPF records.
- Use one SPF TXT record per domain.

## Escalation Criteria

Escalate if:

- DNS records need to be changed
- Domain-wide email delivery is failing
- SPF/DKIM/DMARC authentication is failing
- Mail is being spoofed
- Microsoft 365 or Google Workspace records are unclear
- DNS hosting access is required
- Records conflict with existing services

## Example Ticket Note

```text
User reported external senders receiving bounce messages when emailing company domain.
Checked MX records using nslookup and found MX still pointed to old mail provider.
Confirmed correct Microsoft 365 MX record with senior admin.
Escalated DNS update request for approval and implementation.
Ticket updated with findings and pending DNS change.
```

## Skills Demonstrated

- DNS email record awareness
- MX troubleshooting
- SPF, DKIM, and DMARC basics
- Autodiscover awareness
- Command-line DNS checks
- Mail delivery troubleshooting
