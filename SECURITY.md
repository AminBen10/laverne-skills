# Security Guide — LAVERNE Skills

Enterprise-grade security patterns for luxury e-commerce development. This document covers XSS prevention, template injection, CORS policy, GDPR compliance, and PCI compliance recommendations.

---

## 1. XSS Prevention — HTML & CSS Output Sanitization

Cross-Site Scripting (XSS) is the most common web vulnerability. In Shopify Liquid, **always escape dynamic output**.

### Liquid Escaping Rules

```liquid
<!-- ❌ VULNERABLE: Direct output -->
<h2>{{ product.title }}</h2>
<p>{{ customer.first_name }}</p>
{{ request.path }}

<!-- ✅ SECURE: Escaped output -->
<h2>{{ product.title | escape }}</h2>
<p>{{ customer.first_name | escape }}</p>
```

**Shopify Liquid escape filters:**

| Filter | Use Case | Example |
|--------|----------|---------|
| `\| escape` | Any text output in HTML | `{{ product.title \| escape }}` |
| `\| url_encode` | Values in URL parameters | `{{ tag \| url_encode }}` |
| `\| json` | Output data as JSON in `<script>` | `var p = {{ product \| json }};` |
| `\| strip_html` | Remove HTML from metafields | `{{ bio \| strip_html \| escape }}` |
| `\| metafield_tag` | Shopify-sanitized rich text | `{{ notes \| metafield_tag }}` |

### HTML Sanitization in JavaScript

If you must render HTML dynamically (e.g., fragrance notes from an API), use a sanitizer:

```javascript
// ✅ Safe HTML rendering — DOMPurify library
import DOMPurify from 'dompurify';

function renderFragranceNotes(rawHtml) {
  // Allowlist: only specific safe tags
  const clean = DOMPurify.sanitize(rawHtml, {
    ALLOWED_TAGS: ['span', 'em', 'strong', 'ul', 'li'],
    ALLOWED_ATTR: ['class']
  });
  document.getElementById('fragrance-notes').innerHTML = clean;
}

// ❌ NEVER: Direct innerHTML with unsanitized input
element.innerHTML = userInput; // XSS vulnerability
element.innerHTML = apiResponse.html; // XSS vulnerability
```

### CSS Injection Prevention

Never inject user-controlled values into `style` attributes or CSS:

```javascript
// ❌ VULNERABLE: CSS injection via user input
element.style.cssText = userInput;
element.setAttribute('style', `color: ${userInput}`);

// ✅ SECURE: Use CSS classes only, no dynamic style injection
element.classList.add('theme-navy');
element.dataset.color = sanitizedValue; // data attributes, not style
```

---

## 2. Shopify Liquid Template Injection Prevention

### Safe Variable Patterns

```liquid
<!-- ❌ NEVER: Outputting raw URL parameters in templates -->
{% assign query = request.params.q %}
<p>Buscando: {{ query }}</p>  <!-- XSS if query contains <script> -->

<!-- ✅ ALWAYS: Escape and validate before display -->
{% assign query = request.params.q | strip | escape %}
{% if query.size > 0 and query.size < 100 %}
  <p>Buscando: {{ query }}</p>
{% endif %}

<!-- ✅ For Liquid conditionals — compare values, don't output them -->
{% if request.params.variant == '100ml' %}
  {% assign selected_size = '100ml' %}  <!-- hardcoded safe value -->
{% endif %}
```

### Metafield Security

```liquid
<!-- ✅ Always validate metafield type before rendering -->
{% if product.metafields.custom.fragrance_notes.type == 'multi_line_text_field' %}
  {{ product.metafields.custom.fragrance_notes.value | escape }}
{% endif %}

<!-- ✅ For richtext metafields, use Shopify's sanitized renderer -->
{{ product.metafields.custom.hero_description | metafield_tag }}
```

---

## 3. Content Security Policy (CSP)

CSP headers prevent XSS by whitelisting allowed script/style sources.

### Recommended CSP for Shopify (Theme)

```
Content-Security-Policy:
  default-src 'self';
  script-src
    'self'
    'nonce-{RANDOM_NONCE}'
    https://cdn.shopify.com
    https://cdn.shopifycloud.com
    https://www.googletagmanager.com
    https://www.google-analytics.com;
  style-src
    'self'
    'unsafe-inline'
    https://fonts.googleapis.com
    https://cdn.shopify.com;
  img-src
    'self'
    data:
    blob:
    https://cdn.shopify.com
    https://*.shopifycdn.com
    https://www.google-analytics.com;
  font-src
    'self'
    https://fonts.gstatic.com
    https://cdn.shopify.com;
  connect-src
    'self'
    https://*.shopify.com
    https://api.shopify.com
    https://www.google-analytics.com;
  frame-src
    https://checkout.shopify.com;
  frame-ancestors 'none';
  base-uri 'self';
  form-action 'self' https://checkout.shopify.com;
```

### Implementing CSP in Shopify

