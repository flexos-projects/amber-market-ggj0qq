---
id: database
title: 'Database Schema & Security'
description: 'Detailed specification of the Firestore collections, data schemas, relationships, indexing strategy, and security rules for CraftMart.'
type: doc
subtype: core
status: draft
sequence: 4
tags:
  - data
  - firestore
  - architecture
createdAt: '2026-02-07T18:47:39.761Z'
updatedAt: '2026-02-07T18:47:39.761Z'
---

## 1. Database Overview

We will use Google Cloud Firestore as our primary database. Its NoSQL, document-oriented structure is well-suited for our application's data model, and its real-time capabilities and serverless nature align with our technical strategy. All timestamps will be stored as Firestore Timestamps.

## 2. Collection Schemas

### `users`
Stores public user profile information. The document ID will be the Firebase Auth UID.

| Field           | Type          | Required | Description                                                    |
| --------------- | ------------- | -------- | -------------------------------------------------------------- |
| `email`         | String        | Yes      | User's email address (for contact).                            |
| `displayName`   | String        | Yes      | User's public display name.                                    |
| `photoURL`      | String        | No       | URL to user's profile picture.                                 |
| `role`          | String (Enum) | Yes      | `'buyer'` or `'artisan'`. Determines dashboard access.         |
| `shopName`      | String        | No       | Name of the shop (for `artisan` role only).                    |
| `shopDescription` | String      | No       | Public description of the shop (for `artisan` role only).      |
| `payoutInfo`    | Object        | No       | Contains Stripe Connect ID for payouts (for `artisan` role).   |
| `createdAt`     | Timestamp     | Yes      | Timestamp of account creation.                                 |

### `products`
Contains all product listings.

| Field           | Type          | Required | Description                                                    |
| --------------- | ------------- | -------- | -------------------------------------------------------------- |
| `artisanId`     | String (Ref)  | Yes      | UID of the artisan who owns this product (`users` collection). |
| `name`          | String        | Yes      | Product name.                                                  |
| `slug`          | String        | Yes      | URL-friendly slug.                                             |
| `description`   | String        | Yes      | Detailed product description.                                  |
| `images`        | Array<String> | Yes      | Array of URLs to product images on Cloudinary.                 |
| `price`         | Number        | Yes      | Price in cents (e.g., 3500 for $35.00) to avoid floating point issues. |
| `stock`         | Number        | Yes      | Available inventory. Must be >= 0.                             |
| `categoryId`    | String (Ref)  | Yes      | ID of the category this product belongs to (`categories` collection). |
| `averageRating` | Number        | No       | Calculated average rating (1-5). Updated by a Cloud Function.  |
| `reviewCount`   | Number        | No       | Total number of reviews. Updated by a Cloud Function.          |
| `createdAt`     | Timestamp     | Yes      | Timestamp of product creation.                                 |

### `orders`
Records all completed transactions.

| Field             | Type          | Required | Description                                                        |
| ----------------- | ------------- | -------- | ------------------------------------------------------------------ |
| `buyerId`         | String (Ref)  | Yes      | UID of the purchasing user (`users` collection).                   |
| `items`           | Array<Object> | Yes      | Array of `{productId, artisanId, name, quantity, priceAtPurchase}`. |
| `totalAmount`     | Number        | Yes      | Total order cost in cents.                                         |
| `shippingAddress` | Object        | Yes      | Shipping address object.                                           |
| `status`          | String (Enum) | Yes      | `'pending'`, `'processing'`, `'shipped'`, `'delivered'`, `'cancelled'`. |
| `trackingNumber`  | String        | No       | Shipping tracking number.                                          |
| `createdAt`       | Timestamp     | Yes      | Timestamp of order placement.                                      |

### `reviews`
Stores buyer reviews for products.

| Field       | Type         | Required | Description                                                              |
| ----------- | ------------ | -------- | ------------------------------------------------------------------------ |
| `reviewerId`| String (Ref) | Yes      | UID of the user leaving the review (`users` collection).                 |
| `productId` | String (Ref) | Yes      | ID of the product being reviewed (`products` collection).                |
| `orderId`   | String (Ref) | Yes      | ID of the associated order, to verify purchase (`orders` collection).    |
| `rating`    | Number       | Yes      | Star rating from 1 to 5.                                                 |
| `comment`   | String       | No       | The written text of the review.                                          |
| `createdAt` | Timestamp    | Yes      | Timestamp of review submission.                                          |

### Other Collections
*   **`categories`**: Simple collection with `name` and `slug` fields for product categorization.
*   **`favorites`**: A mapping collection with `userId` and `productId` to track user wishlists.

## 3. Indexing Strategy

To ensure efficient querying, the following composite indexes will be created in Firestore:

1.  **`products` Collection:**
    *   `categoryId` (asc), `price` (asc): For filtering by category and sorting by price.
    *   `categoryId` (asc), `averageRating` (desc): For filtering by category and sorting by rating.
    *   `artisanId` (asc), `createdAt` (desc): For fetching all products for a specific artisan on their dashboard.
2.  **`orders` Collection:**
    *   `buyerId` (asc), `createdAt` (desc): For fetching a user's order history.
    *   `items.artisanId` (array-contains), `createdAt` (desc): For fetching all orders for a specific artisan.
3.  **`reviews` Collection:**
    *   `productId` (asc), `createdAt` (desc): For fetching all reviews for a specific product page.

## 4. Firestore Security Rules

Security rules are critical for protecting user data. The following provides a high-level overview of the planned ruleset.

```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // Users can only read/write their own user document.
    match /users/{userId} {
      allow read, update: if request.auth.uid == userId;
      allow create: if request.auth.uid != null;
    }

    // Anyone can read products, but only the owner artisan can write.
    match /products/{productId} {
      allow get, list: if true;
      allow create: if request.auth.uid == request.resource.data.artisanId;
      allow update, delete: if request.auth.uid == resource.data.artisanId;
    }

    // Categories are read-only for clients.
    match /categories/{categoryId} {
        allow get, list: if true;
    }

    // A user can create an order for themselves. They can read their own orders.
    // Artisans can read orders containing their products.
    match /orders/{orderId} {
        allow create: if request.auth.uid == request.resource.data.buyerId;
        allow read: if request.auth.uid == resource.data.buyerId ||
                    request.auth.uid in resource.data.items.map(item => item.artisanId);
        allow update: if request.auth.uid in resource.data.items.map(item => item.artisanId); // For status updates
    }

    // Users can create reviews only if they purchased the item.
    match /reviews/{reviewId} {
        allow get, list: if true;
        allow create: if request.auth.uid == request.resource.data.reviewerId &&
                       exists(/databases/$(database)/documents/orders/$(request.resource.data.orderId)) &&
                       get(/databases/$(database)/documents/orders/$(request.resource.data.orderId)).data.buyerId == request.auth.uid;
    }
  }
}
```