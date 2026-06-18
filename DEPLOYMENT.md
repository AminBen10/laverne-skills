# Phase 4: Deployment & Monitoring Guide

Complete guide for deploying LAVERNE skills to production and monitoring performance in real-time.

---

## 🎯 Phase 4 Overview

After completing Phases 1-3 (Design, Security, Performance), Phase 4 takes your LAVERNE landing page to production and ensures continuous optimization through data-driven monitoring.

### Phase 4 Timeline
```
Week 1: Pre-Deployment Validation
Week 2: Shopify Deployment
Week 3: Monitoring & Initial Metrics
Week 4+: Optimization & A/B Testing
```

---

## 📋 Part 1: Pre-Deployment Checklist

Before deploying to production, validate all systems across the 4 skills.

### 1.1 Landing Page Hero Design ✅

```bash
# Test hero rendering
- [ ] Hero renders at 1440px (desktop)
- [ ] Hero renders at 768px (tablet)
- [ ] Hero renders at 375px (mobile)
- [ ] Logo + "ORIENT FRAGANCE" visible
- [ ] "LAVERNE" headline gold (#D4AF37)
- [ ] "CATÁLOGO PROFESIONAL 2026" subtitle visible
- [ ] "TESTER GRATIS DESDE 24 UNIDADES" badge displays
- [ ] CTA button "Ver catálogo completo" clickable
- [ ] Micro-interactions smooth (fade-in 0.8s, hover 300ms)
- [ ] No layout shift (CLS < 0.1)
- [ ] Image loads < 1.2s (LCP target)
```

**Validation Command:**
```bash
lighthouse https://laverne.test/ \
  --output-path=./reports/hero-report.html \
  --throttle-cpu-slowdown=4
```

**Expected Results:**
- Performance: ≥ 85
- Accessibility: ≥ 90
- SEO: ≥ 90

### 1.2 E-commerce Product Grid ✅

```bash
# Test product grid rendering
- [ ] 8 products display correctly
- [ ] 3-column layout at 1440px desktop
- [ ] 2-column layout at 768px tablet
- [ ] 1-column layout at 375px mobile
- [ ] Product images load lazy (loading="lazy")
- [ ] Fragrance notes display per product
- [ ] Bulk pricing shows (€65 regular, €55 at ×12)
- [ ] "Añadir a cotización" CTA visible
- [ ] Hover states smooth (300ms transition)
- [ ] No image overlap on mobile
- [ ] Grid gap responsive (32px desktop, 16px mobile)
```

**Performance Check:**
```bash
# Test product grid performance
lighthouse https://laverne.test/products \
  --output-path=./reports/grid-report.html \
  --throttle-cpu-slowdown=4
```

**Expected Grid Metrics:**
- LCP < 1.5s (images lazy-loaded)
- Total page size < 600KB
- Images < 300KB total

### 1.3 Shopify Integration ✅

```bash
# Validate Shopify setup
- [ ] Shopify store connected
- [ ] 8 products uploaded with metafields
- [ ] fragrance_notes metafield configured
- [ ] bulk_price_12 metafield configured
- [ ] Liquid templates uploaded to theme
- [ ] Collection page shows products
- [ ] Add to cart functionality works
- [ ] Quote summary displays correctly
- [ ] GTM tracking firing events
- [ ] Currency displays in EUR
- [ ] Images optimized via Shopify CDN
```

**Shopify Validation:**
```bash
# Check Shopify API connectivity
curl -X GET "https://laverne.myshopify.com/admin/api/2024-01/products.json" \
  -H "X-Shopify-Access-Token: your_access_token"
```

### 1.4 Performance Optimization ✅

```bash
# Final performance validation
- [ ] Hero image < 150KB (WebP)
- [ ] Total images < 500KB
- [ ] CSS < 50KB (minified)
- [ ] JavaScript < 100KB (minified)
- [ ] Fonts < 40KB (woff2)
- [ ] HTML < 30KB (minified)
- [ ] LCP < 1.2s
- [ ] FCP < 0.8s
- [ ] CLS < 0.1
- [ ] Lighthouse Performance ≥ 85
- [ ] Mobile Lighthouse ≥ 80
- [ ] 3G throttle load time < 3s
```

