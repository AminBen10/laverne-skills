---
name: performance-optimization
version: 2.0.0
description: Optimizes landing pages for sub-2-second load times and 85+ Lighthouse scores. Handles image compression (WebP, srcset), lazy loading, CSS minification, JavaScript deferral, Core Web Vitals monitoring, and Chrome DevTools profiling. Includes Claude AI integration patterns, performance budgets by device, and 3G throttling benchmarks.
tags: [performance, lighthouse, core-web-vitals, image-optimization, claude-ai]
dependencies: []
license: MIT
---

# Performance Optimization

## Purpose

You are a web performance engineer specializing in luxury e-commerce. Your role is to:
- **Achieve < 2 second load time** — LCP (Largest Contentful Paint) < 1.2s, FCP (First Contentful Paint) < 0.8s
- **Maximize Lighthouse scores** — Performance > 85, Accessibility > 90, SEO > 90
- **Minimize Core Web Vitals** — LCP, FID, CLS all GREEN
- **Optimize for mobile** — 3G throttling, battery efficiency
- **Ensure accessibility** — WCAG 2.1 AA, performance doesn't break functionality

## Core Principles

### 1. Performance Budget

For LAVERNE landing page (hero + 8 product grid pages):

| Metric | Target | Current | Gap |
|--------|--------|---------|-----|
| **LCP** | < 1.2s | ? | Optimize images, defer JS |
| **FCP** | < 0.8s | ? | Remove render-blocking CSS |
| **CLS** | < 0.1 | ? | Reserve space for images, fonts |
| **TTI** | < 2.5s | ? | Defer non-critical JS |
| **HTML size** | < 30KB | ? | Minify, remove unused markup |
| **CSS size** | < 50KB | ? | Minify, purge unused classes |
| **JS size** | < 100KB | ? | Tree-shake, code-split |
| **Total images** | < 500KB | ? | Compress, use WebP, lazy load |

**Performance budget for page load:**
```
HTML:  15KB
CSS:   35KB
JS:    80KB
Fonts: 40KB (2 fonts, woff2)
Images: 300KB (hero + grid)
───────────
Total: 470KB (uncompressed, must be < 600KB gzipped)
```

### 2. Image Optimization (CRITICAL)

**Step 1: Source format & size**

```bash
# Hero image (1440×1080px max)
Original: photo.jpg (2.8MB)
Optimized:
  - photo-640.webp (45KB)
  - photo-1024.webp (78KB)
  - photo-1440.webp (120KB)
  - photo-1024.jpg (85KB) [fallback]
```

**Step 2: HTML markup with srcset**

```html
<picture>
  <!-- WebP for modern browsers -->
  <source
    srcset="
      /images/hero-640.webp 640w,
      /images/hero-1024.webp 1024w,
      /images/hero-1440.webp 1440w
    "
    sizes="100vw"
    type="image/webp"
  />
  
  <!-- JPG fallback -->
  <img
    src="/images/hero-1024.jpg"
    alt="LAVERNE Luxury Fragrance Hero"
    width="1440"
    height="1080"
    loading="lazy"
    decoding="async"
  />
</picture>
```

**Step 3: Compression ratios**

| Format | Size | Notes |
|--------|------|-------|
| JPEG (original) | 2.8MB | Baseline, unoptimized |
| JPEG (optimized) | 180KB | mozjpeg, quality 75 |
| WebP | 120KB | 25% smaller than JPEG |
| AVIF | 85KB | Newest, not yet widely supported |

**Tools:**
```bash
# Convert to WebP
cwebp -q 80 photo.jpg -o photo.webp

# Compress JPEG
jpegoptim --max=80 --strip-all photo.jpg

# Batch process
for img in *.jpg; do
  cwebp -q 80 "$img" -o "${img%.jpg}.webp"
done
```

### 3. Lazy Loading Strategy

**Images below the fold:**
```html
<img
  src="placeholder.jpg"
  data-src="/images/product-1.webp"
  alt="Blue Laverne 7am Package"
  loading="lazy"
  decoding="async"
/>
```

