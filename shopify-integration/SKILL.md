---
name: shopify-integration
version: 2.0.0
description: Generates secure Shopify Liquid templates for product grids, collections, and checkout flows. Integrates with Shopify's product API, handles dynamic pricing, bulk discounts, analytics tracking, and WCAG 2.1 AA accessibility. Includes PCI compliance guidance, XSS prevention, API key security, and B2B quote flows.
tags: [shopify, liquid, ecommerce, security, pci, b2b, analytics]
dependencies: [ecommerce-product-grid, performance-optimization]
license: MIT
---

# Shopify Integration

## Purpose

You are a Shopify developer specializing in luxury e-commerce experiences. Your role is to:
- **Generate secure Liquid templates** for product grids, collections, and checkout
- **Integrate with Shopify API** — dynamic products, pricing, inventory
- **Implement bulk pricing** — volume discounts (x12 units → lower per-unit price)
- **Optimize performance** — lazy loading, image optimization, fast checkout
- **Track analytics** — GTM events, conversion tracking, A/B testing
- **Ensure accessibility** — WCAG 2.1 AA compliance throughout
- **Apply security hardening** — XSS prevention, input sanitization, PCI compliance

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

### 11. Security — XSS Prevention & Liquid Escaping

**CRITICAL: Always escape dynamic output in Liquid:**

```liquid
<!-- ❌ NEVER: Unescaped output (XSS vulnerability) -->
<h2>{{ product.title }}</h2>
<p>{{ customer.first_name }}</p>

<!-- ✅ ALWAYS: Escaped output -->
<h2>{{ product.title | escape }}</h2>
<p>{{ customer.first_name | escape }}</p>

<!-- ✅ For rich text from metafields, use Shopify's sanitized renderer -->
{{ product.metafields.custom.fragrance_notes | metafield_tag }}

<!-- ✅ For URLs, always validate and encode -->
<a href="{{ product.url | escape }}">{{ product.title | escape }}</a>

<!-- ✅ For image URLs, use Shopify's image_url filter (CDN-validated) -->
<img src="{{ product.featured_image | image_url: width: 500 }}" alt="{{ product.featured_image.alt | escape }}" />
```

**Prevent template injection from search parameters:**
```liquid
<!-- ❌ NEVER: Output raw URL parameters -->
<p>Búsqueda: {{ request.path }}</p>

<!-- ✅ DO: Never output request parameters directly into HTML -->
{% comment %}
  Never reflect URL parameters, form inputs, or customer data
  without Shopify's built-in escaping filters
{% endcomment %}
```

### 12. API Key Security & Environment Management

**NEVER hardcode API keys in Liquid templates or theme files:**

```liquid
<!-- ❌ NEVER: Hardcoded API credentials -->
{% assign api_key = 'sk_live_abc123...' %}

<!-- ✅ DO: Use Shopify Private App credentials stored in settings -->
{% comment %}
  API keys belong in:
  1. Shopify Admin > Apps > Private Apps (server-side only)
  2. Theme Settings (for public-safe tokens like GTM IDs)
  3. Environment variables in serverless functions (Shopify Functions)
  NEVER in Liquid template files or client-side JavaScript
{% endcomment %}

<!-- ✅ Safe: Public GTM ID from theme settings (not sensitive) -->
{% if settings.gtm_id != blank %}
  <script>
    // GTM container ID — safe to expose client-side
    (function(w,d,s,l,i){...})(..., '{{ settings.gtm_id | escape }}');
  </script>
{% endif %}
```

**`.env` best practices for Shopify app development:**
```bash
# .env (NEVER commit — add to .gitignore)
SHOPIFY_API_KEY=shppa_xxxxxxxxxxxxxxxxxxxxx
SHOPIFY_API_SECRET=shpss_xxxxxxxxxxxxxxxxxxxxx
SHOPIFY_ACCESS_TOKEN=shpat_xxxxxxxxxxxxxxxxxxxxx

# .gitignore — always include these
.env
.env.local
.env.production
node_modules/
```

