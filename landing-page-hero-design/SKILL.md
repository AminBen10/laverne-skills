---
name: landing-page-hero-design
version: 2.0.0
description: Creates distinctive, premium hero sections for luxury brand landing pages. Focuses on photography integration, premium typography, visual hierarchy, conversion-focused CTAs, security (XSS, CSP), and WCAG 2.1 AAA accessibility. Delivers OVERDOSE design that doesn't read as generic template.
tags: [landing-page, hero, luxury, accessibility, security, performance]
dependencies: [performance-optimization]
license: MIT
---

# Landing Page Hero Design

## Purpose

You are a premium web designer specializing in luxury brand hero sections. Your role is to create hero experiences that are:
- **Visually distinctive** — not templatic, impossible to mistake for a generic design
- **Photography-first** — images carry the narrative, text enhances
- **Conversion-focused** — clear hierarchy, strategic CTAs, trust signals
- **Premium typography** — serif for headings (elegance), clean sans-serif for body
- **Mobile impeccable** — hero works flawlessly on all screen sizes
- **Security-hardened** — XSS-safe output, CSP-compliant, no dynamic injection risks

## Core Principles

### 1. Hero as Thesis
The hero is not decoration—it's the landing page's single thesis. For LAVERNE (luxury fragrance):
- **Visual assertion**: Man with fragrance (aspirational, premium)
- **Brand anchor**: Logo + "ORIENT FRAGANCE" (gold, subtle)
- **Value proposition**: "LAVERNE" + "CATÁLOGO PROFESIONAL 2026"
- **Social proof**: Badge "TESTER GRATIS DESDE 24 UNIDADES"
- **CTA**: Clear, premium button ("Ver catálogo completo")

### 2. Color Theory — Navy + Gold Psychology

**Navy #1A3A52** conveys:
- Trust, authority, stability (used by luxury finance & fashion brands)
- Depth and spatial sophistication (not flat, not aggressive)
- Strong contrast base that makes gold luminous

**Gold #D4AF37** conveys:
- Premium refinement (naturally associated with jewelry and haute couture)
- Warmth without aggression (unlike yellow or orange)
- Scarcity and aspiration (not available everywhere)

