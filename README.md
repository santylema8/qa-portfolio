# QA Portfolio: Test Design, Bug Reporting and API Testing

This is a sample QA project by **Santiago Fernando Lema Orquera**, a QA engineer with 4+ years of experience and the ISTQB CTFL certification. It shows how I plan, design and run tests and report bugs, using two public practice apps:

| App | Type | What's tested |
|---|---|---|
| [SauceDemo](https://www.saucedemo.com) | Web store (UI) | Login, product sorting, cart, checkout, price/tax calculation |
| [Restful-Booker](https://restful-booker.herokuapp.com) | REST API | Auth, booking CRUD, filtering, error handling |

## What's inside

| File | Contents |
|---|---|
| [`manual/test-plan.md`](manual/test-plan.md) | Scope, risk-based priorities, test design techniques, entry/exit criteria |
| [`manual/test-cases.md`](manual/test-cases.md) | 29 test cases with preconditions, steps, expected results and priority |
| [`manual/bug-reports.md`](manual/bug-reports.md) | 5 bugs found, with severity, priority, steps to reproduce, expected vs. actual |
| [`postman/`](postman) | Postman collection with test scripts, a JSON schema check and chained variables (token, booking ID) |

## Highlights
- **ISTQB techniques:** equivalence partitioning, boundary values, state transitions and error guessing, applied and labeled in the test plan.
- **Risk-based priorities:** checkout totals, access control and API authorization are P1. Sorting is P3.
- **Traceability:** each bug links back to the test case that found it.
- **Business logic checks:** checkout verifies that item total + 8% tax = order total, not just that the page loads.
- **API testing in Postman:** status codes, response bodies, schema, response time, and negative cases (no token, invalid token, unknown ID, missing fields).

## Run the API tests
1. In Postman, import both files from `postman/`.
2. Select the **Restful-Booker (public demo)** environment.
3. Run the collection in order with the Collection Runner. The requests share the token and booking ID automatically.

Two checks fail on purpose, because they expose real bugs (BUG-004 and BUG-005).

## Tools
Postman · Jira-style bug reporting · Chrome DevTools · ISTQB test design techniques
