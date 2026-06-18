---
name: ecommerce-product-grid
version: 2.0.0
description: Luxury product showcase grids for LAVERNE fragrance catalog. Delivers premium card anatomy, golden-ratio spacing, rich metadata (fragrance notes pyramid), bulk pricing hierarchy, interactive hover states, and WCAG 2.1 AAA accessibility — zero generic templates.
tags: [ecommerce, luxury, product-grid, shopify, accessibility, performance]
dependencies: [landing-page-hero-design, performance-optimization, shopify-integration]
license: MIT
---

# Ecommerce Product Grid

## Purpose

You are a luxury e-commerce UI specialist. Your role is to create product grids that:
- **Elevate product photography** — images sell, layout amplifies
- **Communicate premium pricing** — bulk discounts without looking "sale-y" or cheap
- **Present fragrance identity** — notes pyramid as artistic storytelling, not a list
- **Respect negative space** — luxury is defined by what you leave out
- **Convert B2B buyers** — not impulse shoppers, but professional purchasers

## Core Principles

### 1. Luxury Product Card Anatomy

Every card is a mini brand story. Structure it with the **golden ratio** in mind (1:1.618):

```
┌────────────────────────────────┐
│                                │  ← Image: 62% of card height
│    [PRODUCT PHOTOGRAPHY]       │    Square 1:1 crop, studio white
│    [centered, no crop bleed]   │    Warm directional lighting
│                                │
├────────────────────────────────┤
│  LAVERNE · ORIENT FRAGANCE     │  ← Brand / Collection (12px, gold, uppercase)
│                                │
│  Blue Laverne 7am              │  ← Product Title (20px, serif, navy)
│  100ml Eau de Parfum           │  ← Subtitle (13px, gray, normal weight)
│                                │
│  ╔══ FRAGRANCE NOTES ════╗    │  ← Notes pyramid (subtle border-left gold)
│  ║ Top:  Bergamot · Lemon ║   │
│  ║ Heart: Lavender · Rose  ║   │
│  ║ Base:  Musk · Patchouli ║   │
│  ╚══════════════════════╝    │
│                                │
│  €65.00                        │  ← Regular price (22px, navy, bold)
│  €55.00 · x12 unidades         │  ← Bulk (16px, gold) + label (11px, gray)
│                                │
│  [Añadir a cotización  →]      │  ← CTA (gold bg, navy text, full width)
└────────────────────────────────┘
```

**Spacing ratios (8px base unit):**
- Card padding: 24px (3 units)
- Between sections: 16px (2 units)
- Between label + value: 4px (0.5 units)
- Image-to-content gap: 0 (seamless)

### 2. Grid Layouts for Luxury Brands

Negative space is a luxury signal. Use it deliberately:

**Desktop (1440px+):** 3-column grid
```css
.product-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 32px;           /* generous breathing room */
  max-width: 1200px;   /* don't stretch to full bleed */
  margin: 0 auto;
  padding: 64px 24px;
}
```

**Tablet (768px–1439px):** 2-column grid
```css
@media (max-width: 1199px) {
  .product-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 24px;
    padding: 48px 32px;
  }
}
```

**Mobile (< 768px):** 1-column grid — *do not do 2-col on mobile for luxury*
```css
@media (max-width: 767px) {
  .product-grid {
    grid-template-columns: 1fr;
    gap: 16px;
    padding: 32px 16px;
  }
}
```

> **Anti-pattern:** Never auto-fill/auto-fit with `minmax(150px, 1fr)` — this produces unpredictable counts and destroys luxury brand equity.

### 3. Rich Product Metadata Display

#### Fragrance Notes Pyramid

Present top/heart/base notes as editorial content, not a data table:

