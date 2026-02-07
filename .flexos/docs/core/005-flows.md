---
id: flows
title: 'User & System Flows'
description: 'Documents the key user-facing and backend system flows, including success paths and critical error handling scenarios.'
type: doc
subtype: core
status: draft
sequence: 5
tags:
  - ux
  - architecture
  - flows
createdAt: '2026-02-07T18:47:39.761Z'
updatedAt: '2026-02-07T18:47:39.761Z'
---

## 1. Core User Flows

These flows describe the step-by-step paths users will take to accomplish key tasks within CraftMart.

### Flow: `buyer-purchases-item`

*   **Trigger:** Buyer clicks 'Add to Cart' on a `product-detail-page`.
*   **Goal:** To successfully purchase one or more items.
*   **Steps:**
    1.  **Actor (User):** Clicks 'Add to Cart'.
    2.  **Actor (System):** The product is added to the user's cart state, managed by Pinia. A toast notification confirms 'Item added to cart'.
    3.  **Actor (User):** Clicks the cart icon in the navigation, directing them to `/cart`.
    4.  **Actor (User):** Reviews the items, confirms quantities, and clicks 'Proceed to Checkout'.
    5.  **Actor (User):** On `/checkout`, they fill in their shipping address and select a shipping method.
    6.  **Actor (User):** They proceed to the payment step and enter their credit card details into the Stripe Elements form.
    7.  **Actor (User):** They review the final order summary and click 'Place Order'.
    8.  **Actor (System):** The client-side code securely sends payment information to Stripe. Upon successful authorization, a request is sent to our `POST /api/checkout` Firebase Function.
    9.  **Actor (System):** The function validates the cart, calculates the final total, creates a new document in the `orders` collection, and decrements stock for each product in the `products` collection within a transaction.
    10. **Actor (System):** The user is redirected to a confirmation page, and a confirmation email is triggered (see System Flows below).

### Flow: `artisan-lists-product`

*   **Trigger:** Artisan clicks 'Add New Product' in their `seller-dashboard-page`.
*   **Goal:** To successfully list a new item for sale.
*   **Steps:**
    1.  **Actor (User):** Navigates to `/dashboard/seller/product/new`.
    2.  **Actor (User):** Fills in all required fields: name, description, price, stock, and category.
    3.  **Actor (User):** Uploads one or more images. The client-side code uploads them directly to Cloudinary and receives back the image URLs.
    4.  **Actor (User):** Clicks 'Publish Product'.
    5.  **Actor (System):** The frontend sends a request to a Firebase Function containing all product data, including the image URLs.
    6.  **Actor (System):** The function validates all data (e.g., price > 0, stock >= 0). If valid, it creates a new document in the `products` collection.
    7.  **Actor (System):** A success response is returned, and the user is redirected to their seller dashboard with a confirmation message.

## 2. Key System Flows (Backend)

These flows are automated backend processes, typically triggered by events in the database or external services.

### Flow: New Order Notification & Processing

*   **Trigger:** A new document is created in the `orders` collection.
*   **Goal:** To notify the relevant artisan(s) of a new sale.
*   **Steps:**
    1.  A Cloud Function is triggered by `onCreate` on the `orders` collection.
    2.  The function reads the new order document, specifically the `items` array.
    3.  It creates a set of unique `artisanId`s from the items.
    4.  For each `artisanId` in the set, the function:
        a. Fetches the artisan's user document from the `users` collection to get their email address.
        b. Constructs a personalized email payload (e.g., "Congratulations, you've sold these items: ...").
        c. Makes an API call to SendGrid to send the notification email.
    5.  The function also creates an in-app notification document for each artisan (for a future notification center feature).

### Flow: Review Aggregation

*   **Trigger:** A new document is created in the `reviews` collection.
*   **Goal:** To update the corresponding product's average rating and review count.
*   **Steps:**
    1.  A Cloud Function is triggered by `onCreate` on the `reviews` collection.
    2.  The function gets the `productId` from the new review document.
    3.  It performs a query on the `reviews` collection to get *all* reviews where `productId` matches.
    4.  It iterates through the results, calculating the new `averageRating` and the total `reviewCount`.
    5.  It performs a transactional update on the corresponding product document in the `products` collection, setting the new values.

## 3. Error Handling

*   **Payment Failure during Checkout:**
    *   **Scenario:** A user's card is declined by Stripe.
    *   **Handling:** The Stripe API will return an error. Our checkout page will catch this error and display a user-friendly message (e.g., "Your payment could not be processed. Please check your card details and try again."). The 'Place Order' button will be re-enabled, and no `order` document will be created. The user's cart remains intact.

*   **Inventory Conflict (Race Condition):**
    *   **Scenario:** Two users try to buy the last available item at the same time.
    *   **Handling:** The order creation and stock decrement process must be handled within a Firestore transaction. The transaction will read the product's current stock, check if the requested quantity is available, and then write the new stock level. If User A's transaction completes first, User B's transaction will fail when it reads the stock level because it has changed. The backend function will return an error to User B's client, which will display a message like "Sorry, one of the items in your cart has just sold out."

*   **Image Upload Failure:**
    *   **Scenario:** An artisan is listing a product, and an image fails to upload to Cloudinary due to a network issue.
    *   **Handling:** The client-side upload component will have its own error state. It will display an error icon and message next to the failed image. The 'Publish Product' button will be disabled until all images are successfully uploaded or the failed ones are removed.