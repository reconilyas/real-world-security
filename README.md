# Real-World Web Security

Authorized real-world web security research and vulnerability findings.

Technical case studies covering **discovery, exploitation, evidence, impact, and remediation**.

Focused on **Web Application Security, Access Control, Authentication, and HTTP Security**.

All research is conducted within authorized testing scopes.

---

## Research Areas

* Web Application Security
* Access Control
* Authentication & Authorization
* HTTP Security
* Information Disclosure
* Origin & Infrastructure Security
* Vulnerability Research
* Security Testing Methodology

---

## Case Studies

### Coupang-TW

**X-Forwarded-For Origin Access-Control Bypass**

An access-control issue where a client-controlled `X-Forwarded-For` value influenced the observed perimeter access-control behavior.

The case study documents:

* Baseline behavior
* Controlled testing
* HTTP requests and responses
* Evidence
* Observed impact
* Remediation considerations

[View Case Study](./Coupang-TW/X-Forwarded-For-Origin-Access-Control-Bypass/)

---

## Methodology

My security testing workflow generally follows:

1. **Reconnaissance**
2. **Attack Surface Mapping**
3. **Manual Testing**
4. **Controlled Validation**
5. **Evidence Collection**
6. **Impact Assessment**
7. **Technical Documentation**
8. **Remediation Analysis**

I prioritize reproducible evidence and clearly distinguish between what was demonstrated and what was not tested or confirmed.

---

## Documentation

Each case study may include:

```text
Case Study/
├── README.md
└── screenshots/
    ├── evidence-1.png
    ├── evidence-2.png
    └── evidence-3.png
```

Reports focus on the technical behavior of the vulnerability rather than unsupported severity or impact claims.

---

## Scope & Authorization

All findings documented in this repository come from targets where I had authorization to perform security testing.

Testing is limited to the permitted scope and is conducted without intentionally accessing or modifying unnecessary sensitive data.

---

## Purpose

This repository serves as a technical portfolio of my practical experience in **web security research and vulnerability analysis**.

It is also a record of how I approach a finding from initial discovery through validation, evidence collection, impact analysis, and documentation.
