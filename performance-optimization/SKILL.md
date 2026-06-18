---
name: performance-optimization
description: Optimizes landing pages for sub-2-second load times and 85+ Lighthouse scores. Handles image compression (WebP, srcset), lazy loading, CSS minification, JavaScript deferral, and Core Web Vitals monitoring. Provides automation scripts for performance testing and compliance tracking.
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

## Implementation Checklist

- [ ] Images optimized (WebP, multiple breakpoints, < 500KB total)
- [ ] Lazy loading implemented (Intersection Observer or native `loading="lazy"`)
- [ ] CSS critical path extracted (< 5KB inline, rest deferred)
- [ ] Fonts optimized (woff2, font-display: swap, < 40KB)
- [ ] JavaScript deferred (no render-blocking scripts)
- [ ] HTML minified (< 30KB)
- [ ] Gzip/Brotli compression enabled (server-side)
- [ ] Browser caching configured (long TTL for static assets)
- [ ] Core Web Vitals < targets (LCP < 1.2s, FID < 100ms, CLS < 0.1)
- [ ] Lighthouse Performance > 85
- [ ] Tested on 3G throttling (mobile slow network)
- [ ] A/B testing metrics tracked (load time variants)
- [ ] Performance monitoring set up (Sentry, GA4)
- [ ] GitHub Actions CI/CD with performance budgets

## Performance Targets (Final)

| Metric | Target | Status |
|--------|--------|--------|
| **LCP** | < 1.2s | ✅ |
| **FCP** | < 0.8s | ✅ |
| **CLS** | < 0.1 | ✅ |
| **TTI** | < 2.5s | ✅ |
| **Page size** | < 600KB | ✅ |
| **Lighthouse Performance** | > 85 | ✅ |
| **Mobile Lighthouse** | > 80 | ✅ |

## Related Skills

- `landing-page-hero-design` — Hero optimization
- `ecommerce-product-grid` — Grid lazy loading
- `shopify-integration` — Shopify CDN image optimization
