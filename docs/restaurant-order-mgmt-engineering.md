# Restaurant Order Management System — Engineering Plan

## 1. Recommended Tech Stack

### Frontend
| Layer | Choice | Justification |
|-------|--------|---------------|
| **Framework** | **Next.js 14 (App Router)** | Full-stack React framework. Server components for admin pages (SEO not needed, but SSR gives fast initial loads). Client components for interactive views (order creation, KDS). Built-in API routes eliminate need for separate backend service. |
| **UI Library** | **Tailwind CSS + shadcn/ui** | Utility-first CSS for rapid UI development. shadcn/ui provides accessible, customizable components (tables, modals, buttons, forms) without heavy bundle. Touch-friendly by default. |
| **State Management** | **Zustand** | Lightweight, simple store for client-side state (cart, current order). No Redux boilerplate. Works well with React Server Components. |
| **Real-time** | **Socket.IO (client)** | Established WebSocket library with auto-reconnection, fallback to polling (important for kitchen reliability). |
| **Forms** | **React Hook Form + Zod** | Performant forms with schema validation. Zod schemas shared between client and server for type safety. |
| **Charts** | **Recharts** | Lightweight React charting library for reports dashboard. |

### Backend
| Layer | Choice | Justification |
|-------|--------|---------------|
| **Runtime** | **Node.js 20 (LTS)** | Same language as frontend (TypeScript end-to-end). Large ecosystem. Good WebSocket support. |
| **API** | **Next.js API Routes + tRPC** | tRPC for type-safe API calls between Next.js frontend and backend. Eliminates manual API typing. Falls back to REST for external integrations (payment gateways). |
| **Database** | **PostgreSQL 16** | Robust relational DB. Perfect for structured order/payment data. JSONB for flexible fields (modifiers, tax config). Excellent concurrent read performance for KDS. |
| **ORM** | **Prisma** | Type-safe database access. Auto-generated TypeScript types from schema. Easy migrations. Good PostgreSQL support. |
| **Real-time** | **Socket.IO (server)** | Handles WebSocket connections for KDS and order status updates. Rooms per restaurant (future multi-tenant ready). |
| **Auth** | **NextAuth.js (Auth.js v5)** | Built-in session management, JWT, role-based middleware. Email/password provider. Integrates natively with Next.js. |
| **Payments** | **Stripe SDK + Razorpay SDK** | Official SDKs for both gateways. Server-side payment intent creation, webhook verification. |
| **Printing** | **escpos library (Node)** | ESC/POS command generation. Works with network thermal printers via TCP. WebUSB for USB-connected printers (client-side). |

