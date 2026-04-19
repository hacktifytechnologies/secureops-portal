# Assessment — SecureOps Portal: OTP Bypass + JWT Escalation + SSRF Chain

---

## Question 1 — MCQ

**How was the target user's email address discovered?**

- A) SQL Injection against the login endpoint
- B) **A public bulletin board visible without authentication** ✅
- C) DNS lookup of the portal domain
- D) Password reset response leaking registered emails

> **Answer:** B — The portal's public bulletin section displays `john@secureops.in` without requiring authentication, providing a valid target for the account recovery flow.

---

## Question 2 — MCQ

**What was the hardcoded OTP value used to bypass account recovery?**

- A) `0000`
- B) `1234`
- C) **`7255`** ✅
- D) `9999`

> **Answer:** C — The OTP for `john@secureops.in` is hardcoded as `7255` with no rate limiting or expiry.

---

## Question 3 — MCQ

**Which SSRF bypass technique reached the internal service after `http://127.0.0.1` was blocked?**

- A) Using HTTPS instead of HTTP
- B) DNS rebinding
- C) **An alternative IP representation such as `http://127.1/` or `http://[::]:5000/`** ✅
- D) Sending via POST instead of GET

> **Answer:** C — The blacklist matches `127.0.0.1` and `localhost` literally but not `127.1` or `[::]`, which are functionally equivalent loopback representations.

---

## Question 4 — Fill in the Blank

**What is the hardcoded OTP value used to bypass the account recovery step for `john@secureops.in`?**

**Answer:** `7255`

> The OTP is hardcoded in the application with no rate limiting or expiry. Submitting `7255` on the account recovery form for `john@secureops.in` immediately grants password reset access without requiring the real OTP delivery mechanism.

---

## Question 5 — Fill in the Blank

**What weak secret is used to sign JWTs in the SecureOps Portal, enabling privilege escalation to admin?**

**Answer:** `secret123`

> After logging in, the `auth_token` JWT can be decoded, the `"role"` field changed from `"user"` to `"admin"`, and the token re-signed using this weak secret. The server validates the new token and grants admin access.

---

*Lab target:* `http://localhost:8088`