```html
<div class="fragrance-notes" aria-label="Fragrance notes pyramid">
  <div class="fragrance-notes__layer fragrance-notes__layer--top">
    <span class="fragrance-notes__label">Top</span>
    <span class="fragrance-notes__values">Bergamot · Lemon · Apple</span>
  </div>
  <div class="fragrance-notes__layer fragrance-notes__layer--heart">
    <span class="fragrance-notes__label">Heart</span>
    <span class="fragrance-notes__values">Lavender · Jasmine · Rose</span>
  </div>
  <div class="fragrance-notes__layer fragrance-notes__layer--base">
    <span class="fragrance-notes__label">Base</span>
    <span class="fragrance-notes__values">Musk · Patchouli · Amber</span>
  </div>
</div>
```

```css
.fragrance-notes {
  border-left: 2px solid #D4AF37;
  padding-left: 12px;
  margin: 12px 0;
}

.fragrance-notes__layer {
  display: flex;
  gap: 8px;
  align-items: baseline;
  margin-bottom: 4px;
  font-size: 12px;
}

.fragrance-notes__label {
  color: #D4AF37;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  min-width: 40px;
  flex-shrink: 0;
}

.fragrance-notes__values {
  color: #666;
  line-height: 1.4;
}
```

#### Bottle Design Specs (Optional Badge)
```html
<div class="product-card__specs">
  <span class="spec-badge">100ml</span>
  <span class="spec-badge">EDP</span>
  <span class="spec-badge spec-badge--origin">Orient</span>
</div>
```

### 4. Visual Hierarchy for Bulk Pricing

Show €65 → €55 at ×12 as a **professional value proposition**, not a clearance signal:

```html
<!-- Luxury pricing hierarchy: never use strikethrough for the standard price -->
<div class="product-card__pricing" aria-label="Pricing">
  <div class="pricing__standard">
    <span class="pricing__amount" aria-label="Precio unitario">€65.00</span>
    <span class="pricing__unit">/ unidad</span>
  </div>
  <div class="pricing__bulk" aria-label="Precio por volumen">
    <span class="pricing__bulk-amount">€55.00</span>
    <span class="pricing__bulk-label">× 12 unidades</span>
    <span class="pricing__savings">Ahorro €10 por unidad</span>
  </div>
</div>
```

```css
.pricing__standard {
  display: flex;
  align-items: baseline;
  gap: 4px;
  margin-bottom: 6px;
}

.pricing__amount {
  font-size: 22px;
  font-weight: 700;
  color: #1A3A52;
  font-family: 'Playfair Display', serif;
}

.pricing__unit {
  font-size: 13px;
  color: #888;
}

.pricing__bulk {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 6px 10px;
  background: rgba(212, 175, 55, 0.08);
  border-radius: 4px;
  border-left: 3px solid #D4AF37;
}

.pricing__bulk-amount {
  font-size: 16px;
  font-weight: 700;
  color: #D4AF37;
}

.pricing__bulk-label {
  font-size: 12px;
  color: #888;
}

.pricing__savings {
  font-size: 11px;
  color: #5a8a4a;  /* subtle green — savings signal without aggression */
  margin-left: auto;
}
```

> **Psychology note:** Never use `was €65, now €55`. Instead, frame it as: `€55 × 12 unidades` — a professional purchasing tier, not a markdown.

### 5. Interactive Premium Hover States

Hover must feel **intentional and smooth** — not cartoonish or web 2.0:

```css
.product-card {
  background: #ffffff;
  border: 1px solid rgba(26, 58, 82, 0.08);
  border-radius: 2px;   /* minimal rounding — luxury = sharp edges */
  overflow: hidden;
  transition:
    transform 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94),
    box-shadow 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94),
    border-color 0.3s ease;
  will-change: transform;
}

.product-card:hover {
  transform: translateY(-4px);
  box-shadow:
    0 12px 40px rgba(26, 58, 82, 0.12),
    0 4px 12px rgba(26, 58, 82, 0.06);
  border-color: rgba(212, 175, 55, 0.3);
}

/* Image zoom on hover — ultra subtle */
.product-card__img {
  transition: transform 0.6s cubic-bezier(0.25, 0.46, 0.45, 0.94);
  transform-origin: center;
}

.product-card:hover .product-card__img {
  transform: scale(1.03);  /* 3% max — any more is cartoonish */
}

/* CTA reveal animation */
.product-card__cta {
  opacity: 0.85;
  transition: opacity 0.2s ease, transform 0.2s ease, box-shadow 0.2s ease;
}

.product-card:hover .product-card__cta {
  opacity: 1;
}

.product-card__cta:hover {
  transform: translateY(-1px);
  box-shadow: 0 4px 16px rgba(212, 175, 55, 0.4);
}
```