### 13. PCI Compliance for Luxury Checkout

**Payment method recommendations for luxury B2B:**

| Method | Recommendation | Why |
|--------|---------------|-----|
| Shopify Payments | ✅ Primary | PCI DSS Level 1 compliant, native integration |
| Stripe | ✅ Alternative | PCI DSS Level 1, excellent DX, strong fraud prevention |
| American Express | ✅ Recommended | Common for B2B luxury purchases, high-limit cards |
| PayPal Business | ✅ Complementary | International buyers, buyer protection signal |
| Wire Transfer | ✅ For bulk orders | >€1000 orders — invoice + NET30 terms |

**Shopify Checkout security — never customize these:**
```liquid
{% comment %}
  CRITICAL PCI COMPLIANCE RULES:
  1. Never store payment card data — Shopify handles this
  2. Never log or display card numbers, CVVs, or expiry dates
  3. Checkout pages are hosted by Shopify on their PCI-compliant servers
  4. Do not inject custom JavaScript on /checkout/* pages
     (Shopify restricts this for PCI compliance reasons)
  5. Use Shopify's checkout.liquid ONLY if on Shopify Plus
{% endcomment %}
```

**GDPR compliance for EU customers:**
```liquid
<!-- Cookie consent before any tracking -->
{% unless customer.accepts_marketing == true or request.path contains '/checkout' %}
  <div class="cookie-consent" role="dialog" aria-live="polite" aria-label="Consentimiento de cookies">
    <p>Utilizamos cookies para mejorar tu experiencia. Al continuar, aceptas nuestra
    <a href="/policies/privacy-policy">política de privacidad</a>.</p>
    <button class="cookie-consent__accept" onclick="acceptCookies()">Aceptar</button>
    <button class="cookie-consent__reject" onclick="rejectCookies()">Rechazar</button>
  </div>
{% endunless %}
```

### 14. Currency & Multi-Currency Handling

**Euro primary, multi-currency support:**
```liquid
<!-- Display price in customer's currency -->
<span class="price">
  {{ variant.price | money_with_currency }}
</span>

<!-- Bulk price with currency -->
{% if product.metafields.custom.bulk_price_12 %}
  <span class="price-bulk">
    {{ product.metafields.custom.bulk_price_12 | money_with_currency }}
    <small>× 12 unidades</small>
  </span>
{% endif %}

<!-- Shopify Markets — automatic FX conversion -->
{% comment %}
  Configure Shopify Markets in Admin > Settings > Markets
  Enable: EUR (primary), USD, GBP
  FX rates: automatic (Shopify) or manual (set your rates)
  Rounding: €X.00 (no cents for luxury — feels premium)
{% endcomment %}
```

### 15. Advanced Shopify Metafields

**Complete metafield schema for LAVERNE:**

```json
{
  "metafield_definitions": [
    {
      "namespace": "custom",
      "key": "fragrance_notes",
      "name": "Fragrance Notes",
      "type": "multi_line_text_field",
      "description": "Top/Heart/Base notes for fragrance display"
    },
    {
      "namespace": "custom",
      "key": "bulk_price_12",
      "name": "Bulk Price (x12)",
      "type": "money",
      "description": "Price per unit when ordering 12 or more"
    },
    {
      "namespace": "custom",
      "key": "fragrance_family",
      "name": "Fragrance Family",
      "type": "single_line_text_field",
      "description": "Oriental, Fresh, Floral, Woody"
    },
    {
      "namespace": "custom",
      "key": "bottle_size_ml",
      "name": "Bottle Size (ml)",
      "type": "number_integer",
      "description": "Bottle size in milliliters"
    },
    {
      "namespace": "custom",
      "key": "concentration",
      "name": "Concentration",
      "type": "single_line_text_field",
      "description": "EDP, EDT, EDC, Parfum"
    }
  ]
}
```

