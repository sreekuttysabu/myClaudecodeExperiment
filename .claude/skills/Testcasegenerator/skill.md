---
name: create-scenarios
description: Generate functional test scenarios from domain knowledge using 6 thinking lenses
disable-model-invocation: true
argument-hint: "[feature-name or blank for full suite]"
---

You are a test scenario generator. Using the 6 thinking lenses below, generate structured functional test scenarios from the requirements provided.

**6 Thinking Lenses:**
1. Happy path — expected successful flows
2. Negative/error — invalid inputs and failure states
3. Edge cases — boundary conditions and unusual inputs
4. Security — authentication, authorization, and data protection
5. Performance — behavior under load or slow conditions
6. UX/Accessibility — user-facing messages and accessibility concerns

For each scenario, output in this format:

| ID | Title | Preconditions | Steps | Expected Result | Lens |
|----|-------|---------------|-------|-----------------|------|

---

## Requirements and Flows

Below are the requirements to generate scenarios from:

### Login Flow

1. User navigates to /login or clicks "Sign in" from the header.
2. User enters email and password, clicks "Sign in".
3. On success, user is redirected to /home or the previously intended page (redirect after login).

### Form Fields

| Field       | Required | Notes                                      |
|-------------|----------|--------------------------------------------|
| Email       | Required | Must match a registered account.           |
| Password    | Required | Case-sensitive.                            |
| Remember me | Optional | Checkbox. Keeps session alive for 30 days. |

### Business Rules

- **Account lockout** — after 5 consecutive failed login attempts, the account is locked for 15 minutes. User sees: "Your account has been temporarily locked. Try again in 15 minutes or reset your password."
- **Forgot password** — link on login page navigates to /forgot-password. User enters email; if it exists a reset link is sent (valid 1 hour). If email is not found, same success message is shown (to prevent email enumeration).
- **Session timeout** — inactive sessions expire after 30 minutes. User is redirected to /login with message: "Your session has expired. Please sign in again."
- **Redirect after login** — successful login redirects to the last visited page if the user was redirected to login mid-journey (e.g. from cart).
- **Unverified accounts** — login attempt shows: "Please verify your email before logging in. Resend verification email."

### Error Messages

| Scenario              | Message shown                                               |
|-----------------------|-------------------------------------------------------------|
| Wrong email or password | "Incorrect email or password." (generic — does not reveal which is wrong) |
| Account locked        | "Account temporarily locked. Try again in 15 minutes."      |
| Unverified account    | "Please verify your email to continue."                     |
| Empty fields          | "This field is required."                                   |
