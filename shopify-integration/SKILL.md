---
name: shopify-integration
description: Generates Shopify Liquid templates for product grids, collections, and checkout flows. Integrates with Shopify's product API, handles dynamic pricing, bulk discounts, and analytics tracking. Creates clean, performant Liquid code ready to deploy.
license: MIT
---

# Shopify Integration

## Purpose

You are a Shopify developer specializing in luxury e-commerce experiences. Your role is to:
- **Generate Liquid templates** for product grids, collections, and checkout
- **Integrate with Shopify API** — dynamic products, pricing, inventory
- **Implement bulk pricing** — volume discounts (x12 units → lower per-unit price)
- **Optimize performance** — lazy loading, image optimization, fast checkout
- **Track analytics** — GTM events, conversion tracking, A/B testing
- **Ensure accessibility** — WCAG 2.1 AA compliance throughout

## Core Principles

### 1. Shopify Liquid Basics

Liquid is Shopify's template language. Key concepts for LAVERNE:

**Product variables:**
```liquid
{{ product.title }} — Product name
{{ product.featured_image }} — Main image
{{ product.price }} — Price in cents (e.g., 6500 = €65.00)
{{ product.variants }} — Array of product variants
{{ product.metafields }} — Custom data (e.g., fragrance notes)
```

**Loops & conditionals:**
```liquid
{% for product in collection.products %}
  <div class="product-card">
    <h2>{{ product.title }}</h2>
    {% if product.available %}
      <p>In Stock</p>
    {% endif %}
  </div>
{% endfor %}
```

**Images:**
```liquid
{{ product.featured_image | image_url: width: 500 }}
{{ product.featured_image | image_tag }}
```

### 2. Collection Page Template

For LAVERNE catalog pages (8 product pages after hero):

```liquid
<!-- /sections/collection-grid.liquid -->

<section class="collection-grid">
  <div class="collection-header">
    <h1>{{ collection.title }}</h1>
    <p>{{ collection.description }}</p>
  </div>

  <div class="product-grid">
    {% for product in collection.products %}
      {% render 'product-card', product: product %}
    {% endfor %}
  </div>
</section>
```

### 3. Product Card Component (Liquid)

File: `/sections/product-card.liquid`

```liquid
{% comment %}
Renders a single product card with image, pricing, and CTA
Includes: lazy loading, bulk pricing, fragrance notes
{% endcomment %}

<article class="product-card" data-product-id="{{ product.id }}">
  
  {%- comment -%} Image {%- endcomment -%}
  <div class="product-card__image">
    {% if product.featured_image %}
      <img
        src="{{ product.featured_image | image_url: width: 500 }}"
        alt="{{ product.featured_image.alt | escape }}"
        loading="lazy"
        class="product-card__img"
        width="500"
        height="500"
      />
    {% endif %}
  </div>

  {%- comment -%} Content {%- endcomment -%}
  <div class="product-card__content">
    
    {%- comment -%} Title {%- endcomment -%}
    <h2 class="product-card__title">
      <a href="{{ product.url }}" class="product-card__link">
        {{ product.title }}
      </a>
    </h2>

    {%- comment -%} Fragrance Notes (from metafield) {%- endcomment -%}
    {% if product.metafields.custom.fragrance_notes %}
      <div class="product-card__notes">
        {{ product.metafields.custom.fragrance_notes | metafield_tag: 'richtext' }}
      </div>
    {% endif %}

    {%- comment -%} Pricing (with bulk discount) {%- endcomment -%}
    <div class="product-card__pricing">
      {% assign variant = product.selected_or_first_available_variant %}
      
      {%- comment -%} Regular price {%- endcomment -%}
      <span class="product-card__price-regular">
        {{ variant.price | money }}
      </span>

      {%- comment -%} Bulk discount (if exists) {%- endcomment -%}
      {% if product.metafields.custom.bulk_price_12 %}
        <span class="product-card__price-bulk">
          {{ product.metafields.custom.bulk_price_12 | money }} 
          <span class="product-card__price-label">×12 units</span>
        </span>
      {% endif %}
    </div>

    {%- comment -%} CTA Button {%- endcomment -%}
    <button
      class="product-card__cta"
      data-product-id="{{ product.id }}"
      data-variant-id="{{ variant.id }}"
      data-track-event="add_to_quote"
    >
      {% if variant.available %}
        Añadir a cotización
      {% else %}
        Agotado
      {% endif %}
    </button>
  </div>
</article>

<style>
  .product-card {
    display: flex;
    flex-direction: column;
    border: 1px solid #e0e0e0;
    border-radius: 4px;
    overflow: hidden;
    transition: box-shadow 0.2s ease;
  }

  .product-card:hover {
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  }

  .product-card__image {
    aspect-ratio: 1 / 1;
    overflow: hidden;
    background: #f5f5f5;
  }

  .product-card__img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .product-card__content {
    padding: 16px;
    flex: 1;
    display: flex;
    flex-direction: column;
  }

  .product-card__title {
    font-size: 18px;
    font-weight: 600;
    margin: 0 0 8px;
    font-family: serif;
    color: #1a3a52;
  }

  .product-card__notes {
    font-size: 12px;
    color: #666;
    margin-bottom: 12px;
    line-height: 1.5;
  }

  .product-card__pricing {
    display: flex;
    gap: 8px;
    margin-bottom: 12px;
    font-size: 16px;
    font-weight: 600;
  }

  .product-card__price-bulk {
    color: #d4af37;
  }

  .product-card__price-label {
    font-size: 12px;
    font-weight: normal;
    color: #999;
  }

  .product-card__cta {
    padding: 12px 16px;
    background: #d4af37;
    color: #1a3a52;
    border: none;
    border-radius: 4px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.2s ease;
    font-size: 14px;
  }

  .product-card__cta:hover:not(:disabled) {
    transform: scale(1.02);
    box-shadow: 0 4px 12px rgba(212, 175, 55, 0.3);
  }

  .product-card__cta:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }

  @media (max-width: 768px) {
    .product-card__content {
      padding: 12px;
    }

    .product-card__title {
      font-size: 16px;
    }
  }
</style>
```

