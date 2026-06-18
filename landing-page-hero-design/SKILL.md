---
name: landing-page-hero-design
description: Skill para generar hero sections premium “OVERDOSE” para LAVERNE (no genéricas). Produces production-ready HTML/CSS and optional Shopify Liquid, with assets manifest, Canva templates, and Higgsfield pipeline metadata.
license: MIT
version: 1.1.0
tags: [hero, design, luxury, claude-code, canva, higgsfield]
dependencies: [superpowers:>=1.0.0]
outputs_schema: |
  {
    "type": "object",
    "properties": {
      "files": {
        "type": "array",
        "items": {"type": "object", "properties": {"path": {"type": "string"}, "language": {"type":"string"}, "content": {"type":"string"}}}
      },
      "assets": {
        "type": "array",
        "items": {"type": "object", "properties": {"name": {"type":"string"}, "purpose": {"type":"string"}, "srcset": {"type":"array","items":{"type":"string"}}}}
      },
      "preview_html": {"type": "string"},
      "canva_template_instructions": {"type":"string"}
    },
    "required": ["files","assets","preview_html"]
  }
---

# Skill: Landing Page Hero Design (LAVERNE)

Resumen

Este skill instruye a Claude Code a generar hero sections premium para LAVERNE — resultado listo para usar (HTML/CSS y opcional Liquid) y activos preparados para Canva/Higgsfield.

Objetivos clave
- No generar plantillas genéricas: diseño "OVERDOSE" — lujo, tipografía de alta gama, materiales y micro‑interacciones sutiles.
- Cumplir metas de performance y accesibilidad: LCP < 1.2s, Lighthouse Performance ≥ 85, WCAG 2.1 AA/AAA donde sea factible.
- Entregar salida estructurada que pueda ser consumida por pipelines (Higgsfield) y editores de diseño (Canva).

Cuando usarlo
- Solicitar "Crear hero premium para LAVERNE" en Claude Code cuando se necesite un hero para landing pages que vaya directo a deploy o a conversión en Shopify.

