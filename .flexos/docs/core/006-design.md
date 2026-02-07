---
id: design
title: 'Design System & Brand Guidelines'
description: 'Establishes the visual and tonal identity for CraftMart, including brand voice, color palette, typography, core components, and accessibility standards.'
type: doc
subtype: core
status: draft
sequence: 6
tags:
  - design
  - ui
  - ux
  - a11y
createdAt: '2026-02-07T18:47:39.761Z'
updatedAt: '2026-02-07T18:47:39.761Z'
---

## 1. Brand Voice & Tone

CraftMart's voice is **warm, authentic, and inviting**. We aim to build a community, not just a platform. Our communication should always be encouraging and genuine.

*   **Keywords:** Handcrafted, Community, Discovery, Creator, Treasure, Unique, Passion.
*   **Tone:** Supportive and clear. We avoid cold, corporate jargon. For example, instead of "User-generated content policy," we say "Our community guidelines."
*   **Personality:** Like a friendly and knowledgeable owner of a local craft shop. We are passionate about the products and the people who make them.

## 2. Color System

Our color palette is designed to feel warm, natural, and trustworthy.

*   **Primary (`primaryColor`): Amber**
    *   **Hex:** `#f59e0b`
    *   **Usage:** Primary calls-to-action (e.g., 'Add to Cart', 'Sign Up'), active links, and key highlights. It's energetic and draws attention.

*   **Accent (`accentColor`): Cyan**
    *   **Hex:** `#0e7490`
    *   **Usage:** Secondary actions (e.g., 'Continue Shopping'), informational banners, and subtle highlights. It provides a cool, calming contrast to the primary amber.

*   **Neutrals:**
    *   **Dark (`neutralDark`):** `#333333` - Used for all body text and headings for maximum readability.
    *   **Mid-Gray:** `#6b7280` - Used for secondary text, labels, and descriptions.
    *   **Light Gray:** `#e5e7eb` - Used for borders, dividers, and disabled states.
    *   **Background (`neutralLight`):** `#f9fafb` - Used for the main background of pages to provide a soft, clean canvas.

*   **System Colors:**
    *   **Success:** A gentle green (`#10b981`) for success messages.
    *   **Error:** A clear red (`#ef4444`) for error messages and destructive actions.
    *   **Warning:** A soft yellow (`#facc15`) for warnings or informational alerts.

## 3. Typography

Our typography choices balance personality with legibility, reflecting the blend of craft and commerce.

*   **Headings Font (`fontFamily`): Lora**
    *   **Style:** Serif
    *   **Weight:** 600 (Semi-bold)
    *   **Why:** Lora has a classic, artistic feel that evokes craftsmanship and quality. It's used for all `<h1>`, `<h2>`, and `<h3>` tags.

*   **Body Font (`fontFamily`): Inter**
    *   **Style:** Sans-serif
    *   **Weight:** 400 (Regular), 600 (Semi-bold)
    *   **Why:** Inter is a highly readable and modern sans-serif, perfect for UI elements, product descriptions, forms, and all paragraph text. Its clarity ensures a smooth user experience.

## 4. Core UI Components

Consistency will be achieved by building a library of reusable Vue components.

*   **Buttons:**
    *   **Primary:** Solid amber (`#f59e0b`) background with white text. Used for the most important action on a page.
    *   **Secondary:** Cyan (`#0e7490`) outline with cyan text. Used for secondary actions.
    *   **Text/Link:** No background or border, uses the primary amber color for the text.
    *   **States:** All buttons will have defined `hover`, `focus`, and `disabled` states.

*   **Product Card:**
    *   The standard component for displaying a product in a grid (`/shop` page, `/dashboard/buyer` favorites).
    *   **Elements:** High-quality image container, product name (Lora font), artisan's shop name (Inter font), price, and a favorite (heart) icon.

*   **Form Inputs:**
    *   Standardized text inputs, text areas, and select dropdowns with clear labels, placeholder text, and consistent styling for validation states (error, success).

## 5. Accessibility (A11y)

Accessibility is a primary requirement, not an afterthought. We will adhere to WCAG 2.1 AA standards.

*   **Color Contrast:** All text and UI elements will meet the minimum contrast ratio. The primary amber (`#f59e0b`) on white has a ratio of 3.03:1, which is only suitable for large text (18pt+). For smaller text on buttons, we must use a darker shade like `#d97706` (4.55:1).
*   **Keyboard Navigation:** The entire site must be navigable and usable with only a keyboard. Focus states (`focus-visible`) will be clearly defined for all interactive elements.
*   **Semantic HTML:** We will use proper HTML5 elements (`<main>`, `<nav>`, `<article>`, etc.) to provide structure for screen readers.
*   **Image Alt Text:** All `<img>` tags must have descriptive `alt` attributes. For decorative images, `alt=""` will be used.
*   **Forms:** All form inputs will be associated with a `<label>` tag for screen reader accessibility.