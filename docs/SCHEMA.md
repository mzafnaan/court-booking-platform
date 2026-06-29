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

## Table: users

### Columns

| Column            | Type                             | Nullable | Default           | Description                  |
| ----------------- | -------------------------------- | -------- | ----------------- | ---------------------------- |
| id                | BIGINT UNSIGNED                  | No       | AUTO_INCREMENT    | Primary Key                  |
| full_name         | VARCHAR(100)                     | No       | -                 | User's full name             |
| email             | VARCHAR(255)                     | No       | -                 | Unique email address         |
| phone             | VARCHAR(20)                      | No       | -                 | Unique phone number          |
| password          | VARCHAR(255)                     | No       | -                 | Hashed password              |
| role              | ENUM('CUSTOMER','ADMIN','OWNER') | No       | CUSTOMER          | User role                    |
| avatar            | VARCHAR(255)                     | Yes      | NULL              | Profile image path           |
| email_verified_at | TIMESTAMP                        | Yes      | NULL              | Email verification timestamp |
| remember_token    | VARCHAR(100)                     | Yes      | NULL              | Laravel remember token       |
| created_at        | TIMESTAMP                        | No       | CURRENT_TIMESTAMP | Record creation time         |
| updated_at        | TIMESTAMP                        | No       | CURRENT_TIMESTAMP | Last update time             |
| deleted_at        | TIMESTAMP                        | Yes      | NULL              | Soft delete timestamp        |

### Constraints

Primary Key

* id

Unique

* email
* phone

### Foreign Keys

None

### Indexes

* email
* phone
* role

---

## Table: sports

### Columns

| Column        | Type            | Nullable | Default           | Description          |
| ------------- | --------------- | -------- | ----------------- | -------------------- |
| id            | BIGINT UNSIGNED | No       | AUTO_INCREMENT    | Primary Key          |
| name          | VARCHAR(50)     | No       | -                 | Sport name           |
| icon          | VARCHAR(255)    | Yes      | NULL              | Sport icon path      |
| display_order | INT UNSIGNED    | No       | 0                 | Display order        |
| created_at    | TIMESTAMP       | No       | CURRENT_TIMESTAMP | Record creation time |
| updated_at    | TIMESTAMP       | No       | CURRENT_TIMESTAMP | Last update time     |

### Constraints

Primary Key

* id

Unique

* name

### Foreign Keys

None

### Indexes

* name

---

## Table: courts

### Columns

| Column        | Type                                    | Nullable | Default           | Description           |
| ------------- | --------------------------------------- | -------- | ----------------- | --------------------- |
| id            | BIGINT UNSIGNED                         | No       | AUTO_INCREMENT    | Primary Key           |
| sport_id      | BIGINT UNSIGNED                         | No       | -                 | Related sport         |
| name          | VARCHAR(100)                            | No       | -                 | Court name            |
| description   | TEXT                                    | Yes      | NULL              | Court description     |
| status        | ENUM('ACTIVE','MAINTENANCE','INACTIVE') | No       | ACTIVE            | Court status          |
| display_order | INT UNSIGNED                            | No       | 0                 | Display order         |
| created_at    | TIMESTAMP                               | No       | CURRENT_TIMESTAMP | Record creation time  |
| updated_at    | TIMESTAMP                               | No       | CURRENT_TIMESTAMP | Last update time      |
| deleted_at    | TIMESTAMP                               | Yes      | NULL              | Soft delete timestamp |

### Constraints

Primary Key

* id

### Foreign Keys

* sport_id → sports.id

### Indexes

* sport_id
* status

---

## Table: court_prices

### Columns

| Column     | Type                      | Nullable | Default           | Description          |
| ---------- | ------------------------- | -------- | ----------------- | -------------------- |
| id         | BIGINT UNSIGNED           | No       | AUTO_INCREMENT    | Primary Key          |
| court_id   | BIGINT UNSIGNED           | No       | -                 | Related court        |
| day_type   | ENUM('WEEKDAY','WEEKEND') | No       | -                 | Pricing day type     |
| start_time | TIME                      | No       | -                 | Price start time     |
| end_time   | TIME                      | No       | -                 | Price end time       |
| price      | DECIMAL(12,2)             | No       | 0.00              | Hourly price         |
| created_at | TIMESTAMP                 | No       | CURRENT_TIMESTAMP | Record creation time |
| updated_at | TIMESTAMP                 | No       | CURRENT_TIMESTAMP | Last update time     |

### Constraints

Primary Key

* id

### Foreign Keys

* court_id → courts.id

### Indexes

* court_id
* day_type
* start_time
* end_time
