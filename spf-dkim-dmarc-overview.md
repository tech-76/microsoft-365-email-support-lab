# SPF, DKIM, and DMARC Overview

## Purpose

SPF, DKIM, and DMARC are email authentication methods used to help protect domains from spoofing and improve email trust.

## SPF

SPF helps verify which mail servers are authorized to send email for a domain.

## DKIM

DKIM adds a digital signature to email messages so receiving mail servers can verify that the message was not changed in transit.

## DMARC

DMARC works with SPF and DKIM to tell receiving mail servers how to handle messages that fail authentication.

## Common Support Issues

- Website contact form emails going to spam
- Domain email not delivering
- Incorrect TXT records
- Missing SPF record
- Failed DKIM authentication
- DMARC policy not configured

## Troubleshooting Steps

1. Check the domain's DNS TXT records.
2. Confirm SPF includes the correct mail provider.
3. Verify DKIM records are published correctly.
4. Review DMARC policy.
5. Send test email and check message headers.
6. Document changes and test results.

## Skills Demonstrated

- DNS troubleshooting
- Email authentication awareness
- Microsoft 365 support
- Website email support
- Technical documentation
