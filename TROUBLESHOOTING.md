# Troubleshooting Guide — LAVERNE Skills

Solutions to common issues encountered when using LAVERNE skills in development and production.

---

## Table of Contents

1. [Lighthouse & Performance](#1-lighthouse--performance)
2. [Shopify Liquid Issues](#2-shopify-liquid-issues)
3. [Image Optimization](#3-image-optimization)
4. [Responsive Design Breakpoints](#4-responsive-design-breakpoints)
5. [Accessibility Failures](#5-accessibility-failures)
6. [Security Issues](#6-security-issues)
7. [A/B Testing Issues](#7-ab-testing-issues)
8. [Shopify Metafields](#8-shopify-metafields)

---

## 1. Lighthouse & Performance

### Problem: Lighthouse Performance Score < 70

**Symptoms:** Score in red/orange range, multiple failing audits

**Root causes (in order of impact):**

1. **Unoptimized hero image** (biggest impact)
   ```bash
   # Check actual image size:
   curl -sI https://your-store.com/images/hero.jpg | grep content-length
   # Anything > 300KB on desktop is too large
   ```
   **Fix:** Convert to WebP and use `srcset`:
   ```html
   <picture>
     <source srcset="hero-640.webp 640w, hero-1440.webp 1440w" type="image/webp" sizes="100vw" />
     <img src="hero-1440.jpg" width="1440" height="810" fetchpriority="high" alt="..." />
   </picture>
   ```

2. **Render-blocking resources** (CSS/JS in `<head>` without `defer`)
   ```html
   <!-- ❌ Render-blocking -->
   <link rel="stylesheet" href="/css/style.css">
   <script src="/js/app.js"></script>

   <!-- ✅ Non-blocking -->
   <style>/* critical CSS inline — < 5KB */</style>
   <link rel="preload" href="/css/style.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
   <script defer src="/js/app.js"></script>
   ```

3. **Missing image dimensions** (causes CLS)
   ```html
   <!-- ❌ No dimensions = layout shift = CLS score drops -->
   <img src="product.webp" loading="lazy">

   <!-- ✅ Explicit dimensions -->
   <img src="product.webp" width="600" height="600" loading="lazy">
   ```

---

### Problem: LCP (Largest Contentful Paint) > 2.5s

**Diagnosis:** Open Chrome DevTools → Performance → Record page load → Look for LCP marker

**Common causes:**
- Hero image not preloaded
- Hero image served as JPEG without WebP
- Hero image > 200KB

**Fix:**
```html
<!-- In <head> — preload the LCP element -->
<link
  rel="preload"
  href="/images/hero-1440.webp"
  as="image"
  type="image/webp"
  media="(min-width: 768px)"
/>
<link
  rel="preload"
  href="/images/hero-640.webp"
  as="image"
  type="image/webp"
  media="(max-width: 767px)"
/>

<!-- On the image itself -->
<img fetchpriority="high" decoding="async" ... />
```

---

### Problem: CLS (Cumulative Layout Shift) > 0.1

**Diagnosis:** Open Chrome DevTools → Performance → look for layout shift events (purple bars)

**Most common causes:**

1. **Images without width/height attributes**
   ```html
   <!-- Fix: always add explicit dimensions -->
   <img src="product.webp" width="600" height="600" ... />
   ```

2. **Custom font swap causing text reflow**
   ```css
   /* Fix: add size-adjust to prevent font swap shift */
   @font-face {
     font-family: 'Playfair Display';
     font-display: swap;
     size-adjust: 102%; /* tune to match system serif */
   }
   ```

3. **Dynamically injected banner above the fold**
   ```css
   /* Fix: pre-reserve space in CSS, even when empty */
   .announcement-bar {
     min-height: 40px; /* reserve even before content loads */
   }
   ```

4. **Cookie consent banner pushing content down**
   ```css
   /* Fix: use position: fixed — doesn't affect document flow */
   .cookie-consent {
     position: fixed;
     bottom: 0;
     /* NOT position: relative or static */
   }
   ```

---

### Problem: Lighthouse Accessibility Score < 90

**Most common failures:**

| Failure | Fix |
|---------|-----|
| Missing `alt` on images | Add descriptive `alt` text to every `<img>` |
| Buttons without accessible names | Add `aria-label` or visible text inside `<button>` |
| Low color contrast | Use navy (#1A3A52) on gold (#D4AF37) — never gold on white |
| Missing form labels | Add `<label for="id">` for every `<input>` |
| `tabindex > 0` | Remove positive tabindex — use DOM order instead |
| `outline: none` on focus | Remove this — never suppress focus styles |

---

## 2. Shopify Liquid Issues

### Problem: Metafield not displaying

**Symptoms:** `{{ product.metafields.custom.fragrance_notes }}` outputs empty

**Root causes:**
1. Metafield not saved to the product
2. Wrong namespace/key
3. Wrong Liquid syntax for the metafield type

**Diagnosis:**
```liquid
{% comment %} Debug: output all metafields for a product {% endcomment %}
{% for field in product.metafields.custom %}
  {{ field | json }}
{% endfor %}
```

**Fix by metafield type:**
```liquid
{% comment %} For single_line_text_field or number_integer: {% endcomment %}
{{ product.metafields.custom.bottle_size_ml.value | escape }}

{% comment %} For money type: {% endcomment %}
{{ product.metafields.custom.bulk_price_12 | money }}

{% comment %} For multi_line_text_field: {% endcomment %}
{{ product.metafields.custom.fragrance_notes.value | escape | newline_to_br }}

{% comment %} For richtext (json_string): use metafield_tag {% endcomment %}
{{ product.metafields.custom.hero_description | metafield_tag }}
```

---

### Problem: Bulk pricing not showing

**Symptoms:** Only regular price shows, bulk pricing invisible

**Checklist:**
1. ✅ Metafield `custom.bulk_price_12` exists on the product in Shopify Admin
2. ✅ The metafield type is `money` (not `number_decimal`)
3. ✅ Liquid condition is correct

```liquid
{% comment %} Correct check for money metafield {% endcomment %}
{% if product.metafields.custom.bulk_price_12 != blank %}
  {{ product.metafields.custom.bulk_price_12 | money }}
{% endif %}

{% comment %} ❌ This won't work for money type: {% endcomment %}
{% if product.metafields.custom.bulk_price_12 > 0 %}
```

---

### Problem: Images not lazy loading in Liquid

**Symptoms:** All product images load at page load (slow)

**Fix:**
```liquid
{% comment %} ❌ Missing loading="lazy" {% endcomment %}
<img src="{{ product.featured_image | image_url: width: 500 }}" />

{% comment %} ✅ With lazy loading and explicit dimensions {% endcomment %}
<img
  src="{{ product.featured_image | image_url: width: 500 }}"
  srcset="
    {{ product.featured_image | image_url: width: 300 }} 300w,
    {{ product.featured_image | image_url: width: 500 }} 500w,
    {{ product.featured_image | image_url: width: 800 }} 800w
  "
  sizes="(max-width: 767px) 100vw, (max-width: 1199px) 50vw, 33vw"
  alt="{{ product.featured_image.alt | escape }}"
  loading="lazy"
  decoding="async"
  width="500"
  height="500"
/>
```

---

### Problem: A/B test not assigning variant

**Symptoms:** All users see the same CTA text, no variant differentiation

**Diagnosis:**
```javascript
// Check in browser console
console.log(sessionStorage.getItem('laverne_ab_cta'));
// Should return: 'control', 'variant_b', or 'variant_c'
```

**Fix:** Ensure script runs after DOM is ready:
```javascript
// ❌ Running before DOM ready
applyCTAVariant(); // document.querySelectorAll may return empty

// ✅ Wait for DOM
document.addEventListener('DOMContentLoaded', applyCTAVariant);
```

---

## 3. Image Optimization

### Problem: WebP images not loading in older browsers

**Symptoms:** Images show broken in Safari < 14 or Edge < 18

**Fix:** Always use `<picture>` element with JPEG fallback:
```html
<picture>
  <source type="image/webp" srcset="image.webp" />
  <!-- JPEG fallback — always provide this -->
  <img src="image.jpg" alt="..." />
</picture>
```

---

### Problem: Hero image loads slowly on mobile

**Symptoms:** Mobile LCP > 2s even with WebP

**Checklist:**
1. ✅ Mobile-specific WebP image at < 100KB (640px wide)
2. ✅ `<link rel="preload">` for mobile image with `media="(max-width: 767px)"`
3. ✅ `fetchpriority="high"` on the `<img>` element
4. ✅ Image is not loaded via CSS `background-image` (can't preload background images easily)

```html
<!-- ✅ Correct mobile preload setup -->
<head>
  <link rel="preload" href="hero-640.webp" as="image" media="(max-width: 767px)" />
  <link rel="preload" href="hero-1440.webp" as="image" media="(min-width: 768px)" />
</head>
```

---

### Problem: Product images causing layout shift when lazy loading

**Symptoms:** Grid layout jumps as images load

**Root cause:** Missing `width`/`height` attributes + no CSS aspect ratio reserve

**Fix:**
```html
<!-- Option 1: Explicit width/height (simplest) -->
<img src="product.webp" width="600" height="600" loading="lazy" alt="..." />

<!-- Option 2: CSS aspect-ratio container -->
<div style="aspect-ratio: 1 / 1; background: #F5F5F5;">
  <img src="product.webp" loading="lazy" alt="..." style="width:100%; height:100%; object-fit:cover;" />
</div>
```

---

## 4. Responsive Design Breakpoints

### Problem: Product grid shows 2 columns on mobile

**Symptoms:** Cards appear too small and cramped on mobile (< 375px)

**Root cause:** Using `auto-fill` or `minmax` instead of explicit breakpoints

**Fix:**
```css
/* ❌ Unpredictable — may show 2 columns on mobile */
.product-grid {
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
}

/* ✅ Explicit breakpoints — predictable luxury layout */
.product-grid {
  grid-template-columns: repeat(3, 1fr); /* desktop default */
}

@media (max-width: 1199px) {
  .product-grid { grid-template-columns: repeat(2, 1fr); }
}

@media (max-width: 767px) {
  .product-grid { grid-template-columns: 1fr; } /* single column on mobile */
}
```

---

### Problem: Hero section too tall on mobile with virtual keyboard open

**Symptoms:** On iOS Safari with keyboard open, `100vh` is taller than visible viewport

**Fix:**
```css
/* ✅ Use svh (small viewport height) for mobile - available in modern browsers */
.hero {
  min-height: 100vh;           /* fallback */
  min-height: 100svh;          /* modern browsers — accounts for chrome UI */
}

/* Alternative: cap hero height on mobile */
@media (max-width: 767px) {
  .hero {
    min-height: 90vh;          /* leave room for browser chrome */
  }
}
```

---

## 5. Accessibility Failures

### Problem: Gold text fails color contrast check

**Symptoms:** Accessibility tools report contrast failure on `#D4AF37` text

**Root cause:** Gold (#D4AF37) on white (#FFFFFF) has only 2.3:1 contrast — fails WCAG AA (requires 4.5:1)

**Fix — Never use gold text on white:**
```css
/* ❌ Gold text on white — fails WCAG AA */
.price-bulk { color: #D4AF37; background: #FFFFFF; }

/* ✅ Gold text on navy — 7.1:1 ratio (WCAG AAA) */
.hero__cta { color: #D4AF37; background: #1A3A52; }

/* ✅ For gold accent on white background — use gold as a border or bg, navy as text */
.trust-badge {
  color: #1A3A52;        /* navy text on gold background */
  background: #D4AF37;
  border: 1px solid #D4AF37;
}
```

---

### Problem: CTA button not reachable by keyboard

**Symptoms:** Tab key skips the button, screen readers can't find it

**Root cause:** Using `<div>` or `<span>` as buttons

**Fix:**
```html
<!-- ❌ Div used as button — not keyboard accessible -->
<div class="btn" onclick="handleClick()">Añadir a cotización</div>

<!-- ✅ Native button — keyboard accessible by default -->
<button class="product-card__cta" type="button" aria-label="Añadir Blue Laverne a cotización">
  Añadir a cotización
</button>

<!-- ✅ Or a link for navigation actions -->
<a href="/catalogo" class="hero__cta" role="button">
  Ver catálogo completo
</a>
```

---

## 6. Security Issues

### Problem: XSS via Liquid output

**Symptoms:** User-controlled data appears unescaped in page HTML; could execute scripts

**Detection:**
```bash
# Search your Liquid files for unescaped output
grep -rn "{{ " --include="*.liquid" | grep -v "| escape" | grep -v "| json" | grep -v "| metafield_tag" | grep -v "| money" | grep -v "| image_url" | grep -v "| url"
```

**Fix:**
```liquid
<!-- Add | escape to every {{ variable }} output -->
{{ product.title | escape }}
{{ customer.first_name | escape }}
{{ collection.description | escape }}
{{ settings.custom_text | escape }}
```

---

### Problem: API key exposed in client-side code

**Symptoms:** Shopify Admin API key visible in page source or network requests

**Fix:**
1. Immediately rotate the API key in Shopify Admin > Apps > Private Apps
2. Move the key to server-side environment variables
3. Never expose Admin API keys in Liquid, JavaScript, or theme settings

---

### Problem: GTM firing before cookie consent

**Symptoms:** Analytics events appear before user accepts cookies (GDPR violation)

**Fix:** Wrap GTM initialization in consent check:
```javascript
function loadGTM() {
  if (localStorage.getItem('laverne_cookie_consent') !== 'accepted') return;
  
  // Only load GTM if consent was given
  (function(w,d,s,l,i){...})(window, document, 'script', 'dataLayer', 'GTM-XXXXXXX');
}

// Don't call loadGTM() on page load — only after consent
document.querySelector('.cookie-consent__accept')?.addEventListener('click', function() {
  localStorage.setItem('laverne_cookie_consent', 'accepted');
  loadGTM();
});
```

---

## 7. A/B Testing Issues

### Problem: A/B test shows different variants on page refresh

**Symptoms:** User sees "Añadir a cotización" on one visit, "Solicitar muestra" on the next

**Root cause:** Using `Math.random()` without persisting the assignment

**Fix:** Store assignment in `sessionStorage`:
```javascript
function getABVariant() {
  const stored = sessionStorage.getItem('laverne_ab_cta');
  if (stored) return stored; // return same variant for this session

  const variants = ['control', 'variant_b', 'variant_c'];
  const assigned = variants[Math.floor(Math.random() * variants.length)];
  sessionStorage.setItem('laverne_ab_cta', assigned);
  return assigned;
}
```

---

### Problem: A/B test not tracked in GTM

**Symptoms:** GA4 shows no `ab_assignment` events

**Checklist:**
1. GTM container is loaded (check with: `console.log(window.dataLayer)`)
2. `window.dataLayer` push happens after GTM loads (not before)
3. GA4 has a custom event trigger for `ab_assignment`

**Fix:**
```javascript
// Push to dataLayer — if GTM not loaded yet, it queues automatically
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  event: 'ab_assignment',
  ab_test: 'cta_text_v1',
  ab_variant: getABVariant()
});
```

---

## 8. Shopify Metafields

### Problem: Metafield shows raw JSON instead of formatted value

**Symptoms:** `{"value": "Lavender · Rose", "type": "single_line_text_field"}` displays instead of text

**Root cause:** Outputting the whole metafield object instead of `.value`

**Fix:**
```liquid
{% comment %} ❌ Outputs the whole metafield object as JSON {% endcomment %}
{{ product.metafields.custom.fragrance_notes }}

{% comment %} ✅ Access .value for text fields {% endcomment %}
{{ product.metafields.custom.fragrance_notes.value | escape }}

{% comment %} ✅ For money fields, no .value needed {% endcomment %}
{{ product.metafields.custom.bulk_price_12 | money }}
```

---

### Problem: Metafield shows but doesn't update after admin save

**Symptoms:** Changing a metafield value in Shopify Admin doesn't reflect on the storefront

**Fix:**
1. Clear Shopify's CDN cache (via Shopify Admin > Online Store > Themes > ⋯ > Publish)
2. Or append a cache-busting query: test with `?nocache=1` (development only)
3. Check if your theme uses page caching — metafield changes may take up to 5 minutes

---

## Need More Help?

If your issue isn't covered here:

1. **Shopify documentation**: [shopify.dev](https://shopify.dev)
2. **Shopify Community forums**: [community.shopify.com](https://community.shopify.com)
3. **Lighthouse docs**: [developer.chrome.com/docs/lighthouse](https://developer.chrome.com/docs/lighthouse/)
4. **WCAG guidelines**: [w3.org/WAI/WCAG21](https://www.w3.org/WAI/WCAG21/)
5. **Open an issue**: [github.com/AminBen10/laverne-skills/issues](https://github.com/AminBen10/laverne-skills/issues)

---

*This troubleshooting guide is maintained by AminBen10. Submit fixes via PR.*
