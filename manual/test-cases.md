# Test Cases

Bugs found while running these cases are linked by ID to [bug-reports.md](bug-reports.md). API cases can be run with the Postman collection in `postman/`.

## Login
| ID | Title | Preconditions | Steps | Expected result | Priority |
|---|---|---|---|---|---|
| TC-LOGIN-01 | Valid login | On login page | 1. Enter `standard_user` / `secret_sauce` 2. Click Login | Product page opens with 6 products | P1 |
| TC-LOGIN-02 | Locked-out user | On login page | 1. Enter `locked_out_user` / `secret_sauce` 2. Click Login | Error "Sorry, this user has been locked out." User stays on login page | P1 |
| TC-LOGIN-03 | Empty username | On login page | 1. Leave username empty, enter password 2. Click Login | Error "Username is required" | P2 |
| TC-LOGIN-04 | Empty password | On login page | 1. Enter username, leave password empty 2. Click Login | Error "Password is required" | P2 |
| TC-LOGIN-05 | Wrong password | On login page | 1. Enter `standard_user` / `wrong_password` 2. Click Login | Error "Username and password do not match…" | P1 |
| TC-LOGIN-06 | Unknown user | On login page | 1. Enter `unknown_user` / `secret_sauce` 2. Click Login | Same generic error (does not reveal which field is wrong) | P2 |
| TC-LOGIN-07 | Username with spaces | On login page | 1. Enter ` standard_user ` (leading/trailing spaces) 2. Click Login | Login rejected | P3 |
| TC-LOGIN-08 | Direct URL without login | Logged out | 1. Open `/inventory.html` directly | Redirected to login with an access error | P1 |

## Inventory
| ID | Title | Preconditions | Steps | Expected result | Priority |
|---|---|---|---|---|---|
| TC-INV-01 | Sort A→Z | Logged in | Select "Name (A to Z)" | Products in alphabetical order | P3 |
| TC-INV-02 | Sort Z→A | Logged in | Select "Name (Z to A)" | Products in reverse alphabetical order | P3 |
| TC-INV-03 | Sort price low→high | Logged in | Select "Price (low to high)" | Prices ascending | P3 |
| TC-INV-04 | Sort price high→low | Logged in | Select "Price (high to low)" | Prices descending | P3 |
| TC-INV-05 | Sorting for problem_user | Logged in as `problem_user` | Select "Price (low to high)" | Prices ascending. **Fails: BUG-001** | P3 |
| TC-INV-06 | Product images | Logged in as `problem_user` | View product list | Each product has its own image. **Fails: BUG-002** | P2 |

## Cart
| ID | Title | Preconditions | Steps | Expected result | Priority |
|---|---|---|---|---|---|
| TC-CART-01 | Add/remove updates badge | Logged in, empty cart | 1. Add Backpack 2. Add Bike Light 3. Remove Backpack 4. Remove Bike Light | Badge shows 1 → 2 → 1 → hidden | P2 |
| TC-CART-02 | Cart persists | Logged in | 1. Add Backpack 2. Open cart 3. Continue shopping 4. Reload page | Badge still shows 1 | P2 |

## Checkout
| ID | Title | Preconditions | Steps | Expected result | Priority |
|---|---|---|---|---|---|
| TC-CHK-01 | Full purchase | Logged in | 1. Add Backpack ($29.99) and Bike Light ($9.99) 2. Checkout 3. Enter name and postal code 4. Check totals 5. Finish | Item total $39.98, tax = 8%, total = item total + tax, "Thank you for your order!" | P1 |
| TC-CHK-02 | First name required | Item in cart, on checkout form | Leave First Name empty, Continue | Error "First Name is required" | P2 |
| TC-CHK-03 | Last name required | Item in cart, on checkout form | Leave Last Name empty, Continue | Error "Last Name is required" | P2 |
| TC-CHK-04 | Postal code required | Item in cart, on checkout form | Leave Postal Code empty, Continue | Error "Postal Code is required" | P2 |
| TC-CHK-05 | Last name input for problem_user | Logged in as `problem_user` | Type in Last Name | Text appears in Last Name. **Fails: BUG-003** | P1 |

## Booking API
| ID | Title | Request | Expected result | Priority |
|---|---|---|---|---|
| TC-API-01 | Health check | `GET /ping` | 201 | P1 |
| TC-API-02 | Full CRUD flow | POST → GET → PUT → PATCH → DELETE → GET | 200, 200, 200, 200, 201, then 404; body matches each request | P1 |
| TC-API-03 | Filter by name | `GET /booking?firstname=&lastname=` | Created booking ID is in the results | P2 |
| TC-API-04 | Update without token | `PUT /booking/{id}` without cookie | 403 | P1 |
| TC-API-05 | Delete with invalid token | `DELETE /booking/{id}` with `token=invalid` | 403 | P1 |
| TC-API-06 | Unknown ID | `GET /booking/999999999` | 404 | P2 |
| TC-API-07 | Bad credentials | `POST /auth` with wrong password | 401. **Fails: BUG-004** | P2 |
| TC-API-08 | Missing required field | `POST /booking` without `firstname` | 400. **Fails: BUG-005** | P2 |