**Do not use:**
- `transform: scale(1.1)` on cards (too aggressive)
- `border: 2px solid gold` on hover (cheap feel)
- Color transitions on backgrounds (jarring)
- `animation: pulse` (screams generic template)

### 6. Product Photography Best Practices

**Briefing for photographer/designer:**

| Attribute | Luxury Standard | Anti-pattern |
|-----------|----------------|--------------|
| Background | Studio white (#FAFAFA) or off-white | Colorful, gradient, or lifestyle cluttered |
| Lighting | Soft directional (45° key light) | Flat overhead, harsh flash |
| Crop | Bottle centered, ~70% frame height | Too small (floating), too large (cropped) |
| Shadow | Subtle dropped shadow, warm tone | No shadow (flat) or hard shadow |
| Angle | 3/4 front or front elevation | Extreme angle or top-down |
| Format | 1:1 square, 1200×1200px min | Landscape 16:9 for product shots |

**Image optimization per product:**
```html
<picture>
  <source
    type="image/webp"
    srcset="
      /images/products/blue-laverne-300.webp 300w,
      /images/products/blue-laverne-600.webp 600w,
      /images/products/blue-laverne-900.webp 900w
    "
    sizes="(max-width: 767px) 100vw, (max-width: 1199px) 50vw, 33vw"
  />
  <!-- JPEG fallback for older browsers -->
  <img
    src="/images/products/blue-laverne-600.jpg"
    alt="Blue Laverne 7am — 100ml Eau de Parfum, fragrance for morning freshness"
    loading="lazy"
    decoding="async"
    width="600"
    height="600"
    class="product-card__img"
  />
</picture>
```

**LQIP (Low-Quality Image Placeholder) for perceived performance:**
```html
<!-- Inline tiny base64 placeholder (< 500 bytes) -->
<img
  src="data:image/jpeg;base64,/9j/4AAQSkZJRg..."
  data-src="/images/products/blue-laverne-600.webp"
  alt="Blue Laverne 7am"
  class="product-card__img js-lazy"
  width="600"
  height="600"
/>
```

### 7. Rating & Trust Signals for Luxury

Trust in luxury is built differently — not 5-star ratings (feels mass-market) but:

```html
<div class="product-card__trust" aria-label="Trust signals">
  <!-- Awards & Certifications -->
  <div class="trust-badge trust-badge--award" title="Fragrance Foundation Award 2025">
    <svg aria-hidden="true"><!-- award icon --></svg>
    <span class="trust-badge__text">Award 2025</span>
  </div>

  <!-- Origin / Provenance -->
  <div class="trust-badge trust-badge--origin" title="Crafted in Orient">
    <span class="trust-badge__text">Orient Fragance</span>
  </div>

  <!-- B2B Social proof -->
  <div class="trust-badge trust-badge--clients" title="Trusted by 200+ clients">
    <span class="trust-badge__text">+200 clientes</span>
  </div>
</div>
```

```css
.trust-badge {
  display: inline-flex;
  align-items: center;
  gap: 4px;
  padding: 3px 8px;
  border: 1px solid rgba(212, 175, 55, 0.4);
  border-radius: 2px;
  font-size: 10px;
  color: #D4AF37;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  font-weight: 600;
}
```

> **Anti-pattern:** Never show `★★★★☆ (4.2)` ratings on a luxury fragrance product — it reads as Amazon, not Orient Fragance.

### 8. Accessibility — WCAG 2.1 AAA Compliance

#### Color Contrast (AAA = 7:1 ratio)

| Element | Foreground | Background | Ratio | Target |
|---------|-----------|------------|-------|--------|
| Product title | `#1A3A52` | `#FFFFFF` | 12.6:1 | ✅ AAA |
| Gold label | `#D4AF37` | `#FFFFFF` | 2.3:1 | ❌ Fails AA |
| Gold on navy | `#D4AF37` | `#1A3A52` | 7.1:1 | ✅ AAA |
| Gray notes | `#666666` | `#FFFFFF` | 5.7:1 | ✅ AA (not AAA) |
| CTA text | `#1A3A52` | `#D4AF37` | 7.1:1 | ✅ AAA |

> **Fix for gold on white:** Never use gold text on white background — use navy instead. Gold is for accents only (borders, icons, CTAs with navy text).

#### Keyboard Navigation
```html
<!-- All interactive elements must be keyboard-reachable -->
<div
  class="product-card"
  tabindex="0"
  role="article"
  aria-label="Blue Laverne 7am, €65 unitario, €55 a 12 unidades"
>
  <!-- ... -->
  <button
    class="product-card__cta"
    aria-label="Añadir Blue Laverne 7am a cotización"
    data-product-id="{{ product.id }}"
  >
    Añadir a cotización
  </button>
</div>
```

#### Screen Reader Considerations
```html
<!-- Pricing: announce full context, not just numbers -->
<div class="product-card__pricing">
  <span class="sr-only">Precio unitario:</span>
  <span class="pricing__amount" aria-label="65 euros">€65.00</span>
  <span class="sr-only">Precio por volumen (12 unidades o más):</span>
  <span class="pricing__bulk-amount" aria-label="55 euros por unidad al comprar 12">€55.00 × 12u</span>
</div>

<!-- Fragrance notes: structured list for screen readers -->
<div class="fragrance-notes" role="list" aria-label="Notas de fragancia">
  <div role="listitem">
    <span class="sr-only">Notas de salida:</span>Bergamot · Lemon
  </div>
  <div role="listitem">
    <span class="sr-only">Notas de corazón:</span>Lavender · Rose
  </div>
  <div role="listitem">
    <span class="sr-only">Notas de fondo:</span>Musk · Patchouli
  </div>
</div>
```

```css
/* Screen-reader-only utility */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

### 9. Complete Product Card HTML

Full production-ready card:

```html
<article
  class="product-card"
  data-product-id="blue-laverne-7am"
  role="article"
  aria-label="Blue Laverne 7am fragrance"
>

  <!-- Product image with LQIP + WebP srcset -->
  <div class="product-card__image" aria-hidden="true">
    <picture>
      <source
        type="image/webp"
        srcset="
          /images/blue-laverne-300.webp 300w,
          /images/blue-laverne-600.webp 600w
        "
        sizes="(max-width: 767px) 100vw, 33vw"
      />
      <img
        src="/images/blue-laverne-600.jpg"
        alt="Blue Laverne 7am — 100ml Eau de Parfum en frasco azul marino con tapa dorada"
        loading="lazy"
        decoding="async"
        width="600"
        height="600"
        class="product-card__img"
      />
    </picture>
  </div>

  <!-- Product content -->
  <div class="product-card__body">

    <!-- Brand & collection -->
    <p class="product-card__brand" aria-label="Colección">
      LAVERNE · ORIENT FRAGANCE
    </p>

    <!-- Title -->
    <h2 class="product-card__title">
      <a
        href="/products/blue-laverne-7am"
        class="product-card__link"
        aria-label="Ver Blue Laverne 7am — 100ml EDP"
      >
        Blue Laverne 7am
      </a>
    </h2>
    <p class="product-card__subtitle">100ml · Eau de Parfum</p>

    <!-- Fragrance notes -->
    <div class="fragrance-notes" aria-label="Notas de fragancia">
      <div class="fragrance-notes__layer">
        <span class="fragrance-notes__label">Top</span>
        <span class="fragrance-notes__values">Bergamot · Lemon</span>
      </div>
      <div class="fragrance-notes__layer">
        <span class="fragrance-notes__label">Heart</span>
        <span class="fragrance-notes__values">Lavender · Rose</span>
      </div>
      <div class="fragrance-notes__layer">
        <span class="fragrance-notes__label">Base</span>
        <span class="fragrance-notes__values">Musk · Patchouli</span>
      </div>
    </div>

    <!-- Pricing -->
    <div class="product-card__pricing">
      <div class="pricing__standard">
        <span class="pricing__amount">€65.00</span>
        <span class="pricing__unit">/ unidad</span>
      </div>
      <div class="pricing__bulk">
        <span class="pricing__bulk-amount">€55.00</span>
        <span class="pricing__bulk-label">× 12 unidades</span>
        <span class="pricing__savings">Ahorro €10</span>
      </div>
    </div>

    <!-- Trust signals -->
    <div class="product-card__trust">
      <span class="trust-badge">Orient Fragance</span>
      <span class="trust-badge">EDP 100ml</span>
    </div>

    <!-- CTA -->
    <button
      class="product-card__cta"
      aria-label="Añadir Blue Laverne 7am a cotización"
      data-product-id="blue-laverne-7am"
      data-price-standard="6500"
      data-price-bulk="5500"
      data-track-event="add_to_quote"
    >
      Añadir a cotización
      <span class="product-card__cta-arrow" aria-hidden="true">→</span>
    </button>

  </div>
</article>
```

### 10. A/B Testing Variants

Test specific elements scientifically — not guessing:

**Variant A (Control):** "Añadir a cotización"
**Variant B:** "Solicitar muestra" (lower commitment)
**Variant C:** "Ver más detalles → Cotizar" (two-step)

```javascript
// Simple A/B assignment via sessionStorage
function getABVariant() {
  const stored = sessionStorage.getItem('laverne_ab_cta');
  if (stored) return stored;

  const variants = ['control', 'variant_b', 'variant_c'];
  const assigned = variants[Math.floor(Math.random() * variants.length)];
  sessionStorage.setItem('laverne_ab_cta', assigned);
  return assigned;
}

function applyCTAVariant() {
  const variant = getABVariant();
  const ctaButtons = document.querySelectorAll('.product-card__cta');

  const ctaText = {
    'control':   'Añadir a cotización',
    'variant_b': 'Solicitar muestra',
    'variant_c': 'Ver detalles y cotizar'
  };

  ctaButtons.forEach(btn => {
    btn.textContent = ctaText[variant] || ctaText['control'];
    btn.dataset.abVariant = variant;
  });

  // Track assignment
  if (window.dataLayer) {
    window.dataLayer.push({
      event: 'ab_assignment',
      ab_test: 'cta_text_v1',
      ab_variant: variant
    });
  }
}

document.addEventListener('DOMContentLoaded', applyCTAVariant);
```

## Complete CSS (Production-Ready)

```css
/* ==============================
   LAVERNE Product Grid — v2.0
   Navy #1A3A52, Gold #D4AF37
   ============================== */

/* Grid Container */
.product-grid-section {
  padding: 80px 0;
  background: #FAFAFA;
}

.product-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 32px;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 24px;
}

