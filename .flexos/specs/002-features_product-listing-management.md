---
id: product-listing-management
title: Artisan Product Listing & Management
description: >-
  Empowers artisans to effortlessly create detailed product listings with
  multiple images, comprehensive descriptions, pricing, inventory tracking, and
  shipping options. They can also edit or remove listings easily through their
  dashboard.
type: spec
subtype: features
status: draft
sequence: 2
tags:
  - artisan
  - product
  - listing
  - management
  - inventory
relatesTo:
  - pages_add-edit-product-page
  - pages_seller-dashboard-page
  - collections_products
createdAt: 2026-02-07T18:47:39.761Z
updatedAt: '2026-02-07T21:52:18.008Z'
---

This feature is crucial for artisans to populate CraftMart with their unique creations. It provides a comprehensive interface for adding, editing, and managing product details. Artisans will access this functionality primarily through the `Add/Edit Product Page` (`pages_add-edit-product-page`), linked from their `Seller Dashboard` (`pages_seller-dashboard-page`). The system will store all product information in the `products` collection (`collections_products`).

**Key Functionality:**
*   **Product Creation:** Artisans can add new products with a unique name, detailed description, multiple high-resolution images, price, and initial stock quantity.
*   **Inventory Management:** Real-time updates to stock levels are supported, allowing artisans to easily adjust quantities.
*   **Shipping Options:** Artisans define shipping costs and estimated delivery times per product.
*   **Categorization:** Products must be assigned to predefined categories to enhance discoverability.
*   **Editing & Deletion:** Artisans can modify any aspect of an existing listing or remove it entirely from the marketplace.

**Acceptance Criteria:**
*   An artisan can successfully create a new product listing with all required fields.
*   Multiple images (up to 10) can be uploaded and reordered for a single product.
*   Price and stock quantity can be updated, and changes are reflected immediately.
*   Products are correctly categorized and searchable by those categories.
*   An artisan can edit an existing product and save changes.
*   An artisan can delete a product, and it is removed from public view.

**Edge Cases:**
*   Attempting to save a product with missing required fields (e.g., name, price, images).
*   Uploading unsupported image file types or excessively large images.
*   Setting stock to zero, which should mark the product as 'out of stock'.
*   Deleting a product that is currently in a buyer's active shopping cart (should gracefully handle or prevent deletion).
*   Invalid price or stock input (e.g., negative numbers, non-numeric values).