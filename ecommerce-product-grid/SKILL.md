---
name: ecommerce-product-grid
description: Skill para generar grids de productos premium para LAVERNE: tarjetas de producto sofisticadas, precios por volumen, imágenes optimizadas y salida HTML/CSS y Liquid lista para Shopify. Enfoque en UX de lujo, accesibilidad y performance.
license: MIT
version: 1.0.0
tags: [products, grid, ecommerce, liquid, claude-code, canva, higgsfield]
dependencies: [landing-page-hero-design:>=1.0.0]
outputs_schema: |
  {
    "type": "object",
    "properties": {
      "files": {"type":"array","items":{"type":"object"}},
      "assets": {"type":"array","items":{"type":"object"}},
      "preview_html": {"type":"string"}
    },
    "required": ["files","assets","preview_html"]
  }
---

# Skill: Ecommerce Product Grid (LAVERNE)

Resumen

Este skill instruye a Claude Code a generar un grid de productos premium, con tarjetas de producto listas para Shopify (Liquid) y/o HTML estático. Diseñado para marcas de lujo: espacio, tipografía, fotografía, micro‑interacciones y presentación de precios por volumen (€/unit at ×12).

Objetivos clave
- Tarjetas de producto premium (imagen, nombre, notas, precios, CTA)
- Mostrar bulk pricing de forma elegante (p.ej. €65 → €55 at ×12)
- Alta performance (WebP, srcset, lazy load, LQIP)
- Accesibilidad WCAG 2.1 AA/AAA donde aplique
- Salida estructurada consumible por pipelines (Higgsfield) y editores (Canva)

Formato de salida
- JSON con: files[] (product-grid.html, product-card.liquid, product-grid.css), assets[], preview_html
- Cuando se pida output_format "liquid" devolver plantilla Liquid y componente product-card.liquid

Parámetros de entrada (prompt-friendly)
- products (array) — cada producto: {handle, title, price_cents, bulk_price_12_cents, fragrance_notes, featured_image_description}
- layout (string) — "3col"|"2col"|"masonry" (default 3col)
- columns_desktop (int), columns_tablet (int), columns_mobile (int)
- output_format: "html"|"liquid"|"both"
- canva_integration (boolean)
- higgsfield_integration (boolean)

Reglas estrictas
- Escapar todo contenido dinámico en Liquid: use `{{ var | escape }}`
- No incluir scripts inline que exfiltren datos
- Imágenes: srcset (320/500/800/1000), WebP preferible, cada archivo <= 120KB cuando sea posible
- Reservar espacio con width/height para evitar CLS
- Minimizar tags Liquid en loops pesados (prefetch metafields fuera del loop si posible)

Diseño & UX (Directrices)
- Spacing: breathing room — grid gap 24–32px desktop, 12–16px mobile
- Card anatomy: image (2:2 square), title, fragrance notes (short), pricing (regular + bulk small gold), CTA (Añadir a cotización)
- Typography: serif for titles (Playfair Display), sans for body (Inter)
- Micro-interactions: image hover scale 1.03, subtle shadow, CTA hover scale 1.02

Accessibility
- All images with alt text
- Buttons reachable by keyboard, focus visible with gold outline
- Sufficient contrast for price and CTA

Performance
- Use lazy loading for non-LCP images
- Provide LQIP placeholder or blur-up technique if available
- Provide assets manifest for Higgsfield with processing_instructions

Shopify Liquid guidance
- product-card.liquid should read metafields safely:
  - `product.metafields.custom.fragrance_notes` (richtext)
  - `product.metafields.custom.bulk_price_12` (money)
- Avoid heavy Liquid inside loops; use `render 'product-card', product: product` pattern
- Example of safe price display: `{{ product.selected_or_first_available_variant.price | money }}`

A/B Variantes
- Si variants > 1, generar variantes visuales (A: lifestyle image, B: product-focused close-up) y variantes de CTA copy.

Canva & Higgsfield
- Si canva_integration=true -> include canva_template_instructions with artboard sizes and export presets
- If higgsfield_integration=true -> assets[] include processing_instructions and lqip placeholder

Ejemplo de product-card (HTML snippet)

```html
<article class="product-card">
  <div class="product-card__image">
    <img src="/assets/product-500.webp" alt="{{product.title}} - {{product.fragrance_notes_short}}" width="500" height="500" loading="lazy">
  </div>
  <div class="product-card__content">
    <h3 class="product-card__title">{{ product.title }}</h3>
    <div class="product-card__notes">{{ product.fragrance_notes_short }}</div>
    <div class="product-card__pricing">
      <span class="price-regular">€65</span>
      <span class="price-bulk">€55 <small>×12</small></span>
    </div>
    <button class="product-card__cta" data-product-handle="{{ product.handle }}">Añadir a cotización</button>
  </div>
</article>
```

Checklist de implementación y pruebas
- [ ] HTML/CSS entregables generados
- [ ] Liquid templates listos y testados en tema dev
- [ ] Assets manifest creado y sizes válidos
- [ ] Lighthouse performance sobre preview_html >= 85
- [ ] Accessibility Axe audit limpio

Notas de seguridad y operaciones
- Si el skill recibe datos de usuarios (reviews, comentarios), Claude debe redirigir a server-side sanitation y no incrustar datos sin consentimiento.
- No incluir keys/tokens en outputs. Mostrar placeholders.

---

Hecho. Usa este skill desde Claude Code pidiendo: "Genera un product grid premium para LAVERNE con X productos". El output será listo para revisión y deploy.
