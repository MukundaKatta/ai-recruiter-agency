# Restaurant Order Management System — UI/UX Deliverables

## 1. Screen List

| # | Screen | Purpose | Primary Users | Device |
|---|--------|---------|--------------|--------|
| 1 | **Login** | Authenticate user, route to role-appropriate dashboard | All | All |
| 2 | **Dashboard (Home)** | Role-specific landing page with key metrics and quick actions | All | All |
| 3 | **Table Map** | Visual grid of tables with color-coded status; tap to create/view order | Waiter, Cashier | Tablet, Phone |
| 4 | **New Order** | Create order: select type, table, add items/modifiers, customer info | Waiter, Cashier | Tablet, Phone |
| 5 | **Menu Browser** | Browse categories → items → modifiers; add to current order | Waiter, Cashier | Tablet, Phone |
| 6 | **Order Detail** | View/edit a single order: items, status, notes, payment status | All (read), Waiter/Cashier (edit) | All |
| 7 | **Order List** | Filterable list of all orders (today's by default) | Cashier, Manager | Desktop, Tablet |
| 8 | **Kitchen Display (KDS)** | Real-time order queue for kitchen staff | Kitchen | Wall tablet |
| 9 | **Checkout / Payment** | Calculate bill, select payment method, process payment | Cashier | Desktop, Tablet |
| 10 | **Receipt View** | Digital receipt display with print button | Cashier | Desktop, Tablet |
| 11 | **Menu Management** | CRUD for categories, items, modifiers, availability | Admin | Desktop |
| 12 | **Table Management** | CRUD for tables, sections, capacity | Admin | Desktop |
| 13 | **User Management** | CRUD for staff accounts and roles | Admin | Desktop |
| 14 | **Reports Dashboard** | Charts and tables: sales, popular items, cancellations | Manager, Admin | Desktop |
| 15 | **Settings** | Restaurant info, tax config, payment gateway config, printer config | Admin | Desktop |

---

## 2. Low-Fidelity Wireframe Descriptions

### Screen 1: Login
```
┌──────────────────────────────────┐
│         🍽  RestaurantOS          │
│                                  │
│   ┌──────────────────────────┐   │
│   │  Email                   │   │
│   └──────────────────────────┘   │
│   ┌──────────────────────────┐   │
│   │  Password            👁  │   │
│   └──────────────────────────┘   │
│                                  │
│   ┌──────────────────────────┐   │
│   │       LOG IN             │   │
│   └──────────────────────────┘   │
│                                  │
│   Forgot password?               │
└──────────────────────────────────┘
```
- Clean, centered card layout
- Restaurant logo/name at top
- Large touch-friendly input fields (48px height)
- "Forgot password?" link below button

---

### Screen 2: Dashboard (Home)
```
┌─ HEADER: Logo | "Dashboard" | Role Badge | User Avatar ──────────┐
├─ SIDEBAR (desktop) / BOTTOM NAV (mobile) ────────────────────────┤
│                                                                   │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐               │
│  │ Orders  │ │ Revenue │ │ Avg Val │ │ Active  │               │
│  │   42    │ │ ₹12,450 │ │  ₹296   │ │    7    │               │
│  │ today   │ │ today   │ │ today   │ │ orders  │               │
│  └─────────┘ └─────────┘ └─────────┘ └─────────┘               │
│                                                                   │
│  Quick Actions:                                                   │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐           │
│  │ + New Order  │  │  Table Map   │  │   Kitchen    │           │
│  └──────────────┘  └──────────────┘  └──────────────┘           │
│                                                                   │
│  Recent Orders:                                                   │
│  ┌────┬────────┬──────────┬──────────┬────────┐                  │
│  │ #  │ Type   │ Table/Cust│ Total   │ Status │                  │
│  ├────┼────────┼──────────┼──────────┼────────┤                  │
│  │023 │ Dine-in│ Table 5  │ ₹650    │ Ready  │                  │
│  │022 │ Takeout│ Raj K.   │ ₹420    │ Prep.. │                  │
│  │021 │ Deliver│ Priya M. │ ₹890    │ New    │                  │
│  └────┴────────┴──────────┴──────────┴────────┘                  │
└──────────────────────────────────────────────────────────────────┘
```
- 4 metric cards at top (contextual to role)
- Quick action buttons (large, prominent)
- Recent orders table with status badges (color-coded)
- Waiter sees only their assigned tables; Kitchen sees queue shortcut

---

### Screen 3: Table Map
```
┌─ HEADER: "Tables" | Floor Filter: [All] [Main] [Patio] ─────────┐
│                                                                   │
│  ┌──────┐  ┌──────┐  ┌──────┐  ┌──────┐                        │
│  │  T1  │  │  T2  │  │  T3  │  │  T4  │                        │
│  │ 2 ppl│  │ 4 ppl│  │ 4 ppl│  │ 6 ppl│                        │
│  │ GREEN│  │  RED │  │ GREEN│  │YELLOW│                        │
│  │ Avail│  │#019  │  │ Avail│  │Needs │                        │
│  └──────┘  └──────┘  └──────┘  │Clean │                        │
│                                  └──────┘                        │
│  ┌──────┐  ┌──────┐  ┌──────┐                                   │
│  │  T5  │  │  T6  │  │  T7  │                                   │
│  │ 2 ppl│  │ 8 ppl│  │ 4 ppl│                                   │
│  │  RED │  │ GREEN│  │  RED │                                   │
│  │#023  │  │ Avail│  │#021  │                                   │
│  └──────┘  └──────┘  └──────┘                                   │
│                                                                   │
│  Legend: 🟢 Available  🔴 Occupied  🟡 Needs Cleaning            │
│                                                                   │
│  ┌─────────────────────────────┐                                 │
│  │    + New Dine-in Order      │  (FAB button)                   │
│  └─────────────────────────────┘                                 │
└──────────────────────────────────────────────────────────────────┘
```
- Grid of table cards, color-coded by status
- Each card shows: table number, capacity, status, active order # (if occupied)
- Tap available table → opens New Order with table pre-selected
- Tap occupied table → opens active order detail
- Section/floor tabs at top for filtering
- Floating action button for new order

---

### Screen 4: New Order
```
┌─ HEADER: "New Order" | [Cancel] ─────────────────────────────────┐
│                                                                   │
│  Order Type:  (●) Dine-in  ( ) Takeout  ( ) Delivery            │
│                                                                   │
│  Table: [ T5 ▼ ]        (shown only for Dine-in)                │
│                                                                   │
│  Customer: ┌───────────────┐  Phone: ┌───────────────┐          │
│            │ (optional)    │         │ (req takeout) │          │
│            └───────────────┘         └───────────────┘          │
│                                                                   │
│  ┌─ MENU (left 60%) ──────────┬─ CART (right 40%) ────────────┐ │
│  │ [Appetizers][Mains][Drinks]│  Cart (3 items)               │ │
│  │                            │                                │ │
│  │ Butter Chicken    ₹350     │  1x Butter Chicken     ₹350  │ │
│  │  [+ Add]                   │     Size: Full               │ │
│  │ Paneer Tikka      ₹280    │  2x Naan               ₹80   │ │
│  │  [+ Add]                   │  1x Mango Lassi        ₹120  │ │
│  │ Naan               ₹40    │                                │ │
│  │  [+ Add]                   │  ─────────────────────        │ │
│  │ Biryani            ₹320   │  Subtotal:          ₹550      │ │
│  │  [+ Add]                   │  Tax (5%):           ₹28      │ │
│  │                            │  Total:             ₹578      │ │
│  │                            │                                │ │
│  │                            │  Notes: ┌──────────────┐      │ │
│  │                            │         │              │      │ │
│  │                            │         └──────────────┘      │ │
│  │                            │                                │ │
│  │                            │  ┌────────────────────────┐   │ │
│  │                            │  │   PLACE ORDER          │   │ │
│  │                            │  └────────────────────────┘   │ │
│  └────────────────────────────┴────────────────────────────────┘ │
└──────────────────────────────────────────────────────────────────┘
```
- Split layout on tablet/desktop: menu browser left, cart right
- On phone: full-screen menu with floating cart badge; tap to expand cart
- Category tabs at top of menu section
- Each menu item: name, price, "Add" button
- Tapping "Add" on item with modifiers → opens modifier sheet
- Cart shows items with quantity stepper, modifier details, remove button
- Order type selector at top (changes required fields)
- "Place Order" button prominent at bottom of cart

---

### Screen 5: Modifier Sheet (Overlay)
```
┌──────────────────────────────────┐
│   Butter Chicken            ₹350 │
│                                  │
│   Size (Required - pick 1):     │
│   ┌──────────┐ ┌──────────┐    │
│   │ Half     │ │ Full     │    │
│   │  ₹200   │ │  ₹350    │    │
│   └──────────┘ └──────────┘    │
│                                  │
│   Spice Level (Required):       │
│   ┌──────┐ ┌──────┐ ┌──────┐  │
│   │ Mild │ │Medium│ │ Hot  │  │
│   └──────┘ └──────┘ └──────┘  │
│                                  │
│   Add-ons (Optional, up to 3):  │
│   ☐ Extra Gravy       +₹30     │
│   ☐ Bone-in           +₹0      │
│   ☐ Extra Rice         +₹50    │
│                                  │
│   Qty:  [ - ]  1  [ + ]        │
│                                  │
│   Notes: ┌───────────────────┐  │
│           │ No onion         │  │
│           └───────────────────┘  │
│                                  │
│   ┌──────────────────────────┐  │
│   │  ADD TO ORDER     ₹380   │  │
│   └──────────────────────────┘  │
└──────────────────────────────────┘
```
- Bottom sheet (mobile) or modal (desktop)
- Required modifiers shown first with clear labels
- Single-select rendered as toggle buttons
- Multi-select rendered as checkboxes
- Quantity selector
- Running total on the "Add" button
- Item-level notes field

---

### Screen 6: Order Detail
```
┌─ HEADER: "Order #023" | Status: [Preparing ▼] | [Print] ────────┐
│                                                                   │
│  Type: Dine-in  │  Table: T5  │  Waiter: Ravi  │  12:34 PM     │
│  Customer: Walk-in                                                │
│                                                                   │
│  Items:                                                           │
│  ┌──────────────────────────────────────────────────────────┐    │
│  │ 1x Butter Chicken (Full, Hot)              ₹350  [Ready]│    │
│  │ 2x Naan                                     ₹80  [Prep] │    │
│  │ 1x Mango Lassi                             ₹120  [Pend] │    │
│  │    Note: "less sugar"                                    │    │
│  └──────────────────────────────────────────────────────────┘    │
│                                                                   │
│  [+ Add More Items]                                              │
│                                                                   │
│  ──────────────────────────────                                  │
│  Subtotal:                                            ₹550       │
│  Tax (5% GST):                                         ₹28       │
│  Total:                                               ₹578       │
│                                                                   │
│  Payment: Unpaid                                                  │
│                                                                   │
│  ┌────────────────────────┐  ┌────────────────────────┐          │
│  │    PROCEED TO PAY      │  │    CANCEL ORDER        │          │
│  └────────────────────────┘  └────────────────────────┘          │
│                                                                   │
│  Order Timeline:                                                  │
│  ● 12:34 PM  Created by Ravi                                    │
│  ● 12:34 PM  Sent to kitchen                                    │
│  ● 12:36 PM  Kitchen started preparing                           │
└──────────────────────────────────────────────────────────────────┘
```
- Full order info: type, table, customer, server, timestamps
- Item list with per-item status badges
- "Add More Items" button (for open orders)
- Financial summary
- Action buttons contextual to status and user role
- Timeline/audit trail at bottom
- Kitchen staff see a simplified version (no payment)

---

### Screen 7: Order List
```
┌─ HEADER: "Orders" ──────────────────────────────────────────────┐
│                                                                   │
│  Filters: [Today ▼] [All Types ▼] [All Status ▼] [🔍 Search]  │
│                                                                   │
│  ┌────┬──────────┬────────┬──────────┬─────────┬────────┬─────┐ │
│  │ #  │ Type     │ Table/ │ Items    │ Total   │ Status │ Time│ │
│  │    │          │ Cust   │          │         │        │     │ │
│  ├────┼──────────┼────────┼──────────┼─────────┼────────┼─────┤ │
│  │025 │🟢Dine-in│ T3     │ 4 items  │ ₹1,200  │ New    │12:45│ │
│  │024 │🟡Takeout│ Amit   │ 2 items  │ ₹480    │ Ready  │12:40│ │
│  │023 │🟢Dine-in│ T5     │ 3 items  │ ₹578    │ Prep   │12:34│ │
│  │022 │🔵Deliver│ Priya  │ 5 items  │ ₹890    │ Paid   │12:20│ │
│  │021 │🟢Dine-in│ T7     │ 2 items  │ ₹340    │ Done   │12:10│ │
│  │020 │🔴Cancel │ T1     │ 1 item   │ ₹280    │ Cancel │11:55│ │
│  └────┴──────────┴────────┴──────────┴─────────┴────────┴─────┘ │
│                                                                   │
│  Showing 20 of 42 orders  │  [< Prev]  [Next >]                 │
└──────────────────────────────────────────────────────────────────┘
```
- Filterable, sortable table
- Color-coded type and status badges
- Tap row → opens Order Detail
- Search by order #, customer name, or phone
- Pagination for long lists

---

### Screen 8: Kitchen Display System (KDS)
```
┌─ HEADER: "Kitchen" | 🟢 Connected | 🔊 Sound ON | Clock: 12:45 ┐
│                                                                   │
│  ┌─ NEW ──────────┐  ┌─ PREPARING ───────┐  ┌─ READY ─────────┐│
│  │                 │  │                    │  │                  ││
│  │ ┌─────────────┐│  │ ┌────────────────┐ │  │ ┌──────────────┐││
│  │ │ #025 DINE-IN││  │ │ #023 DINE-IN   │ │  │ │ #020 TAKEOUT │││
│  │ │ Table 3     ││  │ │ Table 5  ⏱ 11m │ │  │ │ Raj K.       │││
│  │ │ ⏱ 0 min     ││  │ │                │ │  │ │ ⏱ 22m  🔴    │││
│  │ │             ││  │ │ ✅ Butter Chkn │ │  │ │              │││
│  │ │ □ Dal Tadka ││  │ │ 🔄 Naan (x2)  │ │  │ │ All items    │││
│  │ │ □ Rice (x2) ││  │ │ □ Mango Lassi  │ │  │ │ ready!       │││
│  │ │ □ Raita     ││  │ │                │ │  │ │              │││
│  │ │             ││  │ │ Note: no onion │ │  │ │ [SERVED ✓]   │││
│  │ │ [START →]   ││  │ │                │ │  │ └──────────────┘││
│  │ └─────────────┘│  │ └────────────────┘ │  │                  ││
│  │                 │  │                    │  │                  ││
│  │ ┌─────────────┐│  │ ┌────────────────┐ │  │                  ││
│  │ │ #026 DELIVER││  │ │ #022 TAKEOUT   │ │  │                  ││
│  │ │ Priya M.    ││  │ │ Amit    ⏱ 5m  │ │  │                  ││
│  │ │ ⏱ 0 min     ││  │ │                │ │  │                  ││
│  │ │             ││  │ │ 🔄 Biryani (x2)│ │  │                  ││
│  │ │ □ Biryani   ││  │ │                │ │  │                  ││
│  │ │ □ Kebab (x3)││  │ │                │ │  │                  ││
│  │ │             ││  │ │                │ │  │                  ││
│  │ │ [START →]   ││  │ └────────────────┘ │  │                  ││
│  │ └─────────────┘│  │                    │  │                  ││
│  └─────────────────┘  └────────────────────┘  └──────────────────┘│
└──────────────────────────────────────────────────────────────────┘
```
- 3-column Kanban layout: New → Preparing → Ready
- Each order card: order #, type badge, table/customer, elapsed timer, item checklist
- Timer color coding: white (< 10m), yellow (10–20m), red (> 20m)
- Tap item checkbox → toggles Preparing/Ready
- Tap "Start" → moves entire order to Preparing
- When all items checked → auto-moves to Ready column
- Large fonts, high contrast (designed for kitchen viewing distance)
- Connection status indicator top-right
- Audio chime on new order (configurable)
- Auto-scrolls to show newest orders
- NO payment info shown (kitchen doesn't need it)

---

### Screen 9: Checkout / Payment
```
┌─ HEADER: "Checkout - Order #023" ────────────────────────────────┐
│                                                                   │
│  ┌─ ORDER SUMMARY ──────────────────────────────────────────┐    │
│  │ 1x Butter Chicken (Full, Hot)                      ₹350  │    │
│  │ 2x Naan                                             ₹80  │    │
│  │ 1x Mango Lassi                                     ₹120  │    │
│  │ ──────────────────────────────────────────────────────── │    │
│  │ Subtotal                                            ₹550  │    │
│  │ CGST (2.5%)                                          ₹14  │    │
│  │ SGST (2.5%)                                          ₹14  │    │
│  │ ════════════════════════════════════════════════════════ │    │
│  │ TOTAL                                               ₹578  │    │
│  └──────────────────────────────────────────────────────────┘    │
│                                                                   │
│  Payment Method:                                                  │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐                      │
│  │  💵 CASH │  │  💳 CARD │  │  📱 UPI  │                      │
│  │          │  │ (Stripe) │  │(Razorpay)│                      │
│  └──────────┘  └──────────┘  └──────────┘                      │
│                                                                   │
│  ☐ Split Payment (add another method)                            │
│                                                                   │
│  Amount Tendered: ┌─────────────────┐                            │
│                    │   ₹600          │                            │
│                    └─────────────────┘                            │
│  Change Due:  ₹22                                                │
│                                                                   │
│  ┌──────────────────────────────────────┐                        │
│  │          COMPLETE PAYMENT             │                        │
│  └──────────────────────────────────────┘                        │
└──────────────────────────────────────────────────────────────────┘
```
- Order summary at top (read-only)
- Large payment method buttons (easy tap)
- Cash: shows amount tendered input + auto-calculated change
- Card/UPI: triggers respective payment gateway flow
- Split payment checkbox → reveals second payment row
- "Complete Payment" button: disabled until valid payment entered

---

### Screen 10: Receipt View
```
┌──────────────────────────────────┐
│       RESTAURANT NAME            │
│       123 Main Street            │
│       Ph: +91-9876543210         │
│   ─────────────────────────────  │
│   Order #023 | Dine-in | T5     │
│   Date: 14-Feb-2026  12:34 PM   │
│   Server: Ravi                   │
│   ─────────────────────────────  │
│   1x Butter Chicken (Full) ₹350 │
│   2x Naan               ₹80    │
│   1x Mango Lassi         ₹120  │
│   ─────────────────────────────  │
│   Subtotal:              ₹550   │
│   CGST (2.5%):            ₹14   │
│   SGST (2.5%):            ₹14   │
│   ═════════════════════════════  │
│   TOTAL:                 ₹578   │
│   Paid: Cash             ₹578   │
│   ─────────────────────────────  │
│   Thank you! Visit again!       │
│                                  │
│  ┌────────────┐ ┌────────────┐  │
│  │  🖨 PRINT  │ │ 📄 PDF     │  │
│  └────────────┘ └────────────┘  │
│                                  │
│  [← Back to Orders]             │
└──────────────────────────────────┘
```
- Receipt-style monospace layout
- Print button → sends to configured thermal printer
- PDF button → generates downloadable PDF
- "Back to Orders" returns to order list

---

### Screen 11: Menu Management (Admin)
```
┌─ HEADER: "Menu Management" ──────────────────────────────────────┐
│                                                                   │
│  Categories          │  Items in "Main Course"                   │
│  ┌─────────────────┐ │  ┌──────────────────────────────────────┐ │
│  │ ≡ Appetizers    │ │  │ ┌──────┐ Butter Chicken       ₹350  │ │
│  │ ≡ Main Course  ◄│ │  │ │ IMG  │ Rich, creamy curry         │ │
│  │ ≡ Breads       │ │  │ │      │ 🟢 Available  [Edit][Del] │ │
│  │ ≡ Drinks       │ │  │ └──────┘                              │ │
│  │ ≡ Desserts     │ │  │                                        │ │
│  │                 │ │  │ ┌──────┐ Paneer Tikka         ₹280  │ │
│  │ [+ Add Category]│ │  │ │ IMG  │ Tandoori cottage cheese    │ │
│  └─────────────────┘ │  │ │      │ 🔴 Out of Stock [Edit][Del]│ │
│                       │  │ └──────┘                              │ │
│                       │  │                                        │ │
│                       │  │ [+ Add Item]                          │ │
│                       │  └──────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────────────┘
```
- Two-panel layout: categories left, items right
- Drag handles (≡) for reordering
- Each item card: thumbnail, name, price, availability badge, edit/delete
- "Edit" opens a form/modal with full item details + modifier management
- Availability toggle is quick-access (no need to open edit form)

---

### Screen 12: Table Management (Admin)
```
┌─ HEADER: "Table Management" ─────────────────────────────────────┐
│                                                                   │
│  Sections: [+ Add Section]                                       │
│  ┌─────────────────────────────────────────────────────────────┐ │
│  │ Main Floor                                          [Edit]  │ │
│  │ ┌───────┐ ┌───────┐ ┌───────┐ ┌───────┐ ┌───────┐        │ │
│  │ │ T1    │ │ T2    │ │ T3    │ │ T4    │ │ T5    │        │ │
│  │ │ 2 ppl │ │ 4 ppl │ │ 4 ppl │ │ 6 ppl │ │ 2 ppl │        │ │
│  │ │ [Edit]│ │ [Edit]│ │ [Edit]│ │ [Edit]│ │ [Edit]│        │ │
│  │ └───────┘ └───────┘ └───────┘ └───────┘ └───────┘        │ │
│  │ [+ Add Table]                                               │ │
│  ├─────────────────────────────────────────────────────────────┤ │
│  │ Patio                                               [Edit]  │ │
│  │ ┌───────┐ ┌───────┐                                        │ │
│  │ │ P1    │ │ P2    │                                        │ │
│  │ │ 4 ppl │ │ 4 ppl │                                        │ │
│  │ │ [Edit]│ │ [Edit]│                                        │ │
│  │ └───────┘ └───────┘                                        │ │
│  │ [+ Add Table]                                               │ │
│  └─────────────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────────────┘
```
- Grouped by section
- Each table shows number + capacity
- Edit opens inline form: number, capacity, section
- Add table / Add section buttons

---

### Screen 13: User Management (Admin)
```
┌─ HEADER: "Users" | [+ Add User] ────────────────────────────────┐
│                                                                   │
│  ┌──────────────────┬──────────┬──────────┬──────────┬─────────┐ │
│  │ Name             │ Email    │ Role     │ Status   │ Actions │ │
│  ├──────────────────┼──────────┼──────────┼──────────┼─────────┤ │
│  │ Ravi Kumar       │ ravi@... │ Waiter   │ 🟢Active │ [Edit]  │ │
│  │ Meena S.         │ meena@.. │ Kitchen  │ 🟢Active │ [Edit]  │ │
│  │ Arjun P.         │ arjun@.. │ Cashier  │ 🟢Active │ [Edit]  │ │
│  │ Divya R.         │ divya@.. │ Manager  │ 🔴Inactv │ [Edit]  │ │
│  └──────────────────┴──────────┴──────────┴──────────┴─────────┘ │
│                                                                   │
│  ┌─ Add/Edit User (modal) ─────────────────────────────────────┐ │
│  │ Name:   ┌────────────────┐  Role: [Waiter ▼]               │ │
│  │         └────────────────┘                                   │ │
│  │ Email:  ┌────────────────┐  Status: (●) Active ( ) Inactive│ │
│  │         └────────────────┘                                   │ │
│  │ Password: ┌──────────────┐  [Save] [Cancel]                │ │
│  │           └──────────────┘                                   │ │
│  └─────────────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────────────┘
```
- Simple data table with role badges
- "Add User" opens modal form
- Edit opens same modal pre-filled
- No delete — only deactivate

---

### Screen 14: Reports Dashboard
```
┌─ HEADER: "Reports" | Date Range: [Today ▼] [Custom Range] ──────┐
│                                                                   │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐               │
│  │ Orders  │ │ Revenue │ │ Avg Val │ │ Cancel  │               │
│  │   42    │ │ ₹12,450 │ │  ₹296   │ │   3     │               │
│  │ +12%    │ │ +8%     │ │ -2%     │ │ 7.1%    │               │
│  └─────────┘ └─────────┘ └─────────┘ └─────────┘               │
│                                                                   │
│  ┌─ Revenue Chart (bar, by hour) ──────────────────────────────┐ │
│  │  ██                                                          │ │
│  │  ██ ██                        ██                             │ │
│  │  ██ ██ ██                  ██ ██ ██                          │ │
│  │  ██ ██ ██ ██            ██ ██ ██ ██ ██                      │ │
│  │  10  11  12  1   2   3   6   7   8   9                      │ │
│  └──────────────────────────────────────────────────────────────┘ │
│                                                                   │
│  ┌─ Top Items ──────────────┐  ┌─ Orders by Type ──────────────┐│
│  │ 1. Butter Chicken (28)   │  │  Dine-in:  ████████░░  60%   ││
│  │ 2. Naan (45)             │  │  Takeout:  ████░░░░░░  28%   ││
│  │ 3. Biryani (22)          │  │  Delivery: ██░░░░░░░░  12%   ││
│  │ 4. Mango Lassi (18)      │  │                               ││
│  │ 5. Paneer Tikka (15)     │  │  ┌─ Payment Split ──────┐    ││
│  └──────────────────────────┘  │  │ Cash:  55% │ Card: 30%│   ││
│                                 │  │ UPI:   15% │          │   ││
│  [📥 Export CSV]               │  └──────────────────────┘    ││
│                                 └──────────────────────────────┘│
└──────────────────────────────────────────────────────────────────┘
```
- Summary metric cards with trend indicators
- Revenue by hour bar chart
- Top items leaderboard
- Order type & payment method pie/bar charts
- Date range selector (Today, Yesterday, This Week, Custom)
- CSV export button

---

### Screen 15: Settings (Admin)
```
┌─ HEADER: "Settings" ────────────────────────────────────────────┐
│                                                                   │
│  ┌─ Tabs: [Restaurant] [Tax] [Payments] [Printing] [System] ─┐ │
│                                                                   │
│  Restaurant Info:                                                 │
│  Name:     ┌────────────────────────────┐                        │
│  Address:  ┌────────────────────────────┐                        │
│  Phone:    ┌────────────────────────────┐                        │
│  Currency: [₹ INR ▼]                                             │
│  Logo:     [Upload]                                              │
│                                                                   │
│  Tax Configuration:                                               │
│  ☐ Enable tax on orders                                          │
│  Tax Type: [GST (CGST + SGST) ▼]                                │
│  Tax Rate: [5%]                                                  │
│                                                                   │
│  Payment Gateway:                                                 │
│  Active Gateway: (●) Stripe  ( ) Razorpay                       │
│  Stripe API Key:    ┌──────────────────┐                        │
│  Stripe Secret:     ┌──────────────────┐                        │
│                                                                   │
│  Printer:                                                         │
│  Printer Type: [Network (LAN) ▼]                                 │
│  Printer IP:   ┌──────────────────┐                              │
│  [Test Print]                                                     │
│                                                                   │
│  ┌──────────────┐                                                │
│  │  Save Changes│                                                │
│  └──────────────┘                                                │
└──────────────────────────────────────────────────────────────────┘
```
- Tabbed settings layout
- Restaurant info, tax, payment gateway, printer, system (backup, etc.)
- Test print button for printer configuration
- Save button per section or global save

---

## 3. Navigation Map

```
                              ┌─────────┐
                              │  LOGIN  │
                              └────┬────┘
                                   │
                              ┌────▼────┐
                         ┌────│DASHBOARD│────┐
                         │    └────┬────┘    │
                         │         │         │
            ┌────────────┼─────────┼─────────┼────────────┐
            │            │         │         │            │
       ┌────▼────┐ ┌────▼────┐ ┌──▼───┐ ┌──▼────┐ ┌────▼─────┐
       │TABLE MAP│ │ORDER    │ │KITCHEN│ │REPORTS│ │ SETTINGS │
       └────┬────┘ │LIST     │ │(KDS)  │ └───────┘ ├──────────┤
            │      └────┬────┘ └───┬───┘            │Menu Mgmt │
       ┌────▼────┐      │         │                 │Table Mgmt│
       │NEW ORDER│◄─────┘    (view only)            │User Mgmt │
       └────┬────┘                                  └──────────┘
            │
       ┌────▼─────────┐
       │ MENU BROWSER  │ (with modifier sheet overlay)
       └────┬──────────┘
            │
       ┌────▼──────────┐
       │ ORDER DETAIL   │
       └────┬──────────┘
            │
       ┌────▼──────────┐
       │   CHECKOUT     │
       └────┬──────────┘
            │
       ┌────▼──────────┐
       │    RECEIPT     │
       └───────────────┘
```

### Navigation Rules by Role

| Role | Sidebar/Nav Items | Default Landing |
|------|------------------|----------------|
| **Admin** | Dashboard, Tables, Orders, Kitchen, Menu Mgmt, Table Mgmt, Users, Reports, Settings | Dashboard |
| **Manager** | Dashboard, Tables, Orders, Kitchen, Menu Mgmt, Reports | Dashboard |
| **Cashier** | Dashboard, Tables, Orders, Kitchen | Dashboard (with New Order prominent) |
| **Waiter** | Dashboard (mobile), Table Map, My Orders | Table Map (phone-optimized) |
| **Kitchen** | Kitchen Display (full screen) | Kitchen Display (auto-launches) |

### Responsive Behavior

| Breakpoint | Navigation Style | Layout Adjustments |
|------------|-----------------|-------------------|
| **Desktop** (> 1024px) | Fixed left sidebar | Multi-column layouts, side-by-side panels |
| **Tablet** (768–1024px) | Collapsible sidebar / top bar | Stacked panels, full-width cards |
| **Phone** (< 768px) | Bottom tab navigation (4 tabs max) | Single column, bottom sheets for modals, floating cart badge |

---

*Document version: 1.0*
*Last updated: 2026-02-14*