Integraciones incluidas
- Canva: instrucciones para crear un artboard/template con paleta (Navy #1A3A52, Gold #D4AF37), guías de tipografía y export presets.
- Higgsfield: metadata para pipeline (asset names, sizes, lqip, alt text) y manifest compatible.

Formato de salida (obligatorio)
- JSON con la siguiente estructura (ejemplo en "Ejemplo de respuesta"):
  - files: [{ path, language, content }] → archivos a crear (hero.html, hero.css, hero.liquid)
  - assets: [{ name, purpose, srcset }] → manifest de imágenes (hero-640.webp, hero-1024.webp, etc.)
  - preview_html: HTML simplificado para previsualizar
  - canva_template_instructions: instrucciones para crear template en Canva

Parámetros de entrada (prompt-friendly)
- brand_name (string) — p.ej. "LAVERNE"
- headline (string)
- subtitle (string)
- cta_primary (string)
- badge_text (string) — p.ej. "TESTER GRATIS DESDE 24 UNIDADES"
- hero_image_description (string) — descripción para búsqueda/escena (man holding fragrance, warm lighting)
- variants (int) — cantidad de variantes A/B (default 2)
- output_format (string) — "html", "liquid", "both" (default: "both")
- canva_integration (boolean) — si true, incluye instrucciones y assets listos para Canva
- higgsfield_integration (boolean) — si true, incluye metadata para pipeline

Reglas estrictas (NO NEGOTIABLE)
- No generar scripts inline que exfiltren datos ni código de tracking por defecto.
- No imágenes embebidas en base64; solo referencias a assets/manifest con nombres y tamaños.
- Los tamaños de imagen deben respetar el presupuesto: hero responsive: 640/1024/1440, hero <= 200KB WebP por breakpoint.
- Siempre proveer `width` y `height` en las etiquetas `img` (reserva espacio para CLS).
- Escapar todo contenido dinámico en Liquid: use `{{ var | escape }}`.

Diseño & UX (Directrices de marca)
- Colores: Navy #1A3A52 (fondo o overlay), Gold #D4AF37 (accento), Off-white #F5F5F5
- Tipografía: Serif para H1 (Playfair Display o Prata), Sans-serif para body (Inter)
- Jerarquía: H1 enorme (60–80px desktop, 36px mobile), CTA prominente (48px altura), badge pequeño pero legible
- Micro‑interacciones: fade-in 0.8s stagger, CTA hover (scale 1.02, soft glow), parallax opcional pero desactivable en mobile

Accesibilidad
- Contrast ratio >= 4.5:1 para textos principales
- Alt text descriptivo para imágenes
- Focus visible para botones (outline con color gold rgba)
- Keyboard accessible (tab order lógico)

Performance
- Critical CSS inline máximo 5KB
- Hero images WebP con srcset: 640w, 1024w, 1440w
- Lazy load below the fold; hero should prefer preloading the LCP image with `<link rel="preload">` when deploying

Canva Integration (instrucciones automáticas)
Si canva_integration = true, Claude debe devolver `canva_template_instructions` con:
- Paleta de colores
- Tipografías y weights a usar
- Layout guides (margins, safearea)
- Export presets: PNG 2x (for assets), SVG for vector elements
- Una lista de assets (nombres) que se pueden importar a Canva

Higgsfield Integration (pipeline metadata)
Si higgsfield_integration = true, devolver `assets[]` con campos adicionales:
- `uploaded`: false (el pipeline marcará true luego)
- `lqip`: base64-placeholder hash o descriptor
- `processing_instructions`: {resize: [640,1024,1440], compress: {webp:80}}

A/B Variantes
- Si variants > 1, devolver N variantes completas en `files[]` con sufijos `-variant-a`, `-variant-b`.
- Variantes deben cambiar imagen / overlay / CTA text (no cambiar estructura base)

Ejemplo de prompt (para pegar en Claude Code)

"Generar hero premium para LAVERNE. brand_name: 'LAVERNE', headline: 'LAVERNE', subtitle: 'CATÁLOGO PROFESIONAL 2026', cta_primary: 'Ver catálogo completo', badge_text: 'TESTER GRATIS DESDE 24 UNIDADES', hero_image_description: 'man holding fragrance, warm cinematic lighting, shallow depth of field', variants: 2, output_format: 'both', canva_integration: true, higgsfield_integration: true"

Ejemplo de respuesta esperada (formato JSON - resumido)

{
  "files": [
    {"path":"hero.html","language":"html","content":"<section class=...>...</section>"},
    {"path":"hero.css","language":"css","content":".hero{...}"},
    {"path":"hero.liquid","language":"liquid","content":"{% comment %} ... %}"}
  ],
  "assets": [
    {"name":"hero-640.webp","purpose":"lcp","srcset":["/assets/hero-640.webp 640w","/assets/hero-1024.webp 1024w","/assets/hero-1440.webp 1440w"],"processing_instructions":{"webp_quality":80,"max_width":1440}},
  ],
  "preview_html": "<html>...", 
  "canva_template_instructions": "Create 1440x900 artboard, apply colors..."
}

Pruebas y checklist (rapido)
- Ejecutar Lighthouse sobre `preview_html` o URL de staging
- Ejecutar Axe accessibility
- Validar manifest (assets exist and sizes within budget)

Notas de seguridad
- Si el prompt incluye datos de usuarios (nombres, emails), Claude debe redactarlos en el output o señalizar la necesidad de consentimiento GDPR.
- No embebas keys ni tokens. Mostrar placeholders en outputs (e.g., SHOPIFY_API_TOKEN_PLACEHOLDER).

---

## Implementation checklist (para humanos)
- [ ] Validar outputs de Claude en staging
- [ ] Subir assets a CDN o Higgsfield y reemplazar URLs en `files[]`
- [ ] Revisar variantes A/B y elegir ganador tras 2 semanas

---

Hecho. Pide ahora: "Genera un hero para LAVERNE" con parámetros; Claude Code devolverá archivos listos y assets manifestables.
