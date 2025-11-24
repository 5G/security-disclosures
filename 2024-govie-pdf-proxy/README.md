# gov.ie PDF Proxy Vulnerability (2024)
### Responsible Disclosure Report — James Gallagher

**Date Reported:** 14 May 2024  
**Vendor:** National Cyber Security Centre / CSIRT-IE  
**Status:** Resolved  
**Reporter:** James Gallagher (@5G)

---

## Summary

In May 2024, I identified and reported a security issue affecting the `gov.ie` domain. The public endpoint:

```
https://www.gov.ie/pdf/?file=
```

could be used to proxy **arbitrary external PDF files** through the trusted `gov.ie` domain. This behaviour unintentionally enabled attackers to make untrusted or malicious files appear as if they originated from an official Irish Government source.

The issue was verified by **CSIRT-IE**, forwarded to the relevant team, and fixed shortly thereafter.

---

## Impact

The vulnerability allowed anyone to:

- Serve **any external PDF** through `gov.ie`, gaining implicit government credibility  
- Distribute malicious PDFs with a trusted domain as the delivery mechanism  
- Facilitate phishing, social engineering, or malware distribution  
- Potentially damage the public trust associated with the government’s online services  

No internal systems were exposed and no authentication bypass was involved, but the **trust and reputational impact** made this a meaningful security issue.

---

## Technical Description

The `file` parameter accepted any user-provided URL:

```
https://www.gov.ie/pdf/?file=https://example.com/file.pdf
```

When requested, the server retrieved the remote file and served it from within the `gov.ie/pdf/` namespace.

This effectively turned the endpoint into an **open file proxy**:

- No validation of source domains  
- No content scanning  
- No restriction to internal government assets  

Because government domains carry elevated trust, this behaviour risked enabling convincing and harmful impersonation.

---

## Steps to Reproduce

1. Identify any publicly accessible PDF (benign or test).  
2. Append its URL to the endpoint:

```
https://www.gov.ie/pdf/?file=<external-pdf-url>
```

3. Visit the resulting link.  
4. Observe that the external PDF is delivered as if hosted by `gov.ie`.

This required **no authentication**, **no user interaction**, and had **low complexity**.

---

## Vendor Response

CSIRT-IE acknowledged receipt within 48 hours:

> *“Thank you for your responsible disclosure… I have verified your findings and reported this to the relevant authorities to investigate and fix.”*  
> — CSIRT-IE (16 May 2024)

The behaviour was subsequently remediated.

---

## Timeline

| Date | Event |
|------|--------|
| **14 May 2024** | Issue reported to CSIRT-IE |
| **16 May 2024** | Vulnerability verified; forwarded internally |
| **Late May 2024** | Endpoint behaviour fixed |

---

## Reporter

**James Gallagher**  
Software Engineer — Ireland  
GitHub: https://github.com/5G/

---

## Disclosure Policy

This issue was reported privately and resolved prior to public disclosure, following standard responsible disclosure practices. No sensitive implementation details are included in this document.