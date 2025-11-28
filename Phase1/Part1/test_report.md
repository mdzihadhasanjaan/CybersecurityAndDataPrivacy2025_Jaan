# Booking System – Phase 1 Penetration Test Report

**Tester:** Jaan  
**Date:** 28 November 2025  
**Target application:** http://localhost:8000  
**Test scope:** Registration page (only)  
**Tools used:** Browser (Chrome), OWASP ZAP, Manual testing  

---

## 1. Scope & Objective

The objective of this penetration test was to evaluate the security and functionality of the user **registration process** of the Booking System — the first phase of development.  
Scope was limited to:

- Registration form inputs (email, password, birthdate, role)  
- Manual input validation & injection tests (SQLi, XSS, HTML injection, etc.)  
- Automated vulnerability scan via OWASP ZAP  

Out of scope: login page, resource booking, admin functionalities, database lockdown, performance testing, DoS attacks.

---

## 2. Test Cases & Manual Testing Results
| ID | Test Description | Input / Payload | Expected Result | Actual Result | Status | Notes |
|----|------------------|-----------------|------------------|----------------|--------|-------|
| TC-01 | Valid registration | Valid email, password, birthdate, role | Should register successfully | Registration succeeded | Pass | Works properly |
| TC-02 | Empty email field | Email = "" | Show validation error | Browser shows “required email” | Pass | HTML5 validation |
| TC-03 | Invalid email format | Email = abc | Reject invalid email | Browser shows invalid email message | Pass | Frontend validation |
| TC-04 | Weak password | Password = 123 | Reject weak password | Weak password accepted | Fail | No password rule |
| TC-05 | SQL Injection attempt 1 | ' OR 1=1 -- | Reject or sanitize | Blocked by email validation | Pass | Injection blocked |
| TC-06 | SQL Injection attempt 2 | " OR "1"="1 | Reject or sanitize | Blocked invalid email | Pass | Validation works |
| TC-07 | XSS attempt | <script>alert(1)</script> | Reject / sanitize | Browser does not allow <script> | Pass | XSS blocked |
| TC-08 | HTML Injection attempt | <b>test</b> | Reject or sanitize | Browser rejects < symbol | Pass | Input validation |
| TC-09 | Duplicate email registration | Use already registered email | Should reject duplicate | Allowed duplicate | Fail | No backend duplicate check |
| TC-10 | Under-age registration | Birthdate = 2012 | Reject (age < 15) | Accepted | Fail | No age validation |


## Summary

Out of the 10 executed test cases, **7 tests passed** and **3 tests failed**.

All input-validation related security tests such as **SQL Injection**, **XSS**, and **HTML Injection** were blocked by the browser’s built-in HTML5 validation, indicating that the frontend validation works correctly. However, the failed tests reveal significant backend weaknesses.

The system currently:

- **Accepts weak passwords**
- **Allows duplicate email registration**
- **Does not validate user age**

These failures show a clear pattern:  
the backend relies too heavily on frontend validation and lacks proper **server-side validation**, which makes the application vulnerable if the frontend is bypassed.

Stronger backend validation, error handling, and logic checks are required to improve the overall security and reliability of the registration system.


---

## 3. Automated Scan Findings (OWASP ZAP)

ZAP identified **6 issues** during automated scan on the application. Details below.

| #   | Vulnerability / Issue                          | Risk Level      | Description (short)                          |
|-----|-----------------------------------------------|-----------------|----------------------------------------------|
| A1  | Absence of Anti-CSRF Tokens                    | Medium          | Registration form lacks CSRF token           |
| A2  | Content Security Policy (CSP) header missing  | Medium          | No CSP header → XSS & content injection risk |
| A3  | Missing Anti-Clickjacking header               | Medium          | No X-Frame-Options / frame-ancestor set      |
| A4  | Format String Error                            | Medium          | Registration endpoint returned server error on malformed input |
| A5  | Missing `X-Content-Type-Options: nosniff` header | Low            | Risk of MIME-sniffing and content injection  |
| A6  | User-Agent Fuzzer differences (Informational) | Informational   | Variation in server response to different User-Agents |

### Evidence  
Screenshots of each alert (with details) are here and the full ZAP report are attached in the `BookingSystem-Phase1/` folder as `zap_report.html`.  