### Infrastructure
| Layer | Choice | Justification |
|-------|--------|---------------|
| **Hosting** | **Vercel (frontend) + Railway (DB + WebSocket)** | Vercel: zero-config Next.js deployment, edge functions, automatic HTTPS. Railway: managed PostgreSQL + persistent WebSocket server (Vercel doesn't support long-lived WebSocket connections). |
| **Alternative** | **Single VPS (DigitalOcean/Hetzner)** | If cost is a concern: $12/mo VPS running Docker with Next.js + PostgreSQL + Socket.IO. Simpler ops for a single restaurant. |
| **File Storage** | **Cloudflare R2 or S3** | Menu item images. R2 is cheaper with free egress. |
| **Monitoring** | **Sentry (errors) + Uptime Robot (uptime)** | Free tiers sufficient for MVP. |
| **CI/CD** | **GitHub Actions** | Automated testing on PR, deploy on merge to main. |

### Offline Resilience (Nice-to-have)
| Layer | Choice | Justification |
|-------|--------|---------------|
| **Service Worker** | **Workbox (via next-pwa)** | Cache static assets and API responses. Queue failed mutations. |
| **Local Queue** | **IndexedDB (via Dexie.js)** | Store pending orders locally when offline. Sync with idempotency keys on reconnect. |

### Full Architecture Diagram
```
┌─────────────────────────────────────────────────────────────┐
│                        CLIENTS                               │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │ Desktop  │  │ Tablet   │  │ Phone    │  │ Kitchen  │   │
│  │ (Admin)  │  │ (Cashier)│  │ (Waiter) │  │ (KDS)    │   │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘   │
│       │              │              │              │         │
│       └──────────────┼──────────────┼──────────────┘         │
│                      │         HTTPS + WSS                   │
└──────────────────────┼──────────────────────────────────────┘
                       │
                ┌──────▼──────┐
                │   Vercel    │ (Next.js SSR + API Routes)
                │  + tRPC     │
                └──────┬──────┘
                       │
          ┌────────────┼────────────┐
          │            │            │
   ┌──────▼──────┐ ┌──▼───┐ ┌─────▼──────┐
   │  Railway    │ │ R2/S3│ │  Stripe/   │
   │  PostgreSQL │ │(imgs)│ │  Razorpay  │
   │  + Redis    │ └──────┘ │  (webhooks)│
   │  + Socket.IO│          └────────────┘
   └─────────────┘
```

---

## 2. Milestones

### Week 1: Foundation + Menu + Tables
| Day | Tasks |
|-----|-------|
| **Day 1-2** | Project setup: Next.js, Tailwind, shadcn/ui, Prisma, PostgreSQL. Auth (NextAuth) with role-based middleware. User CRUD (Admin). Login/logout flow. |
| **Day 3-4** | Database schema (all entities). Prisma migrations. Seed script (sample menu, tables, users). |
| **Day 5** | Menu management UI (Admin): categories CRUD, menu items CRUD with image upload, modifier groups + options. |
| **Day 6** | Table management UI (Admin): table CRUD, sections. Table map view (Cashier/Waiter) with status colors. |
| **Day 7** | Settings page: restaurant info, tax config. Integration tests for auth + menu + table APIs. |

**Deliverable:** Admin can log in, manage menu (with modifiers), manage tables, configure restaurant. Staff can log in with assigned roles.

### Week 2: Order Flow + Kitchen Display
| Day | Tasks |
|-----|-------|
| **Day 8-9** | New Order page: order type selection, table assignment, menu browser with category tabs, modifier sheet, cart with quantity/remove, order summary with tax calc. Order creation API. |
| **Day 10** | Order list page with filters (status, type, date). Order detail page with item status, notes, timeline. |
| **Day 11-12** | Socket.IO setup: server rooms, event broadcasting. Kitchen Display System: 3-column Kanban, order cards, item checkboxes, elapsed timers, color-coded urgency, sound alerts. |
| **Day 13** | Order status updates: status transitions (New→Confirmed→Preparing→Ready→Served), item-level status. Real-time sync across all screens. |
| **Day 14** | Add items to existing order. Cancel order flow (with reason, Manager auth). Table status auto-update (Available↔Occupied). |

**Deliverable:** Full order lifecycle works. Kitchen sees orders in real-time. Status updates propagate to all clients. Tables update automatically.

### Week 3: Payments + Receipts + Reports
| Day | Tasks |
|-----|-------|
| **Day 15-16** | Checkout page: order summary, payment method selector, amount tendered/change calc (cash). Stripe integration: payment intent, client-side confirmation, webhook handler. Razorpay integration: order creation, checkout popup, webhook verification. |
| **Day 17** | Split payment support. Payment recording API. Order→Paid transition. Gateway config in Settings (Admin). |
| **Day 18** | Receipt generation: digital view, PDF generation (using @react-pdf/renderer or puppeteer). Thermal printer support (ESC/POS over network). Print button on receipt. |
| **Day 19-20** | Reports dashboard: summary metrics, revenue-by-hour chart, top items, order type breakdown, payment method split, cancellation report. Date range filter. CSV export. |
| **Day 21** | Dashboard home page: role-specific metrics, quick actions, recent orders. Mobile-responsive navigation (bottom tabs for phone). |

**Deliverable:** End-to-end flow complete: order → kitchen → serve → pay → receipt. Reports show daily activity. Dashboard gives at-a-glance overview.

### Week 4: Polish + Offline + Testing + Deploy
| Day | Tasks |
|-----|-------|
| **Day 22-23** | Responsive polish: phone layout for Waiter (table map, order creation), tablet layout for Kitchen, desktop layout for Admin. Touch target audit (44px min). Kitchen display: high contrast, large fonts. |
| **Day 24** | Offline resilience (nice-to-have): Service Worker with Workbox. Offline indicator banner. Local order queue (IndexedDB). Sync on reconnect with idempotency keys. |
| **Day 25-26** | Testing: unit tests (business logic, tax calc, order state machine), integration tests (API routes, payment webhooks), E2E tests (Playwright: order creation → kitchen → payment → receipt). |
| **Day 27** | Security audit: input sanitization, SQL injection (Prisma handles), XSS (React handles), RBAC enforcement on all API routes, payment webhook signature verification, rate limiting on auth endpoints. |
| **Day 28** | Deployment: production database, environment variables, Vercel deploy, Railway deploy. Monitoring: Sentry, uptime checks. Seed production data (menu, tables, users). User acceptance walkthrough. |

**Deliverable:** Production-ready MVP deployed. All critical flows tested. Monitoring active.

---

## 3. Risks & Mitigations

| # | Risk | Likelihood | Impact | Mitigation |
|---|------|-----------|--------|------------|
| 1 | **Payment gateway integration delays** (API changes, verification, sandbox issues) | Medium | High | Start Stripe integration early (Day 15). Use test/sandbox mode throughout development. Have cash-only fallback ready. Razorpay can be deferred to post-MVP if Stripe works. |
| 2 | **Real-time reliability** (WebSocket disconnections in kitchen) | Medium | High | Socket.IO auto-reconnect with exponential backoff. Fallback: poll kitchen endpoint every 5s. Visual connection status indicator on KDS. Audible disconnect warning. |
| 3 | **Thermal printer compatibility** (many printer models, driver issues) | Medium | Medium | Support only ESC/POS-compatible network printers (most common). Provide test print in settings. Fallback: browser print dialog for non-ESC/POS printers. |
| 4 | **Scope creep** (stakeholders request features during MVP) | High | Medium | Strict MVP scope documented in PRD. Out-of-scope list agreed upfront. Track feature requests in backlog, defer to v1.1. |
| 5 | **Offline mode complexity** | Medium | Medium | Marked as nice-to-have. Implement only basic offline queue (order creation). Skip offline for payments (require online). Clear UX for offline state. |
| 6 | **Concurrent order edits** (two staff modify same order) | Low | Medium | Optimistic locking: version field on Order. Reject stale updates with 409 Conflict. Show "order was modified by [user]" toast with refresh option. |
| 7 | **Poor kitchen WiFi** | Medium | High | KDS page designed to be lightweight (< 200KB). Minimal JS. Server-sent events as WebSocket fallback. Consider dedicated router for kitchen tablet. |
| 8 | **Performance with many orders** | Low | Medium | Paginate order list (20 per page). KDS only shows active orders (max ~30). Archive completed orders after 24h (still queryable via reports). Database indexes on status, created_at, type. |

---

## 4. Test Plan

### 4.1 Unit Tests (Jest + React Testing Library)

| Area | What to Test | Example |
|------|-------------|---------|
| **Tax calculation** | Correct subtotal, tax, total for various item/modifier combos | `calcOrderTotal([{price: 350, qty: 1, modifiers: [{adj: 30}]}], taxRate: 0.05)` → `{subtotal: 380, tax: 19, total: 399}` |
| **Order state machine** | Valid transitions, rejected invalid transitions | `canTransition('Preparing', 'Ready')` → `true`; `canTransition('Paid', 'New')` → `false` |
| **Daily order number** | Sequential numbering, daily reset | `nextOrderNumber('2026-02-14', lastNumber: 23)` → `24`; new day → `1` |
| **RBAC middleware** | Role permissions for each endpoint | `canAccess('Waiter', '/api/users')` → `false`; `canAccess('Admin', '/api/users')` → `true` |
| **Cart operations** | Add/remove/update items, modifier price calc | Add item with modifier → cart total updates. Remove item → total decreases. |
| **Payment split validation** | Sum of payments ≥ order total | `validatePayments([{amount: 300}, {amount: 278}], orderTotal: 578)` → `valid` |
| **Receipt formatting** | Correct line items, tax breakdown | Generate receipt for known order → matches snapshot |

**Target: 80%+ coverage on business logic modules.**

### 4.2 Integration Tests (Supertest + Prisma test client)

| Area | What to Test |
|------|-------------|
| **Auth flow** | Login → get JWT → access protected route → refresh token → logout |
| **Menu CRUD** | Create category → create item → add modifier group → update price → toggle availability |
| **Order lifecycle** | Create order → add items → confirm → update item status → mark ready → complete |
| **Payment processing** | Create payment (cash) → verify order marked paid. Stripe webhook → payment recorded. |
| **RBAC enforcement** | Waiter cannot access admin routes. Kitchen cannot create payments. Manager can cancel orders. |
| **Concurrent edits** | Two simultaneous updates → one succeeds, one gets 409 Conflict |
| **Edge cases** | Order with out-of-stock item → rejected. Cancel after preparing → requires Manager. Empty cart → rejected. |

**Target: All API routes have at least 1 happy-path and 1 error-path test.**

### 4.3 End-to-End Tests (Playwright)

| Flow | Steps |
|------|-------|
| **Full order lifecycle** | Login as Cashier → New Order (dine-in) → Select table → Add 3 items with modifiers → Place order → Switch to Kitchen view → Verify order appears → Mark items ready → Switch back → Checkout → Pay cash → Verify receipt → Verify table status changes |
| **Takeout order** | Login → New takeout order → Add items → Place → Kitchen marks ready → Cashier marks picked up → Pay → Receipt |
| **Menu management** | Login as Admin → Create category → Add item with modifiers → Set out of stock → Verify item hidden in order creation → Re-enable → Verify item visible |
| **Kitchen real-time** | Open Kitchen tab → Create order in another tab → Verify order appears on KDS within 5s → Mark items → Verify status updates |
| **Access control** | Login as Waiter → Verify cannot access Settings, Users, Reports → Can access Table Map, Orders |
| **Payment failure** | Start card payment → Simulate failure → Verify retry option → Fall back to cash → Complete |

**Target: 6 critical E2E flows passing before each deploy.**

### 4.4 Manual Testing Checklist (Pre-Launch)

- [ ] Test on actual tablet (iPad or Android) — touch targets, scrolling, orientation
- [ ] Test KDS on wall-mounted tablet — readability at 2m distance
- [ ] Test phone layout (Waiter flow) — order creation usability
- [ ] Test thermal printer — receipt prints correctly, aligned, readable
- [ ] Test with 50 concurrent active orders — no performance degradation
- [ ] Test WiFi disconnect/reconnect — KDS recovers, orders sync
- [ ] Test with real Stripe/Razorpay sandbox transactions
- [ ] Full walkthrough with non-technical restaurant staff (usability test)

---

## 5. Deployment Plan

### 5.1 Environments

| Environment | Purpose | Infrastructure |
|-------------|---------|---------------|
| **Local** | Development | Docker Compose: Next.js (hot reload) + PostgreSQL + Redis |
| **Staging** | Testing & UAT | Vercel Preview (auto-deploy on PR) + Railway staging DB |
| **Production** | Live restaurant use | Vercel Production + Railway production DB + Sentry monitoring |

### 5.2 Deployment Pipeline

```
Developer pushes code
        │
        ▼
┌─────────────────┐
│  GitHub Actions  │
│  ┌────────────┐ │
│  │ Lint (ESLint│ │
│  │ + Prettier) │ │
│  ├────────────┤ │
│  │ Type Check │ │
│  │ (tsc)      │ │
│  ├────────────┤ │
│  │ Unit Tests │ │
│  │ (Jest)     │ │
│  ├────────────┤ │
│  │ Integration│ │
│  │ Tests      │ │
│  ├────────────┤ │
│  │ E2E Tests  │ │
│  │ (Playwright│ │
│  └────────────┘ │
└────────┬────────┘
         │ All pass
         ▼
┌─────────────────┐     ┌─────────────────┐
│ Merge to main   │────▶│ Vercel auto-    │
│                 │     │ deploys to prod  │
└─────────────────┘     └────────┬────────┘
                                 │
                        ┌────────▼────────┐
                        │ Post-deploy:    │
                        │ - Prisma migrate│
                        │ - Health check  │
                        │ - Sentry release│
                        └─────────────────┘
```

### 5.3 Database Migrations

- Prisma Migrate for schema changes
- Migrations run automatically on deploy (via build command or separate CI step)
- Breaking migrations require planned downtime window (announce 30 min before)
- Seed script for initial data (menu, tables, admin user)

### 5.4 Environment Variables

| Variable | Where | Purpose |
|----------|-------|---------|
| `DATABASE_URL` | Railway | PostgreSQL connection string |
| `NEXTAUTH_SECRET` | Vercel | JWT signing secret |
| `NEXTAUTH_URL` | Vercel | App URL for auth callbacks |
| `STRIPE_SECRET_KEY` | Vercel | Stripe server-side key |
| `STRIPE_WEBHOOK_SECRET` | Vercel | Stripe webhook signature verification |
| `RAZORPAY_KEY_ID` | Vercel | Razorpay API key |
| `RAZORPAY_KEY_SECRET` | Vercel | Razorpay server-side secret |
| `RAZORPAY_WEBHOOK_SECRET` | Vercel | Razorpay webhook verification |
| `REDIS_URL` | Railway | Socket.IO adapter (multi-instance) |
| `R2_ACCESS_KEY` / `R2_SECRET_KEY` | Vercel | Image storage credentials |
| `SENTRY_DSN` | Vercel | Error tracking |

### 5.5 Go-Live Checklist

- [ ] Production database provisioned and migrated
- [ ] All environment variables set in Vercel + Railway
- [ ] SSL/HTTPS verified (automatic on Vercel)
- [ ] Custom domain configured (if applicable)
- [ ] Admin account created, initial password set
- [ ] Restaurant info, tax config, payment gateway configured via Settings
- [ ] Menu seeded (categories, items, modifiers)
- [ ] Tables configured
- [ ] Staff accounts created with correct roles
- [ ] Thermal printer connected and test print successful
- [ ] Sentry monitoring active, test error sent
- [ ] Uptime monitoring configured (Uptime Robot)
- [ ] Backup schedule verified (Railway auto-backups)
- [ ] Staff training session completed (30-min walkthrough)
- [ ] Rollback plan documented (revert Vercel deployment + DB backup restore)

### 5.6 Post-Launch Monitoring

| What | Tool | Alert |
|------|------|-------|
| Errors / exceptions | Sentry | Slack/email on new error |
| Uptime | Uptime Robot | Alert if down > 1 min |
| Database performance | Railway metrics | Alert if query > 2s |
| WebSocket connections | Custom health endpoint | Alert if KDS disconnected > 30s |
| Payment webhook failures | Stripe/Razorpay dashboard | Alert on failed webhook |

---

## 6. Post-MVP Roadmap (v1.1+)

| Priority | Feature | Effort |
|----------|---------|--------|
| P1 | Online ordering (customer-facing menu + ordering) | 2 weeks |
| P1 | Inventory management (stock tracking, low-stock alerts) | 1 week |
| P2 | Multi-station kitchen routing | 1 week |
| P2 | Discount / coupon system | 1 week |
| P2 | Third-party delivery integration (Swiggy, Zomato) | 2 weeks |
| P3 | Multi-restaurant / franchise support | 3 weeks |
| P3 | Employee scheduling + shift management | 2 weeks |
| P3 | Customer loyalty program | 1 week |
| P3 | Advanced analytics + demand forecasting | 2 weeks |

---

*Document version: 1.0*
*Last updated: 2026-02-14*
