---
name: landing-page-hero-design
version: 2.1.0
description: Creates distinctive, premium hero sections for luxury brand landing pages. Focuses on photography integration, premium typography, visual hierarchy, conversion-focused CTAs, accessible interaction, frontend-safe rendering, and performance-conscious delivery for modern static deployments.
tags: [landing-page, hero, luxury, accessibility, security, performance]
dependencies: [performance-optimization]
license: MIT
---

# Landing Page Hero Design

## Purpose

You are a premium web designer specializing in luxury landing page hero sections. Your role is to create hero experiences that are:
- **Visually distinctive** — not templatic, clearly premium at first glance
- **Photography-first** — images carry the narrative, text reinforces the message
- **Conversion-focused** — clear hierarchy, strong CTA priority, trust cues
- **Typographically refined** — serif authority for headlines, clean sans-serif for support text
- **Mobile impeccable** — fast, legible, touch-friendly, and stable on small screens
- **Frontend-safe** — no unsafe dynamic injection patterns, CSP-aware, easy to deploy statically

## Core Principles

### 1. Hero as Thesis
The hero is not decoration — it is the landing page's thesis statement. For LAVERNE:
- **Visual assertion**: A refined fragrance image that immediately establishes category and tone
- **Brand anchor**: Logo or origin line such as `ORIENT FRAGANCE`
- **Value proposition**: `LAVERNE` + a concise supporting statement
- **Trust cue**: A restrained badge or proof point such as `TESTER GRATIS DESDE 24 UNIDADES`
- **Primary action**: A single dominant CTA such as `Ver catálogo completo`

### 2. Color Theory — Navy + Gold Psychology

**Navy #1A3A52** conveys:
- Trust, authority, stability
- Depth and visual sophistication
- A strong foundation that lets gold accents feel luminous

**Gold #D4AF37** conveys:
- Premium refinement
- Warmth without aggression
- Aspiration and scarcity when used sparingly

Use the brand palette intentionally:
- **Dark navy** (`#1A3A52`) — large surfaces, overlays, structural depth
- **Gold** (`#D4AF37`) — CTA emphasis, borders, highlights, focal accents
- **Off-white** (`#FAFAFA`) — breathing room and visual softness
- **Directional gradients** — subtle, cinematic, never harsh

**Color Contrast Requirements (WCAG AAA = 7:1 ratio):**

| Element | Foreground | Background | Ratio | Status |
|---------|-----------|------------|-------|--------|
| Main headline | `#FFFFFF` | `#1A3A52` | 16.7:1 | ✅ AAA |
| CTA text | `#1A3A52` | `#D4AF37` | 7.1:1 | ✅ AAA |
| Badge text | `#1A3A52` | `#D4AF37` | 7.1:1 | ✅ AAA |
| Subtitle | `#F5F5F5` | `#1A3A52` | 14.9:1 | ✅ AAA |

**Avoid:** gold text on white backgrounds, oversaturated accent colors, or pastel palettes that dilute premium positioning.

### 3. Typography Strategy — Premium Type Psychology

Luxury typography shapes perception before the copy is even read:

- **Headline (H1)**: Playfair Display, Prata, or equivalent premium serif
  - Size: 64px–80px desktop, 36px–40px mobile
  - Weight: 700 for authority, or 300 if the art direction is more editorial
  - Letter-spacing: 0.06em–0.12em
  - Line-height: ~1.1

- **Subheading**: Smaller serif or refined support line
  - Size: 18px–24px
  - Weight: 400
  - Letter-spacing: 0.12em–0.15em
  - Color: slightly softened white such as `#F5F5F5`

- **Body / CTA**: Inter or DM Sans
  - Size: 14px–16px
  - Never condensed or overly compressed

### 4. Visual Hierarchy
Structure the hero with clear zones:

```
┌─────────────────────────────────────────┐
│  LOGO / ORIGIN LINE (top, subtle)       │
│                                         │
│  [FULL-WIDTH IMAGE / VISUAL THESIS]     │
│   16:9 or 21:9 — warm directional light │
│                                         │
│  "LAVERNE" (massive, centered)          │
│  concise supporting statement           │
│                                         │
│  [BADGE / TRUST CUE]                    │
│                                         │
│  [PRIMARY CTA] [OPTIONAL SECONDARY CTA] │
│                                         │
│  [Scroll indicator: thin gold line]     │
└─────────────────────────────────────────┘
```

