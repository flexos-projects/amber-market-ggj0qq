---
id: features
title: 'Feature Inventory & MVP Scope'
description: 'A comprehensive list of all planned features for CraftMart, prioritized into P0 (MVP), P1 (Fast Follow), and P2 (Future), defining the initial launch scope.'
type: doc
subtype: core
status: draft
sequence: 2
tags:
  - product
  - features
  - mvp
createdAt: '2026-02-07T18:47:39.761Z'
updatedAt: '2026-02-07T18:47:39.761Z'
---

## 1. Feature Prioritization Overview

This document outlines the features for CraftMart, categorized by priority to define a clear roadmap. The Minimum Viable Product (MVP) will consist solely of P0 features, which represent the absolute essential functionality required to launch a usable marketplace. P1 features are critical for post-launch growth and user retention, while P2 features are ideas for future expansion.

## 2. P0: MVP Scope

These features are non-negotiable for the initial launch. They constitute the core loop of listing a product, discovering it, and purchasing it.

*   **`user-authentication-roles` (User Authentication & Roles)**
    *   **Description:** Secure sign-up and login for both buyers and artisans. The system must differentiate roles to provide access to the correct dashboard (`/dashboard/buyer` vs. `/dashboard/seller`).
    *   **Justification:** The fundamental requirement for any transactional, multi-sided platform.

*   **`product-listing-management` (Artisan Product Listing & Management)**
    *   **Description:** Enables authenticated artisans to create, edit, and manage their product listings through the `add-edit-product-page`. This includes images, descriptions, price, and stock levels.
    *   **Justification:** Artisans must be able to add products for the marketplace to have any inventory.

*   **`browse-search-filter` (Curated Browse, Search & Filter)**
    *   **Description:** Allows buyers to explore products on the `/shop` page. This includes navigation by pre-defined categories (from the `categories` collection) and a basic text search.
    *   **Justification:** Buyers must be able to find products. Without discovery, there are no sales.

*   **`secure-cart-checkout` (Secure Shopping Cart & Checkout)**
    *   **Description:** The complete transactional flow, from adding an item to the `/cart` page to completing payment on the `/checkout` page using Stripe.
    *   **Justification:** The core mechanism for generating revenue and fulfilling the platform's purpose.

## 3. P1: Fast Follow (Post-MVP)

These features will be prioritized immediately after a successful MVP launch to enhance community trust, user engagement, and the post-purchase experience.

*   **`product-review-system` (Product & Artisan Review System)**
    *   **Description:** Allows verified buyers to leave star ratings and written reviews on `product-detail-page`s. Data will be stored in the `reviews` collection.
    *   **Justification:** Builds social proof and trust, which is critical for increasing conversion rates for new buyers.
    *   **Dependency:** Depends on `secure-cart-checkout`, as reviews can only be left for purchased items (verified by the `orders` collection).

*   **`favorites-wishlist` (Favorites & Wishlist)**
    *   **Description:** Allows logged-in buyers to save products to a personal list, accessible from their dashboard.
    *   **Justification:** Increases user engagement and provides a low-friction way for users to return, leading to future sales.

*   **`order-tracking-notifications` (Order Tracking & Notifications)**
    *   **Description:** Provides buyers with order status updates (e.g., 'Shipped') and allows artisans to add tracking numbers. This involves both UI updates in the dashboards and transactional emails via SendGrid.
    *   **Justification:** Reduces buyer anxiety and support requests, creating a more professional and trustworthy post-purchase experience.

## 4. P2: Future Enhancements

These are features to be considered for the longer-term roadmap, based on user feedback and business growth.

*   **Dedicated Artisan Storefronts:** Unique, customizable pages for each artisan (e.g., `/shop/chloes-pottery`) to showcase their brand and all their products in one place.
*   **Direct Messaging:** A secure, in-app messaging system for buyers to ask artisans questions about products before purchasing.
*   **Advanced Search & Filtering:** Add more complex filter options like color, shipping location, and custom product attributes.
*   **Gift Cards & Registry:** Allow users to purchase and redeem gift cards or create wishlists for special occasions.
*   **Featured Artisan Program:** An editorial program to highlight top-performing or unique artisans on the `landing-page`, driving discovery and rewarding quality creators.