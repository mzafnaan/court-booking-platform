# DATABASE.md

# Database Design

## Overview

This document describes the database design of **Courta**, a sports court booking platform that currently supports **Futsal** and **Volleyball**.

The purpose of this document is to define the database architecture, business rules, relationships between entities, and design decisions that serve as the foundation for implementation. Detailed table structures, data types, and migrations will be documented separately.

---

# Database Goals

The Courta database is designed to:

* Maintain data integrity.
* Eliminate unnecessary data duplication.
* Support efficient database queries.
* Be scalable and maintainable.
* Provide a solid foundation for future business growth.

---

# Design Principles

The database follows the principles below:

* Normalize data up to at least Third Normal Form (3NF).
* Use Foreign Keys to maintain referential integrity.
* Avoid storing values that can be calculated.
* Separate master data from transactional data.
* Apply soft deletes only where appropriate.
* Preserve transaction history even when master data changes.

---

# Core Entities

The primary entities in the Courta database are:

* Users
* Sports
* Venues
* Courts
* Court Prices
* Promotions
* Facilities
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
* venues
* court_prices
* booking_reviews

---

## Primary Key

Every table uses:

* id

---

## Foreign Keys

Foreign keys follow this format:

* user_id
* venue_id
* court_id
* booking_id

---

## Timestamps

Every table includes:

* created_at
* updated_at

Tables using soft deletes also include:

* deleted_at

---

# Supported Sports

Currently, Courta supports two sports:

* Futsal
* Volleyball

Sports are stored in the `sports` table so additional sports can be added in the future without changing the database structure.

---

# User Roles

The system provides three user roles.

## Customer

Customers can:

* Register and log in.
* Browse venues.
* Book courts.
* Upload payment proof.
* View booking history.
* Leave an optional review.

---

## Admin

Admins assist with daily business operations.

Admins can:

* Manage bookings.
* Verify payments.
* Manage customers.
* Moderate reviews.
* Manage website content.
* Manage banners.
* Manage FAQs.
* Manage galleries.
* Update other website information.

Admins cannot access owner-specific settings.

---

## Owner

Owners have full business management access.

Owners can:

* Access the business dashboard.
* View financial reports.
* Manage venues.
* Manage courts.
* Manage pricing.
* Manage promotions.
* Manage facilities.
* Manage operating hours.
* Manage admin accounts.
* Configure venue settings.

---

# Business Rules

## Venue Ownership

* One Owner can own only one Venue.
* One Venue belongs to only one Owner.
* One Venue can have multiple Courts.

Relationship:

Owner

└── Venue

      ├── Courts

      ├── Gallery

      ├── Facilities

      ├── Bookings

      └── Reviews

---

## Courts

A Court represents a bookable sports field.

A Venue may contain multiple Courts, including different sports.

Examples:

* Futsal Court A
* Futsal Court B
* Indoor Volleyball Court
* Outdoor Volleyball Court

---

# Booking System

Bookings use a **flexible time range** instead of fixed time slots.

Each booking consists of:

* Booking Date
* Start Time
* End Time

Customers are free to choose the booking duration.

Example use cases:

* Casual games
* Friendly matches
* Futsal coaching sessions
* Small tournaments
* Community training

Booking duration is calculated using the start and end time, so a dedicated duration field is unnecessary.

---

# Court Availability

Courta does not use predefined booking slots.

Court availability is calculated dynamically based on existing bookings.

A court cannot have two bookings that overlap during the same time period.

This approach keeps the database simple, flexible, and scalable.

---

# Booking Expiration

Each booking has a payment deadline of **10 minutes** after creation.

If payment is not completed within that period:

* The booking status becomes **EXPIRED**.
* The court becomes available again.
* The booking can no longer be continued.

---

# Pricing

Regular pricing is stored separately from promotional pricing.

Owners can define pricing based on:

* Day
* Operating hours

Examples:

* Weekday
* Weekend
* Morning
* Afternoon
* Night

This allows each court to have multiple pricing rules.

---

# Promotions

Promotions are managed independently from regular pricing.

Each promotion contains:

* Promotion name
* Promotion type
* Promotion value
* Active period
* Status

Promotions **do not modify the regular price**.

Instead, they only affect the calculated booking price during the booking process.

---

# Price Snapshot

When a booking is successfully created, the system stores a snapshot of the applicable price.

This ensures future price changes do not affect historical transactions.

All bookings permanently retain the price that was valid when the booking was made.

---

# Payment

Courta currently uses manual payments through QRIS.

Payment flow:

1. Customer creates a booking.
2. The system displays the QRIS code.
3. The customer completes the payment.
4. The customer uploads the payment proof.
5. An Admin verifies the payment.
6. The booking is confirmed.

Each booking has exactly one payment record.

The payment architecture is designed to support Payment Gateway integration in the future without requiring major database changes.

---

# Reviews

Reviews are optional.

Customers may:

* Give a rating.
* Write a review.
* Skip the review process.

Only customers who have completed a booking are allowed to submit reviews.

---

# Gallery

Galleries belong to Venues.

Owners may upload photos such as:

* Courts
* Playing area
* Prayer room
* Cafeteria
* Parking area
* Events
* Tournaments

Galleries are not attached to individual Courts.

---

# Facilities

Facilities use master data.

Examples:

* Parking Area
* Toilet
* Prayer Room
* Wi-Fi
* Cafeteria
* Changing Room

Owners simply select the available facilities for their venue.

This approach keeps the data consistent.

---

# Operating Hours

Operating hours may vary by day.

Each day stores its own opening and closing time.

---

# Status Definitions

## Booking Status

* PENDING
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

## Venue Status

* ACTIVE
* INACTIVE

---

# Notifications

The database stores notifications for all system users.

Examples:

* Booking confirmed.
* Booking cancelled.
* Payment received.
* New promotion available.
* System announcement.

---

# Soft Delete Strategy

Soft Deletes are applied to:

* Users
* Venues
* Courts
* Galleries
* Reviews

Soft Deletes are **not** applied to:

* Bookings
* Payments

Transactional records are permanently preserved as part of the system history.

---

# Index Strategy

Several columns will be indexed to improve query performance.

Examples:

* email
* phone
* venue_id
* court_id
* booking_date
* status
* payment_status

Detailed index implementation will be documented in the database schema documentation.

---

# Future Scalability

The database architecture is designed to support future enhancements, including:

* Payment Gateway integration
* Voucher system
* Membership program
* Tournament management
* Dynamic pricing
* Additional sports
* Loyalty program
* Push notifications
* Advanced financial reporting

These features can be introduced without requiring major changes to the core database architecture.