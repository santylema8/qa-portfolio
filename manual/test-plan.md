# Test Plan: SauceDemo web store and Restful-Booker API

| | |
|---|---|
| **Author** | Santiago Fernando Lema Orquera |
| **Applications under test** | [SauceDemo](https://www.saucedemo.com) (web UI) and [Restful-Booker](https://restful-booker.herokuapp.com) (REST API) |
| **Test levels** | System, API, regression |
| **Approach** | Risk-based. Exploratory testing first, then scripted test cases; API checks scripted in Postman |

## 1. Objective
Confirm that a customer can log in, browse products, manage a cart and complete a purchase with correct totals. Confirm that the booking API handles create, read, update and delete correctly and enforces authentication.

## 2. Scope

**In scope**
- Login: valid users, locked users, field validation, access control
- Product list: display and sorting (name and price)
- Cart: add, remove, persistence
- Checkout: required fields, price/tax/total calculation, order confirmation
- Booking API: auth token, CRUD, filtering, error handling (401/403/404/400)
- Mobile browser smoke test (login and checkout)

**Out of scope**
- Payment processing (the demo app has none)
- Load/performance testing (only response-time checks)
- Accessibility audit (planned as a follow-up)

## 3. Risks and priorities
| Area | Risk | Priority | Why |
|---|---|---|---|
| Checkout totals | High | P1 | A wrong price or tax directly costs money and trust |
| Login / access control | High | P1 | Security; users must not reach pages without logging in |
| API auth on update/delete | High | P1 | Anyone could change or delete bookings |
| Cart | Medium | P2 | Lost cart items reduce sales |
| Sorting | Low | P3 | Inconvenient but has a workaround |

## 4. Test design techniques
- **Equivalence partitioning:** valid, empty and wrong values for username, password and checkout fields
- **Boundary values:** whitespace around the username, empty strings
- **State transitions:** cart badge 0 → 1 → 2 → 1 → 0
- **Error guessing:** special test users (`problem_user`, `locked_out_user`), invalid tokens, unknown IDs
- **End-to-end scenarios:** full purchase flow, full booking lifecycle

## 5. Environment
- Chrome (desktop) and Chrome on Android for the mobile smoke test
- Postman for API testing (collection runner)
- Bugs tracked with severity and priority (Jira-style format)

## 6. Entry and exit criteria
**Entry:** app is reachable, test data (demo users, admin credentials) is available.
**Exit:** all P1 cases executed and passing, all failures logged as bugs with steps to reproduce, test report published.

## 7. Deliverables
- [Test cases](test-cases.md)
- [Bug reports](bug-reports.md)
- Postman collection with test scripts in `postman/`
- Test summary (pass/fail counts and open bugs)