**Accessing metafields safely in Liquid:**
```liquid
{% comment %} Always check for existence before rendering {% endcomment %}
{% if product.metafields.custom.fragrance_notes != blank %}
  <div class="fragrance-notes" aria-label="Notas de fragancia">
    {% assign notes = product.metafields.custom.fragrance_notes | split: '|' %}
    {% for note in notes %}
      <span class="note-tag">{{ note | strip | escape }}</span>
    {% endfor %}
  </div>
{% endif %}
```

### 16. B2B Quote Flow Optimization

LAVERNE's primary conversion is a **quote request**, not a direct checkout:

```liquid
<!-- /sections/quote-form.liquid -->
<section class="quote-form" aria-label="Formulario de cotización">
  <h2 class="quote-form__title">Solicitar cotización</h2>

  <form
    action="/contact"
    method="POST"
    class="quote-form__fields"
    novalidate
  >
    {% form 'contact' %}

      <div class="form-group">
        <label for="contact_company" class="form-label">
          Empresa <span aria-label="requerido">*</span>
        </label>
        <input
          type="text"
          id="contact_company"
          name="contact[body]"
          required
          autocomplete="organization"
          class="form-input"
          aria-required="true"
          aria-describedby="company-hint"
        />
        <p id="company-hint" class="form-hint">Nombre de tu empresa o negocio</p>
      </div>

      <div class="form-group">
        <label for="contact_quantity" class="form-label">
          Cantidad estimada <span aria-label="requerido">*</span>
        </label>
        <select id="contact_quantity" name="contact[quantity]" class="form-select" required>
          <option value="">Seleccionar</option>
          <option value="24-59">24 – 59 unidades (tester gratis)</option>
          <option value="60-119">60 – 119 unidades</option>
          <option value="120+">120+ unidades (precio especial)</option>
        </select>
      </div>

      <!-- Hidden: Cart items for context -->
      <input type="hidden" name="contact[cart_items]"
        value="{{ cart.items | map: 'title' | join: ', ' | escape }}" />

      <button type="submit" class="btn-quote-submit">
        Enviar solicitud de cotización
      </button>

    {% endform %}
  </form>
</section>
```

**Quote tracking in GTM:**
```javascript
// Fire event when quote form is submitted
document.querySelector('.quote-form__fields')?.addEventListener('submit', function(e) {
  const quantity = document.getElementById('contact_quantity')?.value;
  if (window.dataLayer) {
    window.dataLayer.push({
      event: 'quote_form_submit',
      quantity_tier: quantity,
      currency: 'EUR'
    });
  }
});
```

## Implementation Checklist

- [ ] Shopify metafields configured (fragrance_notes, bulk_price_12, concentration, bottle_size_ml)
- [ ] Liquid templates created (collection-grid, product-card, quote-summary, quote-form)
- [ ] **Security**: All `{{ variable }}` outputs use `| escape` filter
- [ ] **Security**: API keys NOT in Liquid files — stored in Private Apps only
- [ ] **Security**: No custom JS on `/checkout/*` pages (PCI compliance)
- [ ] **GDPR**: Cookie consent dialog implemented before GTM fires
- [ ] Images optimized (srcset, loading="lazy", Shopify CDN image_url filter)
- [ ] Pricing display correct (regular + bulk variant, no strikethrough)
- [ ] Currency: `money_with_currency` filter for multi-currency support
- [ ] CTA buttons functional (add to quote, quote form submission)
- [ ] Analytics tracking: GTM events for add_to_quote, quote_form_submit
- [ ] Mobile responsive (375px, 768px, 1440px)
- [ ] Accessibility verified (WCAG 2.1 AA — all form inputs have labels)
- [ ] Performance tested (Lighthouse > 85, LCP < 2.5s)
- [ ] A/B variants deployed via customer metafield assignment
- [ ] Payment methods configured (Shopify Payments + Stripe + AmEx + PayPal)
- [ ] Quote flow tested end-to-end (form submission → email notification)

## Related Skills

- `ecommerce-product-grid` — Product grid layout & styling
- `performance-optimization` — Image optimization, lazy loading
- `landing-page-hero-design` — Hero CTA → Collection page flow
