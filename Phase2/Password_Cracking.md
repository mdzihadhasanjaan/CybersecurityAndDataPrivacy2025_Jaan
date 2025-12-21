## 🛡️ Phase 2 – Step 3: Password Cracking Report
## 👤 Tester

Md Rasedul Islam and Zihad Hasan Zaan

## 1️⃣ Introduction

The goal of this task was to analyze the password hashes extracted from the Booking System database and attempt to crack at least five of them using any chosen methods.
This helps evaluate password strength, hashing method security, and resistance against real-world cracking attempts.

## 2️⃣ Hash Extraction

1. 5f4dcc3b5aa765d61d8327deb882cf99
2. 202cb962ac59075b964b07152d234b70
3. 25d55ad283aa400af464c76d713c07ad
4. e10adc3949ba59abbe56e057f20f883e
5. d8578edf8458ce06fbc5bb76a58c5ca4
6. 21232f297a57a5a743894a0e4a801fc3
7. 900150983cd24fb0d6963f7d28e17f72

## 3️⃣ Cracking Attempts (Online Tool and Offline Tool)

## Offline Tool
👉 Hashcat failed to crack the password because my laptop does not have enough GPU/CPU memory.
The OpenCL backend could not initialize, showing:
![Hashcat Failed Screenshot](../Phase%202/Screenshot_87.png)
Therefore, the dictionary attack could not run.


## Online Tool
For this reason, I used online tools to crack some common easy passwords.

👉 CrackStation.net

## 4️⃣ Results Screenshot & Explanations

## Screenshots
![Cracked Screenshot](../Phase%202/Cracked_password.png)



| Hash | Tool Used | Result | Explanation |
|------|-----------|--------|-------------|
| `5f4dcc3b5aa765d61d8327deb882cf99` | CrackStation | ✅ password | Very weak MD5 hash |
| `202cb962ac59075b964b07152d234b70` | CrackStation | ✅ 123 | Common password |
| `25d55ad283aa400af464c76d713c07ad` | CrackStation | ✅ 12345678 | Found in all dictionaries |
| `e10adc3949ba59abbe56e057f20f883e` | CrackStation | ✅ 123456 | Extremely weak |
| `d8578edf8458ce06fbc5bb76a58c5ca4` | CrackStation | ✅ qwerty | Known weak password |
| `21232f297a57a5a743894a0e4a801fc3` | CrackStation | ✅ admin | Typical default password |
| `900150983cd24fb0d6963f7d28e17f72` | CrackStation | ✅ abc | Easily guessable |



## 6️⃣ Concept Questions (Teacher Required)

## ❓ Q1: What is the difference between Dictionary and Non-Dictionary attacks?

| Dictionary Attack | Non-Dictionary Attack |
|-------------------|------------------------|
| Uses a list of known/common passwords | Tries every possible combination |
| Very fast | Slow, especially with long passwords |
| Effective only if the password is common | Can crack any password given enough time |
| Example: `"password123"`, `"iloveyou"` | Example: brute-forcing `aaa → zzz` |



## ❓ Q2: What advantage does an attacker gain by having access to password hashes?

They can perform offline brute-force attacks without rate limits

No lockout, no CAPTCHA, no monitoring

They can test millions of passwords per second

They see which accounts share similar hash patterns

If hashing is weak (MD5/SHA1), passwords may be cracked within seconds

In summary:
Access to hashes turns a slow, protected login system into a fast offline cracking problem.



## ❓ Q3: What are the security benefits of using longer passwords?

Increases the number of possible combinations exponentially

Makes brute-force attacks significantly slower

Harder to guess, harder to include in dictionary lists

More resistant to GPU cracking

Even if the hashing algorithm is weak, the length still gives major protection

Example:

6-character password → crackable in minutes

12-character password → can take thousands of years



## 7️⃣ Conclusion

The cracking process showed:

MD5 is extremely weak and unsuitable for password storage

All tested hashes were cracked instantly

The system must upgrade to secure hashing (bcrypt, Argon2, PBKDF2)

Strong password policies are mandatory

The cracked passwords highlight severe security risks and the need for modern security practices.