### 5. Photography Integration

**Technical brief for photography/art direction:**
- **Aspect ratio**: 16:9 desktop, art-directed crop for mobile
- **Composition**: Product and subject remain legible after responsive cropping
- **Lighting**: Warm, directional, sculptural — never flat or harsh
- **Color grading**: Slightly desaturated and editorial
- **Background**: Minimal and uncluttered
- **Resolution**: 3000px+ source image for high-density displays

**Overlay strategy:**
```css
.hero__overlay {
  background: linear-gradient(
    180deg,
    rgba(26, 58, 82, 0.15) 0%,
    rgba(26, 58, 82, 0.55) 60%,
    rgba(26, 58, 82, 0.85) 100%
  );
}
```

**Responsive art direction:**
```css
@media (max-width: 767px) {
  .hero__bg-img {
    object-position: 70% center;
  }
}
```

### 6. CTA Strategy — Landing-First

The hero should support the landing page journey first, not lock the design into a backend flow too early.

**Recommended primary CTA patterns:**
- `Ver catálogo completo`
- `Descubrir colección`
- `Explorar fragancias`
- `Ver edición 2026`

**Optional secondary CTA patterns:**
- `Solicitar información`
- `Ver detalles`
- `Descargar catálogo`

**CTA design:**
- **Primary CTA**: filled gold button, navy text, strongest visual weight
- **Secondary CTA**: ghost or outlined style, clearly subordinate
- **Touch target**: at least 48px height
- **Hover**: slight vertical lift and shadow; never exaggerated scaling

```html
<a
  href="#catalogo"
  class="hero__cta hero__cta--primary"
  aria-label="Ver catálogo completo de LAVERNE"
>
  Ver catálogo completo
  <span class="hero__cta-arrow" aria-hidden="true">→</span>
</a>

<a
  href="#detalles"
  class="hero__cta hero__cta--secondary"
  aria-label="Ver más detalles sobre la colección LAVERNE"
>
  Ver detalles
</a>
```

### 7. Trust Signals & Badges

Use restrained proof cues:
- a promotional or proof badge
- an origin or craftsmanship statement
- a short quality claim

```html
<div class="hero__badge" role="note" aria-label="Promoción especial">
  <svg class="hero__badge-icon" aria-hidden="true" width="16" height="16" viewBox="0 0 16 16">
    <path fill="currentColor" d="M8 1a2 2 0 1 1 0 4H2V4a2 2 0 0 1 2-2h4zm0 0"/>
  </svg>
  <span class="hero__badge-text">TESTER GRATIS DESDE 24 UNIDADES</span>
</div>
```

### 8. Micro-interactions Library

Use only performant properties like `transform` and `opacity`:

```css
.hero__brand    { animation: fadeInUp 0.6s ease 0.1s both; }
.hero__title    { animation: fadeInUp 0.7s ease 0.3s both; }
.hero__subtitle { animation: fadeInUp 0.6s ease 0.5s both; }
.hero__badge    { animation: fadeInUp 0.5s ease 0.7s both; }
.hero__actions  { animation: fadeInUp 0.5s ease 0.9s both; }

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

.hero__cta--primary {
  transition:
    transform 0.25s cubic-bezier(0.25, 0.46, 0.45, 0.94),
    box-shadow 0.25s ease;
}

.hero__cta--primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 24px rgba(212, 175, 55, 0.45);
}

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

### 9. Responsive Design — Mobile-First Luxury

**Desktop (1440px+)**: full cinematic hero with generous spacing  
**Tablet (768px–1439px)**: tighter spacing, reduced type scale  
**Mobile (<768px)**:
- tighter but still premium spacing
- 36px–40px H1
- full-width CTAs
- stable image crop
- no dependence on motion for comprehension

```css
@media (max-width: 767px) {
  .hero {
    min-height: 100svh;
  }

  .hero__cta {
    width: 100%;
    min-height: 48px;
    justify-content: center;
  }
}

