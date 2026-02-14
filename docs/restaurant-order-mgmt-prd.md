# Restaurant Order Management System — MVP PRD

## 1. Problem Statement

Small restaurants managing dine-in, takeout, and delivery orders rely on fragmented tools — handwritten tickets, WhatsApp messages, and disconnected POS systems. This leads to lost orders, kitchen confusion, slow table turns, and zero visibility into daily performance. Staff waste time coordinating manually instead of serving customers.

**Goal:** Build a unified, real-time order management system that a small restaurant team (1–5 people) can adopt in a day and use reliably for daily operations — taking orders, routing them to the kitchen, tracking status, processing payments, and generating receipts.

## 2. Success Metrics (MVP)

| Metric | Target |
|--------|--------|
| Order-to-kitchen latency | < 5 seconds |
| Time to train new staff | < 30 minutes |
| Daily uptime | 99.5%+ |
| Orders processed without error | > 98% |
| End-to-end order time (create → paid) | Trackable from Day 1 |

---

## 3. Users & Roles (RBAC)

| Role | Access | Typical Device |
|------|--------|---------------|
| **Admin** | Full system config: menu, tables, users, settings, reports, payment gateway setup | Desktop/laptop |
| **Manager** | Reports, order overrides, cancellations, refunds, menu edits | Desktop/tablet |
| **Cashier** | Create orders, assign tables, process payments, print receipts | Desktop/tablet |
| **Waiter** | Create/modify dine-in orders, view assigned tables, mark items served | Phone |
| **Kitchen** | View order queue, update item/order status (preparing → ready) | Wall-mounted tablet |

Login: email + password per user, with role assigned by Admin.

---

## 4. Core User Flows

### Flow 1: Take a Dine-In Order
```
Waiter/Cashier opens "New Order"
  → Selects order type: Dine-in
  → Selects/assigns a table (from table map or list)
  → Browses menu by category
  → Adds items, selects modifiers (size, toppings, spice level)
  → Optionally adds customer notes ("no onion", "extra spicy")
  → Reviews order summary
  → Submits → Order appears in Kitchen Queue instantly
```

### Flow 2: Take a Takeout / Delivery Order
```
Cashier opens "New Order"
  → Selects order type: Takeout or Delivery
  → Enters customer name, phone (delivery: + address)
  → Adds items + modifiers
  → Sets estimated pickup/delivery time (optional)
  → Submits → Kitchen Queue
```

### Flow 3: Kitchen Processes Orders
```
Kitchen screen shows orders in real-time (newest at bottom)
  → Each order shows: order #, type badge (dine-in/takeout/delivery),
    table # (if dine-in), items with modifiers, timestamp, notes
  → Kitchen staff taps item → marks "Preparing"
  → When all items ready → marks order "Ready"
  → Waiter/Cashier is notified (visual indicator + optional sound)
```

### Flow 4: Serve / Handoff
```
Dine-in: Waiter sees "Ready" status → delivers to table → marks "Served"
Takeout: Cashier calls customer → hands over → marks "Picked Up"
Delivery: Assigns to driver (MVP: manual) → marks "Out for Delivery" → "Delivered"
```

### Flow 5: Checkout & Payment
```
Cashier opens the order
  → Views itemized bill with taxes
  → Selects payment method (Cash / Card / UPI)
  → For Card/UPI: triggers payment gateway (Stripe or Razorpay)
  → On success: order marked "Paid"
  → Generates receipt (show on screen + option to print)
  → Order moves to completed
```

### Flow 6: Manager Reviews Day
```
Manager opens Reports dashboard
  → Views: total orders, revenue, avg order value
  → Sees popular items, cancellation count
  → Filters by date range, order type
  → Exports as CSV (optional)
```

---

## 5. Functional Requirements (MVP)

### 5.1 Menu Management
- **FR-1:** Admin can create, edit, delete, and reorder menu categories
- **FR-2:** Admin can create, edit, delete menu items (name, description, price, image optional, category, availability toggle)
- **FR-3:** Admin can define modifiers per item or per category (e.g., Size: S/M/L with price adjustments; Toppings: multi-select with per-topping price)
- **FR-4:** Admin/Manager can mark item as "Out of Stock" (hides from order creation, shows indicator in kitchen)
- **FR-5:** Items support tax category assignment (GST/VAT or flat tax rate)

### 5.2 Table Management
- **FR-6:** Admin can configure tables (table number, capacity, floor/section label)
- **FR-7:** Table status: Available, Occupied, Reserved, Needs Cleaning
- **FR-8:** Tables shown as a visual grid/map (simple numbered boxes with color-coded status)
- **FR-9:** Order linked to table; table auto-marked "Occupied" when order is created