```liquid
<!-- In layout/theme.liquid — add nonce to all inline scripts -->
{% assign csp_nonce = 'laverne-' | append: 'time' %}

<script nonce="{{ csp_nonce }}">
  // Inline scripts use nonce — matches CSP header
  window.laverneConfig = {
    currency: {{ shop.currency | json }},
    locale: {{ request.locale.iso_code | json }}
  };
</script>
```

> **Note:** Full CSP implementation requires Shopify Plus or a custom server layer. For standard Shopify themes, use `meta` CSP as a best-effort measure.

---

## 4. CORS Policy for Shopify API Calls

### Shopify's Built-in CORS

Shopify's Storefront API and REST API handle CORS on their end. You don't control it directly — but you should understand the boundaries:

```javascript
// ✅ SAFE: Shopify Storefront API (CORS enabled for your domain)
const response = await fetch(
  'https://your-store.myshopify.com/api/2024-01/graphql.json',
  {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'X-Shopify-Storefront-Access-Token': 'your-storefront-token'
      // Storefront Access Token is safe to use client-side (read-only)
    },
    body: JSON.stringify({ query: '{ shop { name } }' })
  }
);

// ❌ NEVER: Admin API from client-side (exposes admin credentials)
fetch('/admin/api/products.json', {
  headers: { 'X-Shopify-Access-Token': adminToken } // Security breach
});
```

### Secure Request Headers

```javascript
// Standard security headers for any Shopify API call
const secureHeaders = {
  'Content-Type': 'application/json',
  'X-Requested-With': 'XMLHttpRequest', // CSRF protection signal
  // Never add Authorization or admin tokens here
};
```

### Custom API (Node.js / Shopify Functions)

```javascript
// Server-side CORS policy for custom endpoints
const corsOptions = {
  origin: [
    'https://your-store.myshopify.com',
    'https://laverne.yourdomain.com'
  ],
  methods: ['GET', 'POST'],
  allowedHeaders: ['Content-Type', 'X-Shopify-Hmac-Sha256'],
  credentials: false // Don't send cookies cross-origin
};
```

---

## 5. API Key Management

### Rules — No Exceptions

1. **NEVER** hardcode API keys in any file committed to git
2. **NEVER** include API keys in client-side JavaScript or Liquid templates
3. **ALWAYS** use environment variables (`.env`) for development
4. **ALWAYS** use Shopify's private app secrets or encrypted environment variables in production

### `.env` Setup

```bash
# .env — local development only, NEVER commit
SHOPIFY_API_KEY=shppa_xxxxxxxxxxxxxxxxxxxxx
SHOPIFY_API_SECRET=shpss_xxxxxxxxxxxxxxxxxxxxx
SHOPIFY_STOREFRONT_TOKEN=1234567890abcdef  # Read-only, can expose client-side
SHOPIFY_ADMIN_TOKEN=shpat_xxxxxxxxxxxxx    # Admin, NEVER expose client-side
ANTHROPIC_API_KEY=sk-ant-xxxxxxxxxxxxxxxxxxxxx  # NEVER client-side
GTM_CONTAINER_ID=GTM-XXXXXXX  # Safe to expose

# .gitignore — ALWAYS include
.env
.env.*
!.env.example
```

### Correct Token Usage

| Token Type | Where to Use | Where NOT to Use |
|-----------|-------------|-----------------|
| Storefront Access Token | Client-side JS, Liquid | Admin operations |
| Admin Access Token | Server-side only | Client-side, Liquid templates |
| Anthropic API Key | Server-side only | NEVER client-side |
| GTM Container ID | Client-side, Liquid | N/A (public) |
| GA4 Measurement ID | Client-side, Liquid | N/A (public) |

---

## 6. GDPR & Data Privacy Compliance

### EU Customer Data Rules

For LAVERNE's European B2B customers (fragrance professionals):

**What requires consent:**
- Analytics cookies (Google Analytics 4, GTM)
- Marketing cookies (retargeting, Facebook Pixel)
- Session recording (Hotjar, Microsoft Clarity)

**What does NOT require consent:**
- Strictly necessary cookies (cart, session, security)
- Server-side analytics with IP anonymization

### Cookie Consent Implementation

```javascript
// Minimal GDPR-compliant cookie consent
const CONSENT_KEY = 'laverne_cookie_consent';

function hasCookieConsent() {
  return localStorage.getItem(CONSENT_KEY) === 'accepted';
}

function acceptCookies() {
  localStorage.setItem(CONSENT_KEY, 'accepted');
  loadAnalytics(); // Load GTM only after consent
  hideCookieBanner();
}

function rejectCookies() {
  localStorage.setItem(CONSENT_KEY, 'rejected');
  hideCookieBanner();
  // Do NOT load analytics, GTM, or tracking scripts
}

function loadAnalytics() {
  if (!hasCookieConsent()) return;

  // Load GTM only after explicit consent
  (function(w,d,s,l,i){
    w[l]=w[l]||[];
    w[l].push({'gtm.start': new Date().getTime(), event:'gtm.js'});
    var f=d.getElementsByTagName(s)[0],
        j=d.createElement(s),
        dl=l!='dataLayer'?'&l='+l:'';
    j.async=true;
    j.src='https://www.googletagmanager.com/gtm.js?id='+i+dl;
    f.parentNode.insertBefore(j,f);
  })(window,document,'script','dataLayer','GTM-XXXXXXX');
}

// Initialize
document.addEventListener('DOMContentLoaded', function() {
  const consent = localStorage.getItem(CONSENT_KEY);
  if (consent === 'accepted') {
    loadAnalytics();
  } else if (!consent) {
    showCookieBanner();
  }
  // If 'rejected', do nothing (no analytics)
});
```

