# FOLDER_STRUCTURE.md

# Folder Structure & Architecture Guidelines

This document defines the project structure and architectural rules for the entire application.

Every generated file must follow this structure unless explicitly instructed otherwise.

The main goals are:

* Scalability
* Maintainability
* Readability
* Separation of Concerns
* Clean Architecture

---

# Root Structure

```
courta/

├── backend/
├── frontend/
├── docs/
├── screenshots/
├── README.md
└── .gitignore
```

Each folder has a single responsibility.

Do not place frontend files inside backend or vice versa.

---

# Documentation

```
docs/

├── PROJECT_OVERVIEW.md
├── BUSINESS_RULES.md
├── DESIGN.md
├── DATABASE.md
├── API_SPEC.md
├── FOLDER_STRUCTURE.md
```

Documentation is the source of truth.

Before generating features, always follow the existing documentation.

Never make assumptions if documentation already defines the behavior.

---

# Frontend Structure

```
frontend/

src/

assets/

components/

features/

hooks/

layouts/

pages/

routes/

services/

types/

utils/

constants/

config/
```

---

# Components

Reusable UI components only.

Examples:

```
components/

Button

Input

Card

Modal

Navbar

Sidebar

Table

Badge
```

Business logic should never exist here.

Components should be reusable.

---

# Features

Every business feature lives inside its own folder.

Example:

```
features/

auth/

booking/

court/

customer/

payment/

dashboard/
```

Each feature can contain:

```
booking/

components/

hooks/

services/

types/

schemas/

utils/
```

Feature code should remain inside its own module whenever possible.

---

# Pages

Pages represent routes.

Examples:

```
pages/

Landing

Login

Register

Booking

Dashboard

Profile
```

Pages should orchestrate components.

Avoid putting business logic directly inside pages.

---

# Services

Contains API communication only.

Examples:

```
BookingService

AuthService

CourtService
```

Services should never contain UI logic.

---

# Hooks

Contains reusable React hooks.

Examples:

```
useAuth

useBooking

usePagination

useDebounce
```

---

# Types

Shared TypeScript types.

Never duplicate interfaces.

---

# Utils

Contains helper functions.

Examples:

* formatter
* validator
* date helper

Never place business logic here.

---

# Backend Structure

```
backend/

app/

Http/

Models/

Services/

Repositories/

Policies/

Enums/

Exceptions/

Traits/
```

---

# Controllers

Controllers should remain thin.

Responsibilities:

* Receive Request
* Validate Request
* Call Service
* Return Response

Controllers should NOT contain business logic.

---

# Services

Business logic belongs here.

Examples:

```
BookingService

PaymentService

CourtService
```

Services may communicate with multiple repositories.

---

# Repositories

Responsible for database interaction.

Repositories should never contain business rules.

---

# Models

Models should represent database entities.

Avoid placing business logic inside models.

---

# Requests

All validation belongs inside Form Requests.

Never validate directly inside controllers.

---

# Resources

Every API response should use Laravel API Resources.

Avoid returning raw models.

---

# Database

```
database/

migrations/

factories/

seeders/
```

Seeders should generate realistic demo data.

---

# API Rules

Use REST API.

Example:

```
GET    /courts

GET    /bookings

POST   /bookings

PATCH  /bookings/{id}

DELETE /bookings/{id}
```

Avoid inconsistent endpoint naming.

---

# Naming Convention

Components

```
BookingCard.tsx

CourtCard.tsx

DashboardStats.tsx
```

Hooks

```
useBooking.ts

useAuth.ts
```

Services

```
BookingService.ts

CourtService.ts
```

Backend

```
BookingController

BookingService

BookingRepository

BookingRequest
```

Always use PascalCase for components and classes.

Use camelCase for variables and functions.

---

# General Principles

* Keep files small.
* Keep functions focused.
* Prefer composition over duplication.
* Reuse components whenever possible.
* Avoid deeply nested folders.
* Avoid unnecessary abstractions.
* Do not create folders without purpose.

---

# AI Development Rules

When generating code:

* Respect the existing folder structure.
* Do not introduce new architecture patterns without request.
* Do not move existing files.
* Do not duplicate components.
* Prefer extending existing modules.
* Create reusable components whenever possible.
* Keep responsibilities separated.
* Ask for clarification instead of making assumptions.

---

# Maintainability Principles

Every file should have one primary responsibility.

Every folder should represent one concern.

Business logic must never leak into UI.

Database logic must never leak into controllers.

Presentation and business logic must remain separated.

---

# Final Principle

Whenever adding a new feature:

1. Check existing documentation.
2. Reuse existing components.
3. Follow the established architecture.
4. Keep the project consistent.
5. Prioritize maintainability over shortcuts.

A consistent architecture is more important than writing code quickly.