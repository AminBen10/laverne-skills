# laverne-skills

Custom Claude Code skills for building the **LAVERNE luxury fragrance landing page** first — with optional integration guidance for later implementation. Built to extend [superpowers](https://github.com/obra/superpowers) with premium design, landing-page architecture, accessibility, security, and performance optimization patterns.

**Core skills included:**
1. **landing-page-hero-design** — Premium hero sections with photography integration, WCAG AAA, and landing-first CTA strategy
2. **ecommerce-product-grid** — Luxury product grids with golden-ratio spacing, fragrance notes pyramid, and premium product storytelling
3. **performance-optimization** — Lighthouse 85+, Core Web Vitals budgets, and Cloudflare-friendly delivery patterns
4. **shopify-integration** *(optional)* — Platform-specific Liquid templates, metafields, and quote-flow guidance for later integration

---

## 🎯 Purpose

LAVERNE is a premium fragrance catalog experience for Orient Fragance. This skills library gives Claude Code a **landing-first system** for building:

- **A polished luxury landing page** — hero, product storytelling, conversion flow
- **Navy + Gold design language** (`#1A3A52` + `#D4AF37`)
- **Premium UX** — editorial, not templatic
- **Product presentation** — fragrance notes, structured pricing, trust signals
- **Mobile-perfect delivery** — responsive, high-performing, touch-friendly
- **Cloudflare-ready deployment** — static-friendly, performance-conscious output
- **Security-conscious frontend patterns** — XSS prevention, CSP awareness, safe third-party script usage
- **WCAG 2.1 AAA targets** — contrast, focus, motion, screen reader support

---

## 🧭 Recommended Build Order

Build the landing page in this order:

1. **`landing-page-hero-design`** — Define the visual thesis of the page
2. **`ecommerce-product-grid`** — Present the product catalog with premium structure
3. **`performance-optimization`** — Make the landing fast, stable, and deployment-ready
4. **`shopify-integration`** *(optional)* — Add platform-specific ecommerce wiring later if needed

This keeps the project focused on **shipping the landing page first**, without locking the system too early to a single backend.

---

## 🔒 Security Overview

All skills follow secure frontend patterns appropriate for a public landing page:

| Threat | Protection |
|--------|-----------|
| XSS (Cross-Site Scripting) | Escape dynamic output, sanitize injected HTML with DOMPurify when necessary |
| Unsafe third-party embeds | Restrict sources with CSP and load only what is necessary |
| Credential exposure | Keep secrets in environment variables only; never expose private keys client-side |
| Consent violations | Load analytics and marketing scripts only after explicit consent where required |
| Layout/script regressions | Prefer static markup, safe progressive enhancement, and minimal JS |

See [SECURITY.md](SECURITY.md) for the complete security guide.

---

## ♿ Accessibility Overview

All core skills target WCAG 2.1 AAA where practical:

| Feature | Standard |
|---------|---------|
| Color contrast | 7:1 ratio (AAA) for primary text and critical controls |
| Focus indicators | 3px visible focus ring, never suppressed |
| Keyboard navigation | Interactive elements reachable in logical order |
| Screen readers | Semantic HTML, `aria-label` only where needed, `sr-only` support patterns |
| Motion | `prefers-reduced-motion` respected in animations and transitions |
| Touch targets | 48px minimum for key interactive controls |

---

## 📦 Installation

### Claude Code

```bash
# Register this repository as a plugin marketplace
/plugin marketplace add AminBen10/laverne-skills

# Install individual skills as needed
/plugin install landing-page-hero-design@laverne-skills
/plugin install ecommerce-product-grid@laverne-skills
/plugin install performance-optimization@laverne-skills
/plugin install shopify-integration@laverne-skills

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

Create a landing page hero section for LAVERNE with:
- Full-width image (man holding fragrance)
- "LAVERNE" headline in gold, serif
- Navy blue background overlay
- "TESTER GRATIS DESDE 24 UNIDADES" badge
- "Ver catálogo completo" CTA button
- Responsive (1440px, 768px, 375px)
- <1.2s LCP target
- WCAG 2.1 AAA compliant
- Safe, deployment-ready HTML/CSS output
```

### Using the core landing-page skills together

```
I'm building the LAVERNE luxury fragrance landing page for deployment on Cloudflare.

Apply these skills:
1. landing-page-hero-design — Create the hero section and premium page opening
2. ecommerce-product-grid — Design the fragrance product grid and content blocks
3. performance-optimization — Ensure <2s load time, strong Core Web Vitals, and static-friendly delivery

Brand colors: Navy #1A3A52, Gold #D4AF37
Products: 8 fragrances with bulk pricing (€65 → €55 at x12)
Target: Lighthouse Performance 85+, LCP <1.2s, fully responsive, WCAG AAA
```

### Optional integration layer

If the landing page later needs ecommerce/backend wiring:

```
After the landing page UI is complete, use shopify-integration only to adapt the experience to Shopify-specific templates, metafields, and quote-flow requirements.
```

### Superpowers Framework Integration

These skills are designed to work with the [superpowers framework](https://github.com/obra/superpowers):

```
# Brainstorm phase
Use landing-page-hero-design to brainstorm luxury hero variations for LAVERNE

# Plan phase
Create an implementation plan using the landing-first skill stack:
- landing-page-hero-design → ecommerce-product-grid → performance-optimization

# Execute phase
Generate production-ready code for the LAVERNE landing page

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
- Security-aware output patterns for safe frontend rendering

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

**When to use:** Designing product showcase grids with pricing, product storytelling, and responsive layouts.

**Includes:**
- Product card anatomy based on golden ratio (1:1.618 spacing)
- Grid layouts for luxury (negative space, 3-col → 1-col, no auto-fill)
- Fragrance notes pyramid (Top/Heart/Base as editorial storytelling)
- Bulk pricing hierarchy (€65 → €55 at ×12, professional framing)
- Interactive hover states (300ms cubic-bezier, `translateY` only)
- Photography standards (studio lighting, 1:1 square crop, < 100KB WebP)
- Trust signals for luxury (awards, provenance badges — not star ratings)
- WCAG AAA accessibility patterns for cards and pricing

**Example:**
```
Create a product grid for LAVERNE fragrances:
- 8 products (Blue Laverne, Little Garden, Gift Set, etc.)
- Pricing: €65 standard, €55 at x12 units (professional tier framing)
- Fragrance notes: Top/Heart/Base layers with gold border-left
- CTA: "Ver detalles" with accessible labels per product
- Responsive: 1440px (3-col) / 768px (2-col) / 375px (1-col)
- Hover: translateY(-4px) + multi-layer box-shadow
- No auto-fill, no star ratings, no strikethrough pricing
```

---

### performance-optimization

**When to use:** Achieving sub-2-second load times and strong Lighthouse scores for the landing page.

**Includes:**
- Lighthouse score breakdown by metric (weight of LCP, TBT, CLS, FCP)
- Performance budget by device (mobile < 560KB, desktop < 1.2MB)
- Real-world 3G throttling results (before/after benchmarks)
- CLS prevention techniques (image dimensions, font size-adjust, fixed position)
- Chrome DevTools profiling guide (flame chart reading, long task detection)
- Core Web Vitals deep-dive (LCP strategies, CLS prevention, TBT reduction)
- Deployment-friendly delivery patterns for static hosting and Cloudflare

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

### shopify-integration *(optional)*

**When to use:** Only after the landing page is designed and approved, if the project needs Shopify-specific ecommerce integration.

**Includes:**
- Liquid template guidance
- Metafield configuration
- Currency handling
- Consent-aware analytics tracking
- Quote-flow patterns
- Platform-specific security notes

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

# Accessibility audit
npx axe https://laverne.example.com --reporter cli
```

---

## 📚 Resources

- **Superpowers framework**: [obra/superpowers](https://github.com/obra/superpowers)
- **Anthropic skills**: [anthropics/skills](https://github.com/anthropics/skills)
- **Web Vitals**: [web.dev/vitals](https://web.dev/vitals/)
- **Lighthouse**: [developer.chrome.com/docs/lighthouse](https://developer.chrome.com/docs/lighthouse/)
- **WCAG 2.1**: [w3.org/WAI/WCAG21](https://www.w3.org/WAI/WCAG21/)
- **OWASP Top 10**: [owasp.org/Top10](https://owasp.org/Top10/)
- **DOMPurify** (XSS sanitizer): [cure53.de/purify](https://cure53.de/purify)
- **Cloudflare Web Analytics**: [cloudflare.com/web-analytics](https://www.cloudflare.com/web-analytics/)

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
| [SECURITY.md](SECURITY.md) | Frontend security patterns: XSS, CSP, consent, secrets handling |
| [CONTRIBUTING.md](CONTRIBUTING.md) | How to extend skills, commit conventions, quality standards |
| [TROUBLESHOOTING.md](TROUBLESHOOTING.md) | Common issues: Lighthouse failures, image delivery, accessibility, layout stability |

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
  [8 products, fragrance notes, premium pricing]
         │
         ▼
performance-optimization
  [WebP images, critical CSS, Lighthouse 85+, CLS < 0.1]
         │
         ▼
optional platform integration
  [Only if backend/ecommerce wiring is needed later]
```

---

**Made with ❤️ for LAVERNE by AminBen10**