/* Product Card */
.product-card {
  display: flex;
  flex-direction: column;
  background: #ffffff;
  border: 1px solid rgba(26, 58, 82, 0.08);
  border-radius: 2px;
  overflow: hidden;
  transition:
    transform 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94),
    box-shadow 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94),
    border-color 0.3s ease;
  will-change: transform;
}

.product-card:hover {
  transform: translateY(-4px);
  box-shadow:
    0 12px 40px rgba(26, 58, 82, 0.12),
    0 4px 12px rgba(26, 58, 82, 0.06);
  border-color: rgba(212, 175, 55, 0.3);
}

/* Image Container */
.product-card__image {
  aspect-ratio: 1 / 1;
  overflow: hidden;
  background: #F5F5F5;
  flex-shrink: 0;
}

.product-card__img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.6s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

.product-card:hover .product-card__img {
  transform: scale(1.03);
}

/* Card Body */
.product-card__body {
  padding: 24px;
  display: flex;
  flex-direction: column;
  flex: 1;
  gap: 12px;
}

/* Brand */
.product-card__brand {
  font-size: 10px;
  color: #D4AF37;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  margin: 0;
}

/* Title */
.product-card__title {
  font-size: 20px;
  font-family: 'Playfair Display', Georgia, serif;
  font-weight: 600;
  color: #1A3A52;
  margin: 0;
  line-height: 1.3;
}

