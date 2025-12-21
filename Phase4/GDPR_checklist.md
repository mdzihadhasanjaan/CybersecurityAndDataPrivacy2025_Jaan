
# GDPR Compliance Checklist – Booking System (Phase 4)

Legend:
- ✅ Pass  
- ❌ Fail  
- ⚠️ Attention / Partially Implemented  

---

## 1️⃣ Personal Data Mapping & Minimization

| Item | Result | Notes |
|------|--------|-------|
| Identified all personal data (name, email, username, birthdate) | ⚠️ | Data exists but not documented. |
| Only necessary data collected (minimization) | ⚠️ | Birthdate collected without user explanation. |
| Age recorded to verify 15+ booking requirement | ⚠️ | Birthdate stored but not validated in UI/API. |

---

## 2️⃣ User Registration & Management

| Item | Result | Notes |
|------|--------|-------|
| GDPR-compliant consent during registration | ❌ | No checkbox or privacy link. |
| Users can view their own data | ⚠️ | Partial profile visibility only. |
| Users can edit their own data | ❌ | Editing not available. |
| Users can delete their account (right to be forgotten) | ❌ | Feature missing. |
| Admin can delete a reserver | ⚠️ | Not fully visible or confirmed. |
| Under-15 registration restricted | ❌ | No validation found. |

---

## 3️⃣ Booking Visibility

| Item | Result | Notes |
|------|--------|-------|
| Public can view bookings without identity exposure | ⚠️ | Resource list available but booking identity not fully verified. |
| No personal data visible to unauthorized users | ❌ | `/api/users` exposes email to any logged-in user. |

---

## 4️⃣ Access Control & Authorization

| Item | Result | Notes |
|------|--------|-------|
| Only admins can manage resources and reservations | ⚠️ | Admin routes mostly inaccessible. |
| Role-based access control implemented | ⚠️ | Partially works but inconsistent. |
| Admin privileges limited per GDPR | ❌ | Oversharing user data via API endpoints. |

---

## 5️⃣ Privacy by Design (PbD)

| Item | Result | Notes |
|------|--------|-------|
| Privacy by default applied | ❌ | Minimization and consent missing. |
| Logs avoid unnecessary data | ⚠️ | Logging mechanism unclear. |
| Forms designed with minimal fields | ⚠️ | Birthdate collected without justification. |

---

## 6️⃣ Data Security

| Item | Result | Notes |
|------|--------|-------|
| CSRF, XSS, SQL injection protections present | ⚠️ | ZAP required for confirmation. |
| Password hashing (bcrypt/argon2) | ⚠️ | Hashing present but not verified for strength. |
| GDPR-compliant backup & recovery | ❌ | Not documented. |
| Data stored within the EU | ⚠️ | Local environment only. |

---

## 7️⃣ Data Anonymization & Pseudonymization

| Item | Result | Notes |
|------|--------|-------|
| Personal data anonymized when possible | ❌ | No anonymization implemented. |
| Pseudonymization used | ❌ | Not available. |

---

## 8️⃣ Data Subject Rights

| Item | Result | Notes |
|------|--------|-------|
| Users can request/download personal data | ❌ | Feature missing. |
| Users can request deletion | ❌ | Not implemented. |
| Users can withdraw data-processing consent | ❌ | No consent provided. |

---

## 9️⃣ Documentation & Communication

| Item | Result | Notes |
|------|--------|-------|
| Privacy Policy available | ❌ | Page missing → must create. |
| Terms of Service available | ❌ | Page missing → must create. |
| Cookie Policy available | ❌ | Page missing → must create. |
| Data breach response procedure documented | ❌ | Not available. |

---

# 📌 Summary Results

| Category | Status |
|---------|--------|
| Personal Data Handling | ⚠️ Partial |
| User Rights | ❌ Fail |
| Security | ⚠️ Partial |
| Privacy by Design | ❌ Fail |
| Transparency Policies | ❌ Missing |
| Authorization & Exposure | ❌ High Risk |

---

# Overall GDPR Compliance Score: ❌ FAIL

Major issues:
- No consent / transparency  
- No privacy/cookie/terms policies  
- API exposes: `/api/users` → emails visible  
- No user deletion, no data export  
- No anonymization or pseudonymization  



