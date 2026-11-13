# Bug Reports

Found through exploratory and scripted testing. Each bug links to the test case that found it, so it can be retested once fixed. API bugs also have a failing check in the Postman collection.

---

## BUG-001: Product sorting has no effect for `problem_user`
| Field | Value |
|---|---|
| Severity | Minor |
| Priority | P3 |
| Environment | saucedemo.com, Chrome (desktop) |
| Test case | TC-INV-05 |

**Steps to reproduce**
1. Log in as `problem_user` / `secret_sauce`
2. On the Products page, choose "Price (low to high)" from the sort dropdown

**Expected:** Products are reordered from lowest to highest price.
**Actual:** The order does not change. No error is shown.
**Notes:** Works correctly for `standard_user`, so the bug is account/state specific.

---

## BUG-002: All products show the same image for `problem_user`
| Field | Value |
|---|---|
| Severity | Major |
| Priority | P2 |
| Environment | saucedemo.com, Chrome (desktop) |
| Test case | TC-INV-06 |

**Steps to reproduce**
1. Log in as `problem_user` / `secret_sauce`
2. Look at the product list

**Expected:** Each product shows its own photo.
**Actual:** Every product shows the same image, so customers can't tell products apart visually.

---

## BUG-003: Typing in "Last Name" overwrites "First Name" at checkout
| Field | Value |
|---|---|
| Severity | Critical (blocks purchase) |
| Priority | P1 |
| Environment | saucedemo.com, Chrome (desktop) |
| Test case | TC-CHK-05 |

**Steps to reproduce**
1. Log in as `problem_user` / `secret_sauce`
2. Add any item to the cart, open the cart, click Checkout
3. Click the Last Name field and type `Tester`

**Expected:** "Tester" appears in Last Name.
**Actual:** Last Name stays empty and the text goes into First Name instead. Since Last Name is required, the user cannot complete checkout.

---

## BUG-004: `/auth` returns 200 OK for wrong credentials
| Field | Value |
|---|---|
| Severity | Minor |
| Priority | P2 |
| Environment | restful-booker.herokuapp.com |
| Test case | TC-API-07 |

**Request**
```http
POST /auth
Content-Type: application/json

{ "username": "admin", "password": "wrong" }
```
**Expected:** `401 Unauthorized`
**Actual:** `200 OK` with body `{"reason":"Bad credentials"}`
**Impact:** Clients that check only the status code will treat a failed login as a success.

---

## BUG-005: Missing required field returns 500 instead of 400
| Field | Value |
|---|---|
| Severity | Major |
| Priority | P2 |
| Environment | restful-booker.herokuapp.com |
| Test case | TC-API-08 |

**Request:** `POST /booking` with a valid body but without `firstname`.
**Expected:** `400 Bad Request` with a message naming the missing field.
**Actual:** `500 Internal Server Error`.
**Impact:** The server isn't validating input, and the client gets no hint about what to fix.