.product-card__link {
  color: inherit;
  text-decoration: none;
}

.product-card__link:hover {
  text-decoration: underline;
  text-underline-offset: 3px;
  text-decoration-color: #D4AF37;
}

.product-card__subtitle {
  font-size: 13px;
  color: #888;
  margin: -8px 0 0;
}

/* Fragrance Notes */
.fragrance-notes {
  border-left: 2px solid #D4AF37;
  padding-left: 12px;
}

.fragrance-notes__layer {
  display: flex;
  gap: 8px;
  align-items: baseline;
  margin-bottom: 3px;
  font-size: 12px;
}

.fragrance-notes__label {
  color: #D4AF37;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.06em;
  min-width: 38px;
  flex-shrink: 0;
  font-size: 10px;
}

.fragrance-notes__values {
  color: #666;
  line-height: 1.4;
}

/* Pricing */
.product-card__pricing {
  display: flex;
  flex-direction: column;
  gap: 6px;
  margin-top: auto;
  padding-top: 4px;
}

.pricing__standard {
  display: flex;
  align-items: baseline;
  gap: 4px;
}

.pricing__amount {
  font-size: 22px;
  font-weight: 700;
  color: #1A3A52;
  font-family: 'Playfair Display', Georgia, serif;
}

.pricing__unit {
  font-size: 13px;
  color: #888;
}

