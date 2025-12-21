
# OWASP ZAP Automated Scan Report — Round 2  
**Booking System — Phase 1 / Part 2**

**Report Name:** zap_report_round2.md  
**Date:** (Insert Date)  
**Environment:** Local Docker — http://localhost:8001  
**Tool:** OWASP ZAP 2.14.0  
**Scan Type:** Automated Scan + Passive Scan

---

# 🔍 1. Summary

The updated application (Phase 1 / Part 2) was scanned using OWASP ZAP to identify common vulnerabilities.  
This is the second round of security testing.

### **Overall Risk Level:** 🟠 Medium

| Severity | Count |
|---------|--------|
| 🔴 High | 0 |
| 🟠 Medium | 1 |
| 🟡 Low | 3 |
| 🔵 Info | 3 |

The main vulnerability still present is **Absence of Anti-CSRF Tokens**.

---

# 🧩 2. Target Information

- **URL Scanned:** http://localhost:8001/
- **Application:** Booking System — Phase 1 / Part 2  
- **Deployed Using:** Docker Compose  
- **Scan Coverage:**
  - `/register`
  - `/status.html`
  - Static resources (`/static/*`)
- **Testing Method:** Automated scan + passive scan

---

# ⚠️ 3. Detailed Findings

## 🟠 3.1 Absence of Anti-CSRF Tokens  
**Severity:** Medium  
**URL:** `/register`

### Description
The registration form does not include a CSRF protection token.  
Attackers may trick a logged-in user into submitting unintended POST requests.

### Evidence
Extracted from ZAP:

```html
<form action="/register" method="POST">
```

No hidden nonce or token.

### Impact
- Fake registrations  
- Cross-site account creation  
- CSRF attack possible with a crafted HTML form

### Recommendation
Implement one of the following:
- Server-generated CSRF token
- Double Submit Cookie pattern
- SameSite cookie enforcement

---

## 🟡 3.2 Missing Anti-Clickjacking Header (partially fixed)

**Severity:** Low  
Some static resources do not include:

```
X-Frame-Options: DENY
```

### Impact
Minor risk in modern browsers, but recommended for consistency.

### Recommendation
Set clickjacking headers globally for:
- HTML
- JS
- CSS
- Images

---

## 🟡 3.3 Content Security Policy Improvements Needed

**Severity:** Low  
The CSP is good but not fully restrictive.

Current configuration:

```
default-src 'self';
script-src 'self';
style-src 'self';
img-src 'self';
form-action 'self';
frame-ancestors 'none';
```

### Recommendation
Improve CSP:
- Add `object-src 'none'`
- Add `base-uri 'none'`
- Use nonce-based script loading

---

## 🔵 3.4 User-Agent Fuzzer (Informational)

**Severity:** Info  
No exploitable issue found.  
User-Agent fuzzing triggered only informational logs.

---

# 📊 4. Scan Statistics

| Metric | Value |
|--------|-------|
| Total Requests | 42 |
| Total Alerts | 7 |
| High Severity | 0 |
| Medium Severity | 1 |
| Low Severity | 3 |
| Informational | 3 |

---

# 🧪 5. Additional Notes

### Improvements Since Round 1
- Password policy enforced (8+ chars)  
- Duplicate email check added  
- Improved success/error messages  
- Strong CSP added  
- More secure redirect behavior  

### Issues Still Present
- ❌ **CSRF still not implemented**  
- ❌ Backend input validation missing  
- ❌ No rate limiting / CAPTCHA  
- ❌ No MFA  
- ❌ Static assets missing headers  

---

# 📌 6. Recommendations Summary

### **Fix Immediately**
1. Implement Anti-CSRF tokens for `/register`.

### **Fix Soon**
2. Strengthen CSP (add `object-src`, `base-uri`).  
3. Add backend-side validation for email & password.  
4. Apply headers to all static files.  
5. Add CAPTCHA or rate limiting.  

---

# 📝 7. Conclusion

OWASP ZAP Round 2 scan confirms:

- Application improved significantly since Part 1  
- No high-severity issues found  
- CSRF protection is still missing  
- Several low-risk header/configuration weaknesses remain  

This file is stored with:
- **test_report.md**
- **zap_report_round1.md**
- **zap_report_round2.md**


