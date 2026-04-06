# Test Cases — Login Feature

Generated using the 6 thinking lenses: Happy path, Negative/error, Edge cases, Security, Performance, UX/Accessibility.

---

| ID | Title | Preconditions | Steps | Expected Result | Lens |
|----|-------|---------------|-------|-----------------|------|
| TC-01 | Successful login with valid credentials | Registered, verified account exists | 1. Navigate to /login 2. Enter valid email & password 3. Click "Sign in" | User redirected to /home | Happy path |
| TC-02 | Successful login via header "Sign in" link | User is on any page | 1. Click "Sign in" in header 2. Enter valid credentials 3. Click "Sign in" | User redirected to /home | Happy path |
| TC-03 | Remember me keeps session for 30 days | Registered, verified account | 1. Login with valid credentials 2. Check "Remember me" 3. Close browser and reopen after 1 day | User still logged in without re-authenticating | Happy path |
| TC-04 | Redirect to intended page after login | User tries to access /cart while logged out | 1. Navigate to /cart (redirected to /login) 2. Enter valid credentials 3. Click "Sign in" | User redirected to /cart | Happy path |
| TC-05 | Login with wrong password | Registered account exists | 1. Navigate to /login 2. Enter valid email, wrong password 3. Click "Sign in" | Error: "Incorrect email or password." shown | Negative/error |
| TC-06 | Login with unregistered email | No account for email | 1. Navigate to /login 2. Enter unknown email & any password 3. Click "Sign in" | Error: "Incorrect email or password." shown | Negative/error |
| TC-07 | Login with empty email field | User on /login | 1. Leave email blank 2. Enter password 3. Click "Sign in" | Error: "This field is required." on email field | Negative/error |
| TC-08 | Login with empty password field | User on /login | 1. Enter email 2. Leave password blank 3. Click "Sign in" | Error: "This field is required." on password field | Negative/error |
| TC-09 | Login with both fields empty | User on /login | 1. Leave both fields blank 2. Click "Sign in" | "This field is required." shown on both fields | Negative/error |
| TC-10 | Unverified account login attempt | Account created but email not verified | 1. Navigate to /login 2. Enter credentials of unverified account 3. Click "Sign in" | Error: "Please verify your email to continue." with resend link | Negative/error |
| TC-11 | Account lockout after 5 failed attempts | Valid account exists | 1. Enter wrong password 5 times consecutively | Account locked; message: "Your account has been temporarily locked. Try again in 15 minutes or reset your password." | Negative/error |
| TC-12 | Account unlocks after 15 minutes | Account was locked | 1. Wait 15 minutes after lockout 2. Enter correct credentials | Login succeeds, user redirected to /home | Edge case |
| TC-13 | Login attempt on locked account before 15 min | Account locked | 1. Attempt login within 15-minute lockout window | Error: "Account temporarily locked. Try again in 15 minutes." | Edge case |
| TC-14 | 4th failed attempt does not lock account | Valid account exists | 1. Enter wrong password 4 times 2. Enter correct password on 5th attempt | Login succeeds; no lockout | Edge case |
| TC-15 | Password is case-sensitive | Registered account exists | 1. Enter email 2. Enter password with wrong case 3. Click "Sign in" | Error: "Incorrect email or password." | Edge case |
| TC-16 | Session timeout after 30 min inactivity | User is logged in | 1. Leave session idle for 30 minutes | User redirected to /login with: "Your session has expired. Please sign in again." | Edge case |
| TC-17 | Forgot password with registered email | Registered account exists | 1. Click "Forgot password" on /login 2. Enter registered email 3. Submit | Success message shown; reset email sent (link valid 1 hour) | Happy path |
| TC-18 | Forgot password with unregistered email | No account for email | 1. Click "Forgot password" 2. Enter unknown email 3. Submit | Same success message shown (no email enumeration) | Security |
| TC-19 | Forgot password reset link expires after 1 hour | Reset link sent | 1. Wait >1 hour 2. Click reset link | Link expired; user prompted to request a new one | Edge case |
| TC-20 | SQL injection in email field | User on /login | 1. Enter `' OR '1'='1` in email field 2. Enter any password 3. Submit | Login fails; no unauthorized access granted | Security |
| TC-21 | XSS in email field | User on /login | 1. Enter `<script>alert(1)</script>` in email 2. Submit | Input sanitized; no script executes | Security |
| TC-22 | Brute force beyond lockout threshold | Locked account | 1. Continue submitting attempts after lockout | All attempts rejected; lockout timer does not reset | Security |
| TC-23 | Forgot password does not reveal account existence | Unknown email submitted | 1. Submit unregistered email on /forgot-password | Same success message as registered email (prevents enumeration) | Security |
| TC-24 | Login page load under high concurrency | Server under load (e.g. 500 concurrent users) | 1. Simulate concurrent login requests | Page loads and responds within acceptable time; no 500 errors | Performance |
| TC-25 | Session token not exposed in URL | User logs in successfully | 1. Complete login 2. Inspect browser URL and network requests | Session token not visible in URL query parameters | Security |
| TC-26 | Error messages are screen-reader accessible | User on /login with screen reader | 1. Submit invalid credentials 2. Check error announcement | Error messages announced by screen reader; form fields have proper ARIA labels | UX/Accessibility |
| TC-27 | Login form is keyboard navigable | User on /login | 1. Use Tab to move between Email, Password, Remember me, Sign in button 2. Submit with Enter | All fields reachable and submittable via keyboard only | UX/Accessibility |
| TC-28 | "Resend verification email" link is functional | Unverified account login attempt | 1. Trigger unverified account error 2. Click "Resend verification email" | Verification email re-sent; confirmation shown to user | UX/Accessibility |
| TC-29 | Session expiry message is clear and actionable | Session times out mid-journey | 1. Let session expire 2. Observe redirect to /login | Message "Your session has expired. Please sign in again." is visible and prominent | UX/Accessibility |