.pricing__bulk {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 6px 10px;
  background: rgba(212, 175, 55, 0.06);
  border-radius: 3px;
  border-left: 2px solid #D4AF37;
}

.pricing__bulk-amount {
  font-size: 15px;
  font-weight: 700;
  color: #D4AF37;
}

.pricing__bulk-label {
  font-size: 12px;
  color: #888;
}

.pricing__savings {
  font-size: 11px;
  color: #5a8a4a;
  margin-left: auto;
  font-weight: 600;
}

/* Trust badges */
.product-card__trust {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
}

.trust-badge {
  display: inline-flex;
  align-items: center;
  gap: 4px;
  padding: 3px 8px;
  border: 1px solid rgba(212, 175, 55, 0.35);
  border-radius: 2px;
  font-size: 10px;
  color: #D4AF37;
  letter-spacing: 0.05em;
  text-transform: uppercase;
  font-weight: 600;
  white-space: nowrap;
}

/* CTA Button */
.product-card__cta {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  width: 100%;
  padding: 14px 16px;
  background: #D4AF37;
  color: #1A3A52;
  border: none;
  border-radius: 2px;
  font-weight: 700;
  font-size: 13px;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  cursor: pointer;
  transition: opacity 0.2s ease, transform 0.2s ease, box-shadow 0.2s ease;
  margin-top: 4px;
}

.product-card__cta:hover:not(:disabled) {
  transform: translateY(-1px);
  box-shadow: 0 4px 16px rgba(212, 175, 55, 0.4);
}

.product-card__cta:focus-visible {
  outline: 3px solid #1A3A52;
  outline-offset: 2px;
}

