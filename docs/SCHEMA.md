# SCHEMA.md

# Database Schema

## Overview

This document defines the physical database schema of **Courta**, a Sports Center Management System.

It serves as the implementation reference for the database and will later be translated into Laravel migrations.

The schema specification includes:

* Table structures
* Columns
* Data types
* Constraints
* Foreign keys
* Indexes

Business logic is documented separately in:

* `BUSINESS_RULES.md`
* `DOMAIN_MODEL.md`
* `DATABASE.md`

---

# Database Standards

The following standards apply to every table in the database.

## Database Engine

* InnoDB

---

## Character Set

* utf8mb4

Collation:

* utf8mb4_unicode_ci

---

## Primary Key

Every table uses:

* `id` (BIGINT UNSIGNED AUTO_INCREMENT)

---

## Foreign Keys

All foreign keys use:

* BIGINT UNSIGNED

Naming convention:

* user_id
* sport_id
* court_id
* booking_id
* payment_id
* promotion_id

---

## Timestamps

Every table includes:

* created_at
* updated_at

---

## Soft Deletes

Soft Deletes are applied only where appropriate.

Transactional tables should preserve historical records.

---

## Monetary Values

All monetary values use:

* DECIMAL(12,2)

---

## Uploaded Files

The database stores only file paths.

Binary files should never be stored directly in the database.

---

## Authentication Standards

Customer:

* Login using Email or Phone Number

Admin:

* Login using Email

Owner:

* Login using Email

---

# Table Overview

## Master Tables

* users
* sports
* courts
* court_prices
* promotions
* promotion_courts
* business_settings
* operating_hours
* business_holidays
* facilities
* gallery

---

## Transaction Tables

* bookings
* payments
* reviews
* notifications

---

# Table Specifications

Table specifications will be documented in the following sections.

Each table includes:

* Columns
* Constraints
* Foreign Keys
* Indexes
