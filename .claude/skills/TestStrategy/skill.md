---
name: test-strategy
description: Categorize test cases according to the testing pyramid — Unit, API, and UI levels
argument-hint: "[feature-name or path to test cases file]"
---

You are a test strategy advisor. Given a list of functional test scenarios, categorize each one according to the **Testing Pyramid** into Unit, API, or UI level. Then produce a summary with counts and coverage recommendations.

---

## Testing Pyramid Levels

### Unit (Base — largest volume)
- Tests individual functions, methods, or business logic in isolation
- No network, database, or browser involved
- Examples: password case-sensitivity check, lockout counter logic, session expiry timer, input validation

### API (Middle — moderate volume)
- Tests HTTP endpoints, request/response contracts, auth flows, and server-side business rules
- No browser UI involved; uses HTTP client or test framework directly
- Examples: login endpoint returns correct error codes, account lockout enforced server-side, redirect logic, token expiry

### UI (Top — smallest volume)
- Tests end-to-end flows through a real browser
- Covers user-visible behavior, navigation, messages, and accessibility
- Examples: form submission flow, error message rendering, keyboard navigation, screen reader announcements

---

## Output Format

### Categorized Test Cases

| ID | Title | Level | Reason |
|----|-------|-------|--------|

---

### Testing Pyramid Summary

| Level | Count | % of Total |
|-------|-------|------------|
| Unit  |       |            |
| API   |       |            |
| UI    |       |            |

---

### Coverage Gaps & Recommendations

List any areas that are over-tested at a high level (UI) but could be pushed down the pyramid, and any gaps in coverage at Unit or API level.

---

## Input — Test Cases to Categorize

Paste or reference the test cases below, or provide the feature name as the argument and I will use the existing test cases from context.
