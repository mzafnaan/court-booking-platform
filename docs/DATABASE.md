# DATABASE.md

# Database Design

## Overview

This document describes the database architecture of **Courta**, a **Sports Center Management System** designed for managing a single sports center.

The database supports business operations including online court bookings, payment management, business information, content management, and business analytics.

This document focuses on database architecture, business rules, relationships between entities, and design decisions. Detailed table structures, column definitions, and migrations will be documented separately.

---

# Database Goals

The Courta database is designed to:

* Maintain data integrity.
* Eliminate unnecessary data duplication.
* Support efficient database queries.
* Be scalable and maintainable.
* Preserve transaction history.
* Support future business growth.

---

# Design Principles

The database follows these principles:

* Normalize data up to at least Third Normal Form (3NF).
* Use Foreign Keys to maintain referential integrity.
* Separate master data from transactional data.
* Preserve historical transactions using snapshots.
* Avoid storing values that can be calculated.
* Keep business rules independent from presentation logic.

---

# Core Entities

The primary entities are:

* Users
* Business Settings
* Sports
* Courts
* Court Prices
* Promotions
* Facilities
* Operating Hours
* Galleries
* Bookings
* Payments
* Reviews
* Notifications

---

# Naming Convention

## Tables

All database tables use **snake_case**.

Examples:

* users
* courts
* court_prices
* operating_hours
* business_settings

---

## Primary Key

Every table uses:

* id

---

## Foreign Keys

Foreign Keys follow this convention:

* user_id
* court_id
* booking_id
* promotion_id
* sport_id

---

## Timestamps

Every table includes:

* created_at
* updated_at

Tables using soft deletes also include:

* deleted_at

---

# Supported Sports

Currently, Courta supports:

* Futsal
* Volleyball

Sports are stored in the `sports` table so additional sports can be added without changing the overall database structure.

---

# User Roles

The application provides three user roles.

## Customer

Customers can:

* Register and log in.
* Browse available courts.
* Create bookings.
* Upload payment proof.
* View booking history.
* Leave optional reviews.

---

## Admin

Admins assist with daily business operations.

Admins can:

* Manage bookings.
* Verify payments.
* Manage customers.
* Moderate reviews.
* Manage website content.
* Manage gallery.
* Manage facilities.
* Manage FAQs.
* Manage promotions.

Admins cannot modify owner-specific settings.

---

## Owner

The Owner has full access to the business.

Owners can:

* Access the business dashboard.
* View financial reports.
* Manage business settings.
* Manage courts.
* Manage pricing.
* Manage promotions.
* Manage facilities.
* Manage operating hours.
* Manage administrator accounts.
* Configure system settings.

---

# Business Rules

## Business Structure

Courta manages a **single sports center**.

Only one Business Settings record exists in the system.

The Owner manages the entire business.

Relationship:

```text
Business Settings
│
├── Courts
├── Operating Hours
├── Gallery
├── Facilities
├── Promotions
└── Bookings
```

---

## Courts

A Court represents a sports field that can be booked.

The sports center may contain multiple courts.

Examples:

* Futsal Court A
* Futsal Court B
* Indoor Volleyball Court
* Outdoor Volleyball Court

Each court belongs to exactly one sport.

---

# Booking System

Bookings use a **flexible time range** instead of predefined booking slots.

Each booking stores:

* Booking Date
* Start Time
* End Time

Customers are free to choose the booking duration.

Example use cases:

* Casual games
* Friendly matches
* Coaching sessions
* Small tournaments
* Community training

Booking duration is calculated automatically from the selected time range.

---

# Court Availability

Courta does not generate booking slots.

Court availability is calculated dynamically using existing bookings.

A court cannot have overlapping bookings during the same time period.

This approach keeps the database lightweight and flexible.

---

# Booking Expiration

Each booking has a payment deadline of **10 minutes** after creation.

If payment is not completed:

* Booking status becomes **EXPIRED**.
* The reserved court becomes available again.
* The booking cannot continue.

---

# Pricing

Regular pricing is stored separately from promotional pricing.

Pricing rules may vary based on:

* Day
* Operating hours

Examples:

* Weekday
* Weekend
* Morning
* Afternoon
* Evening

Each court may have multiple pricing rules.

---

# Promotions

Promotions are managed independently from regular pricing.

Each promotion contains:

* Promotion Name
* Promotion Type
* Promotion Value
* Active Period
* Status

A promotion may be applied to one or multiple courts.

Promotions never modify the original court price.

Instead, promotions affect only the booking calculation.

---

# Price Snapshot

When a booking is successfully created, the system stores a snapshot of:

* Court Name
* Applied Price
* Applied Promotion
* Total Amount

This ensures historical transactions remain accurate even when pricing changes in the future.

---

# Payment

Courta currently uses manual payment through QRIS.

Payment flow:

1. Customer creates a booking.
2. QRIS is displayed.
3. Customer completes payment.
4. Customer uploads payment proof.
5. Admin verifies payment.
6. Booking becomes confirmed.

Each booking has exactly one payment.

The payment architecture is designed for future Payment Gateway integration.

---

# Reviews

Reviews are optional.

Customers may:

* Give ratings.
* Write reviews.
* Skip the review process.

Only customers who have completed bookings may submit reviews.

---

# Gallery

Gallery belongs to the sports center.

Gallery may contain:

* Court photos
* Facilities
* Events
* Tournaments
* Business activities

Gallery is not attached to individual courts.

---

# Facilities

Facilities are managed using master data.

Examples:

* Parking Area
* Toilet
* Prayer Room
* Wi-Fi
* Cafeteria
* Changing Room

The Owner selects the facilities available in the sports center.

---

# Operating Hours

Operating hours represent the business schedule of the sports center.

Each day may have different opening and closing hours.

---

# Status Definitions

## Booking Status

* WAITING_PAYMENT
* CONFIRMED
* COMPLETED
* CANCELLED
* EXPIRED

---

## Payment Status

* PENDING
* PAID
* FAILED
* REFUNDED

---

## Court Status

* AVAILABLE
* UNAVAILABLE
* MAINTENANCE

---

# Notifications

The database stores notifications for all users.

Examples:

* Booking confirmed
* Booking cancelled
* Payment verified
* Promotion published
* System announcement

---

# Soft Delete Strategy

Soft Deletes are applied to:

* Users
* Courts
* Galleries
* Reviews
* Promotions

Soft Deletes are not applied to:

* Bookings
* Payments

Transaction history must always be preserved.

---

# Index Strategy

Indexes are applied to improve query performance.

Examples:

* email
* phone
* court_id
* booking_date
* status
* payment_status
* sport_id

Detailed index implementation will be documented in `SCHEMA.md`.

---

# Future Scalability

The database architecture is designed to support future expansion without major structural changes.

Potential future features include:

* Payment Gateway Integration
* Membership System
* Voucher System
* Tournament Management
* Dynamic Pricing
* Equipment Rental
* Inventory Management
* Employee Management
* Attendance System
* Point of Sale (POS)
* Loyalty Program
* Push Notifications
* Advanced Financial Reports

The current architecture focuses on a **single sports center**, while remaining modular enough to support future enhancements.