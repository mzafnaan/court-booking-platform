# BUSINESS_RULES.md

# Business Rules

This document defines the business logic and operational rules for the Sports Center Management System.

All features must follow these rules.

AI should never assume business behavior outside this document.

---

# Core Principle

This application is a **Sports Center Management System**, not just an online booking website.

The website is only one booking channel.

The management system must represent the real operational workflow inside the sports center.

---

# Booking Channels

Bookings can originate from multiple sources.

Supported booking sources:

* Website
* Walk-in
* WhatsApp
* Phone

Every booking, regardless of its source, must be stored inside the same booking system.

There must never be separate schedules for online and offline bookings.

---

# Single Source of Truth

The Booking table is the only source of truth.

Landing Page availability, Admin Dashboard, Reports, and Customer Booking History must all use the same booking data.

Do not generate separate availability systems.

---

# Court Availability

A court is considered unavailable when:

* there is an existing confirmed booking
* there is an existing manual booking
* the selected time overlaps another booking

Available schedules shown on the landing page must always reflect the latest booking data.

---

# Manual Booking

Admin can create bookings manually.

Manual booking is intended for:

* Walk-in customers
* WhatsApp reservations
* Phone reservations

Manual booking should require:

* Customer Name
* Phone Number
* Court
* Booking Date
* Start Time
* End Time
* Booking Source

---

# Online Booking

Customers can create bookings only after authentication.

Booking flow:

Login

↓

Select Court

↓

Select Date

↓

Select Time

↓

Booking Created

↓

Upload Payment Proof

↓

Waiting Verification

↓

Admin Verification

↓

Confirmed

---

# Payment Verification

Version 1 uses manual bank transfer.

Customers upload payment proof.

Payment proof alone does NOT confirm payment.

Admin must verify the payment manually before confirming the booking.

---

# Booking Status

Every booking must have one status.

Allowed statuses:

Pending Payment

Waiting Verification

Confirmed

Completed

Cancelled

Avoid introducing additional statuses unless requested.

---

# Booking Source

Each booking must store its origin.

Allowed values:

Website

Walk-in

WhatsApp

Phone

This information is used for reporting and analytics.

---

# Double Booking Prevention

The system must never allow two bookings that overlap on the same court.

Validation must happen on the backend.

Frontend validation alone is not sufficient.

---

# Customer Rules

Customers can:

* Register
* Login
* View schedules
* Create bookings
* Upload payment proof
* View booking history
* Update profile

Customers cannot:

* Confirm payment
* Edit other bookings
* Access admin pages

---

# Admin Rules

Admin can:

* Manage courts
* Create manual bookings
* Edit bookings
* Cancel bookings
* Verify payments
* Manage landing page content
* View reports
* Manage customers

Admin cannot:

* Bypass booking validation
* Create overlapping bookings

---

# Schedule Management

Schedule availability must always be generated from booking data.

Never hardcode available time slots.

The system must calculate availability dynamically.

---

# Landing Page Rules

Visitors can:

* View courts
* View facilities
* View gallery
* View available schedules

Visitors cannot:

* Book courts
* Upload payment proof
* Access dashboards

Booking requires authentication.

---

# Reporting Rules

Reports should include:

* Total Bookings
* Booking by Date
* Booking by Court
* Booking by Source
* Booking Status

Example booking sources:

Website

Walk-in

WhatsApp

Phone

---

# Data Consistency

Every booking created by any source must:

* Block the selected schedule
* Appear in the admin dashboard
* Affect landing page availability
* Be included in reports

There should never be hidden bookings.

---

# Future-Proofing

The architecture should remain modular.

Future features may be added without changing the core booking workflow.

However, Version 1 should focus only on the rules defined in this document.

Do not generate speculative features.

---

# AI Development Rules

When implementing features:

* Always follow this document.
* Never invent new booking rules.
* Never invent new booking statuses.
* Never introduce unsupported booking sources.
* Do not assume business logic.
* Ask for clarification if a rule is missing.

This document is the primary reference for all business logic.