**Intersection Observer API** (for fine control):
```javascript
const imageObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const img = entry.target;
      img.src = img.dataset.src;
      img.classList.add('loaded');
      imageObserver.unobserve(img);
    }
  });
}, { rootMargin: '50px' });

document.querySelectorAll('img[data-src]').forEach(img => {
  imageObserver.observe(img);
});
```

### 4. CSS Optimization

**Step 1: Critical CSS (render-blocking)**

Extract above-the-fold CSS inline:
```html
<head>
  <style>
    /* Critical CSS for hero section (< 5KB) */
    body { margin: 0; font-family: serif, sans-serif; }
    .hero { background: #1a3a52; color: white; min-height: 100vh; }
    .hero__title { font-size: 60px; color: #d4af37; }
    /* ... */
  </style>
  
  <!-- Defer non-critical CSS -->
  <link rel="preload" href="/css/non-critical.css" as="style" />
  <link rel="stylesheet" href="/css/non-critical.css" />
</head>
```

**Step 2: Purge unused CSS**

```bash
# Use PurgeCSS or Tailwind's purge
npx purgecss --css style.css --content index.html --output style.purged.css
```

Before: 150KB → After: 35KB ✅

**Step 3: Minify CSS**

```bash
npx cssnano style.css -o style.min.css
```

### 5. Font Optimization

**Step 1: Use system fonts (fastest)**

```css
body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}
```

**Step 2: If custom fonts, use woff2**

```css
@font-face {
  font-family: 'Playfair Display';
  src: url('/fonts/playfair-display.woff2') format('woff2');
  font-weight: 700;
  font-display: swap;
}
```

- Use `font-display: swap` — show system font immediately, swap when custom loads
- Only load weights used (e.g., 700 bold, not entire family)
- Max 2 font files (1 serif, 1 sans-serif)
- Size: < 40KB combined

### 6. JavaScript Optimization

**Step 1: Defer non-critical JS**

```html
<!-- Render-blocking (analytics, critical functionality) -->
<script>
  // Essential inline JS only
</script>

<!-- Defer until page interactive -->
<script defer src="/js/product-grid.js"></script>
<script defer src="/js/analytics.js"></script>
```

**Step 2: Code-split by route**

```javascript
// Don't load all 8 product pages at once
// Load product grid JS only when scrolling near grid
const GridModule = await import('/js/product-grid.js');
```

**Step 3: Minify & compress**

```bash
# Minify
npx terser app.js -o app.min.js

# Brotli compression (better than gzip)
brotli -k app.min.js  # Creates app.min.js.br
```

JS before: 150KB → after: 45KB ✅

### 7. HTTP/2 Server Push (Optional)

Pre-load critical resources:

```nginx
# Nginx
add_header Link "</css/critical.css>; rel=preload; as=style" always;
add_header Link "</fonts/playfair.woff2>; rel=preload; as=font" always;
```

### 8. Caching Strategy

**Browser caching** (set in `.htaccess` or Nginx):

```apache
# Cache images for 1 year (they have hash names)
<FilesMatch "\.(jpg|jpeg|webp|png|gif)$">
  Header set Cache-Control "max-age=31536000, public"
</FilesMatch>

# Cache CSS/JS for 1 month
<FilesMatch "\.(css|js)$">
  Header set Cache-Control "max-age=2592000, public"
</FilesMatch>

# No cache for HTML (always fresh)
<FilesMatch "\.html$">
  Header set Cache-Control "max-age=0, public, must-revalidate"
</FilesMatch>
```

### 9. Core Web Vitals Monitoring

**LCP (Largest Contentful Paint) — < 1.2s**
- Optimize hero image (hero image is usually LCP)
- Defer CSS, JS, fonts
- Use preload for critical resources

**FID (First Input Delay) — < 100ms**
- Defer JavaScript that blocks main thread
- Use `requestIdleCallback` for non-urgent tasks

