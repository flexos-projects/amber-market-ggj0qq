---
id: buyer-purchases-item
title: Buyer Purchases an Item Flow
description: >-
  A buyer browses products, adds them to a cart, and completes a secure checkout
  process.
type: spec
subtype: flows
status: draft
sequence: 4
tags:
  - buyer
  - purchase
  - checkout
  - cart
  - payment
relatesTo:
  - features_secure-cart-checkout
  - pages_product-detail-page
  - pages_shopping-cart-page
  - pages_checkout-page
  - collections_orders
createdAt: 2026-02-07T18:47:39.761Z
updatedAt: '2026-02-07T21:53:10.298Z'
---

<flex_block type="flow">
{
  "steps": [
    {
      "type": "action",
      "name": "User starts",
      "actor": "user"
    },
    {
      "type": "action",
      "name": "System responds",
      "actor": "system"
    }
  ]
}
</flex_block>

This flow outlines the critical path for a buyer to make a purchase on CraftMart, leveraging the `Secure Shopping Cart & Checkout` feature (`features_secure-cart-checkout`). It spans multiple pages, including the `Product Detail Page` (`pages_product-detail-page`), `Shopping Cart Page` (`pages_shopping-cart-page`), and `Checkout Page` (`pages_checkout-page`), ultimately resulting in a new entry in the `orders` collection (`collections_orders`).

**Flow Steps & Details:**
1.  **Add to Cart:** From a product detail page, the buyer selects quantity and clicks 'Add to Cart'. The item is added to their session-based or user-specific cart.
2.  **View Cart:** The buyer can navigate to `/cart` to review all items, adjust quantities, or remove items. Subtotal and estimated shipping are displayed.
3.  **Proceed to Checkout:** From the cart, the buyer initiates the checkout process. Authentication is required if not already logged in.
4.  **Shipping Information:** On the first step of `/checkout`, the buyer provides or confirms their shipping address and selects a shipping method.
5.  **Payment Information:** On the second step, the buyer enters credit card details or selects a digital wallet option, securely processed via Stripe.
6.  **Review & Place Order:** The final step presents a comprehensive order summary. The buyer confirms and clicks 'Place Order'.
7.  **Order Processing & Confirmation:** The system processes payment, creates an order record, updates product inventory, displays an order confirmation, and sends email notifications to the buyer and relevant artisans.

**Acceptance Criteria:**
*   Items are correctly added to the cart, and quantities can be adjusted.
*   The cart accurately reflects the total price, including estimated shipping.
*   The checkout process is secure (SSL/TLS) and multi-step.
*   Valid shipping and payment information can be successfully submitted.
*   Upon successful payment, an order is created in the `orders` collection.
*   Product stock is decremented correctly after a purchase.
*   Buyer receives an immediate order confirmation email.
*   Artisan(s) receive notification of a new order.

**Edge Cases:**
*   Payment failure (e.g., invalid card, insufficient funds) – appropriate error message displayed.
*   Product goes out of stock between adding to cart and checkout – buyer notified, item removed from cart.
*   Invalid shipping address provided – validation errors displayed.
*   User abandons cart during checkout – items remain in cart for a configurable period.
*   Multiple artisans in one order – system correctly handles splitting order details for artisans and consolidating for buyer.