### 5.3 Order Management
- **FR-10:** Create order with type: Dine-in, Takeout, Delivery
- **FR-11:** Add/remove/update items and modifiers on an open order
- **FR-12:** Each order has a unique sequential order number (daily reset: #001, #002…)
- **FR-13:** Order statuses: `New → Confirmed → Preparing → Ready → Served/Picked Up/Delivered → Paid → Completed`
- **FR-14:** Attach customer info: name, phone (required for takeout/delivery), address (delivery only)
- **FR-15:** Order-level and item-level notes (free text)
- **FR-16:** Cancel order with mandatory reason; only Manager+ can cancel confirmed orders
- **FR-17:** Split bill: divide order total equally among N people (MVP: equal split only)
- **FR-18:** Add items to an existing open order (common for dine-in)

### 5.4 Kitchen Display System (KDS)
- **FR-19:** Real-time order queue: new orders appear within 5 seconds
- **FR-20:** Orders displayed as cards: order #, type badge, table #, items with modifiers, notes, time elapsed since creation
- **FR-21:** Kitchen can mark individual items as Preparing / Ready
- **FR-22:** When all items ready, order auto-transitions to "Ready" (or manual)
- **FR-23:** Color-coded urgency: white (< 10 min), yellow (10–20 min), red (> 20 min)
- **FR-24:** Sound/visual alert on new order
- **FR-25:** Completed orders move to a "Done" column or disappear

### 5.5 Payments
- **FR-26:** Support payment methods: Cash, Card, UPI
- **FR-27:** Stripe integration: card payments via Stripe Terminal or Stripe Payment Links
- **FR-28:** Razorpay integration: UPI + card payments via Razorpay Payment Gateway
- **FR-29:** Admin configures which gateway is active (Stripe or Razorpay, not both simultaneously)
- **FR-30:** Record payment: amount, method, transaction ID (from gateway), timestamp
- **FR-31:** Support partial payments (e.g., split across cash + card)
- **FR-32:** Handle payment failures gracefully (retry, fallback to cash)

### 5.6 Receipts & Billing
- **FR-33:** Generate itemized receipt: restaurant name, order #, items + modifiers + prices, subtotal, tax breakdown, total, payment method, date/time
- **FR-34:** Display receipt on screen (digital)
- **FR-35:** Print receipt via thermal printer (ESC/POS compatible)
- **FR-36:** Optionally generate PDF receipt

### 5.7 Reporting (Basic)
- **FR-37:** Dashboard: today's total orders, revenue, average order value
- **FR-38:** Orders by type breakdown (dine-in / takeout / delivery)
- **FR-39:** Popular items (top 10 by quantity sold)
- **FR-40:** Cancellation report (count, reasons)
- **FR-41:** Date range filter on all reports
- **FR-42:** Revenue by payment method

### 5.8 User Management
- **FR-43:** Admin can create/edit/deactivate user accounts
- **FR-44:** Each user has: name, email, password, role, active/inactive status
- **FR-45:** Login via email + password; session token with expiry
- **FR-46:** Password reset flow (email-based)

---

## 6. Non-Functional Requirements

| Category | Requirement |
|----------|-------------|
| **Performance** | Order creation to kitchen display: < 5 sec. Page load: < 2 sec. Support 50 concurrent orders. |
| **Reliability** | 99.5% uptime. No data loss on server restart (persistent DB). Graceful degradation on payment gateway failure. |
| **Security** | HTTPS everywhere. Passwords hashed (bcrypt). JWT auth with refresh tokens. Role-based API authorization. PCI compliance via gateway (no card data stored). |
| **Scalability** | MVP: single restaurant. Architecture should allow multi-tenant later. |
| **Usability** | Touch-friendly UI (min 44px tap targets). High contrast for kitchen display. Works on 7" tablets and 5" phones. |
| **Offline Resilience** | Nice-to-have: queue order creation locally if connection drops; sync when reconnected. Show clear offline indicator. |
| **Accessibility** | Minimum AA contrast ratios. Large text option for kitchen display. |
| **Data** | Daily automated DB backup. 90-day order history retained. |
| **Printing** | Support ESC/POS thermal printers via WebUSB or network (LAN) print. |

---

## 7. Out of Scope (MVP)

- Online ordering / customer-facing menu (website/app)
- Inventory / stock management
- Employee scheduling / shift management
- Loyalty programs / discounts / coupons (beyond simple % discount)
- Multi-restaurant / franchise management
- Kitchen station-based routing (single queue only for MVP)
- Third-party delivery integration (DoorDash, Swiggy, Zomato)
- Reservation system
- Advanced analytics / ML-based forecasting
- Mobile native apps (PWA web app covers mobile)
- Multi-language support
- Table merge/transfer between orders
- Tip management

---

## 8. Data Model

### Entities & Key Fields

```
┌─────────────────┐     ┌─────────────────┐
│     User         │     │   Restaurant     │
├─────────────────┤     ├─────────────────┤
│ id (PK)         │     │ id (PK)         │
│ name            │     │ name            │
│ email (unique)  │     │ address         │
│ password_hash   │     │ phone           │
│ role (enum)     │     │ tax_config (json)│
│ is_active       │     │ currency        │
│ created_at      │     │ active_gateway  │
│ updated_at      │     │ stripe_config   │
└─────────────────┘     │ razorpay_config │
                         └─────────────────┘

┌─────────────────┐     ┌─────────────────┐
│   Category       │     │   MenuItem       │
├─────────────────┤     ├─────────────────┤
│ id (PK)         │     │ id (PK)         │
│ name            │     │ name            │
│ sort_order      │     │ description     │
│ is_active       │     │ price           │
└─────────────────┘     │ category_id (FK)│
                         │ image_url       │
                         │ is_available    │
                         │ tax_rate        │
                         │ sort_order      │
                         └─────────────────┘

┌──────────────────┐
│   Modifier Group  │
├──────────────────┤
│ id (PK)          │
│ name             │  (e.g., "Size", "Toppings")
│ menu_item_id (FK)│  (or category_id for shared modifiers)
│ is_required      │
│ min_select       │
│ max_select       │
└──────────────────┘

┌──────────────────┐
│  Modifier Option  │
├──────────────────┤
│ id (PK)          │
│ group_id (FK)    │
│ name             │  (e.g., "Large", "Extra Cheese")
│ price_adjustment │  (e.g., +2.00)
│ sort_order       │
└──────────────────┘

┌─────────────────┐
│     Table        │
├─────────────────┤
│ id (PK)         │
│ number          │
│ capacity        │
│ section         │
│ status (enum)   │  Available | Occupied | Reserved | NeedsCleaning
└─────────────────┘

┌──────────────────────┐
│       Order           │
├──────────────────────┤
│ id (PK)              │
│ order_number         │  (daily sequential: #001)
│ type (enum)          │  DineIn | Takeout | Delivery
│ status (enum)        │  New | Confirmed | Preparing | Ready |
│                      │  Served | PickedUp | Delivered | Paid | Completed | Cancelled
│ table_id (FK, null)  │
│ customer_name        │
│ customer_phone       │
│ delivery_address     │
│ notes                │
│ subtotal             │
│ tax_amount           │
│ total                │
│ created_by (FK→User) │
│ created_at           │
│ updated_at           │
│ cancelled_reason     │
└──────────────────────┘

┌──────────────────────┐
│     OrderItem         │
├──────────────────────┤
│ id (PK)              │
│ order_id (FK)        │
│ menu_item_id (FK)    │
│ quantity             │
│ unit_price           │  (snapshot at order time)
│ item_name            │  (snapshot)
│ notes                │
│ status (enum)        │  Pending | Preparing | Ready
└──────────────────────┘

┌──────────────────────────┐
│  OrderItemModifier        │
├──────────────────────────┤
│ id (PK)                  │
│ order_item_id (FK)       │
│ modifier_option_id (FK)  │
│ modifier_name            │  (snapshot)
│ option_name              │  (snapshot)
│ price_adjustment         │  (snapshot)
└──────────────────────────┘

┌──────────────────────┐
│     Payment           │
├──────────────────────┤
│ id (PK)              │
│ order_id (FK)        │
│ amount               │
│ method (enum)        │  Cash | Card | UPI
│ gateway              │  Stripe | Razorpay | None
│ transaction_id       │
│ status (enum)        │  Pending | Success | Failed | Refunded
│ created_at           │
└──────────────────────┘
```

### Key Relationships
- Restaurant 1 → N Users, Categories, Tables
- Category 1 → N MenuItems
- MenuItem 1 → N ModifierGroups → N ModifierOptions
- Order 1 → N OrderItems → N OrderItemModifiers
- Order 1 → N Payments (supports split payments)
- Order N → 1 Table (nullable, dine-in only)
- Order N → 1 User (created_by)

---

## 9. API Requirements

### Authentication
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/login` | Email + password → JWT tokens |
| POST | `/api/auth/refresh` | Refresh access token |
| POST | `/api/auth/logout` | Invalidate session |
| POST | `/api/auth/forgot-password` | Send reset email |
| POST | `/api/auth/reset-password` | Reset with token |

### Users (Admin only)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/users` | List all users |
| POST | `/api/users` | Create user |
| PUT | `/api/users/:id` | Update user |
| DELETE | `/api/users/:id` | Deactivate user |

### Menu
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/categories` | List categories (with items) |
| POST | `/api/categories` | Create category |
| PUT | `/api/categories/:id` | Update category |
| DELETE | `/api/categories/:id` | Delete category |
| GET | `/api/menu-items` | List items (filter by category, availability) |
| POST | `/api/menu-items` | Create item |
| PUT | `/api/menu-items/:id` | Update item |
| PATCH | `/api/menu-items/:id/availability` | Toggle in/out of stock |
| DELETE | `/api/menu-items/:id` | Delete item |
| POST | `/api/menu-items/:id/modifier-groups` | Add modifier group |
| PUT | `/api/modifier-groups/:id` | Update modifier group |
| POST | `/api/modifier-groups/:id/options` | Add modifier option |

### Tables
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/tables` | List all tables with status |
| POST | `/api/tables` | Create table |
| PUT | `/api/tables/:id` | Update table |
| PATCH | `/api/tables/:id/status` | Update table status |
| DELETE | `/api/tables/:id` | Remove table |

### Orders
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/orders` | List orders (filter: status, type, date) |
| POST | `/api/orders` | Create order |
| GET | `/api/orders/:id` | Get order detail |
| PUT | `/api/orders/:id` | Update order (add/remove items) |
| PATCH | `/api/orders/:id/status` | Update order status |
| POST | `/api/orders/:id/cancel` | Cancel order (with reason) |
| GET | `/api/orders/kitchen-queue` | Active orders for kitchen display |

### Payments
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/orders/:id/payments` | Create payment |
| POST | `/api/payments/:id/verify` | Verify gateway payment |
| GET | `/api/orders/:id/payments` | List payments for order |
| POST | `/api/orders/:id/receipt` | Generate receipt (PDF) |

### Reports
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/reports/summary` | Daily/range summary (orders, revenue, avg) |
| GET | `/api/reports/popular-items` | Top items by quantity |
| GET | `/api/reports/cancellations` | Cancellation report |
| GET | `/api/reports/payments` | Revenue by payment method |
| GET | `/api/reports/export` | CSV export |

### Real-Time Events (WebSocket)
| Event | Direction | Description |
|-------|-----------|-------------|
| `order:new` | Server → Kitchen | New order submitted |
| `order:updated` | Server → All | Order items changed |
| `order:status` | Server → All | Order status changed |
| `order:cancelled` | Server → All | Order cancelled |
| `item:status` | Server → All | Individual item status changed |
| `table:status` | Server → All | Table status changed |
| `item:availability` | Server → All | Menu item stock status changed |

---

## 10. Reporting Needs

| Report | Audience | Frequency | Key Data |
|--------|----------|-----------|----------|
| **Daily Sales Summary** | Manager/Admin | Daily | Total orders, total revenue, avg order value, orders by type |
| **Item Popularity** | Manager/Admin | Weekly | Top 10 items by quantity, revenue per item |
| **Cancellation Report** | Manager/Admin | Daily | Count, reasons, % of total orders |
| **Payment Breakdown** | Admin | Daily | Revenue by payment method (Cash/Card/UPI), failed payment count |
| **Peak Hours** | Manager | Weekly | Orders per hour heatmap |
| **Order Turnaround** | Manager | Weekly | Avg time: created → ready, created → completed |

---

## 11. Edge Cases & Business Rules

| Scenario | Handling |
|----------|----------|
| **Item goes out of stock mid-order** | Show warning, allow removal from order. Active orders with that item still proceed. |
| **Customer cancels after kitchen starts** | Require Manager approval. Record reason. Item waste tracked. |
| **Payment gateway timeout** | Show retry button. Allow fallback to cash. Never mark order as paid without confirmation. |
| **Partial payment** | Allow multiple payments per order. Order marked "Paid" only when sum ≥ total. |
| **Adding items to existing order** | Allowed while order is in New/Confirmed/Preparing. New items sent to kitchen as an "update" with clear visual diff. |
| **Power/internet loss (offline)** | Queue operations locally using Service Worker. Show offline banner. Sync when reconnected. Prevent duplicate orders via idempotency keys. |
| **Concurrent edits** | Optimistic locking with version field. Show conflict warning if another user modified the same order. |
| **Modifier price changes** | Order snapshots modifier prices at creation time. Changes only affect new orders. |
| **Daily order number reset** | Order numbers reset at a configurable time (default: 4 AM). Unique constraint: date + number. |
| **Duplicate submit (double-tap)** | Debounce on client (300ms). Idempotency key on server. Return existing order if duplicate detected. |
| **Table occupied but no active order** | Auto-flag for cleanup after 15 minutes. Manager can override. |
| **Refund after payment** | MVP: record refund as negative payment. Trigger gateway refund API if card/UPI. |

---

*Document version: 1.0*
*Last updated: 2026-02-14*