### 4. Bulk Pricing Setup (Shopify Metafields)

For LAVERNE's x12 volume discounts:

**Metafield namespace**: `custom`
**Field names**:
- `fragrance_notes` (richtext) — Fragrance note description
- `bulk_price_12` (money) — Price per unit when buying 12+

**Example metafield JSON**:
```json
{
  "custom": {
    "fragrance_notes": "<ul><li>Top: Lemon, Apple</li><li>Heart: Lavender</li><li>Base: Patchouli</li></ul>",
    "bulk_price_12": 5500
  }
}
```

### 5. Analytics & Tracking (GTM)

Track product interactions for A/B testing:

```liquid
<script>
window.dataLayer = window.dataLayer || [];

function addToQuote(productId, variantId, price) {
  dataLayer.push({
    event: 'add_to_quote',
    product_id: productId,
    variant_id: variantId,
    price: price,
    currency: 'EUR',
    timestamp: new Date().toISOString()
  });
}
</script>

<!-- In product-card.liquid CTA -->
<button onclick="addToQuote({{ product.id }}, {{ variant.id }}, {{ variant.price }})">
  Añadir a cotización
</button>
```

### 6. Image Optimization (Shopify)

Use Shopify's native image optimization:

```liquid
{%- comment -%} Responsive image with srcset {%- endcomment -%}
<img
  src="{{ product.featured_image | image_url: width: 500 }}"
  srcset="
    {{ product.featured_image | image_url: width: 300 }} 300w,
    {{ product.featured_image | image_url: width: 500 }} 500w,
    {{ product.featured_image | image_url: width: 1000 }} 1000w
  "
  sizes="(max-width: 768px) 100vw, 33vw"
  alt="{{ product.featured_image.alt }}"
  loading="lazy"
/>
```

### 7. Mobile Responsive (Liquid + CSS)

```liquid
<style>
  @media (max-width: 768px) {
    .product-grid {
      grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
      gap: 12px;
    }
  }

  @media (max-width: 480px) {
    .product-grid {
      grid-template-columns: 1fr;
    }
  }
</style>
```

### 8. Checkout Optimization

For LAVERNE's bulk quote flow:

```liquid
<!-- /sections/quote-summary.liquid -->

<section class="quote-summary">
  <h2>Tu cotización</h2>
  
  <div class="quote-items">
    {% for item in cart.items %}
      <div class="quote-item">
        <span>{{ item.product.title }} × {{ item.quantity }}</span>
        <span class="quote-item__total">{{ item.final_line_price | money }}</span>
      </div>
    {% endfor %}
  </div>

  <div class="quote-total">
    <strong>Total:</strong>
    <strong>{{ cart.total_price | money }}</strong>
  </div>

  <button class="quote-cta" onclick="submitQuote()">
    Enviar cotización
  </button>
</section>
```

### 9. Performance Checklist (Liquid)

- [ ] All images use `loading="lazy"` and `srcset`
- [ ] No inline CSS (use style tags in `<head>` or external stylesheet)
- [ ] Metafield queries optimized (not in loops)
- [ ] No N+1 queries (use `include` for components)
- [ ] Minimize Liquid tags (use filters instead of conditionals where possible)
- [ ] CSS output < 50KB (minified)
- [ ] JavaScript async/defer where possible
- [ ] Lighthouse Performance > 85

### 10. A/B Testing via Shopify

```liquid
{% assign ab_test = customer.metafields.custom.ab_test_group %}

{% if ab_test == 'variant_a' %}
  <!-- Show variant A: "Añadir a cotización" -->
{% elsif ab_test == 'variant_b' %}
  <!-- Show variant B: "Solicitar muestra" -->
{% endif %}
```

## Implementation Checklist

- [ ] Shopify metafields configured (fragrance_notes, bulk_price_12)
- [ ] Liquid templates created (collection-grid, product-card, quote-summary)
- [ ] Images optimized (srcset, loading="lazy", WebP)
- [ ] Pricing display correct (regular + bulk variant)
- [ ] CTA buttons functional (add to quote, checkout)
- [ ] Analytics tracking set up (GTM events)
- [ ] Mobile responsive (375px, 768px, 1440px)
- [ ] Accessibility verified (WCAG 2.1 AA)
- [ ] Performance tested (Lighthouse > 85, LCP < 2.5s)
- [ ] A/B variants deployed and tracked

## Related Skills

- `ecommerce-product-grid` — Product grid layout & styling
- `performance-optimization` — Image optimization, lazy loading
- `landing-page-hero-design` — Hero CTA → Collection page flow
