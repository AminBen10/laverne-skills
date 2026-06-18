# Contributing to LAVERNE Skills

Thank you for helping improve the LAVERNE skills library. These skills are designed to be expert-level, luxury-focused, and production-ready — every contribution should maintain and elevate that standard.

---

## Philosophy

LAVERNE skills are not generic templates. They encode specific expertise for:
- **Luxury brand positioning** — Navy #1A3A52, Gold #D4AF37, OVERDOSE quality
- **B2B fragrance e-commerce** — Orient Fragance, bulk pricing, quote flows
- **Enterprise security** — XSS prevention, PCI compliance, GDPR
- **WCAG 2.1 AAA accessibility** — not just compliance, but genuine inclusivity
- **Performance leadership** — Lighthouse 85+, LCP < 1.2s, CLS < 0.1

Every change should make the skills *more specific*, *more secure*, and *more professional* — never more generic.

---

## How to Contribute

### 1. Fork and Branch

```bash
git clone https://github.com/AminBen10/laverne-skills.git
cd laverne-skills

# Create a descriptive branch
git checkout -b feature/add-webp-avif-fallback
git checkout -b fix/cta-contrast-ratio
git checkout -b security/prevent-metafield-injection
git checkout -b docs/add-shopify-markets-setup
```

**Branch naming conventions:**
- `feature/` — New functionality or skill content
- `fix/` — Correct an error or anti-pattern
- `security/` — Security hardening or vulnerability fix
- `docs/` — Documentation improvements
- `a11y/` — Accessibility improvements
- `perf/` — Performance optimizations

### 2. What You Can Contribute

#### Skill Enhancements (`*/SKILL.md`)
- New code examples with security built-in (never copy-paste generic code)
- Updated browser support statistics
- New A/B testing variants or UX patterns
- Additional accessibility guidance (WCAG 2.1 AAA)
- Performance improvements with benchmark data

#### Security Updates (`SECURITY.md`)
- New XSS patterns discovered
- Updated CSP header recommendations
- New GDPR requirements or interpretations
- Payment security updates

#### Troubleshooting (`TROUBLESHOOTING.md`)
- Add a problem you encountered + the solution
- Include exact error messages
- Include the root cause, not just the fix

#### New Skills
If you want to add a new skill (e.g., `email-marketing` or `b2b-quote-flow`):
1. Create a new directory: `mkdir new-skill-name`
2. Use the SKILL.md template below
3. Must include: YAML frontmatter, Purpose, Core Principles (≥5), Implementation Checklist, Related Skills

### 3. SKILL.md Template

```markdown
---
name: skill-name
version: 1.0.0
description: One sentence description of what this skill enables, for whom, and what makes it specific to LAVERNE.
tags: [tag1, tag2, tag3]
dependencies: [other-skill-if-needed]
license: MIT
---

# Skill Title

## Purpose

You are a [role] specializing in [domain]. Your role is to:
- **Specific outcome 1** — [how and why]
- **Specific outcome 2** — [how and why]

## Core Principles

### 1. [Principle Name]
[Detailed explanation with code examples where relevant]

### 2. [Principle Name]
[...]

### 3. Security
[Always include a security section — even for UI skills]

### 4. Accessibility
[Always include an accessibility section — WCAG 2.1 minimum]

## Anti-Patterns — What NOT to Do

| ❌ Anti-Pattern | ✅ LAVERNE Standard |
|----------------|-------------------|
| Generic example | Specific LAVERNE implementation |

## Implementation Checklist

- [ ] Item 1
- [ ] Item 2
- [ ] Security validated (XSS, injection, CSP)
- [ ] Accessibility validated (WCAG 2.1 AA minimum)
- [ ] Performance tested (Lighthouse > 85)

## Related Skills

- `skill-name` — [brief description of relationship]
```

### 4. Quality Standards

Before submitting a PR, verify:

#### Content Quality
- [ ] All code examples are complete and runnable (no `...` placeholders)
- [ ] All code examples include security (escaped outputs, no injection risks)
- [ ] Colors reference LAVERNE brand palette (#1A3A52, #D4AF37)
- [ ] No generic stock-photo or generic-template language
- [ ] Anti-patterns section explains *why* each is wrong

#### Security
- [ ] No API keys, tokens, or credentials in any example
- [ ] Liquid examples use `| escape` filter on all dynamic output
- [ ] JavaScript examples use `innerHTML` only with DOMPurify
- [ ] CSP-compatible patterns (no `eval()`, no inline event handlers with dynamic content)

#### Accessibility
- [ ] Color contrast ratios are stated (WCAG AAA = 7:1 target)
- [ ] All interactive elements have `aria-label` where needed
- [ ] `prefers-reduced-motion` addressed where animations are used

#### Performance
- [ ] Images use WebP + JPEG fallback pattern
- [ ] All lazy-loaded images have explicit `width` and `height`
- [ ] CSS uses `transform`/`opacity` for animations (GPU-accelerated)

### 5. Submit a Pull Request

```bash
# Stage changes
git add .

# Commit with descriptive message
git commit -m "feat(ecommerce-product-grid): add AVIF image format with WebP/JPEG fallback"

# Push to your fork
git push origin feature/add-webp-avif-fallback
```

**PR description should include:**
1. **What changed**: brief description of the change
2. **Why**: what problem this solves or what it improves
3. **Security review**: confirm no new XSS or injection risks
4. **Accessibility review**: confirm WCAG compliance maintained
5. **Tested**: how you verified the change (browser, Lighthouse, etc.)

---

## Commit Message Format

Use [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <short description>

[optional body]
[optional footer]
```

**Types:**
- `feat` — New feature or skill content
- `fix` — Bug fix or incorrect guidance
- `security` — Security improvement
- `docs` — Documentation only changes
- `a11y` — Accessibility improvement
- `perf` — Performance improvement
- `refactor` — Restructuring without behavior change

**Scopes** (directory name):
- `landing-page-hero-design`
- `ecommerce-product-grid`
- `shopify-integration`
- `performance-optimization`
- `security`
- `contributing`
- `troubleshooting`
- `readme`

**Examples:**
```
feat(ecommerce-product-grid): add AVIF srcset with JPEG fallback chain
fix(shopify-integration): add escape filter to all metafield outputs
security(shopify-integration): prevent API key exposure in Liquid templates
a11y(landing-page-hero-design): add skip link for keyboard navigation
perf(performance-optimization): add Chrome DevTools profiling guide
```

---

## Code Review Criteria

PRs are reviewed against these criteria:

| Criterion | Standard |
|-----------|----------|
| **Specificity** | LAVERNE-specific, not generic web advice |
| **Security** | No XSS risks, no credential exposure |
| **Accessibility** | WCAG 2.1 AA minimum, AAA preferred |
| **Performance** | Code examples are performance-conscious |
| **Completeness** | Full examples, no placeholders |
| **Consistency** | Matches existing skill style and format |

---

## Reporting Issues

For bugs in skill guidance, security vulnerabilities, or outdated information:

1. **Open a GitHub Issue** with the label `bug`, `security`, or `outdated`
2. For security vulnerabilities, email directly (avoid public disclosure)
3. Include: skill name, section, current text, problem, suggested fix

---

## License

By contributing, you agree your contributions are licensed under the MIT License (same as the repository).

---

*Maintained with ❤️ for LAVERNE by AminBen10 and contributors.*