### Privacy Policy Requirements

Your LAVERNE privacy policy must disclose:
- What data is collected (name, email, company, purchase history)
- Why it's collected (order processing, B2B communication)
- Who it's shared with (Shopify, payment processors)
- How long it's retained (7 years for financial records, 1 year for analytics)
- Customer rights (access, deletion, portability)
- Contact for data requests (DPO email or contact form)

---

## 7. PCI Compliance

### Shopify's Responsibility

Shopify handles PCI DSS Level 1 compliance for:
- Card data capture (never touches your theme code)
- Payment processing
- Secure checkout pages (`/checkout/*`)

**Your responsibility:**
- Never log, store, or transmit card data
- Never add custom JavaScript to checkout pages (violates PCI)
- Keep your Shopify theme updated (security patches)
- Use HTTPS everywhere (Shopify provides SSL automatically)

### Payment Security Checklist

- [ ] Shopify Payments enabled (PCI Level 1)
- [ ] HTTPS enforced (Shopify does this automatically)
- [ ] No card data captured in custom forms
- [ ] No custom JS on `/checkout/*` pages
- [ ] Admin API credentials stored server-side only
- [ ] Fraud prevention enabled (Shopify Fraud Analysis or Signifyd)
- [ ] Chargebacks monitored (via Shopify Payments dashboard)

---

## 8. Session Security

### Shopify Session Tokens

```liquid
{% comment %}
  Shopify manages session cookies securely:
  - HttpOnly: yes (no JS access)
  - Secure: yes (HTTPS only)
  - SameSite: Strict (CSRF protection)
  
  You don't need to manage session cookies — Shopify does.
  For custom session data, use:
  - customer.metafields (server-side, persistent)
  - sessionStorage (client-side, temporary, same tab only)
  - localStorage (client-side, persistent — only for non-sensitive data)
{% endcomment %}
```

**Safe client-side storage:**
```javascript
// ✅ Safe for non-sensitive data (A/B test assignment, UI preferences)
sessionStorage.setItem('laverne_ab_variant', 'control');
localStorage.setItem('laverne_cookie_consent', 'accepted');

// ❌ NEVER store in client-side storage:
// - Tokens, API keys, passwords
// - Payment information
// - Personal data (email, phone) without encryption
```

---

## 9. Security Audit Checklist

Run this checklist before every production deployment:

### Code Review
- [ ] All Liquid output uses `| escape` filter (grep for `{{ ` without `| escape`)
- [ ] No API keys, tokens, or passwords in any template file
- [ ] No `innerHTML =` with user-controlled data in JavaScript
- [ ] No inline event handlers with dynamic content (e.g., `onclick="{{ userInput }}"`)
- [ ] All form inputs have `name` attributes (for Shopify form handling)
- [ ] CSRF token present in all POST forms (`{% form %}` tag handles this)

### Configuration Review
- [ ] `.env` is in `.gitignore` and not committed
- [ ] Shopify Admin API credentials are NOT in theme files
- [ ] GTM is only loaded after cookie consent
- [ ] No tracking pixels fire before consent

### Dependency Review
- [ ] Third-party scripts loaded from CDN only (no unverified sources)
- [ ] All external scripts use Subresource Integrity (SRI) where possible

```html
<!-- SRI for external scripts (where CDN provides hash) -->
<script
  src="https://cdn.example.com/library.min.js"
  integrity="sha384-{hash}"
  crossorigin="anonymous"
></script>
```

---

## 10. Incident Response

If you suspect a security breach:

1. **Immediately**: Revoke all Shopify API keys in Admin > Apps > Manage private apps
2. **Immediately**: Change Shopify admin password + enable 2FA if not already active
3. **Within 1 hour**: Contact Shopify support to report the incident
4. **Within 24 hours**: Notify affected customers if personal data was exposed (GDPR requires this)
5. **Document**: Keep a record of what happened, when, and what was done

**Shopify Security Contact:** security@shopify.com
**GDPR Data Breach Notification:** Required within 72 hours to your local Data Protection Authority

---

*This security guide follows OWASP Top 10 recommendations, PCI DSS Level 1 guidelines, and EU GDPR Article 25 (Privacy by Design). Last updated: 2026.*