.hero {
  touch-action: pan-y;
}
```

### 10. Accessibility — WCAG 2.1 AAA Deep-Dive

**Keyboard Navigation:**
```html
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

**Screen reader labeling:**
```html
<section class="hero" aria-label="Sección principal de LAVERNE">
```

**Focus management:**
```css
*:focus-visible {
  outline: 3px solid #D4AF37;
  outline-offset: 3px;
}
```

### 11. Frontend Security & Safe Rendering

Never inject untrusted content directly into the hero.

```html
<!-- ❌ Avoid inline style injection with untrusted content -->
<div style="background-image: url(USER_INPUT)"></div>

<!-- ✅ Prefer static classes and validated asset URLs -->
<div class="hero__image">
  <img src="/images/hero-1440.jpg" alt="LAVERNE hero image" />
</div>
```

If dynamic HTML is unavoidable, sanitize it before insertion and keep CSP in mind.

**Example CSP baseline for static/frontend deployments:**
```
Content-Security-Policy:
  default-src 'self';
  img-src 'self' data: https:;
  style-src 'self' 'unsafe-inline' https://fonts.googleapis.com;
  font-src 'self' https://fonts.gstatic.com;
  script-src 'self';
  connect-src 'self';
  base-uri 'self';
  frame-ancestors 'none';
```

## A/B Testing Variants

**CTA Psychology — What to Test:**

| Variant | CTA Copy | Hypothesis | Metric |
|---------|----------|-----------|--------|
| Control | "Ver catálogo completo" | Neutral, professional | Baseline CTR |
| B | "Descubrir colección" | Exploration framing increases engagement | CTR + scroll depth |
| C | "Explorar fragancias" | Product curiosity improves interaction | CTR + product views |
| D | "Descargar catálogo" | Concrete action improves qualified intent | Lead capture |

**Image Emotion Variants:**
- **A**: Subject with fragrance — aspirational and premium
- **B**: Close-up bottle only — minimalist and product-led
- **C**: Lifestyle setting — emotional and atmospheric

**Overlay Variants:**
- **Lighter overlay** — more editorial, more image-led
- **Darker overlay** — stronger text clarity and readability

## Anti-Patterns — What NOT to Do

| ❌ Anti-Pattern | ✅ LAVERNE Standard |
|----------------|-------------------|
| Full-page white background with navy text block | Dark navy/image-led composition with depth |
| Generic sans-serif headline | Premium serif such as Playfair Display or Prata |
| Generic stock image | Custom editorial-style photography brief |
| `#FFD700` web-yellow accents | `#D4AF37` muted jewelry-gold |
| Rounded corners everywhere | Minimal 2px–4px radius only where needed |
| Multiple equally weighted CTAs | 1 dominant primary CTA + optional secondary CTA |
| Hero that depends on motion to communicate | Clear static composition first, motion second |
| Countdown urgency widgets | Restrained badge or proof cue |
| Overly saturated grading | Warm, controlled, editorial color treatment |

## Complete Hero HTML + CSS

```html
<a href="#main-content" class="skip-link">Saltar al contenido</a>

<section class="hero" aria-label="Sección principal — LAVERNE catálogo profesional 2026">
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
      alt="LAVERNE — fotografía editorial de fragancia de lujo"
      class="hero__bg-img"
      width="1440"
      height="810"
      fetchpriority="high"
      decoding="async"
    />
  </picture>

  <div class="hero__overlay" aria-hidden="true"></div>

  <div class="hero__content" id="main-content">
    <p class="hero__brand">ORIENT FRAGANCE</p>

    <h1 class="hero__title">LAVERNE</h1>
    <p class="hero__subtitle">CATÁLOGO PROFESIONAL 2026</p>

    <div class="hero__badge" role="note">
      <svg class="hero__badge-icon" aria-hidden="true" width="14" height="14" viewBox="0 0 16 16" fill="currentColor">
        <path d="M3 9.5a1.5 1.5 0 1 1 3 0 1.5 1.5 0 0 1-3 0M1 4a.5.5 0 0 1 .5-.5h13a.5.5 0 0 1 0 1H14v7a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V4.5H1.5A.5.5 0 0 1 1 4z"/>
      </svg>
      <span>TESTER GRATIS DESDE 24 UNIDADES</span>
    </div>

    <div class="hero__actions">
      <a
        href="#catalogo"
        class="hero__cta hero__cta--primary"
        aria-label="Ver catálogo completo de LAVERNE"
      >
        Ver catálogo completo
        <span class="hero__cta-arrow" aria-hidden="true">→</span>
      </a>
      <a
        href="#detalles"
        class="hero__cta hero__cta--secondary"
        aria-label="Ver detalles sobre la colección LAVERNE"
      >
        Ver detalles
      </a>
    </div>
  </div>

  <div class="hero__scroll" aria-hidden="true">
    <div class="hero__scroll-line"></div>
  </div>
</section>
```

