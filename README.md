# laverne-skills

Custom Claude Code skills for LAVERNE luxury fragrance landing page catalog. Built to extend [superpowers](https://github.com/obra/superpowers) with design, e-commerce, and performance optimization capabilities.

**Skills included:**
1. **landing-page-hero-design** — Premium hero sections with photography integration
2. **ecommerce-product-grid** — Luxury product grids with responsive layouts
3. **shopify-integration** — Shopify Liquid templates and checkout flows
4. **performance-optimization** — Sub-2-second load times, 85+ Lighthouse scores

---

## 🎯 Purpose

LAVERNE is a professional fragrance catalog for Orient Fragance. This skills library provides Claude Code with specialized knowledge for building:

- **9-page landing** (1 hero + 8 product pages)
- **Navy + Gold design** (#1A3A52 + #D4AF37)
- **Bulk pricing** (€65 → €55 at x12 units)
- **Premium UX** — not templatic, OVERDOSE quality
- **Conversion optimized** — clear CTAs, trust signals
- **Mobile perfect** — responsive, <2s load, A/B ready

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
```

### Using all skills together

```
I'm building a 9-page LAVERNE fragrance catalog landing page using Shopify.

Apply these skills:
1. landing-page-hero-design — Create hero section
2. ecommerce-product-grid — Design product grid (3-col desktop, 1-col mobile)
3. shopify-integration — Generate Liquid templates
4. performance-optimization — Ensure <2s load time

Brand colors: Navy #1A3A52, Gold #D4AF37
Products: 8 fragrances with bulk pricing (€65 → €55 at x12)
Target: Lighthouse Performance 85+, LCP <1.2s, fully responsive
```

---

## 📋 Skill Descriptions

### landing-page-hero-design

**When to use:** Creating premium hero sections with hero imagery, luxury typography, and conversion CTAs.

**Includes:**
- Photography integration strategy (aspect ratios, overlays, responsive crops)
- Premium typography pairing (serif headlines, sans-serif body)
- Color & material design (navy + gold palette)
- Visual hierarchy and trust signals
- Micro-interactions (fade-ins, parallax, button hovers)
- Mobile responsiveness
- Accessibility (WCAG 2.1 AA)
- Performance optimization (< 1.2s LCP)

**Example:**
```
Create a hero section for LAVERNE with:
- Navy background (#1A3A52)
- Gold accents (#D4AF37)
- Serif headline "LAVERNE"
- Full-width image 16:9 aspect ratio
- CTA "Ver catálogo completo"
```

---

### ecommerce-product-grid

**When to use:** Designing product showcase grids with pricing, bulk discounts, and responsive layouts.

**Includes:**
- 3-column desktop → 2-column tablet → 1-column mobile layouts
- Product card anatomy (image, name, notes, pricing, CTA)
- Bulk pricing display (€65 → €55 at x12)
- Fragrance notes presentation
- High-performance image loading (WebP, lazy load)
- CTA button strategy and micro-interactions
- A/B testing variants
- WCAG accessibility
- Lighthouse optimization

**Example:**
```
Create a product grid for LAVERNE fragrances:
- 8 products (Blue Laverne, Little Garden, Gift Set, etc.)
- Pricing: €65 standard, €55 at x12 units
- Fragrance notes: Top/Heart/Base layers
- CTA: "Añadir a cotización"
- Responsive: 1440px / 768px / 375px
```

---

### shopify-integration

**When to use:** Generating Shopify Liquid templates and connecting backend to frontend.

**Includes:**
- Collection page templates
- Product card Liquid component
- Metafield configuration (fragrance notes, bulk pricing)
- Dynamic pricing (variant prices, bulk discounts)
- Analytics tracking (GTM events, conversion tracking)
- Image optimization (Shopify CDN, srcset)
- Responsive Liquid markup
- Checkout flow optimization
- A/B testing setup

**Example:**
```
Generate Shopify Liquid templates for LAVERNE:
- Collection page showing 8 products
- Product cards with bulk pricing metadata
- GTM tracking for "add to quote" clicks
- Mobile-optimized responsive images
```

---

### performance-optimization

**When to use:** Achieving sub-2-second load times and 85+ Lighthouse scores.

**Includes:**
- Performance budget definition (HTML, CSS, JS, images, fonts)
- Image optimization (WebP, srcset, lazy loading, compression)
- CSS optimization (critical path, minification, purge unused)
- Font optimization (woff2, font-display: swap)
- JavaScript deferral and code-splitting
- HTTP/2 server push
- Browser caching strategies
- Core Web Vitals monitoring (LCP, FID, CLS)
- Lighthouse testing workflow
- GitHub Actions CI/CD with performance budgets

**Example:**
```
Optimize LAVERNE landing page for <2s load:
- Hero image: < 150KB (WebP, responsive)
- Product grid: lazy load images
- CSS: < 50KB (critical path inline)
- JavaScript: defer non-critical
- Target: LCP < 1.2s, Lighthouse Performance 85+
```

---

## 🎨 Design System

### Colors

- **Primary dark**: `#1A3A52` (navy blue) — elegance, trust, luxury
- **Primary accent**: `#D4AF37` (gold) — premium, jewelry-like refinement
- **Neutral light**: `#F5F5F5` (off-white) — breathing room
- **Neutral dark**: `#666` or `#888` (gray) — secondary text
- **Text on dark**: `#FFFFFF` (white) — high contrast

### Typography

- **Headings (H1–H3)**: Serif font (Playfair Display, Prata, Crimson Text)
  - H1: 60–80px desktop, 36px mobile
  - H2: 28–36px desktop, 24px mobile
  - H3: 18–22px desktop, 16px mobile
- **Body text**: Sans-serif (Inter, Lora, Roboto)
  - 16px desktop, 14px mobile
  - Line height: 1.5–1.6

### Spacing

- Base unit: 8px
- Hero section: 100vh min-height
- Product card padding: 16px
- Grid gap: 24px–32px desktop, 12–16px mobile

### Responsive Breakpoints

- **Desktop**: 1440px+
- **Tablet**: 768px–1439px
- **Mobile**: < 768px

---

## 🧪 Testing

Each skill includes implementation checklists. For comprehensive testing:

```bash
# Lighthouse performance test
lighthouse https://laverne.example.com \
  --throttle-cpu-slowdown=4 \
  --output-path=report.html

# Mobile responsiveness
# Test at 375px (iPhone SE), 768px (iPad), 1440px (desktop)

# A/B testing
# Track CTA text variants, image crops, pricing displays
# Use Google Analytics 4 + GTM for event tracking
```

---

## 📚 Resources

- **Superpowers framework**: [obra/superpowers](https://github.com/obra/superpowers)
- **Anthropic skills**: [anthropics/skills](https://github.com/anthropics/skills)
- **Shopify Liquid**: [Shopify Liquid documentation](https://shopify.dev/themes/liquid)
- **Web Vitals**: [web.dev/vitals](https://web.dev/vitals/)
- **Lighthouse**: [developers.google.com/web/tools/lighthouse](https://developers.google.com/web/tools/lighthouse)

---

## 🤝 Contributing

These skills are designed for LAVERNE but can be extended for other luxury e-commerce brands. To contribute:

1. Fork the repository
2. Create a branch (`git checkout -b feature/improvement`)
3. Update the relevant SKILL.md
4. Submit a pull request

---

## 📄 License

MIT License — see LICENSE file for details.

---

## 🎯 Next Steps

1. **Install skills** in Claude Code (see Installation section)
2. **Use with superpowers** for structured workflow (brainstorm → plan → execute → review)
3. **Build LAVERNE landing** using all 4 skills combined
4. **Test & optimize** using performance-optimization skill
5. **Deploy to Shopify** using shopify-integration templates
6. **Monitor & iterate** with A/B testing variants

---

## 📞 Support

For questions or issues:
- Open an issue on GitHub
- Check individual SKILL.md files for detailed guidance
- Reference superpowers documentation for workflow integration

---

**Made with ❤️ for LAVERNE by AminBen10**