---

## Testing Pyramid Categorization

| ID | Title | Level | Reason |
|----|-------|-------|--------|
| TC-01 | Successful login with valid credentials | API | Core auth endpoint behavior; no browser needed |
| TC-02 | Successful login via header "Sign in" link | UI | Requires browser navigation and header interaction |
| TC-03 | Remember me keeps session for 30 days | API | Cookie/token persistence is server-set; verifiable via HTTP headers |
| TC-04 | Redirect to intended page after login | API | Redirect logic is server-side (302/Location header) |
| TC-05 | Login with wrong password | API | Error response from auth endpoint; no UI needed |
| TC-06 | Login with unregistered email | API | Auth endpoint returns generic error; no UI needed |
| TC-07 | Login with empty email field | Unit | Input validation logic; pure function, no network |
| TC-08 | Login with empty password field | Unit | Input validation logic; pure function, no network |
| TC-09 | Login with both fields empty | Unit | Input validation logic; pure function, no network |
| TC-10 | Unverified account login attempt | API | Server checks verification status and returns correct response |
| TC-11 | Account lockout after 5 failed attempts | Unit | Lockout counter logic is a pure business rule |
| TC-12 | Account unlocks after 15 minutes | Unit | Timer/expiry logic is isolated business rule |
| TC-13 | Login attempt on locked account before 15 min | API | Lockout enforcement checked at auth endpoint |
| TC-14 | 4th failed attempt does not lock account | Unit | Counter boundary check — pure business rule |
| TC-15 | Password is case-sensitive | Unit | String comparison logic; no network needed |
| TC-16 | Session timeout after 30 min inactivity | API | Session expiry enforced server-side; redirect via HTTP response |
| TC-17 | Forgot password with registered email | API | Endpoint triggers email and returns response |
| TC-18 | Forgot password with unregistered email | API | Endpoint returns same response to prevent enumeration |
| TC-19 | Forgot password reset link expires after 1 hour | Unit | Token expiry timestamp check — pure business rule |
| TC-20 | SQL injection in email field | API | Input sanitization/parameterized queries enforced at server layer |
| TC-21 | XSS in email field | API | Output encoding enforced server-side; verifiable at API level |
| TC-22 | Brute force beyond lockout threshold | API | Rate limiting and lockout persistence enforced server-side |
| TC-23 | Forgot password does not reveal account existence | API | Consistent API response regardless of email existence |
| TC-24 | Login page load under high concurrency | API | Load test against auth endpoint directly |
| TC-25 | Session token not exposed in URL | API | Inspect HTTP response headers and redirect URL |
| TC-26 | Error messages are screen-reader accessible | UI | Requires real browser + ARIA inspection |
| TC-27 | Login form is keyboard navigable | UI | Requires browser to test Tab order and form submission |
| TC-28 | "Resend verification email" link is functional | UI | Requires browser interaction with rendered error state |
| TC-29 | Session expiry message is clear and actionable | UI | Requires browser to verify visible message and redirect |

---

### Testing Pyramid Summary

| Level | Count | % of Total |
|-------|-------|------------|
| Unit  | 8     | 28%        |
| API   | 16    | 55%        |
| UI    | 5     | 17%        |

---

### Coverage Gaps & Recommendations

**Push-down opportunities:**
- TC-21 (XSS) is sometimes done via browser — keep at API level since output encoding is a server concern.

**Unit layer gaps to consider:**
- Email format validation (regex) — dedicated unit tests for valid/invalid email formats.
- Password minimum length/complexity rules — pure unit-testable if rules exist.
- Reset token generation uniqueness — unit test the token generation function.

**API layer gaps to consider:**
- Auth endpoint single-request latency baseline (beyond TC-24 concurrency).
- Concurrent lockout race condition — two simultaneous failed requests on attempt 4/5; ensure counter is atomic.
- JWT/session token signing and verification — dedicated API-level contract test.

**UI layer is lean (17%) — healthy.** The 5 UI tests cover critical user-visible flows.
