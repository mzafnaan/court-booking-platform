# DOMAIN_MODEL.md

# Domain Model

This document defines the business domains, responsibilities, entities, and relationships of **Courta**, a Sports Center Management System designed for a **single sports center**.

Its purpose is to establish a clear business architecture before designing the database and implementing the application.

This document serves as the bridge between `BUSINESS_RULES.md` and `DATABASE.md`.

---

# Domain Overview

The application consists of six primary business domains.

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
├── Sport
└── Holiday
│
Business Management
│
├── Business Profile
├── Operating Hours
├── Facilities
├── Gallery
└── Promotion
│
User Management
│
├── User
└── Profile
│
Reporting
```

Each domain has a single responsibility.

Domains should communicate through clearly defined relationships.

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
* Role-based Authorization

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

Responsible for managing the complete booking lifecycle.

This is the core business domain of the application.

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
* Preventing overlapping bookings
* Calculating booking prices
* Reserving court schedules
* Tracking booking status
* Handling booking expiration
* Applying promotional pricing
* Storing transaction snapshots

Should NOT:

* Authenticate users
* Manage court information
* Manage business information

---

### Payment

Responsible for:

* Uploading payment proof
* Storing payment information
* Verifying payments
* Tracking payment status
* Recording payment amount

Should NOT:

* Store booking schedules
* Manage customer profiles
* Calculate court availability

---

### Court Pricing

Responsible for:

* Managing pricing rules
* Supporting different prices based on day and operating hours
* Providing pricing calculations
* Serving as the base price for promotions

Should NOT:

* Store booking history
* Store payment information

---

## Dependencies

Requires:

* Authentication Domain
* Court Management Domain
* Business Management Domain

---

## Implementation Priority

Highest

---

# Court Management

## Purpose

Responsible for managing sports courts and their operational availability.

---

## Entities

* Court
* Sport
* Holiday

---

## Responsibilities

### Court

Responsible for:

* Storing court information
* Storing court descriptions
* Managing court status
* Linking courts to supported sports

---

### Sport

Responsible for:

* Managing supported sports
* Categorizing courts

Current supported sports:

* Futsal
* Volleyball

---

### Holiday

Responsible for:

* Defining unavailable dates
* Preventing bookings during holidays
* Supporting public holidays and special closures

---

## Should NOT Handle

* Booking
* Payment
* Authentication
* Landing Page Content

---

## Dependencies

Business Management Domain

---

## Implementation Priority

High

---

# Business Management

## Purpose

Responsible for managing all business-related information of the sports center.

This domain represents the operational profile of a single sports center.

---

## Entities

* Business Profile
* Operating Hours
* Facilities
* Gallery
* Promotion

---

## Responsibilities

### Business Profile

Stores information such as:

* Sports Center Name
* Description
* Address
* Phone Number
* WhatsApp
* Email
* Google Maps
* Logo
* Social Media
* Hero Banner

---

### Operating Hours

Responsible for:

* Opening hours
* Closing hours
* Daily operating schedule

---

### Facilities

Responsible for:

* Managing available facilities
* Displaying facilities on the website

Examples:

* Parking Area
* Toilet
* Prayer Room
* Wi-Fi
* Cafeteria
* Changing Room

---

### Gallery

Responsible for:

* Managing sports center photos
* Displaying gallery images
* Managing event documentation

---

### Promotion

Responsible for:

* Managing promotional campaigns
* Defining promotional periods
* Managing discount rules
* Displaying promotions on the landing page

---

## Should NOT Handle

* Booking
* Authentication
* User Profiles

---

## Dependencies

None.

---

## Implementation Priority

High

---

# User Management

## Purpose

Responsible for managing user information beyond authentication.

---

## Entities

* User
* Profile

---

## Responsibilities

* Managing customer profiles
* Updating personal information
* Viewing booking history

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

Responsible for generating business reports and analytics.

Reporting never owns data.

It aggregates information from other domains.

---

## Entities

No dedicated entities.

Uses data from:

* Booking
* Payment
* Court
* User
* Promotion

---

## Responsibilities

Generate reports such as:

* Total Bookings
* Booking Trends
* Revenue
* Court Utilization
* Peak Booking Hours
* Most Booked Court
* Financial Summary
* Promotion Performance (Future)

---

## Dependencies

* Booking Management
* Court Management
* Business Management
* User Management

---

## Implementation Priority

Medium

---

# Domain Relationships

```text
Authentication
        │
        ▼
User Management

Business Management
        │
        ▼
Court Management
        │
        ▼
Booking Management
        │
        ▼
Reporting

Business Management
        │
        ▼
Landing Page
```

Every domain has a clear responsibility.

Business logic should never be duplicated across domains.

---

# Business Ownership

| Domain              | Owns                            |
| ------------------- | ------------------------------- |
| Authentication      | User Authentication             |
| Booking Management  | Booking, Payment, Court Pricing |
| Court Management    | Court Operations                |
| Business Management | Business Information            |
| User Management     | Customer Profile                |
| Reporting           | Business Analytics              |

---

# Version 1 Scope

The first version of Courta includes:

* Authentication
* Business Management
* Court Management
* Booking Management
* User Management
* Reporting

Only these domains should be implemented.

New domains should not be introduced without updating this document.

---

# AI Development Rules

When generating code:

* Respect domain boundaries.
* Never mix responsibilities across domains.
* Reuse existing entities whenever possible.
* Do not duplicate business logic.
* Database design must originate from this document.
* Follow the defined domain architecture before implementing new features.
* Courta is designed for a **single sports center**, not a marketplace.
* Do not introduce multi-business, multi-venue, or marketplace concepts unless explicitly requested.
* Keep the architecture modular to support future expansion without affecting the current business scope.

This document serves as the primary business architecture reference for the entire project.