<img width="1919" height="1006" alt="image" src="https://github.com/user-attachments/assets/99d80e98-ce15-4dbf-82d6-7b4ed72318df" />
<img width="1919" height="1006" alt="image" src="https://github.com/user-attachments/assets/888bda23-355a-492b-9248-a86733fe2ec9" />
<img width="1919" height="1007" alt="image" src="https://github.com/user-attachments/assets/1b847408-c7b9-4568-960b-cf889221c590" />
<img width="1919" height="1009" alt="image" src="https://github.com/user-attachments/assets/c92c2f3b-6250-4fb7-8a43-5b36b53ebde2" />
<img width="1919" height="1010" alt="image" src="https://github.com/user-attachments/assets/ac5440ed-8faa-4db2-b976-7bf37839cdcd" />
<img width="1919" height="1009" alt="image" src="https://github.com/user-attachments/assets/68c53648-1efc-4f28-9fc2-86d8f90aea3d" />




---

## 4. Vulnerability Details & Recommendations

### **Vulnerability A1 – Absence of Anti-CSRF Tokens**  
**Impact:** Risk of CSRF attacks — attackers might trick users into sending unauthorized requests.  
**Recommendation:** Add CSRF protection to all state-changing forms (tokens + server-side validation).

---

### **Vulnerability A2 – Missing CSP Header**  
**Impact:** Increased risk of XSS, content injection, and script-based attacks.  
**Recommendation:** Add a secure Content Security Policy header, e.g.,  



---

### **Vulnerability A3 – Missing Anti-Clickjacking Header**  
**Impact:** Website could be embedded in malicious iframe → clickjacking attacks.  
**Recommendation:** Use `X-Frame-Options: DENY` or CSP frame-ancestors.

---

### **Vulnerability A4 – Format String / Input Handling Error**  
**Impact:** Server may crash or leak sensitive info on malformed input; potential injection risk.  
**Recommendation:** Sanitize inputs, validate on server-side, avoid unsafe string formatting with user input, and implement proper error handling.

---

### **Vulnerability A5 – Missing X-Content-Type-Options: nosniff**  
**Impact:** Browser MIME sniffing could misinterpret file types → potential content-type based attacks.  
**Recommendation:** Add header:  





---

### **Vulnerability A6 – User-Agent Fuzzer Variation (Informational)**  
**Impact:** Response varies by User-Agent — might indicate partial handling logic or old browser support code paths.  
**Recommendation:** Ensure consistent behavior across different User-Agent strings; log for further manual analysis if needed.

---

## 5. GDPR & Privacy-by-Design Assessment  

| Principle / Requirement               | Status / Note |
|--------------------------------------|----------------|
| Data minimization (only required data) | **OK** — registration asks minimal fields (email, password, birthdate) |
| Consent & Transparency                | No explicit consent prompt / privacy notice → **Needs improvement** |
| Secure data storage & transfer        | TLS/HTTPS not verified (running locally) → **Needs review for production** |
| Input validation & sanitization       | Missing / insufficient → high risk |
| Correct error handling                | Reveals server-error (500) on malformed input → bad practice |
| User rights (data deletion, etc.)     | Not tested / unknown at this phase |

**Conclusion:** While basic data minimization is respected, lack of CSRF, missing headers, input validation and poor error handling violate several Privacy-by-Design principles. Before deployment or further development, these issues must be addressed to ensure compliance and user data protection.

---

## 6. Overall Conclusion & Recommendations

The registration functionality of the Booking System exhibits multiple serious security and privacy issues.  
Some flaws (missing CSRF, CSP, clickjacking protection, input validation) are fundamental and could allow attackers to exploit the system or circumvent protections.  

**Immediate priority fixes:**  
- Add CSRF protection  
- Add security headers (CSP, X-Frame-Options / frame-ancestors, X-Content-Type-Options)  
- Sanitize and validate all user inputs server-side  
- Improve error handling to avoid server crashes or information leakage  

With these changes, the application will be significantly more secure and compliant with privacy best practices.  

---

## 7. References  
- OWASP Top 10, OWASP Cheat Sheets, CWE, general pentest guidelines.  
- ZAP automated scan results (see attached `zap_report.html`)  