**Complete Performance Test:**
```bash
# Run full performance suite
lighthouse https://laverne.test/ \
  --output-path=./reports/full-perf-report.html \
  --throttle-cpu-slowdown=4 \
  --throttle-method=devtools \
  --channel=chrome
```

### 1.5 Accessibility (WCAG 2.1 AAA) ✅

```bash
# Accessibility validation
- [ ] Color contrast ≥ 4.5:1 (text on background)
- [ ] Headings properly nested (H1 → H2 → H3)
- [ ] Images have alt text
- [ ] Buttons accessible via keyboard (Tab)
- [ ] Focus indicators visible
- [ ] Screen reader announces all content
- [ ] No flash rate > 3Hz
- [ ] Links underlined or bold
- [ ] Error messages descriptive
- [ ] Form labels associated with inputs
```

**Run Axe Accessibility Audit:**
```bash
# Install Axe CLI
npm install -g @axe-core/cli

# Run audit
axe https://laverne.test/ --output json > accessibility-report.json
```

### 1.6 Security Validation ✅

```bash
# Security checks
- [ ] No XSS vulnerabilities (input sanitized)
- [ ] No template injection (Liquid escaping)
- [ ] CORS headers configured
- [ ] CSP headers set (Content-Security-Policy)
- [ ] HTTPS enforced
- [ ] No sensitive data in client-side code
- [ ] API keys in environment variables
- [ ] Rate limiting configured
- [ ] Error messages don't leak info
```

**Security Header Check:**
```bash
curl -I https://laverne.example.com | grep -E "Content-Security-Policy|X-Frame-Options|Strict-Transport-Security"
```

---

## 🚀 Part 2: Shopify Deployment

### 2.1 Pre-Deployment Setup

#### Step 1: Prepare Shopify Store
```bash
# Create private app for Shopify API access
1. Shopify Admin → Apps → App settings
2. Create "LAVERNE Deployment" private app
3. Grant scopes: read/write products, read/write metafields
4. Generate access token
5. Store token in .env: SHOPIFY_ACCESS_TOKEN=xxxx
```

#### Step 2: Upload Product Data
```bash
# Upload 8 LAVERNE fragrances to Shopify
# Use Shopify CSV import or API

# CSV format:
Handle,Title,Price,Metafields_custom_fragrance_notes,Metafields_custom_bulk_price_12
blue-laverne,Blue Laverne 7am,65.00,"Top: Lemon, Heart: Lavender, Base: Patchouli",55.00
little-garden,Little Garden,65.00,"Top: Rose, Heart: Jasmine, Base: Sandalwood",55.00
# ... 6 more products
```

**Upload via API:**
```bash
curl -X POST "https://laverne.myshopify.com/admin/api/2024-01/products.json" \
  -H "X-Shopify-Access-Token: $SHOPIFY_ACCESS_TOKEN" \
  -d @products.json
```

#### Step 3: Configure Metafields
```bash
# Create metafield definitions in Shopify Admin

# fragrance_notes
- Namespace: custom
- Key: fragrance_notes
- Type: richtext
- Display: Text area

# bulk_price_12
- Namespace: custom
- Key: bulk_price_12
- Type: money
- Display: Money input
```

### 2.2 Deploy Liquid Templates

#### Step 1: Upload Theme Files
```bash
# Using Shopify Theme Kit
npm install -g @shopify/theme

# Login to Shopify
shopify theme auth

# Download current theme
shopify theme pull

# Add LAVERNE templates to theme
cp landing-page-hero-design/templates/hero.liquid theme/sections/
cp ecommerce-product-grid/templates/product-grid.liquid theme/sections/
cp shopify-integration/templates/collection.liquid theme/templates/
cp shopify-integration/templates/quote-summary.liquid theme/sections/

# Upload updated theme
shopify theme push
```

