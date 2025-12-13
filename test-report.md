# Security Testing Report  
**Booking System – Phase 1 (Part 1)**

---

## 1️⃣ Introduction

### Testers  
- **Name:** Nirdesh Pokharel  
- **Name:** Ashish Thapa  

### Purpose  
The objective of this security assessment was to identify potential security weaknesses in the **registration and authentication-related components** of the Booking System web application. The focus was on discovering vulnerabilities that could impact confidentiality, integrity, or availability of the system.

### Scope  

**Tested Components:**  
- Registration functionality (`/register`)  
- Root endpoint (`/`)  
- Static resources served under `/static/*`  

**Exclusions:**  
- Administrative interfaces  
- Backend API endpoints not directly related to registration  
- Third-party services and integrations  

**Test Approach:**  
- **Black-box testing**, performed without access to application source code  
- Testing conducted against a locally deployed Docker environment  

### Test Environment & Dates  

- **Start Date:** 2025-12-03  
- **End Date:** 2025-12-03  

**Environment Details:**  
- Deployment: Docker (local)  
- Application URL: `http://localhost:8000`  
- Database: Containerized relational database  
- Browser: Mozilla Firefox  
- Security Tool: OWASP ZAP  

### Assumptions & Constraints  

- Testing was limited to the local Docker deployment only  
- No authentication credentials beyond standard registration were provided  
- Time and scope constraints limited testing to automated and basic manual validation  

---

## 2️⃣ Executive Summary  

### Summary  
An automated OWASP ZAP scan identified multiple security issues within the Booking System, including **critical injection-related vulnerabilities** and **missing security controls**. These findings indicate that the application is **not ready for production deployment** without remediation.

**Overall Risk Level:** **High**

### Top 5 Immediate Actions  

1. Sanitize and validate all user inputs to prevent SQL Injection.  
2. Fix path traversal issues in user-controlled input fields.  
3. Implement Anti-CSRF tokens on all forms handling user input.  
4. Add essential HTTP security headers (CSP, X-Frame-Options, X-Content-Type-Options).  
5. Replace default error responses with secure custom error pages.  

---

## 3️⃣ Severity Scale & Definitions  

| Severity | Level | Description | Recommended Action |
|--------|------|------------|--------------------|
| 🔴 | High | Vulnerabilities that may lead to full system compromise or data exposure (e.g., SQL Injection). | Immediate remediation required |
| 🟠 | Medium | Issues requiring certain conditions or user interaction (e.g., CSRF, missing CSP). | Fix as soon as possible |
| 🟡 | Low | Minor security weaknesses or misconfigurations. | Fix during maintenance |
| 🔵 | Info | Informational findings that improve security posture. | Monitor and harden |

---

## 4️⃣ Findings  

| ID | Severity | Finding | Description | Evidence / Proof |
|----|---------|--------|------------|----------------|
| F-01 | 🔴 High | SQL Injection in Registration | Unsanitized username input caused server errors when special characters were submitted, indicating possible SQL Injection. | OWASP ZAP: POST `/register` caused HTTP 500 error |
| F-02 | 🔴 High | Path Traversal Risk | User-controlled input may allow traversal outside intended directories due to insufficient validation. | OWASP ZAP alert on username parameter |
| F-03 | 🟠 Medium | Missing Anti-CSRF Protection | Registration form does not implement CSRF tokens, allowing potential CSRF attacks. | ZAP passive scan |
| F-04 | 🟠 Medium | Missing Content Security Policy | No CSP header was present, increasing the risk of XSS attacks. | ZAP header analysis |
| F-05 | 🟡 Low | Missing X-Content-Type-Options Header | The `nosniff` directive was absent, allowing MIME-type sniffing. | ZAP alert |

---

## 5️⃣ OWASP ZAP Test Report (Attachment)  

### Purpose  
This section contains the automated vulnerability scan results generated using **OWASP ZAP** against the local Booking System instance.

### Attached Reports  

- 📎 **ZAP Report – Part 1:** `zap_report_round1.md`  
- 📎 **ZAP Report – Part 2:** `zap_report_round2.md`  

### Notes  

- The scan was performed exclusively on the local Docker environment.  
- The OWASP ZAP output was included without modification, in compliance with coursework requirements.  