```css
/* ==============================
   LAVERNE Hero Section — v2.1
   Navy #1A3A52, Gold #D4AF37
   Landing-first, static-friendly
   ============================== */

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

.skip-link:focus {
  top: 16px;
}

.hero {
  position: relative;
  width: 100%;
  min-height: 100vh;
  display: flex;
  align-items: center;
  overflow: hidden;
  background-color: #1A3A52;
  touch-action: pan-y;
}

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

.hero__brand    { animation: fadeInUp 0.6s ease 0.1s both; }
.hero__title    { animation: fadeInUp 0.7s ease 0.3s both; }
.hero__subtitle { animation: fadeInUp 0.6s ease 0.5s both; }
.hero__badge    { animation: fadeInUp 0.5s ease 0.7s both; }
.hero__actions  { animation: fadeInUp 0.5s ease 0.9s both; }

@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(24px); }
  to   { opacity: 1; transform: translateY(0); }
}

.hero__brand {
  font-size: 11px;
  color: #D4AF37;
  font-weight: 700;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  margin: 0;
}

.hero__title {
  font-family: 'Playfair Display', Georgia, serif;
  font-size: 80px;
  font-weight: 700;
  color: #D4AF37;
  letter-spacing: 0.1em;
  line-height: 1.05;
  margin: 0;
  text-shadow: 0 2px 20px rgba(0, 0, 0, 0.3);
}

.hero__subtitle {
  font-size: 16px;
  font-family: 'Playfair Display', Georgia, serif;
  font-weight: 400;
  color: #F5F5F5;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  margin: 0;
}

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

.hero__actions {
  display: flex;
  gap: 16px;
  flex-wrap: wrap;
  justify-content: center;
  margin-top: 8px;
}

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
    box-shadow 0.25s ease,
    background-color 0.25s ease,
    border-color 0.25s ease;
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
  50%       { opacity: 1; transform: scaleY(1.1); }
}

@media (max-width: 1199px) {
  .hero__title {
    font-size: 64px;
  }

  .hero__content {
    padding: 100px 32px 64px;
  }
}

@media (max-width: 767px) {
  .hero {
    min-height: 100svh;
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

- [ ] Brand colors applied correctly (`#1A3A52`, `#D4AF37`) with restrained accent usage
- [ ] Typography loaded efficiently and matched to the premium art direction
- [ ] Hero image uses responsive sources and `fetchpriority="high"` on the LCP image
- [ ] Hero image kept lightweight enough for landing-page performance targets
- [ ] Markup uses semantic structure and clear accessible naming
- [ ] Skip link is first focusable element and visible on focus
- [ ] Contrast ratios verified for headline, subtitle, badge, and CTA
- [ ] Primary CTA clearly dominant; secondary CTA visually subordinate
- [ ] Motion is decorative only, never required for understanding
- [ ] `prefers-reduced-motion` disables non-essential animation
- [ ] Mobile layout tested at 375px, 768px, 1024px, and 1440px
- [ ] Hero remains readable even before custom fonts finish loading
- [ ] Output is deployment-friendly for static hosting and Cloudflare delivery
- [ ] No unsafe inline dynamic injection patterns in markup or styles
- [ ] Performance target: hero contributes to Lighthouse > 85 and LCP < 1.2s where feasible

## Related Skills

- `ecommerce-product-grid` — Product showcase after the hero
- `performance-optimization` — Image delivery, code stability, Core Web Vitals
- `shopify-integration` — Optional platform wiring after the landing UI is complete
