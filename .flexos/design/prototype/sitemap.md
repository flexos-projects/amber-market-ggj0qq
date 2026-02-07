---
id: prototype-sitemap
title: "Prototype Sitemap"
description: "Route map for prototype pages"
type: design
subtype: sitemap
status: draft
sequence: 0
tags: [prototype, routing]
relatesTo: []
createdAt: "2026-02-07T18:47:39.761Z"
updatedAt: "2026-02-07T18:47:39.761Z"
---

# Prototype Sitemap

<flex_block type="config">
{
  "routes": {
    "/index": "landing-page-v1.html",
    "/auth": "auth-page-v1.html",
    "/shop": "shop-browse-page-v1.html",
    "/shop/:slug": "product-detail-page-v1.html",
    "/cart": "shopping-cart-page-v1.html",
    "/checkout": "checkout-page-v1.html",
    "/dashboard/buyer": "buyer-dashboard-page-v1.html",
    "/dashboard/seller": "seller-dashboard-page-v1.html",
    "/dashboard/seller/product/:id?": "add-edit-product-page-v1.html",
    "/settings": "settings-page-v1.html"
  },
  "fallback": "404.html",
  "pages": [
    {
      "id": "landing-page",
      "route": "/index",
      "file": "landing-page-v1.html",
      "auth": true
    },
    {
      "id": "auth-page",
      "route": "/auth",
      "file": "auth-page-v1.html",
      "auth": true
    },
    {
      "id": "shop-browse-page",
      "route": "/shop",
      "file": "shop-browse-page-v1.html",
      "auth": true
    },
    {
      "id": "product-detail-page",
      "route": "/shop/:slug",
      "file": "product-detail-page-v1.html",
      "auth": true
    },
    {
      "id": "shopping-cart-page",
      "route": "/cart",
      "file": "shopping-cart-page-v1.html",
      "auth": true
    },
    {
      "id": "checkout-page",
      "route": "/checkout",
      "file": "checkout-page-v1.html",
      "auth": true
    },
    {
      "id": "buyer-dashboard-page",
      "route": "/dashboard/buyer",
      "file": "buyer-dashboard-page-v1.html",
      "auth": true
    },
    {
      "id": "seller-dashboard-page",
      "route": "/dashboard/seller",
      "file": "seller-dashboard-page-v1.html",
      "auth": true
    },
    {
      "id": "add-edit-product-page",
      "route": "/dashboard/seller/product/:id?",
      "file": "add-edit-product-page-v1.html",
      "auth": true
    },
    {
      "id": "settings-page",
      "route": "/settings",
      "file": "settings-page-v1.html",
      "auth": true
    }
  ]
}
</flex_block>

## Routes

| Route | File | Auth |
|-------|------|------|
| /index | landing-page-v1.html | Yes |
| /auth | auth-page-v1.html | Yes |
| /shop | shop-browse-page-v1.html | Yes |
| /shop/:slug | product-detail-page-v1.html | Yes |
| /cart | shopping-cart-page-v1.html | Yes |
| /checkout | checkout-page-v1.html | Yes |
| /dashboard/buyer | buyer-dashboard-page-v1.html | Yes |
| /dashboard/seller | seller-dashboard-page-v1.html | Yes |
| /dashboard/seller/product/:id? | add-edit-product-page-v1.html | Yes |
| /settings | settings-page-v1.html | Yes |

---

*Prototype HTML files pending AI generation*
