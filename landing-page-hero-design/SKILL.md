---
name: landing-page-hero-design
description: Creates distinctive, premium hero sections for luxury brand landing pages. Focuses on photography integration, premium typography, visual hierarchy, and conversion-focused CTAs. Delivers OVERDOSE design that doesn't read as generic template.
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

## Core Principles

### 1. Hero as Thesis
The hero is not decoration—it's the landing page's single thesis. For LAVERNE (luxury fragrance):
- **Visual assertion**: Man with fragrance (aspirational, premium)
- **Brand anchor**: Logo + "ORIENT FRAGANCE" (gold, subtle)
- **Value proposition**: "LAVERNE" + "CATÁLOGO PROFESIONAL 2026"
- **Social proof**: Badge "TESTER GRATIS DESDE 24 UNIDADES"
- **CTA**: Clear, premium button ("Ver catálogo completo")

### 2. Color & Material
Use the brand's color palette intentionally:
- **Dark navy** (#1A3A52) — elegance, luxury, trust
- **Gold** (#D4AF37) — premium accent, jewelry-like refinement
- **White/Off-white** — breathing room, sophistication
- **Subtle gradients** — never harsh, always tasteful

NO: Oversaturated colors, flat design, generic web 2.0 pastels
YES: Deep jewel tones, material depth, cinematic lighting

### 3. Typography Strategy
- **Headline** (H1): Serif font (Playfair Display, Prata, or equivalent)
  - Size: 60px–80px desktop, 36px mobile
  - Weight: Bold or Light (extremes > middle)
  - Color: Gold or white (high contrast vs. background)
- **Subheading**: Serif at smaller scale (20px–24px)
  - Color: White or light gray
- **Body/CTA**: Clean sans-serif (Inter, Lora, or equivalent)
  - 14px–16px for readability
- **Logo text**: Match brand identity (gold, subtle)

### 4. Visual Hierarchy
Structure the hero with clear zones:

```
┌─────────────────────────────────────────┐
│  LOGO + "ORIENT FRAGANCE" (top, subtle) │
│                                         │
│  [FULL-WIDTH IMAGE: Man + Fragrance]    │
│                                         │
│  "LAVERNE" (massive, centered)          │
│  "CATÁLOGO PROFESIONAL 2026" (smaller)  │
│                                         │
│  [BADGE] "TESTER GRATIS DESDE 24U"      │
│                                         │
│  [CTA BUTTON] "Ver catálogo completo"   │
│                                         │
│  [Scroll indicator or decorative line]  │
└─────────────────────────────────────────┘
```

### 5. Photography Integration
- **Aspect ratio**: 16:9 or 21:9 (cinema-like)
- **Composition**: Man occupies ~60% of frame, product visible in hand
- **Lighting**: Warm, directional lighting (not flat studio)
- **Color grading**: Warm tones, slightly desaturated (premium feel)
- **Overlay**: Subtle dark gradient (45°, navy to transparent) to ensure text readability
- **Mobile**: Crop intelligently (portrait-oriented on small screens)

### 6. CTA Button Strategy
- **Primary CTA**: "Ver catálogo completo" or "Descargar catálogo"
- **Style**: Gold button with navy text (high contrast, premium)
- **Size**: 48px height (large enough for mobile touch)
- **Hover**: Subtle scale (+2%), slight glow, smooth transition (300ms)
- **Secondary CTA** (optional): "Solicitar muestras" (outline button, gold border)

### 7. Trust Signals & Badges
- **"TESTER GRATIS DESDE 24 UNIDADES"** badge:
  - Gold background (#D4AF37)
  - Navy text
  - Small gift icon
  - Positioned right of headline (or mobile: below)

### 8. Micro-interactions (OVERDOSE)
- **Fade-in on load**: Hero elements fade in smoothly (0.8s stagger)
- **Parallax effect** (optional): Image moves slightly on scroll (subtle, not distracting)
- **Hover on buttons**: Scale + shadow increase
- **Text reveal**: Heading text typewriter reveal (optional, fast ~1.5s)

### 9. Responsive Design
- **Desktop (1440px+)**: Full hero, image 100% viewport height
- **Tablet (768px–1439px)**: Image height 70–80vh, adjust spacing
- **Mobile (< 768px)**:
  - Stack hero elements vertically
  - Image height 50–60vh (faster load)
  - Font sizes scale down (H1: 36px)
  - Buttons full-width (below image)
  - Remove parallax (performance)

### 10. Performance (CRITICAL)
- **Image optimization**:
  - WebP format (with fallback JPG)
  - Multiple breakpoints: 640px, 1024px, 1440px
  - Lazy load with LQIP (low-quality image placeholder)
- **No hero image > 200KB** (responsive sizes)
- **CSS animations**: Use `transform` + `opacity` only (GPU-accelerated)
- **Load time target**: Hero fully rendered in < 1.2 seconds

## Implementation Checklist

- [ ] Brand colors defined (Navy #1A3A52, Gold #D4AF37)
- [ ] Fonts selected (Serif for headings, sans-serif for body)
- [ ] Photography brief defined (composition, lighting, aspect ratio)
- [ ] Hero HTML structure created (semantic, accessible)
- [ ] CSS written (no hero image > 200KB, GPU-accelerated animations)
- [ ] Responsive breakpoints tested (1440px, 768px, 375px)
- [ ] Accessibility checked (color contrast ≥ 4.5:1, alt text, keyboard nav)
- [ ] Performance verified (Lighthouse > 85, load < 1.2s)
- [ ] A/B ready (CTA text variants, image variants tracked)
- [ ] Mobile tested on real devices (iOS Safari, Android Chrome)

## Example HTML Structure

```html
<section class="hero" role="region" aria-label="Hero Section">
  <div class="hero__image" style="background-image: url(...)">
    <div class="hero__overlay"></div>
  </div>

  <div class="hero__content">
    <div class="hero__brand">
      <span class="hero__brand-text">ORIENT FRAGANCE</span>
    </div>

    <h1 class="hero__title">LAVERNE</h1>
    <p class="hero__subtitle">CATÁLOGO PROFESIONAL 2026</p>

    <div class="hero__badge">
      <span class="hero__badge-text">TESTER GRATIS DESDE 24 UNIDADES</span>
    </div>

    <button class="hero__cta hero__cta--primary">
      Ver catálogo completo
    </button>
  </div>
</section>
```

## Related Skills

- `ecommerce-product-grid` — Product showcase (after hero)
- `performance-optimization` — Image & code optimization
- `shopify-integration` — Connect hero CTA to Shopify collection