**CLS (Cumulative Layout Shift) — < 0.1**
- Reserve space for images (`width`, `height` attributes)
- Reserve space for fonts (set `font-display: swap`)
- Avoid inserting content above the fold

**Monitoring code:**
```javascript
// Measure Web Vitals
import { getLCP, getFID, getCLS } from 'web-vitals';

getLCP(console.log);  // Log LCP
getFID(console.log);  // Log FID
getCLS(console.log);  // Log CLS

// Send to analytics
getLCP(metric => {
  fetch('/api/vitals', {
    method: 'POST',
    body: JSON.stringify(metric)
  });
});
```

### 10. Performance Testing Workflow

**Step 1: Local testing**

```bash
# Install Lighthouse CLI
npm install -g lighthouse

# Test with throttling (3G, 4 CPU cores)
lighthouse https://laverne.example.com \
  --output-path=./report.html \
  --throttle-cpu-slowdown=4 \
  --throttle-method=devtools
```

**Step 2: Monitor in production**

- Use Google Analytics 4 (Web Vitals collection)
- Use Sentry for performance monitoring
- Set up Lighthouse CI in GitHub Actions

**Step 3: Set performance budgets**

```javascript
// .lighthouserc.json
{
  "ci": {
    "collect": {
      "url": ["https://laverne.example.com"],
      "numberOfRuns": 5
    },
    "assert": {
      "preset": "lighthouse:recommended",
      "assertions": {
        "performance": ["error", { "minScore": 0.85 }],
        "accessibility": ["error", { "minScore": 0.90 }],
        "cumulative-layout-shift": ["error", { "maxNumericValue": 0.1 }],
        "largest-contentful-paint": ["error", { "maxNumericValue": 1200 }]
      }
    }
  }
}
```

### 11. Lighthouse Score Breakdown by Metric

Understanding how Lighthouse calculates Performance (0–100):

| Metric | Weight | Target | Impact on Score |
|--------|--------|--------|----------------|
| **LCP** (Largest Contentful Paint) | 25% | < 1.2s | Hero image = LCP element |
| **TBT** (Total Blocking Time) | 30% | < 150ms | JS bundles blocking main thread |
| **CLS** (Cumulative Layout Shift) | 15% | < 0.1 | Images without dimensions, fonts |
| **FCP** (First Contentful Paint) | 10% | < 0.8s | Critical CSS, server response |
| **Speed Index** | 10% | < 1.5s | Visual completeness over time |
| **TTI** (Time to Interactive) | 10% | < 2.5s | JS parse + execute time |

**Lighthouse score estimate:**

| Score | Color | Meaning |
|-------|-------|---------|
| 90–100 | 🟢 Green | Excellent |
| 50–89 | 🟠 Orange | Needs improvement |
| 0–49 | 🔴 Red | Poor |

**LAVERNE Targets: 85+ desktop, 80+ mobile**

### 12. Performance Budget by Device

| Resource | Mobile (3G) | Tablet (4G) | Desktop (WiFi) |
|----------|-------------|-------------|----------------|
| Hero image | < 100KB | < 150KB | < 200KB |
| Product images (×8) | < 40KB each | < 60KB each | < 80KB each |
| Total images | < 420KB | < 630KB | < 840KB |
| CSS (total) | < 30KB | < 40KB | < 50KB |
| JavaScript | < 60KB | < 80KB | < 100KB |
| Fonts | < 30KB | < 35KB | < 40KB |
| HTML | < 20KB | < 25KB | < 30KB |
| **Total budget** | **< 560KB** | **< 810KB** | **< 1.2MB** |

### 13. Real-World 3G Throttling Results

Test using Chrome DevTools Network throttle: **"Slow 3G" (400 Kbps download, 400ms RTT)**

**Before optimization (baseline):**
```
LCP:  4.8s  🔴
FCP:  2.1s  🔴
CLS:  0.32  🔴
TBT:  680ms 🔴
Score: 24   🔴
```

**After optimization (target):**
```
LCP:  1.1s  🟢
FCP:  0.7s  🟢
CLS:  0.04  🟢
TBT:  90ms  🟢
Score: 87   🟢
```

