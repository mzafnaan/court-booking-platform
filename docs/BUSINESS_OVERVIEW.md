# BUSINESS_RULES.md

# Business Rules

This document defines the business logic and operational rules for **Courta**, a Sports Center Management System.

All features must follow these rules.

Business logic should never be assumed outside this document.

---

# Core Principle

Courta is a **Sports Center Management System**, not just an online court booking website.

The website is one of the booking channels used by customers, while the system itself manages the complete business operations of a single sports center.

All business processes should reflect real operational workflows.

---

# Business Scope

Version 1 focuses on managing **one sports center**.

The system is not designed as a marketplace or a multi-business platform.

Future expansion is possible, but the current business model assumes a single sports center managed by one Owner.

---

# Booking Channels

Bookings may originate from multiple channels.

Supported booking sources:

* Website
* Walk-in
* WhatsApp
* Phone

Every booking, regardless of its source, must be stored in the same booking system.

Separate schedules for online and offline bookings are not allowed.

---

# Single Source of Truth

The `bookings` table is the only source of truth for booking information.

The following features must always use booking data from the same source:

* Landing Page Availability
* Admin Dashboard
* Owner Dashboard
* Reports
* Customer Booking History

Availability must never be generated using a separate scheduling system.

---

# Court Availability

Court availability is calculated dynamically.

A court is considered unavailable when:

* There is an active booking during the selected time.
* The requested booking period overlaps another active booking.

Completed, cancelled, or expired bookings do not block court availability.

---

# Manual Booking

Admins may create bookings manually.

Manual bookings are intended for:

* Walk-in customers
* WhatsApp reservations
* Phone reservations

Manual bookings require:

* Customer Name
* Phone Number
* Court
* Booking Date
* Start Time
* End Time
* Booking Source

Optional:

* Booking Notes

---

# Online Booking

Customers must authenticate before creating a booking.

Booking flow:

```text
Login

↓

Select Sport

↓

Select Court

↓

Select Date

↓

Select Time

↓

Review Booking Summary

↓

Create Booking

↓

QRIS Payment

↓

Upload Payment Proof

↓

Admin Verification

↓

Booking Confirmed
```

---

# Booking Expiration

Every newly created booking has a payment deadline of **10 minutes**.

If payment is not verified before the deadline:

* Booking status becomes **EXPIRED**
* Reserved court becomes available again
* The booking cannot continue

---

# Payment Verification

Version 1 uses manual QRIS payment.

Customers upload payment proof after completing payment.

Uploading payment proof does **not** automatically confirm payment.

An Admin must manually verify the payment before confirming the booking.

---

# Pricing Rules

Court pricing may vary depending on:

* Day
* Operating hours

Weekend pricing may differ from weekday pricing.

Promotional pricing must not modify the original pricing rules.

Price calculations are determined during the booking process.

---

# Booking Status

Each booking must have exactly one status.

Allowed statuses:

* WAITING_PAYMENT
* CONFIRMED
* COMPLETED
* CANCELLED
* EXPIRED

No additional booking statuses should be introduced without updating this document.

---

# Payment Status

Each payment must have exactly one status.

Allowed statuses:

* PENDING
* PAID
* FAILED
* REFUNDED

---

# Booking Source

Each booking stores its origin.

Allowed values:

* Website
* Walk-in
* WhatsApp
* Phone

Booking source is used for reporting and business analytics.

---

# Double Booking Prevention

The system must never allow overlapping bookings for the same court.

Validation must always occur on the backend.

Frontend validation alone is not sufficient.

---

# Customer Rules

Customers can:

* Register
* Login
* View courts
* View availability
* Create bookings
* Upload payment proof
* View booking history
* Update profile
* Submit optional reviews

Customers cannot:

* Verify payments
* Access dashboards
* Edit other users' bookings

---

# Admin Rules

Admins can:

* Manage bookings
* Create manual bookings
* Verify payments
* Cancel bookings
* Manage courts
* Manage promotions
* Manage galleries
* Manage facilities
* Manage FAQs
* Manage customers
* Manage website content

Admins cannot:

* Override booking validation
* Create overlapping bookings
* Access owner-only business reports

---

# Owner Rules

The Owner has full access to the system.

Owners can:

* Access the business dashboard
* View financial reports
* View business analytics
* Manage business settings
* Manage administrator accounts
* Manage courts
* Manage pricing
* Manage promotions
* Manage operating hours
* Manage facilities
* View customer statistics
* Monitor business performance

---

# Landing Page Rules

Visitors may:

* View available courts
* View facilities
* View gallery
* View promotions
* View business information

Visitors cannot:

* Create bookings
* Upload payment proof
* Access dashboards

Booking requires authentication.

---

# Reporting Rules

Reports should include:

* Total Bookings
* Booking Trends
* Booking by Date
* Booking by Court
* Booking by Source
* Booking Status
* Revenue
* Court Utilization
* Peak Booking Hours
* Most Booked Court

Reports always use transactional data.

---

# Owner Dashboard

The Owner Dashboard is designed for monitoring business performance.

It provides information such as:

* Today's Revenue
* Monthly Revenue
* Total Bookings
* Court Utilization
* Peak Booking Hours
* Most Booked Court
* Recent Bookings
* Recent Payments
* Customer Statistics

Dashboard analytics must always originate from booking and payment records.

---

# Data Consistency

Every booking, regardless of its source, must:

* Block the selected schedule
* Appear in the Admin Dashboard
* Appear in the Owner Dashboard
* Affect court availability
* Be included in reports

Hidden bookings are not allowed.

---

# Future-Proofing

The architecture should remain modular.

Future enhancements such as:

* Payment Gateway
* Membership
* Tournament Management
* Inventory Management
* Equipment Rental
* Point of Sale (POS)

may be added without changing the core booking workflow.

---

# AI Development Rules

When implementing features:

* Always follow this document.
* Never invent new business rules.
* Never invent unsupported booking statuses.
* Never introduce unsupported booking sources.
* Never introduce marketplace or multi-business concepts.
* Reuse existing business entities whenever possible.
* Ask for clarification whenever business rules are incomplete.

This document serves as the primary business logic reference for the entire project.