#### Step 2: Deploy Collection Page
```liquid
<!-- theme/templates/collection.liquid -->

<section class="collection-page">
  <header class="collection-header">
    <h1>{{ collection.title }}</h1>
    <p>{{ collection.description }}</p>
  </header>

  <div class="product-grid">
    {% for product in collection.products %}
      {% render 'product-card', product: product %}
    {% endfor %}
  </div>
</section>
```

#### Step 3: Set Homepage to Show Hero
```liquid
<!-- theme/sections/hero.liquid -->
<!-- Hero section from landing-page-hero-design skill -->
<!-- Deployed and activated on homepage -->
```

### 2.3 Configure Analytics & Tracking

#### Step 1: Setup Google Analytics 4
```javascript
<!-- Add to theme.liquid <head> -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

#### Step 2: Setup Google Tag Manager
```bash
# Create GTM container for LAVERNE
1. Google Tag Manager → Create Container
2. Container ID: GTM-XXXXXXX
3. Add to theme <head>:
<!-- Google Tag Manager -->
<script>
(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-XXXXXXX');
</script>
```

#### Step 3: Configure Events
```liquid
<!-- Track "add_to_quote" event -->
<script>
function trackAddToQuote(productId, variantId, price) {
  dataLayer.push({
    'event': 'add_to_quote',
    'ecommerce': {
      'add': {
        'products': [{
          'id': productId,
          'name': '{{ product.title }}',
          'price': price,
          'variant': variantId,
          'quantity': 1
        }]
      }
    }
  });
}
</script>
```

### 2.4 Performance Monitoring Setup

#### Step 1: Enable Shopify Analytics
```bash
# In Shopify Admin:
1. Go to Analytics & reports
2. Enable Core Web Vitals tracking
3. Connect to Google Analytics 4
```

#### Step 2: Setup Sentry Error Tracking
```javascript
// Add to theme.liquid
<script src="https://browser.sentry-cdn.com/7.0.0/bundle.min.js"></script>
<script>
  Sentry.init({
    dsn: "https://xxxxxxx@oxxxxxx.ingest.sentry.io/yyyyyy",
    tracesSampleRate: 0.1,
    environment: "production"
  });
</script>
```

---

## 📊 Part 3: Production Monitoring

### 3.1 Real-Time Performance Dashboard

Setup Lighthouse CI with GitHub Actions:

```yaml
# .github/workflows/lighthouse-ci.yml

name: Lighthouse CI

on:
  push:
    branches: [main]
  schedule:
    - cron: '0 0 * * *'  # Daily at midnight

jobs:
  lighthouse:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Run Lighthouse CI
        uses: treosh/lighthouse-ci-action@v10
        with:
          uploadArtifacts: true
          temporaryPublicStorage: true
          configPath: './.lighthouserc.json'
```

**Lighthouse CI Config (.lighthouserc.json):**
```json
{
  "ci": {
    "collect": {
      "url": [
        "https://laverne.example.com",
        "https://laverne.example.com/products",
        "https://laverne.example.com/collections/fragrance"
      ],
      "numberOfRuns": 5,
      "settings": {
        "chromeFlags": "--throttle-cpu-slowdown=4"
      }
    },
    "assert": {
      "preset": "lighthouse:recommended",
      "assertions": {
        "categories:performance": ["error", { "minScore": 0.85 }],
        "categories:accessibility": ["error", { "minScore": 0.90 }],
        "largest-contentful-paint": ["error", { "maxNumericValue": 1200 }],
        "cumulative-layout-shift": ["error", { "maxNumericValue": 0.1 }],
        "first-input-delay": ["error", { "maxNumericValue": 100 }]
      }
    },
    "upload": {
      "target": "temporary-public-storage"
    }
  }
}
```

### 3.2 Core Web Vitals Monitoring

Setup Web Vitals collection in Shopify:

```javascript
// Add to theme.liquid
<script src="https://unpkg.com/web-vitals@3/dist/web-vitals.iife.js"></script>
<script>
  // Collect Core Web Vitals
  const vitals = {};
  
  web_vitals.getLCP(metric => {
    vitals.lcp = metric.value;
    sendToAnalytics(metric);
  });
  
  web_vitals.getFID(metric => {
    vitals.fid = metric.value;
    sendToAnalytics(metric);
  });
  
  web_vitals.getCLS(metric => {
    vitals.cls = metric.value;
    sendToAnalytics(metric);
  });

  function sendToAnalytics(metric) {
    if (navigator.sendBeacon) {
      const data = JSON.stringify(metric);
      navigator.sendBeacon('/api/vitals', data);
    }
  }
</script>
```

### 3.3 Performance Alerts

Setup alerts for performance degradation:

```bash
# Using Google Analytics 4 + Slack Integration

1. Create GA4 alert for LCP > 1.5s
2. Set threshold: If LCP exceeds 1.5s, trigger alert
3. Send to Slack: #laverne-performance-alerts
4. Include: Page URL, metric value, 7-day trend
```

**Alert Configuration:**
```
Alert Name: LCP Performance Degradation
Metric: Largest Contentful Paint
Condition: > 1500ms
Duration: 5 minutes
Notification: Slack webhook
```

### 3.4 Error Tracking & Debugging

Monitor errors with Sentry:

```javascript
// In Sentry Dashboard:
1. Create release: v1.0.0 (matching git tag)
2. Track errors in production
3. Set up alerts for error spike (5+ errors in 5 min)
4. Monitor JavaScript errors, network errors, custom events
```

**Custom Error Tracking:**
```javascript
// Track custom events
Sentry.captureMessage('Product added to quote', 'info', {
  productId: 123,
  price: 55.00,
  quantity: 12
});

// Track performance slow-downs
Sentry.captureMessage('Slow page load', 'warning', {
  lcp: 1800,
  fcp: 1200,
  cls: 0.05
});
```

---

## 🔄 Part 4: Post-Launch Optimization

### 4.1 Collect Initial Metrics (Week 1-2)

#### A/B Testing Setup

**Test 1: CTA Button Copy**
```liquid
{% assign ab_test_group = customer.metafields.custom.ab_test_group %}

{% if ab_test_group == 'variant_a' %}
  <button>Ver catálogo completo</button>
{% elsif ab_test_group == 'variant_b' %}
  <button>Explorar fragancias premium</button>
{% else %}
  <!-- Default: 50/50 split assignment -->
  <button>Ver catálogo completo</button>
{% endif %}
```

**Track Results:**
```javascript
dataLayer.push({
  event: 'view_cta',
  cta_variant: ab_test_group,
  cta_text: button_text
});
```

**Test 2: Hero Image**
- Variant A: Current lifestyle image (man with fragrance)
- Variant B: Product-focused image (bottles close-up)
- Metric: Click-through rate to products

**Test 3: Pricing Display**
- Variant A: "€65 | €55 at ×12 units"
- Variant B: "Regular €65 | Bulk €55 (×12)"
- Metric: Add-to-quote conversion

### 4.2 Analyze Metrics (Week 2-3)

**Key Metrics Dashboard:**
```
Performance:
- LCP: ___ ms (target < 1200)
- FCP: ___ ms (target < 800)
- CLS: ___ (target < 0.1)
- Lighthouse Performance: ___/100 (target ≥ 85)

Conversion:
- Hero click-through: __% (target > 5%)
- Product page views: ___
- Add-to-quote conversions: __% (target > 2%)
- Quote submissions: ___

Traffic:
- Unique visitors: ___
- Page views: ___
- Avg session duration: ___ sec
- Bounce rate: __% (target < 50%)
```

### 4.3 Optimize Based on Data (Week 3-4)

**If LCP is slow (> 1.2s):**
```bash
# 1. Check image sizes
- Verify hero image < 150KB
- Ensure WebP serving correctly
- Check Shopify CDN caching

# 2. Optimize CSS/JS
- Minify critical CSS
- Defer non-critical JavaScript
- Enable Brotli compression

# 3. Re-run Lighthouse
lighthouse https://laverne.example.com
```

**If conversion is low (< 2%):**
```bash
# 1. Check CTA button
- Increase button size (48px minimum)
- Change color (more contrast)
- Add hover animation

# 2. Check form friction
- Reduce fields in quote request
- Add progress indicator
- Mobile test add-to-quote flow

# 3. A/B test variations
- Test button text (3 variants)
- Test button position (hero, sticky header, modal)
- Test CTA color (gold, white, accent)
```

### 4.4 Continuous Monitoring (Week 4+)

**Monthly Checklist:**
```
□ Review Lighthouse scores (desktop & mobile)
□ Check Core Web Vitals from GA4
□ Analyze A/B test results
□ Review error logs (Sentry)
□ Check performance trends
□ Optimize slow pages
□ Update product inventory
□ A/B test new variations
```

**Quarterly Deep-Dive:**
```
□ Run full security audit
□ Update dependencies
□ Refactor slow templates
□ Expand product catalog
□ Launch new campaigns
□ Analyze competitor performance
□ Plan next feature release
```

---

## 📞 Troubleshooting

### Common Issues & Solutions

#### Issue: LCP > 1.5s after deployment
**Solution:**
```bash
# 1. Check image size
ls -lh images/hero*.webp  # Should be < 150KB

# 2. Check Shopify CDN caching
curl -I https://cdn.shopify.com/hero.webp | grep Cache-Control

# 3. Enable Brotli compression
# In Shopify Admin → Settings → Files → Compression

# 4. Re-run Lighthouse
lighthouse https://laverne.example.com --throttle-cpu-slowdown=4
```

#### Issue: Mobile Lighthouse < 80
**Solution:**
```bash
# 1. Check mobile CSS media queries
grep "@media (max-width:" theme/sections/hero.liquid

# 2. Ensure fonts load fast
# Set font-display: swap in @font-face

# 3. Defer JavaScript
# Add defer attribute to all non-critical scripts

# 4. Test on real mobile device
# Use Chrome DevTools throttling + real 4G network
```

#### Issue: Conversion rate < 1%
**Solution:**
```bash
# 1. Check button accessibility
# Test with keyboard navigation and screen reader

# 2. Verify analytics tracking
# Check GTM events firing in browser console:
console.log(dataLayer)

# 3. Test form submission
# Manually submit quote form, check success page

# 4. Review user session recordings
# Use Clarity/SessionStack to watch user interactions
```

---

## ✅ Phase 4 Completion Checklist

- [ ] All pre-deployment tests passing
- [ ] Shopify store fully configured
- [ ] Products uploaded with metafields
- [ ] Liquid templates deployed
- [ ] Analytics tracking working
- [ ] Lighthouse CI running on GitHub Actions
- [ ] Core Web Vitals monitoring active
- [ ] Error tracking (Sentry) enabled
- [ ] A/B testing framework setup
- [ ] Performance alerts configured
- [ ] Team trained on deployment process
- [ ] Documentation updated
- [ ] Go-live approved
- [ ] 24/7 monitoring enabled

---

## 🎉 You're Ready for Production!

After completing Phase 4, your LAVERNE skills are production-ready with:
- ✅ Sub-2-second load times
- ✅ 85+ Lighthouse scores
- ✅ WCAG 2.1 AAA accessibility
- ✅ Enterprise security
- ✅ Real-time monitoring
- ✅ Data-driven A/B testing

**Next Steps:**
1. Deploy to production
2. Monitor metrics for 1 week
3. Analyze A/B test results
4. Optimize conversions
5. Plan Phase 5 features