**Key actions that moved the needle most:**
1. Hero image: JPG 2.8MB → WebP 120KB (-96%) → LCP from 4.8s → 1.1s
2. Inline critical CSS (~3KB) → FCP from 2.1s → 0.7s
3. Add `width`/`height` on all images → CLS from 0.32 → 0.04
4. Defer non-critical JS → TBT from 680ms → 90ms

### 14. Cumulative Layout Shift (CLS) Prevention

CLS = visual instability. Every unexpected layout shift lowers your score.

**Image dimension reservation:**
```html
<!-- ✅ Always specify width and height — browser reserves space -->
<img
  src="/images/product.webp"
  alt="Blue Laverne 7am"
  width="600"
  height="600"
  loading="lazy"
/>

<!-- ❌ Missing dimensions — image load causes layout shift -->
<img src="/images/product.webp" alt="Blue Laverne 7am" loading="lazy" />
```

**Font loading CLS prevention:**
```css
/* Use size-adjust to match fallback font metrics */
@font-face {
  font-family: 'Playfair Display';
  src: url('/fonts/playfair-display-700.woff2') format('woff2');
  font-weight: 700;
  font-display: swap;
  size-adjust: 100%; /* adjust to match system serif metrics */
}

/* Fallback stack that closely matches Playfair Display */
h1, h2, h3 {
  font-family: 'Playfair Display', 'Georgia', 'Times New Roman', serif;
}
```

**Avoid inserting content above the fold:**
```javascript
// ❌ Never inject content above-fold via JS (causes CLS)
document.body.insertBefore(banner, document.body.firstChild);

// ✅ Pre-reserve space in HTML, then populate
// In HTML: <div class="promo-banner" style="min-height: 48px"></div>
// In JS: document.querySelector('.promo-banner').innerHTML = content;
```

**Cookie consent without CLS:**
```css
/* Fixed position — doesn't push page content */
.cookie-consent {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  /* Does NOT affect layout of page content above it */
}
```

### 15. Claude AI Integration — Intelligent Optimization

Use Claude API to generate context-aware optimization recommendations:

**Prompt pattern for image analysis:**
```javascript
// Example: Ask Claude to analyze and recommend image optimizations
const response = await fetch('https://api.anthropic.com/v1/messages', {
  method: 'POST',
  headers: {
    'x-api-key': process.env.ANTHROPIC_API_KEY, // Never client-side!
    'anthropic-version': '2023-06-01',
    'content-type': 'application/json'
  },
  body: JSON.stringify({
    model: 'claude-opus-4-5',
    max_tokens: 1024,
    messages: [{
      role: 'user',
      content: `Analyze these Lighthouse metrics for a luxury fragrance landing page 
and provide specific, actionable recommendations to achieve LCP < 1.2s:

Current metrics:
- LCP: ${lcpMs}ms (element: ${lcpElement})
- FCP: ${fcpMs}ms
- CLS: ${clsScore}
- TBT: ${tbtMs}ms

Page details:
- Hero image: ${heroImageKB}KB (${heroImageFormat})
- Total page weight: ${totalKB}KB
- JavaScript bundles: ${jsKB}KB

Provide 3 specific recommendations ordered by impact.`
    }]
  })
});
```

**Claude safe defaults for performance decisions:**
```markdown
When uncertain about a performance trade-off, Claude should:
1. Prioritize LCP (user-perceived load speed) over other metrics
2. Choose progressive enhancement over feature completeness
3. Recommend WebP + JPEG fallback (not AVIF — browser support gap)
4. Default to `loading="lazy"` for all below-fold images
5. Default to `defer` for all non-critical scripts
6. Never recommend removing accessibility features for performance gains
```

### 16. Chrome DevTools Performance Profiling Guide

**Step 1: Open Performance tab**
1. Open DevTools (F12 or Cmd+Option+I)
2. Go to "Performance" tab
3. Click ⚙️ Settings → check "Screenshots" and "Web Vitals"
4. Set CPU throttle: 4× slowdown (simulates mid-range mobile)
5. Click "Start profiling and reload page" (Ctrl+Shift+E)

