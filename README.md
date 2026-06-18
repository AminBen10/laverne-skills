# laverne-skills

Custom Claude Code skills for LAVERNE luxury fragrance landing page catalog. Built to extend [superpowers](https://github.com/obra/superpowers) with design, e-commerce, and performance optimization capabilities — with enterprise-grade security and WCAG 2.1 AAA accessibility.

**Skills included:**
1. **landing-page-hero-design** — Premium hero sections with photography integration, WCAG AAA, XSS-safe
2. **ecommerce-product-grid** — Luxury product grids with golden-ratio spacing and fragrance notes pyramid
3. **shopify-integration** — Shopify Liquid templates with PCI compliance and B2B quote flows
4. **performance-optimization** — Lighthouse 85+, LCP < 1.2s, Claude AI integration patterns

---

## 🎯 Purpose

LAVERNE is a professional fragrance catalog for Orient Fragance. This skills library provides Claude Code with specialized knowledge for building:

- **9-page landing** (1 hero + 8 product pages)
- **Navy + Gold design** (#1A3A52 + #D4AF37)
- **Bulk pricing** (€65 → €55 at x12 units)
- **Premium UX** — not templatic, OVERDOSE quality
- **Conversion optimized** — clear CTAs, trust signals
- **Mobile perfect** — responsive, <2s load, A/B ready
- **Enterprise secure** — XSS prevention, PCI compliance, GDPR ready
- **WCAG 2.1 AAA** — exceeds accessibility standards

---

## 🔒 Security Overview

All skills follow enterprise security practices:

| Threat | Protection |
|--------|-----------|
| XSS (Cross-Site Scripting) | All Liquid outputs use `\| escape` filter; JavaScript uses DOMPurify |
| Template Injection | Never output raw URL params; validate metafield types |
| Credential Exposure | API keys in `.env` only, never in Liquid or client-side JS |
| CSRF | Shopify's `{% form %}` tag provides CSRF tokens automatically |
| PCI Violation | No custom JS on `/checkout/*` pages; card data handled by Shopify |
| GDPR | Cookie consent before GTM fires; privacy policy template included |

See [SECURITY.md](SECURITY.md) for the complete security guide.

---

## ♿ Accessibility Overview

All skills target WCAG 2.1 AAA (the highest level):

| Feature | Standard |
|---------|---------|
| Color contrast | 7:1 ratio (AAA) for all primary text |
| Focus indicators | 3px solid gold ring, never suppressed |
| Keyboard navigation | All interactive elements tab-accessible |
| Screen readers | `aria-label`, `role`, `sr-only` patterns throughout |
| Motion | `prefers-reduced-motion` respected in all animations |
| Touch targets | 48px minimum (iOS HIG standard) |

---

## 📦 Installation

### Claude Code

```bash
# Register this repository as a plugin marketplace
/plugin marketplace add AminBen10/laverne-skills

# Install individual skills as needed
/plugin install landing-page-hero-design@laverne-skills
/plugin install ecommerce-product-grid@laverne-skills
/plugin install shopify-integration@laverne-skills
/plugin install performance-optimization@laverne-skills

# Reload plugins to activate
/reload-plugins
```

### Local Development

Clone the repository:

```bash
git clone https://github.com/AminBen10/laverne-skills.git
cd laverne-skills
```

Each skill is a directory with a `SKILL.md` file. Reference them in Claude by pasting their content into your prompt.

---

## 🚀 Quick Start

### Using a single skill

Paste this into Claude Code:

```
Read the landing-page-hero-design skill from https://github.com/AminBen10/laverne-skills/blob/main/landing-page-hero-design/SKILL.md

Create a hero section for LAVERNE fragrance catalog with:
- Full-width image (man holding fragrance)
- "LAVERNE" headline in gold, serif
- Navy blue background overlay
- "TESTER GRATIS DESDE 24 UNIDADES" badge
- "Ver catálogo completo" CTA button
- Responsive (1440px, 768px, 375px)
- <1.2s LCP target
- WCAG 2.1 AAA compliant
- XSS-safe Liquid output
```

### Using all skills together

```
I'm building a 9-page LAVERNE fragrance catalog landing page using Shopify.

Apply these skills:
1. landing-page-hero-design — Create hero section (WCAG AAA, XSS-safe)
2. ecommerce-product-grid — Design product grid (3-col desktop, 1-col mobile, luxury spacing)
3. shopify-integration — Generate Liquid templates (PCI compliant, GDPR, B2B quote flow)
4. performance-optimization — Ensure <2s load time (Lighthouse 85+, LCP < 1.2s)

Brand colors: Navy #1A3A52, Gold #D4AF37
Products: 8 fragrances with bulk pricing (€65 → €55 at x12)
Target: Lighthouse Performance 85+, LCP <1.2s, fully responsive, WCAG AAA
```

### Superpowers Framework Integration

These skills are designed to work with the [superpowers framework](https://github.com/obra/superpowers):

```
# Brainstorm phase
Use landing-page-hero-design to brainstorm luxury hero variations for LAVERNE

# Plan phase  
Create an implementation plan using all 4 skills:
- landing-page-hero-design → ecommerce-product-grid → shopify-integration → performance-optimization

# Execute phase
Generate production-ready code for LAVERNE landing page

# Review phase
Review the output against:
- SECURITY.md security checklist
- WCAG 2.1 AAA requirements
- Lighthouse Performance > 85 targets
```

---

## 📋 Skill Descriptions

### landing-page-hero-design

**When to use:** Creating premium hero sections with hero imagery, luxury typography, and conversion CTAs.

**Includes:**
- Photography integration strategy (aspect ratios, overlays, responsive crops)
- Premium typography pairing (Playfair Display + color theory)
- Navy + Gold color psychology (why these colors signal luxury)
- WCAG 2.1 AAA accessibility (7:1 contrast, skip links, keyboard nav)
- Micro-interactions library (GPU-accelerated, `prefers-reduced-motion` safe)
- Anti-patterns section (what NOT to do — flat design, generic stock images)
- A/B testing variants (CTA copy psychology, image emotion variants)
- Security: XSS-safe Liquid, CSP header recommendations

**Example:**
```
Create a hero section for LAVERNE with:
- Navy background (#1A3A52)
- Gold accents (#D4AF37)
- Serif headline "LAVERNE" (Playfair Display, 80px)
- Full-width image 16:9 aspect ratio, warm directional lighting
- CTA "Ver catálogo completo" (gold bg, navy text, 7.1:1 contrast)
- Skip link for keyboard users
- Fade-in animations disabled for prefers-reduced-motion
```

---

### ecommerce-product-grid

**When to use:** Designing product showcase grids with pricing, bulk discounts, and responsive layouts.

**Includes:**
- Product card anatomy based on golden ratio (1:1.618 spacing)
- Grid layouts for luxury (negative space, 3-col → 1-col, no auto-fill)
- Fragrance notes pyramid (Top/Heart/Base as editorial storytelling)
- Bulk pricing hierarchy (€65 → €55 at ×12, professional framing)
- Interactive hover states (300ms cubic-bezier, `translateY` only)
- Photography standards (studio lighting, 1:1 square crop, < 100KB WebP)
- Trust signals for luxury (awards, provenance badges — NOT star ratings)
- WCAG AAA accessibility (roles, aria-labels, sr-only pricing context)

**Example:**
```
Create a product grid for LAVERNE fragrances:
- 8 products (Blue Laverne, Little Garden, Gift Set, etc.)
- Pricing: €65 standard, €55 at x12 units (professional tier framing)
- Fragrance notes: Top/Heart/Base layers with gold border-left
- CTA: "Añadir a cotización" with aria-label per product
- Responsive: 1440px (3-col) / 768px (2-col) / 375px (1-col)
- Hover: translateY(-4px) + multi-layer box-shadow
- No auto-fill, no star ratings, no strikethrough pricing
```

---

### shopify-integration

**When to use:** Generating Shopify Liquid templates and connecting backend to frontend.

**Includes:**
- Collection page templates with security-hardened Liquid
- Product card Liquid component (metafields, bulk pricing, image srcset)
- Metafield configuration (fragrance_notes, bulk_price_12, concentration)
- Currency handling (EUR primary, multi-currency via Shopify Markets)
- Analytics tracking (GTM events, GDPR-compliant consent flow)
- PCI compliance guidance (no custom JS on checkout pages)
- API key security (environment variables, never in Liquid)
- B2B quote flow (contact form → email notification → quote tracking)
- A/B testing via customer metafields

**Example:**
```
Generate Shopify Liquid templates for LAVERNE:
- Collection page showing 8 products (all outputs | escape filtered)
- Product cards with bulk pricing metadata (money type metafield)
- GTM tracking only after cookie consent (GDPR compliant)
- Mobile-optimized responsive images (Shopify CDN srcset)
- B2B quote form with company name + quantity tier selection
```

---

### performance-optimization

**When to use:** Achieving sub-2-second load times and 85+ Lighthouse scores.

**Includes:**
- Lighthouse score breakdown by metric (weight of LCP, TBT, CLS, FCP)
- Performance budget by device (mobile < 560KB, desktop < 1.2MB)
- Real-world 3G throttling results (before/after benchmarks)
- Claude AI integration for intelligent optimization recommendations
- CLS prevention techniques (image dimensions, font size-adjust, fixed position)
- Chrome DevTools profiling guide (flame chart reading, long task detection)
- Core Web Vitals deep-dive (LCP strategies, CLS prevention, TBT reduction)
- GitHub Actions CI with `.lighthouserc.json` performance budgets

**Example:**
```
Optimize LAVERNE landing page for <2s load:
- Hero image: WebP srcset, fetchpriority="high", <150KB at 1440px
- LCP preload: <link rel="preload"> in <head> for hero image
- Product grid: lazy load images, explicit width/height for zero CLS
- CSS: <5KB critical inline, rest deferred via preload
- Fonts: woff2, font-display: swap, size-adjust for zero font CLS
- Target: LCP < 1.2s, CLS < 0.1, TBT < 150ms, Lighthouse 85+
```

---

## 🎨 Design System

### Colors (with Psychology)

- **Primary dark**: `#1A3A52` (navy blue) — elegance, trust, luxury — large areas
- **Primary accent**: `#D4AF37` (gold) — premium refinement — CTAs, borders, highlights only
- **Neutral light**: `#F5F5F5` (off-white) — breathing room, sophistication
- **Neutral dark**: `#666` (gray) — secondary text (AA contrast minimum)
- **Text on dark**: `#FFFFFF` or `#F5F5F5` (high contrast: 16.7:1 on navy)

**Contrast ratios:**
- Gold on Navy: 7.1:1 ✅ AAA
- White on Navy: 16.7:1 ✅ AAA
- Gold on White: 2.3:1 ❌ Fails AA — never use

### Typography

- **Headings (H1–H3)**: Playfair Display (luxury serif)
  - H1: 64–80px desktop, 36–40px mobile
  - H2: 28–36px desktop, 24px mobile
  - H3: 18–22px desktop, 16px mobile
  - Letter-spacing: 0.06em–0.12em (generous tracking = luxury signal)
- **Body text**: Inter or DM Sans (clean sans-serif)
  - 16px desktop, 14px mobile — never below 12px
  - Line height: 1.5–1.6

### Spacing

- Base unit: 8px
- Hero section: 100vh (`100svh` on mobile for browser chrome)
- Product card padding: 24px desktop, 16px mobile
- Grid gap: 32px desktop, 24px tablet, 16px mobile

### Responsive Breakpoints

- **Desktop**: 1440px+
- **Large tablet**: 1200px
- **Tablet**: 768px
- **Mobile**: < 768px (single column, no auto-fill)

---

## 🧪 Testing

Each skill includes implementation checklists. For comprehensive testing:

```bash
# Lighthouse performance test (desktop)
lighthouse https://laverne.example.com \
  --form-factor=desktop \
  --throttle-cpu-slowdown=1 \
  --output html \
  --output-path=report-desktop.html

# Lighthouse performance test (mobile — 4x CPU throttle, Slow 3G)
lighthouse https://laverne.example.com \
  --form-factor=mobile \
  --throttle-cpu-slowdown=4 \
  --throttle-method=devtools \
  --output html \
  --output-path=report-mobile.html

# Mobile responsiveness targets
# 375px (iPhone SE): single column, 48px touch targets
# 768px (iPad): 2-column grid
# 1440px (Desktop): 3-column grid, full hero

# Accessibility audit
npx axe https://laverne.example.com --reporter cli

# A/B testing
# Track CTA text variants via GTM custom events
# Use GA4 custom dimensions for variant assignment
```

---

## 📚 Resources

- **Superpowers framework**: [obra/superpowers](https://github.com/obra/superpowers)
- **Anthropic skills**: [anthropics/skills](https://github.com/anthropics/skills)
- **Shopify Liquid**: [Shopify Liquid documentation](https://shopify.dev/themes/liquid)
- **Web Vitals**: [web.dev/vitals](https://web.dev/vitals/)
- **Lighthouse**: [developer.chrome.com/docs/lighthouse](https://developer.chrome.com/docs/lighthouse/)
- **WCAG 2.1**: [w3.org/WAI/WCAG21](https://www.w3.org/WAI/WCAG21/)
- **OWASP Top 10**: [owasp.org/Top10](https://owasp.org/Top10/)
- **DOMPurify** (XSS sanitizer): [cure53.de/purify](https://cure53.de/purify)

---

## 🤝 Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for full contribution guidelines, commit conventions, and quality standards.

**Quick start:**
1. Fork the repository
2. Create a branch (`git checkout -b feature/improvement`)
3. Update the relevant `SKILL.md`
4. Verify: security (no XSS), accessibility (WCAG AA+), performance (no regressions)
5. Submit a pull request

---

## 🔧 Support Files

| File | Purpose |
|------|---------|
| [SECURITY.md](SECURITY.md) | Enterprise security patterns: XSS, CORS, GDPR, PCI |
| [CONTRIBUTING.md](CONTRIBUTING.md) | How to extend skills, commit conventions, quality standards |
| [TROUBLESHOOTING.md](TROUBLESHOOTING.md) | Common issues: Lighthouse failures, Shopify Liquid, metafields |

---

## 📄 License

MIT License — see LICENSE file for details.

---

## 🎯 Skill Composition Flow

```
User visits LAVERNE landing page
         │
         ▼
landing-page-hero-design
  [Navy hero, gold headline, CTA, WCAG AAA]
         │
         ▼ (scroll)
ecommerce-product-grid
  [8 products, fragrance notes, bulk pricing]
         │
         ▼ (click "Añadir a cotización")
shopify-integration
  [Liquid templates, metafields, quote form, GTM]
         │
         ▼ (all above)
performance-optimization
  [WebP images, critical CSS, Lighthouse 85+, CLS < 0.1]
```

---

**Made with ❤️ for LAVERNE by AminBen10**
