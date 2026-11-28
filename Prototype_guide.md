Joshua Samuel\
Talan Aria\
Brent Grossman

# Tool Library Prototype Guide

A quick reference guide for navigating the County Tool Library — Interactive Prototype HTML file.

 Here is the link to [prototype](https://rubberducky-66.github.io/Tool-Library-project-for-CS361/county_tool_library_prototype.html) 

---

## 1. Test IDs & Passwords

### Staff Login (`2. Staff Login`)
In the prototype, any non-empty username and password logs you in. When first loading the page (or refreshing) you will be automatically logged in as Jordan Lee and will be limited to clerk pages. This will restrict access from Staff Role Administration and Inventory Managment pages unless logging in at the appropraite role authorization level. Matching a seeded employee ID loads that account's role:

| Employee Name | Username / ID | Password | Role | Access Notes |
| :--- | :--- | :--- | :--- | :--- |
| **Jordan Lee** | `jlee` | *(any non-empty text, e.g., `prototype`)* | **Clerk** | Default pre-filled user; blocked from Inventory & Staff Admin |
| **Chloe Adams** | `cadams` | *(any non-empty text)* | **Cataloger** | Can access Inventory; blocked from Staff Admin |
| **Brenda Wells** | `bwells` | *(any non-empty text)* | **Manager** | Full access including Staff Admin |


### Patron Card Numbers
Reference date in the prototype logic: **Nov 25, 2026** (`2026-11-25`).

| Card # | Patron Name | Age | Waiver | Active Loans | Notes / Test Scenario |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `2841` | **Alex Morgan** | 34 | Signed | `TL-00118` (due `2026-11-23`), `TL-00409` (due `2026-11-21`) | Overdue loans (due before `2026-11-25`) — useful for testing checkout restrictions and check-in late fees |
| `1930` | **Sam Rivera** | 29 | Signed | `TL-00620` (due `2026-11-28`) | Active loan not yet due; good for standard check-in |
| `4502` | **Jamie Chen** | 42 | Not signed | None | Good for testing waiver signing (`openWaiver()`) and waiver-blocked checkouts |
| `7716` | **Taylor Youth** | 16 | Not signed | None | Under 18 — triggers age restriction blocks |
| `5520` | **Maya Patel** | 38 | Signed | None (`notes` says `"5 active tool loans"`) | `loans` array is `[]`, so the 5-loan cap won't trigger automatically unless populated |

### Sample Tool Barcodes
* `TL-00421` — **Cordless Drill Kit** (`Ready to Borrow`, 7-day loan, 4 components, $1/day rate)
* `TL-00118` — **Circular Saw** (`Checked Out` to Alex Morgan `2841`, due `2026-11-23`, 2 waiting)
* `TL-00409` — **Pressure Washer** (`Checked Out` to Alex Morgan `2841`, due `2026-11-21`, $3/day rate)
* `TL-00620` — **Wet Tile Saw** (`Checked Out` to Sam Rivera `1930`, due `2026-11-28`, $3/day rate)
* `TL-00333` — **Lawn Mower** (`In Maintenance`, 14-day loan, $3/day rate)
* `TL-00502`, `TL-00710`, `TL-00801`, `TL-00822` — Additional `Ready to Borrow` items

---

## 2. How to Navigate & Reach Features

### Top Bar & Launcher
* **All views**: Returns to the Prototype view selection grid from any screen.
* **Role / View badge**: Shows the active view or role (`View selection`, `Patron`, `Staff sign in`, or the signed-in staff role).

### Patron Features
1. Entering `1. Patron Catalog & Account` demonstrates the features for the patron portion of the system.
   * Filter tools via **Tool Type**, **Category**, or **Availability** checkboxes on the left side. There is also a search bar and location filter on the top.
   * To join a waitlist or reserve a tool click **Join Waitlist / Reserve** or **Join Waitlist** on any tool card, enter a patron card number (such as `4502`), and confirm.
2. Selecting `My Account` allows patrons to manage their current loans and holds, sign waivers, and view fees.
   * The **Renew Loan** button will work once per item unless the waitlist has someone in it. **Cancel Hold** cancels an active hold or leaves the waitlist. **Review / Sign Waiver** pulls up a sample waiver for the patron to sign. The bottom of the tab shows late fees and directs payments to the service desk.
3. **Electronic Liability Waiver**
   * Open via the **Electronic Liability Waiver** button, **My Account -> Review / Sign Waiver**, or **Open waiver kiosk** on the **Tool Check-Out** screen.

### Staff Features
1. **Staff Login**:
* Returning to All views, select `2. Staff Login` to view the login screen. It is recomended to use the County Employee ID / Username `bwells` for full acess during this guide if you haven't done so already.
2. **Staff Dashboard**:
* This screen can be accessed either by signing in on the staff login screen or from the All views menu `3. Main Staff Dashboard`. Once inside the staff workspace, use the left **STAFF MENU**.
* The dashboard shows the number of items in maintenance, the items currently on hold/when the hold expires, and a quick search for patrons (by name or card #, such as `1930` or `Sam Rivera`) to open **Place waitlist hold** (for staff to assist with reservations).
3. **Check Out Tools**:
* This screen will be accessible from the **STAFF MENU** on the side or from All views for quick prototype access (`4. Tool Check-Out`).
* The workflow for check-out:
  1. Enter a card number (example `2841`) and click **Verify Patron**.
  2. Check **Photo ID verified** and confirm waiver status.
  3. Enter a tool barcode (such as `TL-00421`) in **Scan tool / kit barcode** and click **Add to cart** (up to 5 total loans).
  4. Click **Complete Checkout**.
* If a patron has a restriction, then checkout will be blocked until it is dealt with.
4. **Check In Tools**:
* This screen will be accessible from the **STAFF MENU** on the side or from All views for quick prototype access (`5. Tool Check-In & Maintenance`).
* The workflow for check-in:
  1. Enter a checked-out barcode (such as `TL-00409` or `TL-00118`) and click **Load tool**.
  2. Uncheck any missing kit items in **Kit contents checklist**, review late fees, and use **Mark Paid In-Person** or **Mark Item Lost** if needed.
  3. Select **Ready to Borrow** or **In Maintenance**, check **Needs servicing** as needed, enter **Condition notes** (required if a kit item is unchecked), and click **Complete Check-In**.
5. **Inventory Management**:
* Only accessible by Cataloger & Manager.
* This screen will be accessible from the **STAFF MENU** on the side or from All views for quick prototype access (`6. Inventory Managment`).
* Search with **Search inventory**, add an item via **+ Add New / Donated Tool**, click any row to edit, click **Retire** on non-checked-out tools, or use **Print Barcode Label** and **Export / Print List** (CSV).
6. **Reports**:
* This screen will be accessible from the **STAFF MENU** on the side or from All views for quick prototype access (`7. Operational Reports`).
* Switch between **Overdue Items**, **Currently Checked Out**, **Tools in Repair**, and **Most-Borrowed Tools**, then use **Export CSV** or **Print Report**.
7. **Staff Admin**:
* Only accessible by Manager.
* This screen will be accessible from the **STAFF MENU** on the side or from All views for quick prototype access (`Staff Role Administration`).
* Click **Add Staff Member**, change roles in the role dropdown (`Clerk`, `Cataloger`, `Manager`), or click **Remove / Restore**.

---