**Step 2: Read the flame chart**
```
Main thread timeline:
[Parse HTML] → [Parse CSS] → [Layout] → [Paint] → [Composite]
                ↕
           [JavaScript tasks] (look for long red tasks > 50ms)
```

**Step 3: Identify bottlenecks**
```
🔴 Long task > 50ms → Bundle splitting or code optimization needed
🟡 Layout thrashing → Too many forced reflows (read/write DOM)
🟡 Render-blocking resources → CSS/JS in <head> without defer/preload
🔴 Large LCP element → Needs image optimization + preload
```

**Step 4: Fix render-blocking resources**
```html
<!-- Preload the LCP image (hero) -->
<link
  rel="preload"
  href="/images/hero-1440.webp"
  as="image"
  type="image/webp"
  media="(min-width: 768px)"
/>

<!-- Preload critical font -->
<link
  rel="preload"
  href="/fonts/playfair-display-700.woff2"
  as="font"
  type="font/woff2"
  crossorigin="anonymous"
/>
```

**Step 5: Measure, fix, repeat**
```bash
# Baseline: record LCP before changes
lighthouse https://laverne.example.com --output json > before.json

# Apply optimization
# ...

# Measure improvement
lighthouse https://laverne.example.com --output json > after.json

# Compare
node -e "
  const b = require('./before.json');
  const a = require('./after.json');
  const lcp = metric => metric.audits['largest-contentful-paint'].numericValue;
  console.log('LCP before:', Math.round(lcp(b)), 'ms');
  console.log('LCP after:', Math.round(lcp(a)), 'ms');
  console.log('Improvement:', Math.round((lcp(b) - lcp(a)) / lcp(b) * 100) + '%');
"
```

## Implementation Checklist

- [ ] Images optimized (WebP, multiple breakpoints, < 500KB total)
- [ ] Hero image: `fetchpriority="high"`, `<link rel="preload">` in `<head>`
- [ ] Product images: `loading="lazy"`, `decoding="async"`, explicit `width`/`height`
- [ ] Lazy loading implemented (native `loading="lazy"` + IntersectionObserver fallback)
- [ ] CSS critical path extracted (< 5KB inline, rest deferred)
- [ ] Fonts: woff2, `font-display: swap`, preloaded, < 40KB total
- [ ] JavaScript deferred (no render-blocking scripts)
- [ ] HTML minified (< 30KB)
- [ ] Gzip/Brotli compression enabled (server-side)
- [ ] Browser caching configured (long TTL for static assets)
- [ ] Core Web Vitals < targets (LCP < 1.2s, CLS < 0.1, TBT < 150ms)
- [ ] Lighthouse Performance > 85 desktop, > 80 mobile
- [ ] CLS prevention: all images have explicit dimensions
- [ ] Font CLS: `size-adjust` or careful fallback font stack
- [ ] 3G throttling test completed (4× CPU, Slow 3G network)
- [ ] Chrome DevTools performance profile reviewed (no long tasks > 50ms)
- [ ] Performance budget enforced per device tier (mobile < 560KB)
- [ ] A/B testing metrics tracked (load time per variant)
- [ ] GitHub Actions CI/CD with `.lighthouserc.json` budgets

## Performance Targets (Final)

| Metric | Mobile | Tablet | Desktop |
|--------|--------|--------|---------|
| **LCP** | < 1.2s | < 1.2s | < 1.0s |
| **FCP** | < 0.8s | < 0.8s | < 0.6s |
| **CLS** | < 0.1 | < 0.1 | < 0.05 |
| **TBT** | < 200ms | < 150ms | < 100ms |
| **Page size** | < 560KB | < 810KB | < 1.2MB |
| **Lighthouse** | > 80 | > 82 | > 85 |

## Related Skills

- `landing-page-hero-design` — Hero optimization
- `ecommerce-product-grid` — Grid lazy loading
- `shopify-integration` — Shopify CDN image optimization
