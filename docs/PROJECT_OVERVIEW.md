# PROJECT_OVERVIEW.md

# WIFA Sport Center Management System

> A modern web-based sports center booking and management system built as a portfolio project with production-level architecture.

---

# Project Goal

This project aims to build a modern web application for managing a single sports center.

The application is designed to solve real operational problems such as:

* Online court booking
* Schedule management
* Payment confirmation
* Customer management
* Booking history
* Business dashboard

This project is intended as a portfolio project that demonstrates software engineering skills rather than just CRUD implementation.

---

# Project Scope

This project is built for **ONE sports center only**.

It is **NOT** a marketplace.

It is **NOT** a multi-tenant SaaS.

It is **NOT** designed for multiple sport center owners.

Future expansion is possible, but it is outside the scope of Version 1.

---

# Main Users

## Guest

Can:

* View landing page
* View available courts
* View facilities
* View gallery
* View schedule
* Register account
* Login

Cannot:

* Make booking
* Upload payment

---

## Customer

Can:

* Login
* View available schedule
* Book court
* Upload payment proof
* View booking history
* Edit profile

Cannot:

* Verify payment
* Manage courts

---

## Admin

Can:

* Manage courts
* Manage schedules
* Manage bookings
* Verify payments
* Manage customers
* Update landing page content
* View reports

---

# Business Concept

Booking is performed online.

Payment uses **manual bank transfer**.

Customers upload payment proof.

Admin verifies payment manually after checking bank transactions.

There is **NO payment gateway** in Version 1.

---

# Main Features

## Public Website

* Landing Page
* Facilities
* Court Information
* Gallery
* About
* Contact
* Available Schedule Preview

---

## Authentication

* Register
* Login
* Logout
* Email Verification
* Forgot Password
* Reset Password

---

## Customer

* Book Court
* Booking History
* Upload Payment Proof
* Profile Management

---

## Admin

* Dashboard
* Court Management
* Booking Management
* Customer Management
* Payment Verification
* Gallery Management
* Landing Page Management
* Reports

---

# Booking Flow

Guest

↓

Browse Website

↓

Login

↓

Choose Court

↓

Choose Date

↓

Choose Time

↓

Booking Created

↓

Upload Payment Proof

↓

Waiting Verification

↓

Admin Verifies Payment

↓

Booking Confirmed

↓

Customer Arrives

↓

Booking Completed

---

# Booking Status

Available

↓

Pending Payment

↓

Waiting Verification

↓

Confirmed

↓

Completed

or

Cancelled

---

# Payment Flow

Customer transfers payment manually.

Customer uploads payment proof.

Admin checks bank transaction manually.

If payment is valid:

Booking status becomes **Confirmed**.

If payment is invalid:

Booking remains **Waiting Verification** or becomes **Rejected**.

---

# Design Philosophy

The application should feel like a modern startup product.

UI Characteristics:

* Minimalist
* Premium
* Spacious layout
* Large typography
* Rounded corners
* Soft shadows
* Sports lifestyle aesthetic
* Mobile responsive

Avoid:

* Bootstrap-looking UI
* AdminLTE style
* AI-generated generic layouts
* Overly colorful interface

---

# Technology Stack

Frontend

* React
* TypeScript
* Vite
* Tailwind CSS
* TanStack Query
* React Router
* Axios
* React Hook Form
* Zod
* shadcn/ui

Backend

* Laravel
* Sanctum
* Form Request Validation
* REST API

Database

* PostgreSQL

---

# Project Principles

This project prioritizes:

1. Clean Architecture
2. Readable Code
3. Maintainability
4. Scalability
5. Consistent UI
6. User Experience
7. Business-oriented Features

---

# AI Development Rules

When generating code:

* Follow existing project architecture.
* Do not introduce unnecessary libraries.
* Do not change folder structure without request.
* Prefer reusable components.
* Prefer composition over duplication.
* Keep code clean and readable.
* Follow TypeScript strict typing.
* Follow Laravel best practices.
* Ask for clarification instead of making assumptions.

---

# Version 1 Scope

Included

* Landing Page
* Authentication
* Court Management
* Booking System
* Manual Payment Upload
* Payment Verification
* Dashboard
* Reports

Excluded

* Payment Gateway
* Multi Branch
* Multi Tenant
* Membership
* Promo System
* QR Check-in
* Push Notification
* AI Features

---

# Long-term Roadmap

Version 1.0

* Stable booking system

Version 1.1

* QR Code Check-in

Version 1.2

* Dashboard improvements

# Current Project Scope

This project only focuses on building a complete management system for a **single sports center**.

The goal is to deliver a polished, production-like application with a complete booking workflow.

Only features described in this document should be implemented.

Avoid adding new features unless explicitly requested.

---

# Development Principle

This project follows the MVP (Minimum Viable Product) approach.

Every implemented feature must be:

- Complete
- Stable
- Responsive
- Production-ready

Do not implement future ideas or speculative features.

Focus on quality over quantity.

---

# Final Goal

Build a modern, clean, and production-quality sports center management system that demonstrates real-world software engineering practices and serves as a professional portfolio project.