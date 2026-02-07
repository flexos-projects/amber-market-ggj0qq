---
id: pages
title: 'Pages, Sitemap & User Journeys'
description: 'Defines the sitemap, page inventory, primary user journeys, and navigation structure for the CraftMart application.'
type: doc
subtype: core
status: draft
sequence: 3
tags:
  - ux
  - architecture
  - navigation
createdAt: '2026-02-07T18:47:39.761Z'
updatedAt: '2026-02-07T18:47:39.761Z'
---

## 1. Sitemap

This sitemap outlines the hierarchical structure of the CraftMart application.

```
/
├── /shop
│   └── /shop/:slug (Product Detail)
├── /auth (Login / Signup)
├── /cart
├── /checkout
├── /dashboard
│   ├── /dashboard/buyer
│   └── /dashboard/seller
│       ├── /dashboard/seller/product/new
│       └── /dashboard/seller/product/edit/:id
└── /settings
```

## 2. Page Inventory

This inventory details each page, its purpose, and the primary features it supports. All pages will use the core navigation and footer components.

*   **`/` (Landing Page)**
    *   **Description:** Introduces CraftMart's value proposition. Features curated categories and products to draw users in. The primary goal is to convert visitors into either shoppers or registered artisans.
    *   **Related Features:** `browse-search-filter` (for featured products).

*   **`/auth` (Login / Signup)**
    *   **Description:** A single page with toggleable forms for user registration and login. Includes a clear choice between 'Buyer' and 'Artisan' roles during signup.
    *   **Related Features:** `user-authentication-roles`.

*   **`/shop` (Shop / Browse Products)**
    *   **Description:** The main marketplace view. A grid of products with sidebar/top navigation for categories and filters. Infinite scroll or pagination will be used for performance.
    *   **Related Features:** `browse-search-filter`, `favorites-wishlist`.

*   **`/shop/:slug` (Product Detail Page)**
    *   **Description:** A detailed view of a single product. Showcases images, description, price, artisan info, and reviews. The main call to action is 'Add to Cart'.
    *   **Related Features:** `product-listing-management`, `secure-cart-checkout`, `favorites-wishlist`, `product-review-system`.

*   **`/cart` (Shopping Cart)**
    *   **Description:** A summary page for items the user has added to their cart. Users can adjust quantities or remove items before proceeding to checkout.
    *   **Related Features:** `secure-cart-checkout`.

*   **`/checkout` (Checkout)**
    *   **Description:** A secure, multi-step form to collect shipping and payment information to complete the purchase.
    *   **Related Features:** `secure-cart-checkout`.

*   **`/dashboard/buyer` (Buyer Dashboard)**
    *   **Description:** A personalized hub for buyers to view their order history, track shipments, and manage their saved items.
    *   **Related Features:** `order-tracking-notifications`, `favorites-wishlist`, `product-review-system`.

*   **`/dashboard/seller` (Seller Dashboard)**
    *   **Description:** The control center for artisans. They can view sales metrics, manage their product listings, and handle incoming orders.
    *   **Related Features:** `product-listing-management`, `order-tracking-notifications`.

*   **`/dashboard/seller/product/new` & `/dashboard/seller/product/edit/:id` (Add/Edit Product)**
    *   **Description:** A form-based interface for artisans to create or update their product listings.
    *   **Related Features:** `product-listing-management`.

## 3. Core User Journeys

### Journey 1: New Buyer's First Purchase

1.  **Discovery:** Mark (Buyer Persona) arrives on the `Landing Page` via a social media link.
2.  **Exploration:** He clicks on the 'Home Decor' category, taking him to the `/shop` page, pre-filtered for that category.
3.  **Refinement:** He uses the search bar to look for "ceramic mug" and further filters by a price range.
4.  **Consideration:** He clicks on a promising product, navigating to its `Product Detail Page` (`/shop/handmade-blue-mug`). He scrolls through the images and reads positive reviews from other buyers.
5.  **Action:** He clicks 'Add to Cart', and a mini-cart overlay confirms the addition. He then navigates to the `/cart` page.
6.  **Checkout:** He proceeds to `/checkout`, signs up for a new buyer account, enters his shipping and payment details, and places the order.
7.  **Confirmation:** He receives an order confirmation email and is redirected to a thank you page.

### Journey 2: New Artisan's First Listing

1.  **Onboarding:** Chloe (Artisan Persona) clicks 'For Artisans' on the `Landing Page` and then signs up on the `/auth` page, selecting the 'Artisan' role.
2.  **Setup:** After verifying her email, she is guided to her `Seller Dashboard` (`/dashboard/seller`). A welcome modal prompts her to set up her shop name.
3.  **Creation:** She clicks 'Add New Product', which takes her to `/dashboard/seller/product/new`.
4.  **Data Entry:** She meticulously fills out the form: product name, a detailed description, uploads 5 high-quality images, sets the price to $35, and stock to 10.
5.  **Publishing:** She clicks 'Publish'. The system validates the data, creates a new document in the `products` collection, and redirects her back to her `Seller Dashboard`, where she sees her new item in the 'Current Listings' table.

## 4. Navigation

*   **Global Header (Unauthenticated):**
    *   Logo (links to `/`)
    *   Shop
    *   Categories (Dropdown)
    *   Sign In (links to `/auth`)
    *   Sign Up (Primary Button, links to `/auth`)

*   **Global Header (Authenticated Buyer):**
    *   Logo
    *   Shop
    *   Categories (Dropdown)
    *   Cart Icon (with item count badge, links to `/cart`)
    *   Profile Avatar (Dropdown: My Dashboard, My Favorites, Settings, Logout)

*   **Global Header (Authenticated Artisan):**
    *   Logo
    *   My Shop (links to `/dashboard/seller`)
    *   Shop (to browse the marketplace)
    *   Profile Avatar (Dropdown: Public Profile, Settings, Logout)