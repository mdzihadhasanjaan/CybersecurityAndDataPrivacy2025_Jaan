
# Booking System — Penetration Testing Report  
### Phase 1 — Part 2  
### Tester: Md Rasedul Islam and Zihad Hassan Zaan

---

# 1️⃣ Introduction

### **Purpose**
The purpose of this test is to verify the security improvements made in Phase 1 / Part 2 of the Booking System application, re-test all functionalities from Part 1, identify remaining vulnerabilities, and confirm whether previously reported issues have been fixed.

### **Scope**
**Included:**
- Registration form  
- Registration POST endpoint  
- Client-side and server-side validation  
- Error handling  
- Security headers (CSP, XFO)  
- Duplicate email handling  
- Password policy  
- OWASP ZAP Round 2 scan  

**Excluded:**
- Login page  
- Admin panel  
- Reservation system  
- Authentication/session management beyond registration  

### **Testing Approach**
- **Gray-box testing**
- Manual testing + automated ZAP scan
- Browser DevTools inspection
- Dockerized local environment testing

### **Test Environment**
- **OS:** Windows 10  
- **Browser:** Google Chrome  
- **Tools:** OWASP ZAP 2.14.0  
- **Environment:**  
  - `cybersec-web-phase1-part2` (Web App)  
  - `cybersec-db-phase1-part2` (PostgreSQL)  
- **URL:** http://localhost:8001  

### **Time Constraints**
- Limited to registration-related functionality  
- No direct access to backend source code  

---

# 2️⃣ Executive Summary

Phase 1 / Part 2 introduced several improvements to the registration system.  
All **critical issues from Part 1** were successfully fixed:

### ✔ Fixed from Part 1:
- SQL Injection no longer works  
- Weak passwords blocked  
- Duplicate email detection added  
- Empty fields validation enforced  
- Success/error messages improved  

### ❌ Still Not Fixed (new or remaining)
- Missing CSRF protection  
- CSP not fully restrictive  
- Rate limiting absent  
- Privacy notice absent  
- Some static files missing security headers  

### **Overall Security Rating:** 🟠 Medium  

### **Top 5 Immediate Actions**
1. Implement CSRF tokens for all POST requests  
2. Add backend-side email validation  
3. Add CAPTCHA or rate limiting  
4. Improve CSP with additional directives  
5. Provide a GDPR-compliant Privacy Notice  

---

# 3️⃣ Severity Scale

| Severity | Meaning | Action |
|---------|---------|--------|
| 🔴 High | Critical vulnerability that leads to compromise | Fix immediately |
| 🟠 Medium | Significant risk under certain conditions | Fix ASAP |
| 🟡 Low | Minor issue, usability/security weakness | Fix soon |
| 🔵 Info | No direct risk but recommended hardening | Optional |

---

# 4️⃣ Findings — Phase 1 / Part 2

Below are the final findings after retesting.

---

## F-01 — Absence of CSRF Protection  
**Severity:** 🟠 Medium  
**Status:** ❌ Not Fixed  

### Description  
The registration form still does not contain any anti-CSRF token.  
ZAP’s Round 2 scan flagged: *“Absence of Anti-CSRF Tokens”*

### Evidence  
Form HTML extracted:  
```html
<form action="/register" method="POST">
```

### Impact  
Malicious sites can auto-submit the form, creating accounts without user consent.

### Recommendation  
Add backend-generated CSRF tokens and validate them server-side.

---

## F-02 — Email Validation Done Only on Client-Side  
**Severity:** 🟡 Low  
**Status:** ❌ Not Fixed  

### Description  
The backend accepts invalid email formats if bypassing browser validation.

### Evidence  
DevTools shows browser blocks request. Backend never validates email syntax.

### Impact  
Attackers can bypass validation with direct POST requests.

### Recommendation  
Add backend email pattern validation.

---

## F-03 — No Rate Limiting or CAPTCHA  
**Severity:** 🟠 Medium  
**Status:** ❌ Not Fixed  

### Description  
ZAP fuzzing & manual testing confirm unlimited rapid submissions.

### Impact  
Attackers can generate thousands of accounts automatically.

### Recommendation  
Add rate limits or CAPTCHA.

---

## F-04 — Missing Privacy Notice (GDPR Transparency Failure)  
**Severity:** 🟡 Low  
**Status:** ❌ Not Fixed  

### Description  
No privacy or data processing notice is displayed anywhere.

### Impact  
Violates GDPR Articles 12–14.

### Recommendation  
Add a privacy notice and explain user rights.

---

## F-05 — CSP Present but Needs Additional Directives  
**Severity:** 🔵 Informational  
**Status:** ⚠️ Partially Fixed  

### Description  
Good CSP exists but missing:
- `object-src 'none'`
- `base-uri 'none'`
- Nonce-based scripts

### Impact  
XSS risk minimized but not eliminated.

---

# 5️⃣ Verification of Part 1 Findings

### ✔ 1. SQL Injection Vulnerability  
**Status:** FIXED  
- SQLi attempts did not execute  
- No DB errors or stack traces  
- Request blocked by HTML5 and safe backend response

---

### ✔ 2. Weak Password Policy  
**Status:** FIXED  
- Passwords under 8 characters rejected  
- Shows: **“Password must be at least 8 characters long”**

---

### ✔ 3. Duplicate Email Allowed  
**Status:** FIXED  
Backend now checks DB for existing email.  
Message displayed: **“Email already in use!”**

---

### ✔ 4. Missing Required Field Validation  
**Status:** FIXED  
Browser correctly shows: **“Please fill out this field”**

---

### ✔ 5. No User Feedback  
**Status:** FIXED  
Clear success and error status pages implemented.

---

# 6️⃣ OWASP ZAP Round 2 Summary

### **Scan Results**
- High alerts: **0**  
- Medium alerts: **1** → Missing CSRF  
- Low alerts: **3**  
- Info alerts: **3**

### Key ZAP Alerts:
- Absence of Anti-CSRF Tokens (Medium)  
- Missing headers on static files (Low)  
- CSP improvement recommended  
- User-Agent fuzzing (info only)

Full scan details included in:  
**`zap_report_round2.md`**

---

# 7️⃣ Conclusion

The Booking System has significantly improved from Phase 1 Part 1.  
The most critical issues—SQLi and weak passwords—are now fixed.  
However, some important security controls such as CSRF protection, backend validation, GDPR transparency, and bot protection remain missing.

### **Final Rating:** 🟠 Medium Risk

The system is improving but not yet ready for production use.

---

# 8️⃣ Attachments

- `test_report.md`  
- `zap_report_round1.md`  
- `zap_report_round2.md`  

---


