
## 🛡️ Authorization Test Report – Booking System (Phase 3)
## 👤 Tester

Md Rasedul Islam & Zihad Hasan Zaan

## 📅 Test Date

Phase 3 — Authorization Testing

## 🎯 Purpose

Evaluate whether Guest, Reserver, and Admin roles follow the authorization rules defined in the Booking System specification.

Focus areas:

What each role CAN and CANNOT do

API access control

Hidden endpoints

Backend authorization correctness

GDPR compliance

Comparison with expected specifications

## 1️⃣ Guest Role Authorization Analysis

Guest = Not logged in

## Guest – Allowed Actions

| Action | URL / Endpoint | Result | Matches Spec? |
|--------|----------------|--------|----------------|
| View Home Page | `/` | ✔ Works | ✔ Yes |
| View Login Page | `/login` | ✔ Works | ✔ Yes |
| View Registration Page | `/register` | ✔ Works | ✔ Yes |
| View Public Resources | `/resources` | ✔ Works | ✔ Yes |

## Guest – Blocked Actions

| Action | URL / Endpoint | Result | Matches Spec? |
|--------|----------------|--------|----------------|
| Access Logged Page | `/logged` | ❌ Unauthorized | ✔ Yes |
| Access Reservation Page | `/reservation` | ❌ Unauthorized | ✔ Yes |
| Access Profile Page | `/profile` | ❌ Unauthorized | ✔ Yes |
| Access Admin Dashboard | `/admin` | ❌ Unauthorized / Not Found | ✔ Yes |
| Access API Users | `/api/users` | ❌ Unauthorized | ✔ Yes |
| Access API Reservations | `/api/reservations` | ❌ Empty JSON | ✔ Yes |
| Access API Resources | `/api/resources` | ❌ Empty JSON | ✔ Yes |

## 2️⃣ Reserver Role Authorization Analysis

Reserver = Registered + Logged In

## Reserver – Allowed Actions

| Action | URL / Endpoint | Result | Matches Spec? |
|--------|----------------|--------|----------------|
| View Home Page | `/` | ✔ Works | ✔ Yes |
| Access Logged Page | `/logged` | ✔ Works | ✔ Yes |
| Access Reservation Page | `/reservation` | ✔ Works | ✔ Yes |
| View Resources | `/resources` | ✔ Works | ✔ Yes |


## Guest – API Behavior

| Endpoint | Result | Severity | Expected |
|----------|--------|----------|----------|
| `/api/users` | ❌ Returns user emails | 🔴 Critical | Should be blocked |
| `/api/resources` | ✔ Empty JSON | 🟡 OK | Public allowed |
| `/api/reservations` | ✔ Empty JSON | 🟡 OK | Allowed (spec 8) |


## Reserver – Blocked Actions (Correct)

| Action | URL / Endpoint | Result | Matches Spec? |
|--------|----------------|--------|----------------|
| Access Admin Dashboard | `/admin` | ❌ Not Found | ✔ Yes |
| Access Admin Resource Management | `/admin/resources` | ❌ Not Found | ✔ Yes |
| Access Admin User Management | `/admin/users` | ❌ Not Found | ✔ Yes |

## Reserver – API Behavior

| Endpoint | Result | Severity | Expected |
|----------|--------|----------|----------|
| `/api/users` | ❌ Returns user emails | 🔴 Critical | Only admin should access |
| `/api/resources` | ✔ Empty JSON | 🟡 OK | Allowed |
| `/api/reservations` | ✔ Empty JSON | 🟡 OK | Allowed |


## Reserver – Incorrect / Security Issues

| Endpoint | Result | Severity | Expected |
|----------|--------|----------|----------|
| `/api/users` | ❌ Displays user emails + IDs | 🔴 Critical | Should be admin-only |

⚠ Why this is serious:

A reserver can see all user identities

Violates GDPR (personal data exposure)

Breaks system specification (Only admins manage users)

## 3️⃣ Admin Role Authorization Analysis

Admin = Privileges should be highest, but current Phase 3 implementation is incomplete.

I discovered:

Admin pages are NOT available

Admin cannot access /admin/* endpoints

Admin UI does not exist

## Admin – Missing or Broken Features

| Missing Feature | Expected | Actual |
|----------------|----------|--------|
| Admin Dashboard | Should exist | ❌ Not Found |
| Manage Resources | Should be allowed | ❌ Not Found |
| Manage Reservations | Should be allowed | ❌ Not Found |
| Delete / Manage Users | Should be allowed | ❌ Not Found |

## Admin – API Behavior

| Endpoint | Result | Severity | Expected |
|----------|--------|----------|----------|
| `/api/users` | ✔ Returns user emails | ✔ OK | Admin should see users |
| `/api/resources` | ✔ Empty JSON | ✔ OK | No resources added yet |
| `/api/reservations` | ✔ Empty JSON | ✔ OK | No reservations yet |


🔴 This violates multiple project specifications:

Spec: Admin can manage resources → ❌ Not implemented

Spec: Admin can manage reservations → ❌ Not implemented

Spec: Admin can delete reserver → ❌ Not implemented

## 4️⃣ API Endpoint Summary Table

| Endpoint | Guest Response | Reserver Response | Admin Response | Severity | Notes |
|----------|----------------|-------------------|----------------|----------|--------|
| `/api/users` | ✔ Returns user email JSON | ✔ Returns user email JSON | ✔ Returns user email JSON | 🔴 CRITICAL | Should NOT be accessible by Guest/Reserver (GDPR violation) |
| `/api/resources` | ✔ Returns empty JSON | ✔ Returns empty JSON | ✔ Returns empty JSON | 🟡 Low | No data created yet |
| `/api/reservations` | ✔ Returns empty JSON | ✔ Returns empty JSON | ✔ Returns empty JSON | 🟡 Low | No data created yet |


## 5️⃣ Major Findings Summary (Short & Clear)
## API Security Findings (Updated)

| ID | Severity | Finding | Description |
|----|----------|----------|-------------|
| API-01 | 🔴 High | `/api/users` accessible by ALL roles | Guest + Reserver can see email/ID → GDPR violation |
| API-02 | 🟠 Medium | No role-based authorization in API | Backend does not enforce any role checks |
| API-03 | 🟡 Low | `/api/resources` always empty | Expected behavior if no resources created |
| API-04 | 🟡 Low | `/api/reservations` always empty | Expected if no reservations exist |


## 6️⃣ Conclusion

✔ Guest and Reserver permissions partially correct

❌ Admin permissions are completely missing

❌ API /api/users exposes user data to reserver (critical issue)

Overall Phase 3 Authorization Status: ❌ NOT SECURE

## 7️⃣ Attachments

zap_report_round4.md
