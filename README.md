<div align="center">

# 🛡️ DVWA Security Research Portfolio

### *From Vulnerability to Defence — A Practical Study*

**Conducted in an isolated lab environment**  
Kali Linux → Metasploitable2 (DVWA) | VirtualBox Host-Only Network

[![TryHackMe](https://img.shields.io/badge/TryHackMe-Profile-red?style=flat-square&logo=tryhackme)](https://tryhackme.com/p/YOURUSERNAME)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=flat-square&logo=linkedin)](https://linkedin.com/in/YOURUSERNAME)
[![GitHub](https://img.shields.io/badge/GitHub-Portfolio-black?style=flat-square&logo=github)](https://github.com/YOURUSERNAME)

</div>

---

## ⚠️ Legal Disclaimer

> All research documented in this repository was conducted 
> exclusively in an isolated VirtualBox lab environment on 
> systems I personally own and control. No real systems, 
> networks, users, or organisations were involved at any point. 
> This work exists solely for educational purposes — to 
> understand how vulnerabilities work so they can be identified, 
> reported, and remediated professionally.
>
> **Do not replicate any of this outside of your own controlled 
> lab environment.**

---

## 👋 A Note Before You Read

Most DVWA write-ups on GitHub look the same. Someone runs a 
tool, gets a result, takes a screenshot, and calls it done. 
That is not what this is.

I spent time at every security level — Low, Medium, High, and 
Impossible — not to collect screenshots but to understand the 
*gap* between them. The Impossible level is where the real 
learning happens. It is the answer key. It shows what a 
correctly written application looks like and why every control 
matters.

This portfolio documents that gap. For every vulnerability I 
asked three questions — what makes this broken, what does a 
fixed version look like at the code level, and where has this 
exact vulnerability caused real damage in production systems. 
The answers to those three questions are what a SOC analyst 
actually needs to detect and respond to threats effectively.

---

## 🧪 Lab Environment

| Component | Details |
|-----------|---------|
| Attacker Machine | Kali Linux 2024.x (VirtualBox VM) |
| Target Machine | Metasploitable2 running DVWA 1.0.7 |
| Network Configuration | VirtualBox Host-Only Adapter — no internet access |
| Primary Tools | Burp Suite Community, browser DevTools |
| Security Levels Tested | Low · Medium · High · Impossible |

📁 See [lab-setup](lab-setup/README.md) for full environment 
configuration with screenshots.

---

## 📂 Repository Structure
```
dvwa-security-research/
│
├── README.md                  ← You are here
├── lessons-learned.md         ← Cross-cutting insights
│
├── lab-setup/                 ← Environment documentation
├── 01-brute-force/            ← Authentication attacks
├── 02-command-injection/      ← OS command execution
├── 03-sql-injection/          ← Database query manipulation
├── 04-xss-reflected/          ← Reflected cross-site scripting
├── 05-xss-stored/             ← Stored cross-site scripting
├── 06-csrf/                   ← Cross-site request forgery
├── 07-file-inclusion/         ← Local and remote file inclusion
└── 08-file-upload/            ← Malicious file upload
```

---

## 🔍 Vulnerability Index

Each entry below links to a full write-up covering all four 
security levels, annotated screenshots, PHP source code 
comparison between the vulnerable and secure versions, 
real-world breach examples, and the correct remediation.

---

### 01 — Brute Force

📁 [Full Write-Up](01-brute-force/README.md)

**What it is:** Systematically attempting username and password 
combinations against a login form with no rate limiting or 
lockout mechanism in place.

**Why it matters to a SOC analyst:** Brute force and credential 
stuffing attacks generate distinctive patterns in authentication 
logs — repeated failures from a single IP, multiple accounts 
targeted from one source, or geographically impossible login 
sequences. Detecting this pattern is a daily SOC tier 1 task.

**Security level progression:**

| Level | Defence Present | Why It Falls Short |
|-------|----------------|-------------------|
| Low | None | No protection at all |
| Medium | Time delay | Slows attack but does not stop it |
| High | CSRF token | Breaks simple tools but not stateful ones |
| Impossible | Lockout + logging + CAPTCHA | Correct layered defence |

**MITRE ATT&CK:** T1110 — Brute Force  
**OWASP:** A07:2021 — Identification and Authentication Failures

---

### 02 — Command Injection

📁 [Full Write-Up](02-command-injection/README.md)

**What it is:** Passing user input directly into a system 
command without sanitisation, allowing shell metacharacters 
to chain additional commands onto the intended one.

**Why it matters to a SOC analyst:** Command injection 
vulnerabilities in web-facing applications appear in EDR 
telemetry as unusual child processes spawned by web server 
processes — a web server spawning a shell is a high-confidence 
detection signal.

**Security level progression:**

| Level | Defence Present | Why It Falls Short |
|-------|----------------|-------------------|
| Low | None | Unsanitised input direct to system call |
| Medium | Basic blacklist | Blacklist is incomplete — bypassed |
| High | Extended blacklist | Still bypassable with creative input |
| Impossible | Strict whitelist | Only valid IP characters permitted |

**MITRE ATT&CK:** T1059 — Command and Scripting Interpreter  
**OWASP:** A03:2021 — Injection

---

### 03 — SQL Injection

📁 [Full Write-Up](03-sql-injection/README.md)

**What it is:** Manipulating the structure of a SQL query by 
injecting code through user input, allowing an attacker to 
bypass authentication, extract data, or modify database 
contents.

**Why it matters to a SOC analyst:** SQLi attacks generate 
distinctive signatures in web application logs — unusual 
characters in URI parameters, abnormally long query strings, 
SQL keywords in GET/POST parameters, and error messages 
containing database information in HTTP responses.

**Security level progression:**

| Level | Defence Present | Why It Falls Short |
|-------|----------------|-------------------|
| Low | None | Raw string concatenation into query |
| Medium | Basic escaping | Applied incorrectly — still injectable |
| High | Different approach | Implementation introduces new flaw |
| Impossible | Parameterised queries | Input treated as data, never code |

**MITRE ATT&CK:** T1190 — Exploit Public-Facing Application  
**OWASP:** A03:2021 — Injection

---

### 04 — XSS Reflected

📁 [Full Write-Up](04-xss-reflected/README.md)

**What it is:** User input that is immediately reflected back 
in the HTTP response without encoding, allowing script 
injection that executes in the victim's browser when they 
visit a crafted URL.

**Why it matters to a SOC analyst:** Reflected XSS is 
frequently used in phishing campaigns — a legitimate domain 
URL is crafted to include a payload, making it pass URL 
reputation checks. SOC analysts encounter this when 
investigating user-reported suspicious links that appear 
to come from trusted domains.

**Security level progression:**

| Level | Defence Present | Why It Falls Short |
|-------|----------------|-------------------|
| Low | None | Input reflected directly |
| Medium | Tag blacklist | Specific tag blocked — others work |
| High | Extended blacklist | Attribute-based bypass possible |
| Impossible | Entity encoding | All special characters neutralised |

**MITRE ATT&CK:** T1059.007 — JavaScript  
**OWASP:** A03:2021 — Injection

---

### 05 — XSS Stored

📁 [Full Write-Up](05-xss-stored/README.md)

**What it is:** Malicious script that is saved to the database 
and executes in every user's browser that loads the affected 
page — no crafted link required.

**Why it matters to a SOC analyst:** Stored XSS is the 
mechanism behind web-based supply chain attacks. The 2018 
British Airways breach used stored XSS on the payment page 
to skim 500,000 customers' card details for 22 days. 
Detecting unexpected external script loads and unusual 
outbound POST requests from customer browsers are the 
detection signals.

**Security level progression:**

| Level | Defence Present | Why It Falls Short |
|-------|----------------|-------------------|
| Low | None | Payload stored and replayed |
| Medium | Length limit + filter | Limit bypassable, filter incomplete |
| High | Stronger filter | Still incomplete |
| Impossible | Encoding + PDO | Input neutralised before storage |

**MITRE ATT&CK:** T1059.007 — JavaScript  
**OWASP:** A03:2021 — Injection

---

### 06 — CSRF

📁 [Full Write-Up](06-csrf/README.md)

**What it is:** Tricking an authenticated user into 
submitting a forged request to a web application they 
are already logged into, causing an unintended action 
on their behalf.

**Why it matters to a SOC analyst:** CSRF is a social 
engineering vector that leaves minimal traces — the 
HTTP request appears to come from the legitimate user's 
browser. Detection relies on anomaly analysis — actions 
occurring without corresponding user-initiated navigation, 
or state-changing requests arriving directly without a 
preceding GET request to the form page.

**Security level progression:**

| Level | Defence Present | Why It Falls Short |
|-------|----------------|-------------------|
| Low | None | Any request with valid cookie accepted |
| Medium | Referer check | Header can be stripped or manipulated |
| High | CSRF token | Proper synchroniser token pattern |
| Impossible | Token + password re-entry | Requires knowledge attacker cannot have |

**MITRE ATT&CK:** T1185 — Browser Session Hijacking  
**OWASP:** A01:2021 — Broken Access Control

---

### 07 — File Inclusion

📁 [Full Write-Up](07-file-inclusion/README.md)

**What it is:** Manipulating a file path parameter to 
include files the application did not intend to serve — 
locally (LFI) to read sensitive files, or remotely (RFI) 
to execute attacker-controlled code.

**Why it matters to a SOC analyst:** LFI vulnerabilities 
appear in web logs as requests containing `../` sequences, 
URL-encoded path traversal strings, or references to 
known sensitive paths like `/etc/passwd` or 
`/proc/self/environ`. These are high-confidence IOCs 
in HTTP access logs.

**Security level progression:**

| Level | Defence Present | Why It Falls Short |
|-------|----------------|-------------------|
| Low | None | Direct user input to include() |
| Medium | Basic traversal filter | Incomplete — encoding bypasses it |
| High | Stricter filter | Still bypassable |
| Impossible | Strict whitelist | Only specific permitted files allowed |

**MITRE ATT&CK:** T1083 — File and Directory Discovery  
**OWASP:** A01:2021 — Broken Access Control

---

### 08 — File Upload

📁 [Full Write-Up](08-file-upload/README.md)

**What it is:** Uploading a file with a dangerous type — 
such as a server-side script — when the application fails 
to validate what is actually being uploaded beyond surface 
checks like file extension or Content-Type header.

**Why it matters to a SOC analyst:** Successful file upload 
attacks result in web shells — persistent backdoors giving 
the attacker command execution via HTTP. In EDR telemetry 
this appears as a web server process executing unexpected 
child commands. In web logs it appears as POST requests to 
an upload endpoint followed by GET requests to the uploaded 
file path.

**Security level progression:**

| Level | Defence Present | Why It Falls Short |
|-------|----------------|-------------------|
| Low | None | Any file accepted |
| Medium | Extension + MIME check | Client-supplied MIME is untrusted |
| High | Server-side extension check | Magic bytes not verified |
| Impossible | Magic bytes + rename + path restriction | Full validation stack |

**MITRE ATT&CK:** T1505.003 — Web Shell  
**OWASP:** A04:2021 — Insecure Design

---

## 📖 Key Lessons Learned

📄 [Read the full lessons document](lessons-learned.md)

Three things became clear working through every vulnerability 
at every level:

**Blacklists always lose.** Every Medium and High level in 
DVWA relies on blocking known bad input. Every one of them 
gets bypassed. The Impossible level always uses a whitelist 
— defining exactly what is permitted and rejecting everything 
else. This pattern holds in real production security too.

**Defence in depth is not optional.** The Impossible level 
never adds one control. It adds three or four simultaneously. 
Because any single control can fail. A CSRF token without 
rate limiting is weaker than a CSRF token with rate limiting 
and re-authentication. Layers exist because attackers look 
for the one layer that is missing.

**Understanding beats tooling.** The most valuable skill 
developed here was reading PHP source code and identifying 
which specific line creates the vulnerability. That skill 
transfers directly to code review, threat modelling, and 
writing detection rules — the actual work of a SOC analyst 
and application security engineer.

---

## 🧰 Tools Used

| Tool | Purpose | Cost |
|------|---------|------|
| VirtualBox | Lab virtualisation | Free |
| Kali Linux | Attacker operating system | Free |
| Metasploitable2 | Vulnerable target VM | Free |
| DVWA | Vulnerable web application | Free |
| Burp Suite Community | HTTP proxy and Intruder | Free |
| Firefox DevTools | DOM inspection, request analysis | Free |

---

## 📚 References and Further Reading

- [OWASP Top 10 2021](https://owasp.org/www-project-top-ten/)
- [MITRE ATT&CK Framework](https://attack.mitre.org/)
- [DVWA Documentation](https://github.com/digininja/DVWA)
- [PortSwigger Web Security Academy](https://portswigger.net/web-security) 
  — free labs covering every vulnerability documented here
- [OWASP Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)

---

## 🤝 Connect

I am actively working toward a SOC analyst role with a 
focus on blue team operations, log analysis, and web 
application security. If you are reviewing this portfolio 
and want to discuss any of the work documented here, 
I would welcome the conversation.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=flat-square&logo=linkedin)](https://linkedin.com/in/YOURUSERNAME)
[![TryHackMe](https://img.shields.io/badge/TryHackMe-Profile-red?style=flat-square&logo=tryhackme)](https://tryhackme.com/p/YOURUSERNAME)
[![Email](https://img.shields.io/badge/Email-Contact-green?style=flat-square&logo=gmail)](mailto:YOUREMAIL)

---

<div align="center">

*Built in a lab. Documented with intent. Aimed at defence.*

</div>