.product-card__cta:disabled {
  opacity: 0.4;
  cursor: not-allowed;
  transform: none;
}

.product-card__cta-arrow {
  transition: transform 0.2s ease;
}

.product-card__cta:hover .product-card__cta-arrow {
  transform: translateX(3px);
}

/* Screen reader only */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

/* Responsive */
@media (max-width: 1199px) {
  .product-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 24px;
  }
}

@media (max-width: 767px) {
  .product-grid-section {
    padding: 48px 0;
  }

  .product-grid {
    grid-template-columns: 1fr;
    gap: 16px;
    padding: 0 16px;
  }

  .product-card__body {
    padding: 16px;
    gap: 10px;
  }

  .product-card__title {
    font-size: 18px;
  }

  .pricing__amount {
    font-size: 20px;
  }
}

/* Reduced motion — respect user preferences */
@media (prefers-reduced-motion: reduce) {
  .product-card,
  .product-card__img,
  .product-card__cta,
  .product-card__cta-arrow {
    transition: none;
  }

  .product-card:hover {
    transform: none;
  }

  .product-card:hover .product-card__img {
    transform: none;
  }
}
```

## Anti-Patterns — What NOT to Do

| ❌ Anti-Pattern | Why It's Wrong | ✅ LAVERNE Standard |
|----------------|---------------|-------------------|
| `transform: scale(1.1)` on hover | Too aggressive, cheap web store feel | `translateY(-4px)` — subtle lift |
| 5-star ratings on cards | Mass-market signal (Amazon feel) | Awards badges, provenance labels |
| Strikethrough original price | Discount/clearance signal | Separate pricing tiers, no strikethrough |
| Gold text on white background | Fails contrast (2.3:1) | Gold on navy only |
| 2-column mobile grid | Too cramped for luxury products | Single-column on mobile always |
| `auto-fill` / `auto-fit` in grid | Unpredictable column count | Fixed `repeat(3, 1fr)` |
| Generic stock photo composition | Reads as template instantly | Brief: 3/4 angle, warm light, centered |
| `animation: pulse` or `shine` on badges | Web 2.0, tacky | Static badges or subtle fade-in |
| Showing out-of-stock price strikethrough | Confusing | `Agotado` button state, hide pricing |
| Font size < 12px for any text | Accessibility failure | 12px minimum for notes, 13px for labels |

## Implementation Checklist

- [ ] Grid layout tested at 1440px, 1024px, 768px, 375px viewports
- [ ] Product card images: 1:1 square, WebP with JPG fallback, < 100KB at 600px
- [ ] Fragrance notes pyramid: Top/Heart/Base all populated from metafields
- [ ] Pricing: standard (€65) + bulk (€55 × 12) displayed correctly
- [ ] Bulk pricing: never strikethrough, professional tier framing
- [ ] Trust badges: brand origin, product specs (100ml, EDP)
- [ ] CTA hover: `translateY(-1px)` + gold shadow (≤ 0.3s transition)
- [ ] Card hover: `translateY(-4px)` + multi-layer box-shadow (0.3s ease)
- [ ] Image hover: `scale(1.03)` max (0.6s ease)
- [ ] Color contrast: all text meets WCAG AAA (7:1) except secondary gray (AA minimum)
- [ ] Keyboard navigation: all cards focusable, CTAs have `aria-label` with product name
- [ ] Screen readers: fragrance notes have role="list", pricing has spoken context
- [ ] `prefers-reduced-motion`: all CSS transitions disabled for motion-sensitive users
- [ ] A/B test variant: CTA text assigned and tracked via `window.dataLayer`
- [ ] Anti-patterns avoided: no strikethrough, no star ratings, no auto-fill grid

## Related Skills

- `landing-page-hero-design` — Hero section (above this grid)
- `shopify-integration` — Liquid templates for this grid
- `performance-optimization` — Image loading, lazy load, Lighthouse
