# 🏨 Hotel Management System

A full-stack web-based hotel management system built with **React + TypeScript**, **Node.js + Express**, **PostgreSQL (Prisma ORM)**, **Redis**, and **Socket.io**.

---

## 📋 Table of Contents

- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Development Roadmap](#development-roadmap)
  - [Step 1 – Project Setup](#step-1--project-setup)
  - [Step 2 – Room Management](#step-2--room-management)
  - [Step 3 – Reservations & Bookings](#step-3--reservations--bookings)
  - [Step 4 – Guest Check-in / Check-out](#step-4--guest-check-in--check-out)
  - [Step 5 – Billing & Invoicing](#step-5--billing--invoicing)
  - [Step 6 – Housekeeping](#step-6--housekeeping)
  - [Step 7 – Staff Management](#step-7--staff-management)
  - [Step 8 – Reports & Analytics](#step-8--reports--analytics)
  - [Step 9 – Online Booking Portal](#step-9--online-booking-portal)
  - [Step 10 – Restaurant POS & Inventory](#step-10--restaurant-pos--inventory)
  - [Step 11 – Events & Catering Management](#step-11--events--catering-management)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React + TypeScript + Tailwind CSS |
| Backend | Node.js + Express.js |
| Database | PostgreSQL |
| ORM | Prisma |
| Auth | JWT + bcrypt |
| Real-time | Socket.io |
| Caching | Redis |
| File Storage | Cloudinary |
| Notifications | SendGrid (email) · Twilio (SMS) |
| Payments | Stripe · M-Pesa |

---

## Project Structure

```
hotel-management/
├── server/                     # Node.js + Express backend
│   ├── prisma/
│   │   ├── schema.prisma       # Database schema
│   │   └── migrations/         # Auto-generated migrations
│   ├── src/
│   │   ├── modules/
│   │   │   ├── rooms/
│   │   │   ├── reservations/
│   │   │   ├── guests/
│   │   │   ├── billing/
│   │   │   ├── housekeeping/
│   │   │   ├── staff/
│   │   │   ├── restaurant/
│   │   │   ├── inventory/
│   │   │   ├── events/
│   │   │   └── reports/
│   │   ├── middleware/
│   │   │   ├── auth.ts         # JWT verification
│   │   │   └── errorHandler.ts
│   │   ├── utils/
│   │   └── app.ts
│   └── package.json
│
├── client/                     # React + TypeScript frontend
│   ├── src/
│   │   ├── pages/
│   │   ├── components/
│   │   ├── hooks/
│   │   └── main.tsx
│   └── package.json
│
├── docker-compose.yml          # PostgreSQL + Redis local setup
└── README.md
```

---

## Development Roadmap

### Step 1 – Project Setup

**Goal:** Scaffold the entire project, wire up the database, and implement authentication.

- [ ] Initialise Node.js + Express backend with TypeScript
- [ ] Set up React + TypeScript frontend with Tailwind CSS
- [ ] Configure `docker-compose.yml` for PostgreSQL and Redis
- [ ] Create Prisma schema with base models (`User`, `Room`, `Guest`, `Reservation`)
- [ ] Run initial Prisma migrations
- [ ] Implement JWT-based authentication (register, login, refresh token)
- [ ] Add role-based access control (RBAC) middleware — roles: `admin`, `receptionist`, `housekeeping`, `staff`
- [ ] Set up error handling and request validation middleware

**Key files:** `prisma/schema.prisma`, `src/middleware/auth.ts`, `src/modules/auth/`

---

### Step 2 – Room Management

**Goal:** Allow staff to create, view, update, and delete rooms and room types.

- [ ] Define `Room` and `RoomType` models in Prisma schema
- [ ] REST endpoints: `GET /rooms`, `POST /rooms`, `PUT /rooms/:id`, `DELETE /rooms/:id`
- [ ] Room statuses: `available`, `occupied`, `cleaning`, `maintenance`, `out_of_order`
- [ ] Room type management: single, double, suite, etc.
- [ ] Amenities and pricing per room type
- [ ] Frontend: room list view, room detail, create/edit forms

**Key files:** `src/modules/rooms/rooms.controller.ts`, `src/modules/rooms/rooms.service.ts`

---

### Step 3 – Reservations & Bookings

**Goal:** Implement the full booking lifecycle, preventing double bookings.

- [ ] Define `Reservation` model in Prisma schema
- [ ] Availability check logic (query rooms not booked in a date range)
- [ ] Booking statuses: `pending` → `confirmed` → `checked_in` → `checked_out` → `cancelled`
- [ ] REST endpoints: `POST /reservations`, `GET /reservations`, `PUT /reservations/:id`, `DELETE /reservations/:id`
- [ ] Date conflict validation (prevent double booking)
- [ ] Guest assignment to reservation
- [ ] Frontend: booking calendar, reservation form, reservation list with filters

**Key files:** `src/modules/reservations/reservations.service.ts`, availability query logic

---

### Step 4 – Guest Check-in / Check-out

**Goal:** Handle the guest arrival and departure workflow.

- [ ] Define `Guest` model (profile, ID document, history)
- [ ] Check-in endpoint: verify reservation → assign room → update room status to `occupied`
- [ ] Check-out endpoint: update room status to `cleaning` → generate folio → trigger billing
- [ ] Guest profile management (create, update, view history)
- [ ] Frontend: check-in/check-out screens with guest lookup

**Key files:** `src/modules/guests/`, `src/modules/reservations/checkin.service.ts`

---

### Step 5 – Billing & Invoicing

**Goal:** Generate accurate bills, record payments, and issue receipts.

- [ ] Define `Invoice`, `Payment`, and `InvoiceItem` models
- [ ] Auto-generate invoice on check-out (room charges + extras)
- [ ] Support additional charges (room service, minibar, laundry)
- [ ] Payment methods: cash, card (Stripe), M-Pesa
- [ ] Mark invoices as `paid`, `partial`, `unpaid`
- [ ] PDF receipt generation
- [ ] Frontend: invoice view, payment form, receipt download

**Key files:** `src/modules/billing/`, Stripe and M-Pesa integration

---

### Step 6 – Housekeeping

**Goal:** Track and manage room cleaning tasks in real time.

- [ ] Define `HousekeepingTask` model (room, status, assigned staff, notes)
- [ ] Task statuses: `pending`, `in_progress`, `done`, `inspected`
- [ ] Assign tasks to housekeeping staff automatically on check-out
- [ ] Socket.io integration — broadcast room status changes to all connected clients
- [ ] Frontend: housekeeping dashboard with live room status board

**Key files:** `src/modules/housekeeping/`, `src/socket.ts`

---

### Step 7 – Staff Management

**Goal:** Manage hotel staff accounts, roles, and shift assignments.

- [ ] Define `Staff` and `Shift` models
- [ ] CRUD for staff profiles linked to user accounts
- [ ] Role assignment: admin, receptionist, housekeeping, manager
- [ ] Shift scheduling (date, start time, end time, assigned staff)
- [ ] Frontend: staff list, staff profile, shift calendar

**Key files:** `src/modules/staff/`, RBAC middleware updates

---

### Step 8 – Reports & Analytics

**Goal:** Provide management with actionable insights through dashboards and exports.

- [ ] Occupancy rate report (by day / week / month)
- [ ] Revenue report (total, by room type, by payment method)
- [ ] Reservation summary (bookings, cancellations, no-shows)
- [ ] Housekeeping performance report
- [ ] Export reports to CSV and PDF
- [ ] Frontend: analytics dashboard with charts (Recharts or Chart.js)

**Key files:** `src/modules/reports/`, aggregation queries in Prisma

---

### Step 9 – Online Booking Portal

**Goal:** Allow guests to search availability and make direct bookings online.

- [ ] Public-facing route — no authentication required for browsing
- [ ] Room availability search by date range and room type
- [ ] Booking form: guest details, room selection, payment
- [ ] Stripe / M-Pesa payment integration for upfront deposit
- [ ] Booking confirmation email via SendGrid
- [ ] Frontend: public booking page with date picker and room cards

**Key files:** `src/modules/booking-portal/`, `client/src/pages/BookingPortal.tsx`

---

### Step 10 – Restaurant POS & Inventory

**Goal:** Provide a Point of Sale system for the hotel restaurant, fully integrating operations with the hotel's guest profiles, billing, and accounting systems.

- [ ] **Order Management:** Table management (assign/status), cart system, and direct ticket printing to the kitchen printer upon order submission
- [ ] **Payment Process:** Process direct POS payments (cash, card, M-Pesa) and handle split bills
- [ ] **Inventory Tracking:** Automatically deduct sold goods/ingredients from the store (inventory area) when orders are completed
- [ ] **Reports:** Generate daily sales, shift, and tax reports directly linked to the central accounting system
- [ ] **Guest Profile & Room Billing Integration:** Link orders directly to the guest profile and selectively post restaurant charges to their room folio
- [ ] Define `Table`, `MenuItem`, `Order`, `OrderItem`, and `InventoryItem` models
- [ ] Frontend: POS interface with visual tables, menu categories, cart, and payment processing

**Key files:** `src/modules/restaurant/`, `src/modules/inventory/`, kitchen printer integration

---

### Step 11 – Events & Catering Management

**Goal:** Provide a comprehensive events module to handle bookings, schedules, space allocation, catering, and billing.

- [ ] **Event Booking & Schedule:** Manage event timelines, dates, and recurring schedules
- [ ] **Space Allocation:** Assign and manage specific venues and halls within the hotel for events
- [ ] **Catering Management:** Organize food and beverage requirements for events
- [ ] **Billing & Payment Tracking:** Track deposits, issue invoices, and manage event-related payments
- [ ] **Integrations:**
  - **Room Booking:** Link event bookings to block group rooms
  - **POS:** Interface with the restaurant POS for event-related sales and catering charges
  - **Marketing:** Tools for promoting events, sending email invites, and managing RSVPs
- [ ] Define `Event`, `Venue`, `CateringRequest`, and `EventAttendee` models
- [ ] Frontend: Event management dashboard, interactive schedule calendar, and catering selection interface

**Key files:** `src/modules/events/`, event schedule interface

---

## Getting Started

### Prerequisites

- Node.js >= 18
- Docker & Docker Compose
- npm or yarn

### Installation

```bash
# Clone the repo
git clone https://github.com/your-org/hotel-management.git
cd hotel-management

# Start PostgreSQL and Redis with Docker
docker-compose up -d

# Install backend dependencies
cd server
npm install

# Apply database migrations
npx prisma migrate dev

# Install frontend dependencies
cd ../client
npm install
```

### Running the App

```bash
# Backend (from /server)
npm run dev

# Frontend (from /client)
npm run dev
```

---

## Environment Variables

Create a `.env` file in `/server`:

```env
DATABASE_URL=postgresql://user:password@localhost:5432/hotel_db
REDIS_URL=redis://localhost:6379
JWT_SECRET=your_jwt_secret
JWT_REFRESH_SECRET=your_refresh_secret
STRIPE_SECRET_KEY=sk_test_...
MPESA_CONSUMER_KEY=...
MPESA_CONSUMER_SECRET=...
SENDGRID_API_KEY=SG....
TWILIO_ACCOUNT_SID=...
TWILIO_AUTH_TOKEN=...
AWS_S3_BUCKET=...
AWS_ACCESS_KEY_ID=...
AWS_SECRET_ACCESS_KEY=...
```

---

> Built step-by-step. Each module is independent and can be developed, tested, and deployed incrementally.
