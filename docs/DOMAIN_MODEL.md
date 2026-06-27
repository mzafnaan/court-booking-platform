# DOMAIN_MODEL.md

# Domain Model

This document defines the business domains, entities, responsibilities, and relationships of the Sports Center Management System.

Its purpose is to establish a clear business architecture before designing the database and implementing the application.

This document is the bridge between `BUSINESS_RULES.md` and `DATABASE.md`.

---

# Domain Overview

The application consists of six main domains.

```text
Authentication
│
├── User Authentication
│
Booking Management
│
├── Booking
├── Payment
└── Court Pricing
│
Court Management
│
├── Court
├── Court Image
├── Operating Hour
└── Holiday
│
Content Management
│
├── Gallery
├── Facility
├── FAQ
└── Settings
│
User Management
│
├── User
└── Profile
│
Reporting
```

Each domain has a single responsibility.

Domains should communicate through well-defined relationships.

Avoid mixing responsibilities across domains.

---

# Authentication Domain

## Purpose

Responsible for authenticating users and controlling access to the application.

Authentication should never contain booking or business logic.

---

## Entities

* User

---

## Responsibilities

* Register
* Login
* Logout
* Email Verification
* Password Reset
* Session Authentication

---

## Should NOT Handle

* Booking
* Payment
* Court Management
* Reports

---

## Dependencies

None.

---

## Implementation Priority

High

---

# Booking Management

## Purpose

Responsible for managing the entire booking lifecycle.

This is the core domain of the application.

---

## Entities

* Booking
* Payment
* Court Pricing

---

## Responsibilities

### Booking

Responsible for:

* Creating bookings
* Preventing double booking
* Calculating booking price
* Tracking booking status
* Reserving court schedules
* Managing booking source

Should NOT:

* Authenticate users
* Store gallery information
* Manage court information

---

### Payment

Responsible for:

* Uploading payment proof
* Storing payment information
* Payment verification
* Tracking payment status
* Recording payment amount

Should NOT:

* Store booking schedule
* Manage customer profile
* Calculate booking availability

---

### Court Pricing

Responsible for:

* Managing pricing rules
* Defining hourly price ranges
* Supporting different prices throughout the day
* Providing price calculation data

Should NOT:

* Store booking history
* Store payment data

---

## Dependencies

Requires:

* Authentication Domain
* Court Management Domain

---

## Implementation Priority

Highest

---

# Court Management

## Purpose

Responsible for managing sports courts and operational information.

---

## Entities

* Court
* Court Image
* Operating Hour
* Holiday

---

## Responsibilities

### Court

* Store court information
* Store court description
* Store court status
* Store capacity
* Store court type

---

### Court Image

* Store court gallery
* Manage multiple images for each court

---

### Operating Hour

* Store opening hours
* Store closing hours
* Define booking slot duration

---

### Holiday

* Store closed dates
* Prevent booking on holidays

---

## Should NOT Handle

* Booking
* Payment
* Authentication

---

## Dependencies

None.

---

## Implementation Priority

High

---

# Content Management

## Purpose

Responsible for managing all landing page content.

The landing page should not contain hardcoded business content.

All content should be manageable through the admin dashboard.

---

## Entities

* Gallery
* Facility
* FAQ
* Settings

---

## Responsibilities

Gallery

* Store landing page images

Facility

* Store available facilities

FAQ

* Store frequently asked questions

Settings

* Store business information

Examples:

* Sport Center Name
* Address
* Phone Number
* WhatsApp
* Email
* Social Media
* Hero Banner

---

## Dependencies

None.

---

## Implementation Priority

Medium

---

# User Management

## Purpose

Responsible for customer information beyond authentication.

---

## Entities

* User
* Profile

---

## Responsibilities

* Manage customer profile
* Update personal information
* View booking history

---

## Should NOT Handle

* Authentication
* Payment
* Court Management

---

## Dependencies

Authentication Domain

---

## Implementation Priority

Medium

---

# Reporting

## Purpose

Responsible for generating business reports.

Reporting should never own data.

It only aggregates data from other domains.

---

## Entities

No dedicated entities.

Uses data from:

* Booking
* Payment
* Court
* User

---

## Responsibilities

Generate reports such as:

* Total Bookings
* Booking Trends
* Revenue
* Court Utilization
* Booking Source Statistics
* Peak Booking Hours

---

## Dependencies

* Booking Management
* Court Management
* User Management

---

## Implementation Priority

Low

---

# Domain Relationships

```text
Authentication
        │
        ▼
User Management
        │
        ▼
Booking Management
        │
        ▼
Court Management

Booking Management
        │
        ▼
Reporting

Content Management
        │
        ▼
Landing Page
```

Every domain has a clear responsibility.

Avoid placing logic into unrelated domains.

---

# Business Ownership

| Domain             | Owns                      |
| ------------------ | ------------------------- |
| Authentication     | User Authentication       |
| Booking Management | Booking, Payment, Pricing |
| Court Management   | Court Operations          |
| Content Management | Landing Page Content      |
| User Management    | Customer Profile          |
| Reporting          | Business Analytics        |

---

# Version 1 Scope

Included:

* Authentication
* Court Management
* Booking Management
* Content Management
* User Management
* Reporting

Only these domains should be implemented.

Do not introduce additional domains without updating this document.

---

# AI Development Rules

When generating code:

* Respect domain boundaries.
* Never mix responsibilities between domains.
* Reuse existing entities whenever possible.
* Do not duplicate business logic.
* Do not introduce new domains without approval.
* Follow this document before designing the database.
* Database design must originate from the entities defined here.

This document serves as the business architecture reference for the entire project.
