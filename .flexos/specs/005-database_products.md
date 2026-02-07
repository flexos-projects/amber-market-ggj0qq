---
id: products
title: Products Data Collection
description: Contains details for all handmade products listed by artisans.
type: spec
subtype: database
status: draft
sequence: 5
tags:
  - data
  - collection
  - products
  - artisan
  - inventory
relatesTo:
  - features_product-listing-management
  - pages_shop-browse-page
  - pages_product-detail-page
  - collections_users
  - collections_categories
createdAt: 2026-02-07T18:47:39.761Z
updatedAt: '2026-02-07T21:52:33.940Z'
---

The `products` collection is central to CraftMart, storing all information about the handmade items available for sale. This collection is directly managed via the `Artisan Product Listing & Management` feature (`features_product-listing-management`) and is extensively queried by the `Shop / Browse Products Page` (`pages_shop-browse-page`) and `Product Detail Page` (`pages_product-detail-page`). It maintains relationships with `users` (for artisans) and `categories` collections.

**Fields:**
*   `id` (string, required): Unique identifier for the product.
*   `artisanId` (reference, required): Links to the `uid` in the `users` collection, identifying the artisan.
*   `name` (string, required): Product title.
*   `slug` (string, required): URL-friendly identifier, derived from the name.
*   `description` (richtext, required): Detailed product description, supporting rich formatting.
*   `images` (array of URLs, required): List of image URLs hosted on Cloudinary.
*   `price` (number, required): Product price in USD.
*   `stock` (number, required): Current inventory count. Must be non-negative.
*   `categoryId` (reference, required): Links to the `id` in the `categories` collection.
*   `shippingOptions` (object, optional): Contains `cost` (number) and `estimatedDelivery` (string).
*   `averageRating` (number, optional): Denormalized average rating from `reviews`.
*   `reviewCount` (number, optional): Denormalized count of reviews.
*   `createdAt` (date, required): Timestamp of creation.
*   `updatedAt` (date, required): Timestamp of last modification.

**Relationships:**
*   `artisanId` -> `users` (`uid`)
*   `categoryId` -> `categories` (`id`)
*   Implicitly referenced by `orders` (`items[].productId`) and `reviews` (`productId`).

**Acceptance Criteria:**
*   All required fields must be present and valid for a product to be saved.
*   `stock` cannot be negative.
*   `price` must be a positive number.
*   `artisanId` and `categoryId` must reference existing entries in their respective collections.
*   `slug` is automatically generated and unique.
*   `averageRating` and `reviewCount` are updated whenever a new review is submitted or an existing one is modified/deleted.
*   Data consistency is maintained across related collections (e.g., deleting an artisan may require handling their products).

**Edge Cases:**
*   Attempting to save a product with an `artisanId` or `categoryId` that does not exist.
*   Concurrent updates to `stock` (e.g., multiple purchases at once) requiring atomic operations.
*   Large number of images for a single product (performance implications).
*   Long descriptions impacting database read/write performance.