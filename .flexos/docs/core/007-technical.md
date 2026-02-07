---
id: technical
title: 'Technical Architecture & Stack'
description: 'An overview of the technical architecture, technology stack, key integrations, API design, and deployment strategy for CraftMart.'
type: doc
subtype: core
status: draft
sequence: 7
tags:
  - architecture
  - firebase
  - nuxt
  - devops
createdAt: '2026-02-07T18:47:39.761Z'
updatedAt: '2026-02-07T18:47:39.761Z'
---

## 1. High-Level Architecture

CraftMart will be a modern web application built on a **serverless architecture**, leveraging the Google Firebase ecosystem. This approach minimizes infrastructure management, provides excellent scalability, and allows the development team to focus on building features.

*   **Frontend:** A **Nuxt 4 (Vue 3)** single-page application (SPA) with Server-Side Rendering (SSR) for improved SEO and initial page load performance. It will be hosted on **Firebase Hosting**.
*   **Backend:** A suite of **Firebase Functions** (written in TypeScript) will handle all business logic, from payment processing to user notifications.
*   **Database:** **Cloud Firestore** will serve as our primary NoSQL database for its real-time capabilities and flexible data model. See the [Database Schema](./004-database.md) for details.
*   **Authentication:** **Firebase Authentication** will manage user identity, supporting both email/password and social logins (Google).
*   **File Storage:** **Cloudinary** will be used for all user-uploaded images. Its API provides powerful on-the-fly image transformation, optimization, and a robust CDN, which is superior to Firebase Storage for this use case.

## 2. Technology Stack

### Frontend

*   **Framework:** Nuxt 4
*   **Language:** TypeScript
*   **State Management:** Pinia
*   **Styling:** TailwindCSS
*   **Key Libraries:** `@stripe/stripe-js` for payment forms, `vue-sonner` for toast notifications.

### Backend

*   **Runtime:** Node.js (via Firebase Functions)
*   **Language:** TypeScript
*   **Database:** Firebase Firestore
*   **Authentication:** Firebase Authentication

### Key Third-Party Integrations

*   **Payments:** **Stripe Connect** is critical. It allows us to process payments from buyers and handle complex payouts to multiple artisans, managing compliance and 1099 form generation.
*   **Transactional Email:** **SendGrid** will be used to send all automated emails, such as order confirmations, password resets, and shipping notifications. Its reliability and analytics are essential.
*   **Image Management:** **Cloudinary** for storing, optimizing, and delivering all product and profile images.

## 3. API & Serverless Functions

Our backend logic will be exposed through HTTPS-callable Firebase Functions. This creates a secure and scalable API for our Nuxt frontend.

**Key Endpoints:**

*   `POST /api/webhooks/stripe`
    *   **Description:** A critical webhook endpoint that listens for events from Stripe (e.g., `checkout.session.completed`). This is the primary mechanism for confirming a successful payment and triggering the order creation process. This function is responsible for creating the `order` document.
    *   **Security:** Secured using Stripe's webhook signing secrets.

*   `POST /api/checkout/create-session`
    *   **Description:** Called by the client when the user clicks 'Place Order'. It takes the cart contents, validates them against the database (checking price and stock), and creates a Stripe Checkout Session, returning the session ID to the client to redirect to Stripe.
    *   **Security:** Authenticated users only.

*   `POST /api/products`
    *   **Description:** Creates a new product listing. Handles data validation before writing to the `products` collection.
    *   **Security:** Authenticated users with the 'artisan' role only.

*   `PUT /api/orders/:orderId/status`
    *   **Description:** Allows an artisan to update the status of an order they are fulfilling (e.g., from 'processing' to 'shipped') and add a tracking number.
    *   **Security:** Authenticated artisan who is part of the specified order.

## 4. Deployment & DevOps

We will adopt a modern CI/CD workflow to ensure rapid and reliable deployments.

*   **Source Control:** A monorepo on **GitHub** containing both the Nuxt frontend app and the Firebase Functions code.
*   **CI/CD Pipeline:** **GitHub Actions** will be used to automate our workflow.
    *   **On Pull Request:** Automatically run linting (ESLint) and any unit/integration tests.
    *   **On Merge to `main`:**
        1.  The GitHub Action checks out the code.
        2.  It builds the Nuxt application for production.
        3.  It deploys the static assets and SSR function to **Firebase Hosting** using the Firebase CLI.
        4.  It deploys the backend functions to **Firebase Functions**.

*   **Environments:**
    *   **Development:** Local development using the Firebase Emulator Suite.
    *   **Staging:** A separate Firebase project for testing features before production release.
    *   **Production:** The live CraftMart application project.
*   **Environment Variables:** All secrets (API keys for Stripe, SendGrid, Cloudinary) will be managed using Firebase Functions environment configuration and will not be stored in the repository.