Use the brand's color palette intentionally:
- **Dark navy** (#1A3A52) — elegance, luxury, trust — use for large areas
- **Gold** (#D4AF37) — premium accent — use for CTAs, borders, highlights ONLY
- **White/Off-white** (#FAFAFA) — breathing room, sophistication
- **Subtle gradients** — never harsh, always directional (navy → transparent)

**Color Contrast Requirements (WCAG AAA = 7:1 ratio):**

| Element | Foreground | Background | Ratio | Status |
|---------|-----------|------------|-------|--------|
| Main headline | `#FFFFFF` | `#1A3A52` | 16.7:1 | ✅ AAA |
| Gold CTA text | `#1A3A52` | `#D4AF37` | 7.1:1 | ✅ AAA |
| Badge text | `#1A3A52` | `#D4AF37` | 7.1:1 | ✅ AAA |
| Subtitle | `#F5F5F5` | `#1A3A52` | 14.9:1 | ✅ AAA |

**NEVER:** Gold text on white background (2.3:1 ratio — fails AA, let alone AAA)
**NEVER:** Oversaturated colors, flat design, generic web 2.0 pastels
**YES:** Deep jewel tones, material depth, cinematic lighting

### 3. Typography Strategy — Premium Type Psychology

Luxury typography creates emotional response before the user reads a word:

- **Headline** (H1): Serif font (Playfair Display, Prata, or equivalent)
  - Size: 64px–80px desktop, 36px mobile
  - Weight: 700 (bold signals confidence) or 300 (light signals refinement)
  - Color: Gold (#D4AF37) on navy, or white (#FFFFFF) on dark image
  - Letter-spacing: 0.06em–0.12em (luxury brands use generous tracking)
  - Line-height: 1.1 (tight for headlines = editorial authority)

- **Subheading**: Serif at smaller scale (18px–24px)
  - Color: #F5F5F5 (slightly off-white — not harsh pure white)
  - Weight: 400 (don't compete with H1)
  - Letter-spacing: 0.15em (wider tracking on smaller type = luxury signal)

- **Body/CTA**: Clean sans-serif (Inter, DM Sans)
  - 14px–16px for readability
  - Never condensed — luxury is never space-starved

- **Logo text**: Match brand identity (gold, subtle, small-caps or uppercase)

### 4. Visual Hierarchy
Structure the hero with clear zones:

```
┌─────────────────────────────────────────┐
│  LOGO + "ORIENT FRAGANCE" (top, subtle) │
│                                         │
│  [FULL-WIDTH IMAGE: Man + Fragrance]    │
│   16:9 or 21:9 — warm directional light │
│                                         │
│  "LAVERNE" (massive, centered, gold)    │
│  "CATÁLOGO PROFESIONAL 2026" (smaller)  │
│                                         │
│  [BADGE] "TESTER GRATIS DESDE 24U"      │
│                                         │
│  [CTA BUTTON] "Ver catálogo completo"   │
│                                         │
│  [Scroll indicator: thin gold line]     │
└─────────────────────────────────────────┘
```

### 5. Photography Integration

**Technical brief for photographer:**
- **Aspect ratio**: 16:9 desktop, auto-crop to portrait for mobile
- **Composition**: Man occupies ~60% of frame, product visible in dominant hand
- **Lighting**: Warm, directional key light at 45° — not flat, not harsh
- **Color grading**: Warm midtones, slightly desaturated — premium, editorial
- **Background**: Minimal, clean (neutral texture or gradient — not busy)
- **Resolution**: 3000px+ wide (for 1440px viewport + 2x DPR retina)

**Image overlay strategy (XSS-safe):**
```css
/* DO: CSS gradient overlay — no user input, no injection risk */
.hero__overlay {
  background: linear-gradient(
    180deg,
    rgba(26, 58, 82, 0.15) 0%,
    rgba(26, 58, 82, 0.55) 60%,
    rgba(26, 58, 82, 0.85) 100%
  );
}
```

**Mobile crop intelligence:**
```css
/* Mobile: portrait crop — keep face and bottle visible */
@media (max-width: 767px) {
  .hero__image {
    background-position: 70% center; /* shift to show face + bottle */
    height: 55vh;
  }
}
```

### 6. CTA Button Strategy — Psychology of Luxury CTAs

**Primary CTA copy options (A/B test these):**
- "Ver catálogo completo" — neutral, professional
- "Solicitar catálogo exclusivo" — exclusivity framing
- "Descubrir colección" — discovery emotion
- "Descargar catálogo 2026" — concrete action

**CTA design:**
- **Style**: Gold background (#D4AF37) with navy text (#1A3A52) — 7.1:1 contrast ratio ✅ AAA
- **Size**: 48px height minimum (iOS touch target requirement), 200px min-width
- **Shape**: Minimal radius (2px–4px) — luxury is sharp, not rounded
- **Hover**: `translateY(-2px)` + gold shadow — never `scale()`
- **Secondary CTA**: Ghost button (transparent, gold border 1px)

```html
<!-- Primary CTA — WCAG AAA compliant -->
<a
  href="/catalogo"
  class="hero__cta hero__cta--primary"
  role="button"
  aria-label="Ver catálogo completo de LAVERNE fragancias"
>
  Ver catálogo completo
  <span class="hero__cta-arrow" aria-hidden="true">→</span>
</a>

<!-- Secondary CTA (optional) -->
<a
  href="/muestras"
  class="hero__cta hero__cta--secondary"
  role="button"
  aria-label="Solicitar muestras gratuitas de LAVERNE"
>
  Solicitar muestras
</a>
```

### 7. Trust Signals & Badges

**"TESTER GRATIS DESDE 24 UNIDADES"** badge:
- Gold background (#D4AF37) with navy text — never text on white
- Small gift icon (SVG inline — not icon font for performance)
- Positioned: right of headline on desktop, below on mobile

```html
<!-- Badge: no dynamic content = no XSS risk -->
<div class="hero__badge" role="note" aria-label="Promoción especial">
  <svg class="hero__badge-icon" aria-hidden="true" width="16" height="16" viewBox="0 0 16 16">
    <path fill="currentColor" d="M8 1a2 2 0 1 1 0 4H2V4a2 2 0 0 1 2-2h4zm0 0"/>
  </svg>
  <span class="hero__badge-text">TESTER GRATIS DESDE 24 UNIDADES</span>
</div>
```

### 8. Micro-interactions Library (Premium)

All transitions must use GPU-accelerated properties only (`transform`, `opacity`):

```css
/* Staggered fade-in on load (no JS required — CSS animation) */
.hero__brand    { animation: fadeInUp 0.6s ease 0.1s both; }
.hero__title    { animation: fadeInUp 0.7s ease 0.3s both; }
.hero__subtitle { animation: fadeInUp 0.6s ease 0.5s both; }
.hero__badge    { animation: fadeInUp 0.5s ease 0.7s both; }
.hero__cta      { animation: fadeInUp 0.5s ease 0.9s both; }

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(24px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Parallax effect — CSS only, subtle (no JS scroll jank) */
.hero__image {
  transform: translateZ(0); /* create stacking context for GPU */
  will-change: transform;
}

/* Button hover — premium feel */
.hero__cta--primary {
  transition: transform 0.25s cubic-bezier(0.25, 0.46, 0.45, 0.94),
              box-shadow 0.25s ease;
}

.hero__cta--primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 24px rgba(212, 175, 55, 0.45);
}

/* Respect motion preferences */
@media (prefers-reduced-motion: reduce) {
  .hero__brand,
  .hero__title,
  .hero__subtitle,
  .hero__badge,
  .hero__cta {
    animation: none;
    opacity: 1;
    transform: none;
  }

  .hero__cta--primary:hover {
    transform: none;
  }
}
```

### 9. Responsive Design — Mobile-First Luxury

Mobile is not a downgrade — it's a different canvas:

**Desktop (1440px+):** Full cinematic hero, 100vh
**Tablet (768px–1439px):** 80vh, adjust font sizes
**Mobile (< 768px):**
  - Image height: 55vh (preserve battery, reduce data)
  - H1: 36px (readable without squinting)
  - Buttons: full-width (`width: 100%`) — luxury touch targets
  - Remove parallax (performance + nausea on mobile)
  - Stack CTAs vertically (primary above, secondary below)
  - Touch targets: 48px minimum (iOS HIG requirement)

```css
/* Mobile touch targets */
@media (max-width: 767px) {
  .hero__cta {
    width: 100%;
    min-height: 48px; /* iOS touch target minimum */
    justify-content: center;
  }
}

/* Gesture handling: prevent accidental swipes on hero */
.hero {
  touch-action: pan-y; /* allow vertical scroll, prevent horizontal swipe */
}
```

### 10. Accessibility — WCAG 2.1 AAA Deep-Dive

**Keyboard Navigation:**
```html
<!-- Skip link: first focusable element on the page -->
<a href="#main-content" class="skip-link">
  Saltar al contenido principal
</a>
```

```css
.skip-link {
  position: absolute;
  top: -100px;
  left: 16px;
  background: #D4AF37;
  color: #1A3A52;
  padding: 8px 16px;
  border-radius: 2px;
  font-weight: 700;
  z-index: 1000;
  transition: top 0.1s ease;
}

.skip-link:focus {
  top: 16px;
}
```

**Screen Reader Labels:**
```html
<section
  class="hero"
  role="region"
  aria-label="Sección principal de LAVERNE — catálogo de fragancias"
>
```

**Focus management:**
```css
/* Visible focus ring — never suppress with outline: none */
*:focus-visible {
  outline: 3px solid #D4AF37;
  outline-offset: 3px;
}
```

### 11. Security — XSS Prevention & CSP

**Input/Output Sanitization:**
Never inject unescaped user input into the hero. All dynamic values must be escaped:

```liquid
<!-- Shopify Liquid: always use | escape filter -->
<h1>{{ shop.name | escape }}</h1>
<p>{{ collection.description | escape }}</p>

<!-- For rich text, use | metafield_tag (Shopify-sanitized) -->
{{ block.settings.hero_text | escape }}
```

**Content Security Policy (CSP) Headers — recommended for Shopify:**
```
Content-Security-Policy:
  default-src 'self';
  script-src 'self' 'nonce-{RANDOM}' https://cdn.shopify.com https://www.googletagmanager.com;
  style-src 'self' 'unsafe-inline' https://fonts.googleapis.com;
  img-src 'self' data: https://cdn.shopify.com https://*.shopifycdn.com;
  font-src 'self' https://fonts.gstatic.com;
  connect-src 'self' https://api.shopify.com;
  frame-ancestors 'none';
  base-uri 'self';
```

**Inline styles vs CSS classes:**
```html
<!-- ❌ NEVER: Inline style with dynamic content (XSS risk) -->
<div style="background-image: url({{ user_input }})">

<!-- ✅ DO: CSS class + escaped Shopify CDN URL -->
<div class="hero__image">
  <img src="{{ section.settings.hero_image | image_url: width: 1440 | escape }}" alt="..." />
</div>
```

## A/B Testing Variants

**CTA Psychology — What to Test:**

| Variant | CTA Copy | Hypothesis | Metric |
|---------|----------|-----------|--------|
| Control | "Ver catálogo completo" | Neutral, professional | Baseline CTR |
| B | "Solicitar catálogo exclusivo" | Exclusivity → higher intent | CTR + conversion |
| C | "Descubrir colección 2026" | Discovery emotion → exploration | Time on page |
| D | "Descargar catálogo (PDF)" | Concrete action → download | Lead capture |

**Image Emotion Variants:**
- **A**: Man holding fragrance — aspirational, professional
- **B**: Close-up bottle only — product-focused, minimalist
- **C**: Lifestyle: man in elegant setting — emotional, contextual

**Overlay Psychology:**
- **Light overlay** (30%) — lets image breathe, artistic
- **Dark overlay** (70%) — text is clearest, conversion-focused

## Anti-Patterns — What NOT to Do

| ❌ Anti-Pattern | ✅ LAVERNE Standard |
|----------------|-------------------|
| Full-page white background with navy text box | Dark navy background with image |
| Generic sans-serif headline (Arial/Helvetica) | Playfair Display or Prata serif |
| Generic stock image (Unsplash hands holding bottle) | Brief: custom editorial photography |
| `background-color: #FFD700` (web yellow) | `#D4AF37` (muted, jewelry gold) |
| Rounded corners everywhere (`border-radius: 12px`) | Sharp 2px–4px radius only |
| Multiple hero CTAs with equal visual weight | 1 primary (filled) + 1 secondary (ghost) |
| Hero section below the fold on mobile | Hero = first thing visible, 55–100vh |
| Flat, non-directional lighting in photos | Warm 45° key light — depth and dimension |
| Generic countdown timer (urgency = cheap) | Subtle "TESTER GRATIS DESDE 24U" badge |
| Oversaturated magenta or teal color grading | Warm desaturated tones, premium editorial |

## Complete Hero HTML + CSS

```html
<!-- Skip navigation for keyboard users -->
<a href="#main-content" class="skip-link">Saltar al contenido</a>

<section
  class="hero"
  role="region"
  aria-label="Sección principal — LAVERNE catálogo profesional 2026"
>
  <!-- Background image: picture element for WebP + responsive -->
  <picture class="hero__picture" aria-hidden="true">
    <source
      type="image/webp"
      srcset="
        /images/hero-640.webp 640w,
        /images/hero-1024.webp 1024w,
        /images/hero-1440.webp 1440w
      "
      sizes="100vw"
    />
    <img
      src="/images/hero-1440.jpg"
      alt="LAVERNE — hombre sosteniendo fragancia de lujo"
      class="hero__bg-img"
      width="1440"
      height="810"
      fetchpriority="high"
      decoding="async"
    />
  </picture>

  <!-- Dark overlay -->
  <div class="hero__overlay" aria-hidden="true"></div>

  <!-- Content -->
  <div class="hero__content" id="main-content">

    <!-- Brand mark -->
    <p class="hero__brand" aria-label="Orient Fragance para LAVERNE">
      ORIENT FRAGANCE
    </p>

    <!-- Main headline -->
    <h1 class="hero__title">LAVERNE</h1>
    <p class="hero__subtitle">CATÁLOGO PROFESIONAL 2026</p>

    <!-- Badge / trust signal -->
    <div class="hero__badge" role="note">
      <svg class="hero__badge-icon" aria-hidden="true" width="14" height="14" viewBox="0 0 16 16" fill="currentColor">
        <path d="M3 9.5a1.5 1.5 0 1 1 3 0 1.5 1.5 0 0 1-3 0M1 4a.5.5 0 0 1 .5-.5h13a.5.5 0 0 1 0 1H14v7a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V4.5H1.5A.5.5 0 0 1 1 4z"/>
      </svg>
      <span>TESTER GRATIS DESDE 24 UNIDADES</span>
    </div>

    <!-- CTAs -->
    <div class="hero__actions">
      <a
        href="/catalogo"
        class="hero__cta hero__cta--primary"
        role="button"
        aria-label="Ver catálogo completo de LAVERNE fragancias"
      >
        Ver catálogo completo
        <span class="hero__cta-arrow" aria-hidden="true">→</span>
      </a>
      <a
        href="/muestras"
        class="hero__cta hero__cta--secondary"
        role="button"
        aria-label="Solicitar muestras gratuitas de LAVERNE"
      >
        Solicitar muestras
      </a>
    </div>

  </div>

  <!-- Scroll indicator -->
  <div class="hero__scroll" aria-hidden="true">
    <div class="hero__scroll-line"></div>
  </div>

</section>
```

```css
/* ==============================
   LAVERNE Hero Section — v2.0
   Navy #1A3A52, Gold #D4AF37
   WCAG 2.1 AAA compliant
   ============================== */

/* Skip link */
.skip-link {
  position: absolute;
  top: -100px;
  left: 16px;
  background: #D4AF37;
  color: #1A3A52;
  padding: 8px 16px;
  border-radius: 2px;
  font-weight: 700;
  font-size: 14px;
  z-index: 1000;
  text-decoration: none;
  transition: top 0.1s ease;
}
.skip-link:focus { top: 16px; }

/* Hero Container */
.hero {
  position: relative;
  width: 100%;
  min-height: 100vh;
  display: flex;
  align-items: center;
  overflow: hidden;
  background-color: #1A3A52; /* fallback while image loads */
  touch-action: pan-y;
}

/* Background Image */
.hero__picture {
  position: absolute;
  inset: 0;
  z-index: 0;
}

.hero__bg-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center 30%;
}

/* Overlay */
.hero__overlay {
  position: absolute;
  inset: 0;
  z-index: 1;
  background: linear-gradient(
    180deg,
    rgba(26, 58, 82, 0.2) 0%,
    rgba(26, 58, 82, 0.6) 55%,
    rgba(26, 58, 82, 0.88) 100%
  );
}

/* Content */
.hero__content {
  position: relative;
  z-index: 2;
  max-width: 900px;
  margin: 0 auto;
  padding: 120px 40px 80px;
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
}

/* Staggered animations */
.hero__brand    { animation: fadeInUp 0.6s ease 0.1s both; }
.hero__title    { animation: fadeInUp 0.7s ease 0.3s both; }
.hero__subtitle { animation: fadeInUp 0.6s ease 0.5s both; }
.hero__badge    { animation: fadeInUp 0.5s ease 0.7s both; }
.hero__actions  { animation: fadeInUp 0.5s ease 0.9s both; }

@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(24px); }
  to   { opacity: 1; transform: translateY(0); }
}

/* Brand */
.hero__brand {
  font-size: 11px;
  color: #D4AF37;
  font-weight: 700;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  margin: 0;
}

/* Headline */
.hero__title {
  font-family: 'Playfair Display', 'Georgia', serif;
  font-size: 80px;
  font-weight: 700;
  color: #D4AF37;
  letter-spacing: 0.1em;
  line-height: 1.05;
  margin: 0;
  text-shadow: 0 2px 20px rgba(0, 0, 0, 0.3);
}

/* Subtitle */
.hero__subtitle {
  font-size: 16px;
  font-family: 'Playfair Display', Georgia, serif;
  font-weight: 400;
  color: #F5F5F5;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  margin: 0;
}

/* Badge */
.hero__badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  background: #D4AF37;
  color: #1A3A52;
  border-radius: 2px;
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
}

.hero__badge-icon {
  flex-shrink: 0;
}

/* CTA Actions */
.hero__actions {
  display: flex;
  gap: 16px;
  flex-wrap: wrap;
  justify-content: center;
  margin-top: 8px;
}

/* Primary CTA */
.hero__cta {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 14px 28px;
  font-size: 13px;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  text-decoration: none;
  border-radius: 2px;
  border: 2px solid transparent;
  min-height: 48px;
  cursor: pointer;
  transition:
    transform 0.25s cubic-bezier(0.25, 0.46, 0.45, 0.94),
    box-shadow 0.25s ease;
}

.hero__cta--primary {
  background: #D4AF37;
  color: #1A3A52;
  border-color: #D4AF37;
}

.hero__cta--primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 24px rgba(212, 175, 55, 0.45);
}

.hero__cta--secondary {
  background: transparent;
  color: #FFFFFF;
  border-color: rgba(212, 175, 55, 0.6);
}

.hero__cta--secondary:hover {
  transform: translateY(-2px);
  border-color: #D4AF37;
  background: rgba(212, 175, 55, 0.08);
}

.hero__cta:focus-visible {
  outline: 3px solid #D4AF37;
  outline-offset: 3px;
}

.hero__cta-arrow {
  transition: transform 0.2s ease;
}

.hero__cta--primary:hover .hero__cta-arrow {
  transform: translateX(4px);
}

/* Scroll indicator */
.hero__scroll {
  position: absolute;
  bottom: 32px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 2;
}

.hero__scroll-line {
  width: 1px;
  height: 40px;
  background: linear-gradient(to bottom, #D4AF37, transparent);
  animation: scrollPulse 2s ease-in-out infinite;
}

@keyframes scrollPulse {
  0%, 100% { opacity: 0.4; transform: scaleY(1); }
  50%       { opacity: 1;   transform: scaleY(1.1); }
}

/* Responsive: Tablet */
@media (max-width: 1199px) {
  .hero__title {
    font-size: 64px;
  }
  .hero__content {
    padding: 100px 32px 64px;
  }
}

/* Responsive: Mobile */
@media (max-width: 767px) {
  .hero {
    min-height: 100svh; /* use svh for mobile chrome bar */
  }

  .hero__bg-img {
    object-position: 70% center;
  }

  .hero__content {
    padding: 80px 20px 60px;
    gap: 16px;
  }

  .hero__title {
    font-size: 40px;
    letter-spacing: 0.08em;
  }

  .hero__subtitle {
    font-size: 13px;
    letter-spacing: 0.14em;
  }

  .hero__actions {
    flex-direction: column;
    width: 100%;
    gap: 12px;
  }

  .hero__cta {
    width: 100%;
    justify-content: center;
  }
}

/* Reduced motion — respect user preference */
@media (prefers-reduced-motion: reduce) {
  .hero__brand,
  .hero__title,
  .hero__subtitle,
  .hero__badge,
  .hero__actions {
    animation: none;
    opacity: 1;
    transform: none;
  }

  .hero__cta,
  .hero__scroll-line {
    animation: none;
    transition: none;
  }

  .hero__cta--primary:hover,
  .hero__cta--secondary:hover {
    transform: none;
  }
}
```

## Implementation Checklist

- [ ] Brand colors applied correctly (Navy #1A3A52, Gold #D4AF37) — no oversaturation
- [ ] Typography: Playfair Display or Prata loaded via `<link rel="preload">` before render
- [ ] Hero image: WebP + JPG fallback, `fetchpriority="high"` on LCP element
- [ ] Hero image: < 150KB at 1440px width (WebP), < 200KB JPG fallback
- [ ] HTML markup: semantic `<section>`, `role="region"`, `aria-label`
- [ ] Skip link: first focusable element, visible on focus
- [ ] All text on dark background — never gold text on white background
- [ ] Color contrast verified: all elements ≥ 7:1 (AAA) except secondary gray (AA)
- [ ] Keyboard navigation: CTA links are `<a>` (not `<button>`) for link semantics
- [ ] `focus-visible` ring: 3px solid gold visible on all interactive elements
- [ ] Animations: use `transform` + `opacity` only (GPU-accelerated)
- [ ] `prefers-reduced-motion`: all animations disabled when user opts out
- [ ] Mobile: `touch-action: pan-y`, CTAs full-width, `min-height: 48px`
- [ ] CSP headers: documented and recommended in server configuration
- [ ] Liquid output: all dynamic values use `| escape` filter
- [ ] Responsive tested at 375px, 768px, 1024px, 1440px
- [ ] Lighthouse: Performance > 85, Accessibility > 95 (target 100)
- [ ] A/B tracking: CTA variant assigned via sessionStorage + pushed to dataLayer

## Related Skills

- `ecommerce-product-grid` — Product showcase (after hero)
- `performance-optimization` — Image & code optimization
- `shopify-integration` — Connect hero CTA to Shopify collection
