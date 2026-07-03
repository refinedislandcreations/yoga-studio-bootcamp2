# AGENTS.md

## Overall Purpose
These documents collectively define a complete pipeline for building, optimizing, and deploying a Jekyll + Tailwind CSS website with CloudCannon CMS and Netlify hosting, optimized for both traditional SEO and AI-driven search (GEO).

## High-Level Structure
The consolidated document reorganizes all content into a logical execution flow:
- **Phase 1**: Repository Discovery & Analysis
- **Phase 2**: GEO Implementation (AI search optimization)
- **Phase 3**: Production Build & Deploy Validation
- **Phase 4**: Security Audit & Hardening
- **Phase 5**: Performance & Bandwidth Optimization
- **Phase 6**: Cache Strategy & Invalidation
- **Phase 7**: Final Production Sign-off

---

# Consolidated Master Document

# Jekyll + Tailwind CSS: Complete GEO, Security, Performance & Deployment Pipeline

**Stack**: Jekyll · TailwindCSS · CloudCannon CMS · Netlify Forms · Netlify Functions/Edge · Third-Party APIs

**Document Version**: 1.0 (Consolidated)
**Last Updated**: 2026-07-03

---

## Table of Contents

1. [Overview](#overview)
2. [Architecture](#architecture)
3. [Requirements](#requirements)
4. [Phase 0: Repository Discovery](#phase-0-repository-discovery)
5. [Phase 1: GEO Implementation](#phase-1-geo-implementation)
6. [Phase 2: Production Build Validation](#phase-2-production-build-validation)
7. [Phase 3: Security Audit & Hardening](#phase-3-security-audit--hardening)
8. [Phase 4: Performance & Bandwidth Optimization](#phase-4-performance--bandwidth-optimization)
9. [Phase 5: Cache Strategy & Invalidation](#phase-5-cache-strategy--invalidation)
10. [Phase 6: CloudCannon CMS Validation](#phase-6-cloudcannon-cms-validation)
11. [Phase 7: Netlify Platform Validation](#phase-7-netlify-platform-validation)
12. [Phase 8: Final Production Sign-off](#phase-8-final-production-sign-off)
13. [Troubleshooting](#troubleshooting)
14. [Best Practices](#best-practices)
15. [Future Improvements](#future-improvements)
16. [Appendix](#appendix)

---

## Overview

### Project Purpose

This pipeline transforms a Jekyll + Tailwind CSS website into a production-ready, AI-optimized, secure, and high-performance web property deployed on Netlify with CloudCannon CMS editing capabilities.

### Goals

1. **GEO Optimization**: Structure the site so AI tools (ChatGPT, Perplexity, Google AI Overviews, Bing Copilot, Claude) can accurately read and cite it
2. **Production Readiness**: Zero deployment blockers, validated builds, correct TailwindCSS purge
3. **Security Hardening**: Comprehensive security audit covering HTTP headers, CSP, dependency vulnerabilities, secrets management, form protection, and API security
4. **Performance Optimization**: Reduce bandwidth usage while maintaining or improving Core Web Vitals
5. **Cache Reliability**: Ensure users always receive the latest deployment without manual cache clearing
6. **CMS Editability**: Verify CloudCannon CMS can edit all content, SEO, navigation, and media fields

### Scope

This document covers the complete implementation pipeline from initial repository inspection through final production deployment sign-off. It addresses:

- Generative Engine Optimization (GEO) for AI search visibility
- Traditional SEO (metadata, sitemaps, robots.txt, structured data)
- TailwindCSS build optimization and content path auditing
- Netlify deployment configuration, forms, functions, and edge
- CloudCannon CMS configuration and editability
- Comprehensive security auditing (OWASP Top 10 alignment)
- Performance and bandwidth optimization
- Cache invalidation strategy
- Accessibility compliance (WCAG 2.1 AA)

---

## Architecture

### Components

| Component | Purpose | Configuration File(s) |
|-----------|---------|----------------------|
| **Jekyll** | Static site generator | `_config.yml`, `_config.production.yml` |
| **TailwindCSS** | Utility-first CSS framework | `tailwind.config.js`, `postcss.config.js` |
| **CloudCannon CMS** | Git-based CMS for content editors | `cloudcannon.config.yml` |
| **Netlify** | Hosting, forms, serverless functions, edge | `netlify.toml` |
| **Netlify Forms** | Form handling with spam protection | Inline HTML attributes + Netlify dashboard |
| **Netlify Functions** | Serverless backend logic | `netlify/functions/` directory |
| **Netlify Edge Functions** | Edge-computed logic (geolocation, A/B testing) | `netlify/edge-functions/` directory |
| **Third-Party APIs** | External service integrations | Environment variables in Netlify |

### System Design

```
[Content Editors] → CloudCannon CMS → Git Repository
                                         ↓
[Developers] → Local Dev → Git Push → GitHub
                                         ↓
                                   Netlify Build
                                   (Jekyll + TailwindCSS)
                                         ↓
                                   Netlify CDN
                                   (_site/ deployed)
                                         ↓
                              [End Users / AI Crawlers]
```

### Relationships

- **Jekyll** generates static HTML from Markdown, Liquid templates, and `_data/` files
- **TailwindCSS** processes utility classes and purges unused CSS during production build
- **CloudCannon** reads `cloudcannon.config.yml` to provide editable regions for content teams
- **Netlify** builds the Jekyll site, deploys to CDN, and handles forms/functions/redirects
- **Netlify Functions** provide serverless backend capabilities (API proxies, form processing)
- **Netlify Edge Functions** run at the CDN edge for low-latency logic

---

## Requirements

### Dependencies

#### Ruby Gems
```ruby
# Gemfile
gem "jekyll", "~> 4.3"
gem "jekyll-seo-tag"        # SEO metadata generation
gem "jekyll-sitemap"        # Sitemap generation
gem "jekyll-feed"           # RSS feed
gem "jekyll-postcss"        # TailwindCSS integration (if not using npm build)
gem "bundler-audit"         # Dependency vulnerability scanning
```

#### Node Packages
```json
{
  "devDependencies": {
    "tailwindcss": "^3.4.0",
    "postcss": "^8.4.0",
    "autoprefixer": "^10.4.0",
    "cssnano": "^6.0.0"
  }
}
```

### Environment

- **Ruby**: 3.x.x (pinned in `.ruby-version` and `netlify.toml`)
- **Node**: 20.x (pinned in `.nvmrc` and `netlify.toml`)
- **Build Environment**: `JEKYLL_ENV=production` (must be explicitly set)
- **Deployment Target**: Netlify (with GitHub Actions if custom plugins needed)

### Prerequisites

Before starting any phase:
1. Repository must be cloned locally
2. All dependencies installed (`bundle install`, `npm install`)
3. Access to Netlify dashboard, CloudCannon dashboard, and GitHub repository settings
4. Production domain configured in Netlify
5. Environment variables documented and available

---

## Phase 0: Repository Discovery

### Objective
Inspect the repository completely before making any changes. Every subsequent phase depends on accurate discovery findings.

### Why This Is Needed
Assumptions about the Jekyll setup, TailwindCSS integration, and deployment configuration will cause broken builds and incorrect implementations. Discovery prevents rework.

### Prerequisites
- Repository cloned locally
- Read access to all configuration files

### Steps

#### 0.1 — Jekyll Configuration

Read `_config.yml` (and `_config.production.yml` or environment overrides if present).

Identify:
- `url` and `baseurl` (needed for absolute URLs in schema and `llms.txt`)
- `title`, `description`, `email`, `timezone`
- Plugins listed under `plugins:` or `gems:` (specifically check for `jekyll-seo-tag`, `jekyll-sitemap`, `jekyll-feed`, `jekyll-postcss`, `jekyll-compose`, `jekyll-redirect-from`, `jekyll-paginate`)
- `permalink` style (affects URL structure)
- `defaults:` settings for front matter
- `collections:` block (collections beyond posts/pages)

**Command**:
```bash
cat _config.yml
cat _config.production.yml 2>/dev/null || echo "No production config found"
```

**Expected Output**: Complete inventory of Jekyll configuration values.

#### 0.2 — Tailwind CSS Setup

Determine which integration pattern is used:

- **Option A — PostCSS via package.json**: Look for `package.json`, `postcss.config.js`, `tailwind.config.js`. Build step: `npm run build`.
- **Option B — Tailwind CLI standalone**: Look for `tailwindcss` binary, no `package.json` or minimal devDeps.
- **Option C — Jekyll Asset Pipeline with gem**: Look for `jekyll-postcss` in Gemfile.
- **Option D — CDN only**: Look for `<script src="https://cdn.tailwindcss.com">` in layouts.

Identify the CSS entry file (e.g., `assets/css/main.css`, `_sass/main.scss`) and output file path.

**Commands**:
```bash
cat package.json 2>/dev/null || echo "No package.json"
cat postcss.config.js 2>/dev/null || echo "No PostCSS config"
cat tailwind.config.js 2>/dev/null || echo "No Tailwind config"
grep -r "cdn.tailwindcss.com" _layouts/ _includes/ 2>/dev/null || echo "No CDN Tailwind found"
```

#### 0.3 — Layout and Includes Structure

Explore `_layouts/` and `_includes/` directories.

Identify:
- Root/base layout (where global `<head>` content goes)
- Dedicated head include (e.g., `_includes/head.html`, `_includes/seo.html`)
- Dedicated scripts include or footer scripts block
- Homepage (from `index.html`, `index.md`, or front matter)
- Other layouts (`post.html`, `page.html`, `service.html`)

**Commands**:
```bash
ls -la _layouts/
ls -la _includes/
grep -r "{% seo %}" _layouts/ _includes/ 2>/dev/null || echo "No jekyll-seo-tag usage found"
```

#### 0.4 — Pages and Content Inventory

List all pages and collections:
- Pages in root or `/pages/`: list filenames and titles
- Posts in `_posts/`: count, topics, categories
- `_data/` files: structured data inventory
- Existing FAQ content (in `_data/`, pages, or collections)
- Service/product/location pages
- Existing `robots.txt` and `llms.txt`

**Commands**:
```bash
ls -la *.html *.md 2>/dev/null
ls -la _posts/ 2>/dev/null | head -20
ls -la _data/ 2>/dev/null
ls -la _pages/ 2>/dev/null || echo "No _pages directory"
cat robots.txt 2>/dev/null || echo "No robots.txt"
cat llms.txt 2>/dev/null || echo "No llms.txt"
cat llms-full.txt 2>/dev/null || echo "No llms-full.txt"
```

#### 0.5 — Deployment and Hosting

Determine deployment target and plugin restrictions.

Check for:
- `github-pages` gem in Gemfile → GitHub Pages deployment (whitelist restrictions)
- GitHub Actions workflow (`.github/workflows/*.yml`) → custom build, no restrictions
- Netlify config (`netlify.toml`, `_headers`)
- Vercel config (`vercel.json`)
- Production config or CI env vars

**Critical**: GitHub Pages (without Actions) cannot run most custom plugins. If site deploys via `github-pages` gem, note which features require workarounds.

**Commands**:
```bash
grep "github-pages" Gemfile 2>/dev/null || echo "No github-pages gem"
cat netlify.toml 2>/dev/null || echo "No netlify.toml"
ls -la .github/workflows/ 2>/dev/null || echo "No GitHub Actions workflows"
```

#### 0.6 — Business Context

Read homepage, About page, and `_config.yml` to determine:
- Business type (local business, personal blog, portfolio, agency, product, docs site)
- Business name, location, contact details
- Services or products offered
- Social media links
- Domain (from `url:` in `_config.yml`)

#### 0.7 — Discovery Summary Output

Compile a structured summary:

```markdown
DISCOVERY SUMMARY
=================
Jekyll version: [from Gemfile.lock]
Tailwind setup: [Option A/B/C/D — with specifics]
SEO plugin: [jekyll-seo-tag / none / other]
Sitemap plugin: [jekyll-sitemap / manual / none]
Base layout file: [path]
Head include file: [path or "none — head is in layout directly"]
Deployment target: [GitHub Pages (gem) / GitHub Actions / Netlify / Vercel / other]
Plugin restrictions: [yes — github-pages gem limits plugins / no restrictions]
Business type: [type]
Domain: [from _config.yml url:]
Existing robots.txt: [yes/no]
Existing llms.txt: [yes/no]
Collections beyond posts: [list]
_data/ files: [list]
FAQ content exists: [yes — location / no]
```

### Files Modified
- None (read-only phase)

### Success Verification
- [ ] All configuration files read and documented
- [ ] Tailwind integration pattern identified
- [ ] Deployment target confirmed with plugin restrictions noted
- [ ] Business context understood
- [ ] Discovery summary complete and accurate

---

## Phase 1: GEO Implementation

### Objective
Implement Generative Engine Optimization to make the site accurately readable and citable by AI tools (ChatGPT, Perplexity, Google AI Overviews, Bing Copilot, Claude).

### Why This Is Needed
- 83% of AI Overview citations come from pages outside the Google top 10
- AI referral traffic converts ~5× higher than traditional search
- AI search grew 527% YoY in H1 2025
- Traditional SEO alone does not optimize for AI citation

### Prerequisites
- Phase 0 discovery complete
- Business context and content inventory documented
- Deployment target and plugin restrictions known

---

### Step 1.1 — `robots.txt`

#### What This Does
Configures which AI crawlers can access the site for search/citation purposes versus training-only bots that provide no citation benefit.

#### Implementation

1. Check if `robots.txt` exists at the Jekyll root
2. If it exists, read it fully and preserve all existing content
3. Create or update `robots.txt` with merged rules

**File**: `robots.txt` (at repository root — Jekyll copies it to `_site/` automatically)

```
# GEO — Search & citation bots (allow: generate AI recommendations and citations)
User-agent: OAI-SearchBot
Allow: /

User-agent: ChatGPT-User
Allow: /

User-agent: PerplexityBot
Allow: /

User-agent: Perplexity-User
Allow: /

User-agent: Claude-SearchBot
Allow: /

User-agent: Claude-User
Allow: /

User-agent: Google-Agent
Allow: /

# GEO — Training-only bots (block: no citation benefit)
User-agent: GPTBot
Disallow: /

User-agent: ClaudeBot
Disallow: /

User-agent: CCBot
Disallow: /

User-agent: Meta-ExternalAgent
Disallow: /

# GEO — Opt-out token
User-agent: Google-Extended
Disallow: /

# GEO — Non-compliant undeclared crawlers
User-agent: Bytespider
Disallow: /

# Sitemap
Sitemap: {{ site.url }}/sitemap.xml
```

**Note**: Replace `{{ site.url }}` with the actual URL from `_config.yml`. Jekyll does not process Liquid in `robots.txt` unless it has YAML front matter, which would break the file format. Use the actual production URL directly.

4. If Netlify `_headers` or Cloudflare rules exist (found in Phase 0.5), flag them — they may block bots at a level `robots.txt` cannot override.

#### Success Verification
- [ ] File at Jekyll root (not in `_layouts` or `_includes`)
- [ ] Existing rules preserved
- [ ] GEO crawler rules added
- [ ] Sitemap reference present if `jekyll-sitemap` is active
- [ ] CDN/server-level config checked for conflicting bot blocks

---

### Step 1.2 — `llms.txt`

#### What This Does
Provides a concise, structured summary of the site specifically formatted for LLM consumption. This is the primary file AI tools read to understand and cite a website.

#### Implementation

1. Read `_config.yml`, homepage, About page, and `_data/` files for site content
2. Generate `llms.txt` with real content only — no placeholder text
3. Place at Jekyll repo root (copied to `_site/` automatically)

**File**: `llms.txt` (at repository root)

```markdown
# [Actual site title from _config.yml]

> [Actual description from _config.yml or one-sentence summary from homepage content]

## Key Pages

- [Homepage]([full URL]) — [What the homepage is about]
- [About]([full URL]/about/) — [From About page content]
[List every significant page found in Phase 0.4 — use actual URLs]

## Services / Products

[Include only if site has services/products. List each with one-line description.
Skip entirely for personal blogs or docs-only sites.]

## Posts / Content

[If blog or substantial post archive exists:]
- Blog: [full URL]/blog/ — [Topic focus]
- Latest posts cover: [summarize 3-5 most recent post topics]

## About

[2-3 factual sentences from About content. No marketing fluff. Include location if relevant.]

## Contact

- Email: [from _config.yml if present]
- Location: [if present]
- [Social links if present]
```

**Content Requirements**:
- Use actual production URLs (not Jekyll `{{ site.url }}` Liquid tags — this is a plain text file)
- Write for an AI audience — factual, complete, no marketing language
- Only include information actually present on the site

#### After Creating

Submit URL to:
- [directory.llmstxt.cloud](https://directory.llmstxt.cloud)
- [llmstxt.site](https://llmstxt.site)

If this site links to related projects or sibling domains, reference them in `llms.txt`.

#### Success Verification
- [ ] `llms.txt` at Jekyll root — real content, no placeholders
- [ ] Correct production URLs used
- [ ] Accessible at `https://[domain]/llms.txt` after build
- [ ] Submitted to LLM directories

---

### Step 1.3 — `llms-full.txt`

#### What This Does
Provides a detailed 1,000–3,000 word comprehensive briefing about the site. This is the expanded version for AI tools that need deeper context.

#### Implementation

Generate `llms-full.txt` at the Jekyll root. Pull content from:
- `_config.yml` title, description, author block
- Homepage and About page content
- All service/product/portfolio pages
- FAQ content in `_data/` files or existing pages
- Post categories and recent post summaries
- Contact and location info

**Format**: Substantive prose, not a list of headings. Structure as a thorough briefing a person could read to understand the site fully.

**File**: `llms-full.txt` (at repository root)

#### Content Guidelines
- 1,000–3,000 words
- Factual, complete, no marketing fluff
- Only include information present in the project
- Write for AI understanding, not human persuasion

#### Success Verification
- [ ] `llms-full.txt` at Jekyll root
- [ ] 1,000+ words of substantive content
- [ ] No placeholder or template text
- [ ] Accessible at `https://[domain]/llms-full.txt` after build

---

### Step 1.4 — Organisation Schema

#### What This Does
Adds JSON-LD structured data so AI tools and search engines understand the organization behind the site. Primarily benefits Bing/Copilot and Google rich results.

#### Implementation

1. Gather: site title, URL, logo path, description, address, phone, email, social links — from `_config.yml`, `_data/` files, footer includes, and page content
2. Determine injection method based on Phase 0.3 findings:

**If `jekyll-seo-tag` is installed**:
Add schema as a separate block in the head include or base layout (do not remove `{% seo %}` tag).

**If no SEO plugin**:
Add schema block directly to `<head>` section of base layout.

**File**: `_includes/head.html` (or base layout if no head include exists)

```html
<!-- GEO: Organisation Schema -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "{{ site.title | escape }}",
  "url": "{{ site.url }}",
  "logo": "{{ site.url }}/assets/images/logo.png",
  "description": "{{ site.description | escape }}",
  "email": "{{ site.email }}",
  "sameAs": [
    {% if site.twitter %}"https://twitter.com/{{ site.twitter }}"{% endif %}
    {% if site.github %}"https://github.com/{{ site.github }}"{% endif %}
    {% if site.linkedin %}"https://linkedin.com/in/{{ site.linkedin }}"{% endif %}
  ]
}
</script>
```

**Logo Path**: Find the actual logo asset path. Common locations:
- `assets/images/logo.png`
- `assets/img/logo.svg`
- `images/logo.png`
- Defined as `site.logo` in `_config.yml`

Use whichever exists. If none exists, omit the `logo` field.

**Social Links**: Adjust `sameAs` keys to match actual social variable names in `_config.yml` (they vary by theme).

#### Success Verification
- [ ] Organisation schema in base layout `<head>` — using Liquid variables, no hardcoded values
- [ ] Logo path points to existing asset (or field omitted if no logo)
- [ ] No empty or placeholder values
- [ ] Validates with [Google Rich Results Test](https://search.google.com/test/rich-results) after deploy

---

### Step 1.5 — LocalBusiness / Entity Schema

#### What This Does
Adds a second JSON-LD block with the specific entity type matching the business. This must be a separate block from the Organisation schema.

#### Implementation

Based on business type from Phase 0.6, select the correct `@type`:

| Business type | `@type` to use |
|---|---|
| General local business | `LocalBusiness` |
| Hotel / resort | `Hotel` or `LodgingBusiness` |
| Restaurant / café | `Restaurant` |
| Tourist attraction | `TouristAttraction` |
| Personal blog / portfolio | `Person` (use instead of Organization if it's an individual) |
| Agency / studio | `ProfessionalService` |
| Software / SaaS | `SoftwareApplication` or `Organization` |
| Docs / open source project | `SoftwareSourceCode` or `Organization` |

Add as a **separate** `<script type="application/ld+json">` block (never merge with Organisation schema):

```html
<!-- GEO: LocalBusiness / Entity Schema -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "[Correct type from table]",
  "name": "{{ site.title | escape }}",
  "url": "{{ site.url }}",
  "description": "{{ site.description | escape }}",
  "image": "{{ site.url }}/assets/images/[hero or og image found in project]",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "[from _config.yml or page content if available]",
    "addressLocality": "[city]",
    "addressCountry": "[country code]"
  },
  "telephone": "[if found]",
  "email": "{{ site.email }}"
}
</script>
```

**Critical**: Omit any field that has no real value. An empty or null field is worse than omitting it.

#### Success Verification
- [ ] Separate script block from Organisation schema
- [ ] Correct `@type` for business
- [ ] No fields with empty or placeholder values
- [ ] Validates with Google Rich Results Test after deploy

---

### Step 1.6 — FAQ Section

#### What This Does
Adds FAQ content with FAQPage JSON-LD schema. Benefits Bing/Copilot rich results and provides structured content for AI citation.

#### Implementation

1. Check `_data/` for existing FAQ data (`_data/faq.yml`, `_data/faqs.yml`). If found, use it — do not duplicate.
2. Check existing pages for FAQ content.
3. If no FAQ content exists, generate 5–8 questions and answers derived from the site's actual services, posts, and page copy. Do not invent facts.

**Option A — Via `_data/faq.yml`** (preferred for maintainability)

Create or update `_data/faq.yml`:

```yaml
- question: "[Question from site content]"
  answer: "[Answer — complete factual sentence]"

- question: "[Question]"
  answer: "[Answer]"
```

Then in the page or layout where FAQ should appear:

```html
{% if site.data.faq %}
<section class="faq py-12" aria-label="Frequently Asked Questions">
  <h2 class="text-2xl font-bold mb-6">Frequently Asked Questions</h2>

  {% for item in site.data.faq %}
  <details class="mb-4 border border-gray-200 rounded-lg">
    <summary class="cursor-pointer px-4 py-3 font-medium text-gray-900 hover:bg-gray-50">
      {{ item.question }}
    </summary>
    <div class="px-4 py-3 text-gray-700">
      <p>{{ item.answer }}</p>
    </div>
  </details>
  {% endfor %}
</section>

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {% for item in site.data.faq %}
    {
      "@type": "Question",
      "name": {{ item.question | jsonify }},
      "acceptedAnswer": {
        "@type": "Answer",
        "text": {{ item.answer | jsonify }}
      }
    }{% unless forloop.last %},{% endunless %}
    {% endfor %}
  ]
}
</script>
{% endif %}
```

**Option B — Inline in a page** (for single-page FAQ)

Add FAQ HTML directly in the page's Markdown file.

**Tailwind Classes**: Adjust classes to match the project's existing design tokens. Check `tailwind.config.js` for custom colors, spacing scale, and font size config.

**Important Research Note**: Pure FAQ-only pages reduce AI citation impact. FAQ sections are primarily for Bing/Copilot rich results. Keep FAQ answers substantive and specific — avoid restating content already on the page.

#### Success Verification
- [ ] FAQ data source identified (new `_data/faq.yml` or existing content)
- [ ] FAQ section on homepage before footer
- [ ] FAQPage JSON-LD uses Liquid loop from same data source (no content drift)
- [ ] Tailwind classes on FAQ elements are in `tailwind.config.js` content paths
- [ ] Accordion works without JavaScript (native `<details>`/`<summary>`)

---

### Step 1.7 — Sitemap

#### What This Does
Ensures all indexable pages are discoverable by search engines and AI crawlers.

#### Implementation

**If `jekyll-sitemap` plugin is installed AND deployment allows it**:
- Confirm plugin is in `Gemfile` and `plugins:` list
- Verify not in `exclude:` list in `_config.yml`
- Plugin auto-generates `sitemap.xml`
- To exclude specific pages, add `sitemap: false` to their front matter

**If no sitemap plugin and custom build**:
Create `sitemap.xml` at root with YAML front matter so Jekyll processes it as Liquid:

```xml
---
layout: null
---
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  {% for page in site.pages %}
  {% unless page.sitemap == false or page.url contains 'feed' %}
  <url>
    <loc>{{ site.url }}{{ page.url | remove: 'index.html' }}</loc>
    <lastmod>{{ page.date | date_to_xmlschema | default: site.time | date_to_xmlschema }}</lastmod>
    <changefreq>monthly</changefreq>
  </url>
  {% endunless %}
  {% endfor %}
  {% for post in site.posts %}
  <url>
    <loc>{{ site.url }}{{ post.url }}</loc>
    <lastmod>{{ post.date | date_to_xmlschema }}</lastmod>
    <changefreq>monthly</changefreq>
  </url>
  {% endfor %}
  {% for collection in site.collections %}
  {% unless collection.label == "posts" %}
  {% for doc in collection.docs %}
  {% unless doc.sitemap == false %}
  <url>
    <loc>{{ site.url }}{{ doc.url }}</loc>
    <lastmod>{{ doc.date | date_to_xmlschema | default: site.time | date_to_xmlschema }}</lastmod>
    <changefreq>monthly</changefreq>
  </url>
  {% endunless %}
  {% endfor %}
  {% endunless %}
  {% endfor %}
</urlset>
```

**Note on GitHub Pages**: `jekyll-sitemap` is on the GitHub Pages whitelist — it will work without custom Actions.

#### Success Verification
- [ ] `sitemap.xml` generated (via plugin or manual file)
- [ ] All key pages included
- [ ] Accessible at `domain/sitemap.xml` after build
- [ ] `robots.txt` references sitemap URL

---

### Step 1.8 — IndexNow (Bing)

#### What This Does
Notifies Bing (and other IndexNow-compatible search engines) immediately when content changes, rather than waiting for crawlers to rediscover pages.

#### Implementation

**GitHub Actions deploy**:
Add to deploy workflow:

```yaml
- name: Notify IndexNow
  run: |
    curl -X POST "https://api.indexnow.org/indexnow" \
      -H "Content-Type: application/json; charset=utf-8" \
      -d '{
        "host": "[domain without https://]",
        "key": "${{ secrets.INDEXNOW_KEY }}",
        "keyLocation": "https://[domain]/[key].txt",
        "urlList": ["https://[domain]/"]
      }'
```

Create the key file at Jekyll root (e.g., `abc123.txt` containing just the key string). Store the key as a GitHub Actions secret.

**Netlify deploy**:
Add a build plugin or Netlify Function triggered on deploy success.

**Manual / no CI**:
Create the key file at repo root and submit URLs manually via [Bing IndexNow](https://www.bing.com/indexnow) after each publish.

#### Success Verification
- [ ] Key file at Jekyll root (will be copied to `_site/`)
- [ ] Deploy workflow or manual process in place

---

### Step 1.9 — Speakable Schema *(Optional)*

#### What This Does
Marks sections of the homepage as suitable for text-to-speech reading by voice assistants.

#### Implementation

Identify the most informative sections on the homepage (intro/hero text, main value proposition). Add to `<head>` of homepage:

```html
{% if page.url == "/" %}
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "WebPage",
  "speakable": {
    "@type": "SpeakableSpecification",
    "cssSelector": [
      "[CSS class or ID of intro/hero text]",
      "[CSS class of main services section]",
      "h1"
    ]
  },
  "url": "{{ site.url }}/"
}
</script>
{% endif %}
```

Adjust CSS selectors to match actual Tailwind class names or IDs found in the homepage template.

---

### Step 1.10 — Tailwind Content Paths Audit

#### What This Does
Ensures TailwindCSS does not purge utility classes used in newly added GEO sections during production build.

#### Implementation

Open `tailwind.config.js` and confirm the `content` array includes:

```js
content: [
  './_layouts/**/*.html',
  './_includes/**/*.html',
  './_posts/**/*.{html,md}',
  './_pages/**/*.{html,md}',
  './index.{html,md}',
  './*.{html,md}',
  // add any other collection paths found in Phase 0.4
]
```

If FAQ, schema, or other GEO sections introduced new HTML files or includes, verify those paths are covered.

**Command**:
```bash
cat tailwind.config.js | grep -A 20 "content:"
```

After updating, run a production build and verify the output CSS contains the new classes:

```bash
JEKYLL_ENV=production bundle exec jekyll build
grep "faq\|schema\|geo" _site/assets/css/*.css || echo "Verify manually — check CSS output"
```

#### Success Verification
- [ ] `tailwind.config.js` content paths include all new GEO files
- [ ] Production build tested — new HTML sections not purged from CSS
- [ ] No orphaned Tailwind classes in GEO additions

---

### What NOT to Implement (GEO Anti-Patterns)

Do not add any of the following — no confirmed support from any major AI system:

- `<meta name="ai-content-url">` or `<meta name="llms">` — no spec, no adoption
- `/.well-known/ai.txt` — multiple competing proposals, no winner
- HTML comments with AI hints — stripped before AI reads content
- User-Agent sniffing to serve different content to bots — cloaking, penalized by Google
- Unofficial AI meta tags not documented by a named AI provider

---

### GEO Verification Checklist

```
ROBOTS.TXT
[ ] File at Jekyll root with GEO crawler rules
[ ] Existing rules preserved
[ ] Sitemap reference present
[ ] CDN/server-level bot blocks checked

LLMS FILES
[ ] llms.txt with real content, no placeholders, correct production URLs
[ ] llms-full.txt with 1,000+ words of substantive content
[ ] Submitted to directory.llmstxt.cloud and llmstxt.site

SCHEMA
[ ] Organisation schema in base layout <head> — Liquid variables, no hardcoded values
[ ] LocalBusiness/entity schema as separate block — correct @type
[ ] Both validated with Google Rich Results Test (after deploy)
[ ] No empty or placeholder fields

FAQ
[ ] FAQ data source identified
[ ] FAQ section on homepage before footer
[ ] FAQPage JSON-LD from same data source
[ ] Tailwind classes covered in content paths

SITEMAP
[ ] sitemap.xml generated and accessible
[ ] robots.txt references sitemap URLINDEXNOW
[ ] Key file at Jekyll root
[ ] Deploy workflow or manual process configured

TAILWIND BUILD
[ ] Content paths cover all new GEO files
[ ] Production CSS not purging GEO classes
```

---

## Phase 2: Production Build Validation

### Objective
Confirm the Jekyll + TailwindCSS build succeeds in production mode with no errors, correct CSS output, and all assets properly generated.

### Why This Is Needed
A failing or broken production build is the most fundamental deployment blocker. Issues here cascade to all subsequent phases.

### Prerequisites
- Phase 1 GEO implementation complete (new files may affect build)
- All dependencies installed

### Steps

#### 2.1 — Jekyll Production Build

**Command**:
```bash
JEKYLL_ENV=production bundle exec jekyll build
```

**Expected Output**:
```
Build successful
Exit code: 0
```

Verify:
- No fatal build errors
- No unresolved Liquid tags or filters
- No missing layouts or includes
- No broken front matter (malformed YAML)
- No missing assets referenced in templates
- No orphaned pages (pages with no layout assigned)
- No missing collections referenced in `_config.yml`
- `_site/` directory generated and populated
- No Jekyll deprecation warnings (log any that appear)

**If build fails**: **STOP. Fix build errors before continuing. Create blocker report.**

#### 2.2 — Production Config Verification

Verify `_config.yml` production settings:

```yaml
JEKYLL_ENV: production
url: "https://yourdomain.com"   # Must be set; affects canonical URLs and sitemap
baseurl: ""                      # Confirm correct for deployment path
```

#### 2.3 — TailwindCSS Purge Verification

Verify `tailwind.config.js` content paths cover all template sources:

```js
content: [
  './_layouts/**/*.html',
  './_includes/**/*.html',
  './_pages/**/*.html',
  './_posts/**/*.html',
  './_data/**/*.yml',
  './assets/js/**/*.js',
]
```

Check:
- No legitimate utility classes stripped from production CSS
- Dynamically constructed class names safelisted:

```js
safelist: [
  'bg-red-500',
  { pattern: /^text-(sm|md|lg)$/ },
]
```

Inspect CSS output:

```bash
ls -lh _site/assets/css/
```

Verify:
- Production CSS file size is reasonable (target under 20KB gzipped for typical sites)
- No critical styles missing from rendered pages
- No layout breakage on mobile, tablet, or desktop viewports
- Dark mode classes (if used) render correctly

#### 2.4 — Custom CSS / PostCSS Verification

Verify:
- No `@apply` directives reference purged utilities
- PostCSS plugins (autoprefixer, cssnano) applied correctly
- No vendor prefix issues on target browsers

#### Success Verification
- [ ] `JEKYLL_ENV=production` build exits 0
- [ ] No fatal errors, warnings documented
- [ ] `_site/` directory populated correctly
- [ ] TailwindCSS purge correct — no missing classes, no excessive CSS size
- [ ] All config files validated for production settings

---

## Phase 3: Security Audit & Hardening

### Objective
Perform comprehensive security audit and implement security best practices without breaking existing functionality. Aligns with OWASP Top 10 and production security requirements.

### Why This Is Needed
Security vulnerabilities in production can lead to data breaches, defacement, SEO spam injection, and loss of user trust. This phase must complete before deployment.

### Prerequisites
- Phase 2 production build passing
- Access to Netlify dashboard for environment variables
- Understanding of all third-party integrations

### Scope
This consolidated security audit covers:
- HTTP security headers (aligned with FINALIZE.md Phase 3)
- Content Security Policy (CSP)
- Dependency vulnerability scanning
- Secrets and environment variable audit
- Form security (spam protection, input validation)
- Netlify Functions/Edge security
- Third-party API security
- Frontend security (XSS, CSRF, sensitive data exposure)
- Authentication and session management (if applicable)
- File upload security (if applicable)
- Database security (if applicable)
- Logging and monitoring security
- Clickjacking and iframe protection
- CORS configuration

---

### Step 3.1 — HTTP Security Headers

#### What This Does
Configures browser security policies that protect against common web attacks (clickjacking, MIME sniffing, XSS, data exfiltration).

#### Implementation

Set headers in `netlify.toml` under `[[headers]]` for all routes (`/*`):

```toml
[[headers]]
  for = "/*"
  [headers.values]
    X-Frame-Options = "DENY"
    X-Content-Type-Options = "nosniff"
    X-XSS-Protection = "1; mode=block"
    Referrer-Policy = "strict-origin-when-cross-origin"
    Permissions-Policy = "camera=(), microphone=(), geolocation=(), payment=()"
    Strict-Transport-Security = "max-age=31536000; includeSubDomains; preload"
```

Verify HSTS:
- `max-age` at minimum 31536000 (1 year)
- `includeSubDomains` present
- `preload` present if submitting to HSTS preload list

#### Success Verification
- [ ] All six security headers configured in `netlify.toml`
- [ ] Headers applied to `/*` route
- [ ] HSTS configured with correct parameters
- [ ] Tested via `curl -I https://[domain]` after deploy

---

### Step 3.2 — Content Security Policy (CSP)

#### What This Does
Prevents XSS attacks by controlling which resources (scripts, styles, images, fonts, connections) the browser is allowed to load.

#### Implementation

Build a strict CSP. Start from deny-all baseline and add only required sources.

Example production CSP:

```toml
Content-Security-Policy = "default-src 'self'; script-src 'self' https://trusted-cdn.com; style-src 'self' 'unsafe-inline'; img-src 'self' data: https:; font-src 'self' https://fonts.gstatic.com; connect-src 'self' https://api.yourservice.com; form-action 'self'; frame-ancestors 'none'; base-uri 'self'; upgrade-insecure-requests;"
```

**Rules**:
- `default-src 'self'` as baseline
- No `unsafe-eval` unless absolutely required (document exception if present)
- `unsafe-inline` for scripts must be replaced with hashes or nonces if possible
- All third-party domains explicitly listed (analytics, fonts, APIs, CDNs)
- `frame-ancestors 'none'` or `frame-ancestors 'self'` set
- `upgrade-insecure-requests` present
- `form-action` restricted to `'self'` unless forms post to external services

**Testing**: After deployment, check browser DevTools console. Zero CSP violations permitted in production.

#### Success Verification
- [ ] CSP configured in `netlify.toml`
- [ ] All required third-party domains listed
- [ ] No `unsafe-eval` without documented exception
- [ ] Zero CSP violations in browser console after deploy

---

### Step 3.3 — Mixed Content

#### What This Does
Ensures all assets load over HTTPS — no insecure HTTP references that could trigger browser warnings or enable man-in-the-middle attacks.

#### Implementation

Search for HTTP references:

```bash
grep -r "http://" _site/ --include="*.html" --include="*.css" --include="*.js"
```

Expected: no results pointing to external resources over HTTP.

Verify:
- All asset URLs use HTTPS
- No `http://` references in templates, CSS, or JavaScript
- No protocol-relative URLs (`//`) that could resolve to HTTP
- Netlify `upgrade-insecure-requests` directive applied (from CSP)

#### Success Verification
- [ ] Zero HTTP references to external resources
- [ ] All URLs use HTTPS
- [ ] No protocol-relative URLs

---

### Step 3.4 — Dependency & Secrets Audit

#### What This Does
Identifies vulnerable dependencies and exposed secrets before they reach production.

#### 3.4.1 — Ruby/Bundler Dependency Audit

```bash
gem install bundler-audit  # if not installed
bundle audit check --update
```

Verify:
- No known CVEs in gem dependencies
- All gems pinned to specific versions in `Gemfile.lock`
- `Gemfile.lock` committed to version control

Address all HIGH and CRITICAL CVEs before deployment. Document any accepted LOW/MEDIUM risks.

#### 3.4.2 — Node/npm Dependency Audit

```bash
npm audit --audit-level=high
```

Verify:
- No HIGH or CRITICAL vulnerabilities in production dependencies
- `package-lock.json` committed to version control

#### 3.4.3 — Secrets and Environment Variable Audit

```bash
git log --all --full-history -- "**/*.env"
git grep -i "api_key\|secret\|password\|token\|private_key" -- '*.yml' '*.yaml' '*.js' '*.rb' '*.json' '*.toml'
```

Verify:
- `.env` files listed in `.gitignore`
- No API keys, tokens, or passwords in `_config.yml`, `netlify.toml`, or any committed file
- All secrets stored in Netlify Environment Variables UI
- No secrets in Jekyll front matter or `_data/` files
- No secrets in JavaScript files bundled into `_site/`

Specifically check `_config.yml`:

```bash
grep -i "key\|secret\|token\|password\|auth" _config.yml
```

#### 3.4.4 — Git History Secrets Scan

```bash
git log --all -p | grep -i "api_key\|secret\|password\|token" | head -50
```

If any secrets found in history: **rotate immediately** and use `git filter-repo` to purge.

#### Success Verification
- [ ] Zero HIGH/CRITICAL CVEs in Ruby dependencies
- [ ] Zero HIGH/CRITICAL CVEs in Node dependencies
- [ ] No secrets in repository or committed files
- [ ] All secrets in Netlify environment variables
- [ ] `.env` in `.gitignore`

---

### Step 3.5 — Netlify Forms Security

#### What This Does
Ensures forms are protected against spam bots and configured correctly for production.

#### 3.5.1 — Form Detection and Configuration

Verify each HTML form has Netlify detection attributes:

```html
<form name="contact" method="POST" data-netlify="true" netlify-honeypot="bot-field">
  <input type="hidden" name="form-name" value="contact" />
  ...
</form>
```

Verify:
- `data-netlify="true"` present on every form
- `name` attribute unique per form
- Hidden `form-name` input matches form `name`
- `method="POST"` set

#### 3.5.2 — Spam and Bot Protection

**Honeypot (minimum required)**:

```html
<p class="hidden" aria-hidden="true">
  <label>Don't fill this out: <input name="bot-field" /></label>
</p>
```

Hide the honeypot with CSS (not `display:none`):

```css
.hidden { position: absolute; left: -9999px; }
```

**CAPTCHA (recommended for high-traffic/sensitive forms)**:

```html
<div data-netlify-recaptcha="true"></div>
```

Requirements:
- Honeypot present on all forms as baseline
- CAPTCHA added to forms handling sensitive data, account creation, or file uploads

#### 3.5.3 — Form Submission Testing

Test each form in Netlify deploy preview:
- Submit with valid data → confirm submission in Netlify Forms dashboard
- Submit with honeypot filled → confirm submission blocked
- Submit with empty required fields → confirm validation triggers
- Verify success/error redirect URLs work

#### 3.5.4 — Form Notifications and GDPR

Verify:
- Email notifications configured in Netlify Forms settings for each form
- Notification email addresses are production addresses
- Privacy policy linked near each form collecting personal data
- Consent checkbox present where required by jurisdiction

#### Success Verification
- [ ] All forms have `data-netlify="true"` and honeypot protection
- [ ] Form submissions tested end-to-end
- [ ] Notifications configured
- [ ] GDPR/privacy compliance verified

---

### Step 3.6 — Netlify Functions & Edge Security

#### What This Does
Ensures serverless and edge functions are secure, input-validated, and error-handled.

#### 3.6.1 — Functions Directory Configuration

Verify `netlify.toml`:

```toml
[build]
  functions = "netlify/functions"
```

Verify:
- All `.js` or `.ts` function files present in functions directory
- No orphaned function references in frontend code
- Function filenames match how they are called from frontend

#### 3.6.2 — Function Code Review

For each function, verify:
- **Input validation** on all incoming parameters (type, length, format)
- **No direct use of user input** in shell commands, file paths, or database queries
- **Error responses** do not expose stack traces or internal paths
- **Secrets accessed via `process.env`**, not hardcoded
- **CORS headers** set correctly:

```js
const headers = {
  'Access-Control-Allow-Origin': 'https://yourdomain.com',
  'Access-Control-Allow-Headers': 'Content-Type',
  'Content-Type': 'application/json',
};
```

**Critical**: `Access-Control-Allow-Origin` must never be `*` for authenticated or sensitive endpoints.

#### 3.6.3 — Edge Functions Configuration

```toml
[[edge_functions]]
  path = "/api/*"
  function = "api-handler"
```

Verify edge functions do not perform operations requiring Node.js-only APIs.

#### 3.6.4 — Error Handling and Rate Limiting

Verify every function:
- Returns appropriate HTTP status codes (200, 400, 401, 403, 404, 500)
- Has try/catch wrapping main logic
- Logs errors without exposing sensitive data
- Rate limiting applied to high-traffic or sensitive functions

#### 3.6.5 — Function Testing

```bash
netlify dev
```

Test:
- All functions respond correctly locally
- Functions called by forms tested end-to-end
- API-calling functions return correct data under success and failure conditions

#### Success Verification
- [ ] All functions have input validation
- [ ] All functions have error handling
- [ ] CORS configured correctly
- [ ] Rate limiting applied where appropriate
- [ ] All functions tested locally

---

### Step 3.7 — Third-Party API Security

#### What This Does
Ensures third-party API integrations don't expose secrets, break the site, or leak user data.

#### Verification Points

For each third-party API:

**Authentication**:
- API keys stored in Netlify environment variables only
- API keys never exposed in `_site/` output, JavaScript bundles, or HTML
- Separate API keys for staging and production
- Keys scoped to minimum required permissions

**Request Validation**:
- All API responses validated before use (null checks, schema validation)
- Failed API responses handled gracefully (fallback content or error state)
- No site-breaking behavior if API is temporarily unavailable

**Rate Limiting**:
- API rate limits documented and monitored
- Retry logic with exponential backoff where appropriate
- Caching layer for non-real-time data to reduce API calls

**Data Sanitization**:
- All third-party API data sanitized before rendering in HTML
- JSON responses parsed safely; never passed to `eval()` or `innerHTML` directly
- File uploads processed through validated, sandboxed pipelines

**Fallback and Resilience**:
- Site does not hard-fail if API is down
- Graceful degradation UI when API data unavailable
- Timeouts configured on all outbound API calls

**Privacy**:
- No personal user data sent to third-party APIs without consent
- Data handling reviewed against applicable regulations
- DPAs signed with vendors handling personal data where required

#### Success Verification
- [ ] All API keys in environment variables, not in code
- [ ] All API responses validated and sanitized
- [ ] Fallback UI present for each API integration
- [ ] Rate limiting and retry logic configured

---

### Step 3.8 — Additional Security Checks

#### Clickjacking Protection
- `X-Frame-Options: DENY` set (or `SAMEORIGIN` if iframes used internally)
- CSP `frame-ancestors` directive matches `X-Frame-Options` policy

#### Third-Party Script Risk
For every third-party script:
- Source domain listed in CSP `script-src`
- Script loaded from pinned version URL (not `latest`)
- Subresource Integrity (SRI) hash applied:

```html
<script
  src="https://cdn.example.com/lib.min.js"
  integrity="sha384-HASH"
  crossorigin="anonymous">
</script>
```

Generate SRI hashes:
```bash
openssl dgst -sha384 -binary FILE | openssl base64 -A
```

#### Frontend Security
- Prevent XSS by properly escaping user input
- Sanitize HTML rendering
- Avoid storing sensitive tokens in localStorage
- Ensure forms include CSRF protection where applicable
- Prevent exposure of secrets in client-side code

#### Authentication and Session Management (if applicable)
- Secure JWT/session implementation
- Validate token expiration and refresh flow
- Secure cookie configuration (HttpOnly, Secure, SameSite)
- Proper logout invalidates sessions/tokens

#### Logging Security
- No sensitive information in logs
- Security-related events logged appropriately
- Production logs do not expose secrets

#### Success Verification
- [ ] Clickjacking protection configured
- [ ] SRI hashes on third-party scripts
- [ ] Frontend security best practices applied
- [ ] Authentication reviewed (if applicable)
- [ ] Logging security verified

---

## Phase 4: Performance & Bandwidth Optimization

### Objective
Identify and fix bandwidth bottlenecks, optimize Core Web Vitals, and improve Lighthouse scores for both Mobile and Desktop.

### Why This Is Needed
Excessive bandwidth usage increases costs, slows page loads, and degrades user experience. AI search engines favor fast, well-optimized sites.

### Prerequisites
- Phase 2 production build passing
- Phase 3 security audit complete (security changes may affect performance)

---

### Step 4.1 — Performance Audit (Baseline)

#### What This Does
Captures baseline performance metrics before optimization begins.

#### Commands

Run Lighthouse audit (via Chrome DevTools or CLI):

```bash
npx lighthouse https://[domain] --output html --output-path ./lighthouse-before.html
```

Or use PageSpeed Insights: https://pagespeed.web.dev

**Capture baseline metrics**:

| Metric | Mobile Before | Desktop Before | Target |
|--------|--------------|----------------|--------|
| LCP | ___s | ___s | < 2.5s |
| INP | ___ms | ___ms | < 200ms |
| CLS | ___ | ___ | < 0.1 |
| FCP | ___s | ___s | < 1.8s |
| TTFB | ___ms | ___ms | < 600ms |
| Performance Score | ___ | ___ | 90+ |
| Accessibility Score | ___ | ___ | 90+ |
| Best Practices Score | ___ | ___ | 90+ |
| SEO Score | ___ | ___ | 90+ |

---

### Step 4.2 — Image Optimization

#### What This Does
Reduces image payload (typically the largest bandwidth consumer) through format conversion, compression, and responsive loading.

#### Implementation

1. **Convert images to modern formats**:
   - WebP for photos (lossy compression, excellent browser support)
   - AVIF for even better compression (check browser support)
   - Keep original formats as fallback via `<picture>` element

2. **Compress images**:
   - Hero images: target under 200KB
   - Content images: target under 100KB
   - Thumbnails: target under 30KB

3. **Implement responsive images**:

```html
<img
  src="/assets/img/hero-800w.webp"
  srcset="/assets/img/hero-400w.webp 400w,
          /assets/img/hero-800w.webp 800w,
          /assets/img/hero-1200w.webp 1200w"
  sizes="(max-width: 600px) 400px,
         (max-width: 1000px) 800px,
         1200px"
  alt="[Descriptive alt text]"
  width="1200"
  height="630"
  loading="eager"  <!-- For hero/LCP images -->
>
```

4. **Loading strategy**:
   - Hero images (above fold): `loading="eager"` or no lazy attribute
   - Below-fold images: `loading="lazy"`
   - LCP image preloaded:

```html
<link rel="preload" as="image" href="/assets/img/hero.webp">
```

5. **Accessibility**: Every content image must have descriptive `alt` text. Decorative images use `alt=""` and `role="presentation"`.

#### Success Verification
- [ ] Images converted to WebP/AVIF where appropriate
- [ ] Image sizes within target ranges
- [ ] Responsive `srcset`/`sizes` on all large images
- [ ] LCP image preloaded
- [ ] `loading="lazy"` on below-fold images
- [ ] All content images have `alt` text

---

### Step 4.3 — JavaScript Optimization

#### What This Does
Reduces JavaScript bundle size and eliminates render-blocking scripts.

#### Implementation

1. **Script loading attributes**:
   - `defer` for non-critical scripts (executes after HTML parse, in order)
   - `async` only where script order independence is confirmed
   - No render-blocking scripts in `<head>` without `defer` or `async`

2. **Bundle optimization**:
   - Remove unused JavaScript
   - Remove duplicate dependencies
   - Review bundle size (target: under 50KB gzipped for inline JS)

3. **Code splitting** (if applicable):
   - Route-based code splitting
   - Dynamic imports for non-critical functionality
   - Lazy load components that aren't immediately visible

#### Success Verification
- [ ] No render-blocking scripts in `<head>`
- [ ] `defer` used on non-critical scripts
- [ ] JavaScript bundle size reviewed
- [ ] Unused code removed

---

### Step 4.4 — CSS Optimization

#### What This Does
Ensures Tailwind purge is correct and CSS delivery is optimized.

#### Implementation

1. **Tailwind purge verification** (already done in Phase 2.3):
   - CSS file size target under 20KB gzipped
   - No unused utilities in production CSS

2. **CSS delivery**:
   - Critical CSS inlined or preloaded where applicable
   - No additional large unused CSS bundles

3. **PostCSS optimization**:
   - `cssnano` for minification
   - `autoprefixer` for vendor prefixes

#### Success Verification
- [ ] Production CSS file under 20KB gzipped
- [ ] No critical styles missing
- [ ] CSS delivery optimized

---

### Step 4.5 — Font Optimization

#### What This Does
Prevents layout shifts and improves perceived load speed for custom fonts.

#### Implementation

1. **Font display strategy**:

```css
@font-face {
  font-family: 'CustomFont';
  src: url('/assets/fonts/custom.woff2') format('woff2');
  font-display: swap;  /* Text visible immediately, font swaps when loaded */
}
```

2. **Preload critical fonts**:

```html
<link rel="preload" href="/assets/fonts/font.woff2" as="font" type="font/woff2" crossorigin>
```

3. **Font subsetting**: Include only characters needed (Latin, etc.)
4. **System font fallback**: Define in CSS until custom font loads

#### Success Verification
- [ ] `font-display: swap` or `optional` on all web fonts
- [ ] Critical fonts preloaded
- [ ] System font fallback defined
- [ ] No layout shift during font loading (CLS check)

---

### Step 4.6 — Caching Headers

#### What This Does
Configures browser and CDN caching to reduce repeat downloads.

#### Implementation

Add to `netlify.toml`:

```toml
# Long cache for fingerprinted assets
[[headers]]
  for = "/assets/*"
  [headers.values]
    Cache-Control = "public, max-age=31536000, immutable"

# No cache for HTML (always fetch latest)
[[headers]]
  for = "/*.html"
  [headers.values]
    Cache-Control = "public, max-age=0, must-revalidate"
```

#### Success Verification
- [ ] Cache headers configured in `netlify.toml`
- [ ] Fingerprinted assets use long cache with `immutable`
- [ ] HTML uses `max-age=0, must-revalidate`

---

### Step 4.7 — Performance Re-Audit

#### What This Does
Measures post-optimization performance and compares to baseline.

**Command**:
```bash
npx lighthouse https://[domain] --output html --output-path ./lighthouse-after.html
```

**Compare metrics**:

| Metric | Before | After | Improvement |
|--------|--------|-------|-------------|
| LCP | ___s | ___s | ___% |
| INP | ___ms | ___ms | ___% |
| CLS | ___ | ___ | ___% |
| FCP | ___s | ___s | ___% |
| TTFB | ___ms | ___ms | ___% |
| Performance Score | ___ | ___ | ___ |
| Accessibility Score | ___ | ___ | ___ |
| Best Practices Score | ___ | ___ | ___ |
| SEO Score | ___ | ___ | ___ |

#### Success Verification
- [ ] All Core Web Vitals within "Good" thresholds
- [ ] Performance score 90+ (both Mobile and Desktop)
- [ ] Accessibility score 90+
- [ ] Best Practices score 90+
- [ ] SEO score 90+

---

## Phase 5: Cache Strategy & Invalidation

### Objective
Identify all caching mechanisms and ensure every production deployment automatically serves the latest application version without requiring users to manually clear cache.

### Why This Is Needed
Stale caches are the most common cause of post-deployment bugs. Users seeing old HTML with new CSS/JS can break the site. This phase prevents cache-related deployment issues.

### Prerequisites
- Phase 2 production build passing
- Phase 4 caching headers configured

---

### Step 5.1 — Identify All Caching Layers

#### What This Does
Catalogs every cache that could serve stale content.

#### Cache Inventory

For each layer, document:
- Where it's configured
- Why it exists
- Whether it can cause stale content after deployment

**Common caching layers in Jekyll + Netlify stack**:

1. **Browser cache**: Controlled by `Cache-Control` headers (Phase 4.6)
2. **Netlify CDN cache**: Managed by Netlify automatically, respects `Cache-Control` headers
3. **Service Worker** (if PWA): Cache strategies defined in service worker JavaScript
4. **Asset fingerprinting**: Jekyll/Liquid or build tool generates hashed filenames
5. **API response cache**: If Netlify Functions cache responses
6. **CloudCannon preview cache**: CMS preview may cache old builds

#### Success Verification
- [ ] All caching layers identified
- [ ] Documentation for each layer complete

---

### Step 5.2 — Implement Cache-Busting Strategy

#### What This Does
Ensures every deployment automatically invalidates old caches and serves new content.

#### Implementation

1. **Asset fingerprinting**:
   Jekyll doesn't natively fingerprint assets. Options:
   - Use a plugin (`jekyll-assets`)
   - Add build step in Netlify to rename files with content hash
   - Use query string versioning: `/assets/css/main.css?v={{ site.time | date: '%s' }}`

   Recommended approach for Jekyll + Netlify:

```html
<link rel="stylesheet" href="{{ '/assets/css/main.css' | relative_url }}?v={{ site.time | date: '%s' }}">
```

2. **Service Worker** (if exists):
   Ensure service worker immediately activates new version:

```js
self.addEventListener('install', event => {
  self.skipWaiting(); // Activate immediately
});

self.addEventListener('activate', event => {
  event.waitUntil(
    clients.claim() // Take control of all clients
    .then(() => caches.keys().then(keys => 
      Promise.all(keys.filter(key => key !== CACHE_NAME).map(key => caches.delete(key)))
    ))
  );
});
```

3. **CDN cache purging**:
   Netlify automatically purges CDN cache on deploy. Verify this is working:
   - Deploy a change
   - Check response headers for new `ETag` values
   - Confirm old asset URLs return 404

4. **API cache** (if applicable):
   - Ensure API responses have appropriate `Cache-Control` headers
   - Add version prefix to API endpoints if needed
   - Use `no-cache` or `max-age=0` for dynamic data

#### Success Verification
- [ ] Asset versioning in place (query string or fingerprinting)
- [ ] Service worker activates immediately (if present)
- [ ] Old caches deleted during activation
- [ ] API responses not unintentionally cached
- [ ] CDN cache purges automatically on deploy

---

### Step 5.3 — Verify Cache Invalidation

#### What This Does
Confirms that after a fresh production deployment, users automatically receive the latest version.

#### Test Procedure

1. Deploy a visible change (e.g., add a comment to HTML, change a CSS color)
2. Open the site in a browser
3. Verify the change is visible immediately
4. Open DevTools → Application → Cache Storage
5. Verify old cache entries are removed
6. Check Network tab — no requests for old asset URLs

#### Success Verification
- [ ] Users receive latest HTML without hard refresh
- [ ] Old JS/CSS bundles never reused after deployment
- [ ] No stale Service Worker remains active
- [ ] New API changes immediately reflected
- [ ] Existing performance optimizations preserved

---

## Phase 6: CloudCannon CMS Validation

### Objective
Verify CloudCannon CMS can edit all content, SEO, navigation, and media fields as intended by content editors.

### Why This Is Needed
If the CMS cannot edit content, the site is not production-ready for content teams.

### Prerequisites
- Phase 2 production build passing
- Phase 1 GEO implementation complete (CMS must be able to edit new content)

---

### Step 6.1 — Configuration File

Verify `cloudcannon.config.yml` (or `cloudcannon.config.js`) is present and valid:

```bash
cat cloudcannon.config.yml
```

Verify:
- Collections defined for all editable content types
- `_schema` or `schemas` defined for structured data
- No broken paths in collection configuration

---

### Step 6.2 — Editable Regions

Verify the following are editable by CMS users:

**SEO**:
- Page title
- Meta description
- Social (OG) image
- Canonical URL override (if needed)

**Navigation**:
- Header navigation items and URLs
- Footer navigation items and URLs
- CTA button labels and URLs

**Content**:
- Hero headline, subheadline, and CTA
- Body page content
- Blog/post content

**Media**:
- Image sources
- Image alt text (editable per image)

**Forms**:
- No hardcoded recipient email addresses (use environment variables)

---

### Step 6.3 — Front Matter Schema

Verify:
- All editable fields have correct types (`text`, `rich_text`, `image`, `boolean`, etc.)
- Required fields marked as required in schema
- No fields that allow arbitrary HTML input without sanitization
- Image fields enforce allowed file types

---

### Step 6.4 — Previews and Permissions

Verify:
- Visual editor previews render correctly for all collection types
- Live editing does not break layout
- Preview URLs accessible to CMS users
- CMS user roles configured to minimum required access
- Editors cannot modify `_config.yml` or `netlify.toml` via CMS
- Publishing workflow configured (if draft/review flow required)

#### Success Verification
- [ ] `cloudcannon.config.yml` valid
- [ ] All content types have editable regions
- [ ] SEO fields editable
- [ ] Navigation editable
- [ ] Media with alt text editable
- [ ] Front matter schema correct
- [ ] Previews working
- [ ] User permissions appropriate

---

## Phase 7: Netlify Platform Validation

### Objective
Verify Netlify deployment configuration is correct, secure, and production-ready.

### Why This Is Needed
Misconfigured Netlify settings cause deployment failures, broken redirects, missing HTTPS, or environment variable issues.

### Prerequisites
- Phase 2 production build passing
- All previous phases complete
- Netlify dashboard access

---

### Step 7.1 — Build Configuration

Verify `netlify.toml`:

```toml
[build]
  command = "JEKYLL_ENV=production bundle exec jekyll build"
  publish = "_site"
  functions = "netlify/functions"

[build.environment]
  RUBY_VERSION = "3.x.x"
  NODE_VERSION = "20"
```

Verify:
- `JEKYLL_ENV=production` explicitly set in build command or environment
- `publish` directory matches Jekyll `destination` in `_config.yml`
- Ruby and Node versions pinned

---

### Step 7.2 — Branch Deploys and Previews

Verify:
- Production branch (`main` or `master`) set as production branch
- Deploy previews enabled for pull requests
- Branch deploy settings configured as intended

Set noindex header for deploy previews:

```toml
[[context.deploy-preview.headers]]
  for = "/*"
  [context.deploy-preview.headers.values]
    X-Robots-Tag = "noindex"
```

---

### Step 7.3 — Redirects

Verify each redirect rule in `netlify.toml` or `_redirects`:
- Source path correct
- Destination path correct
- HTTP status code appropriate (301 permanent, 302 temporary)
- No redirect loops

Test redirects:

```bash
curl -I https://yourdomain.com/old-path
```

Expected: `Location: /new-path` header present.

---

### Step 7.4 — Custom 404

Verify:
- Custom 404 page exists at `404.html` or `404/index.html` in `_site/`
- 404 page includes site navigation for recovery
- Netlify configured to serve custom 404

---

### Step 7.5 — Environment Variables

Verify in Netlify UI (Site Settings → Environment Variables):
- All variables referenced in Functions and build hooks present
- Values set for `Production` scope
- No sensitive values in `netlify.toml`
- No unused variables remaining

---

### Step 7.6 — HTTPS and Domain

Verify:
- SSL certificate provisioned and active
- Custom domain HTTPS confirmed
- HSTS header applied (Phase 3.1)
- Primary domain configured (with and without `www`)
- Preferred domain set
- Non-preferred domain redirects correctly
- DNS propagation confirmed

#### Success Verification
- [ ] `netlify.toml` build configuration valid
- [ ] Deploy previews use noindex header
- [ ] Redirects tested
- [ ] Custom 404 present
- [ ] All environment variables complete
- [ ] HTTPS active with no mixed content
- [ ] Domain configuration correct

---

## Phase 8: Final Production Sign-off

### Objective
Complete final risk review and determine if the project is production-ready.

### Why This Is Needed
Systematic sign-off prevents deployment of projects with known critical issues.

### Prerequisites
- All previous phases complete
- All verification checklists passed

---

### Step 8.1 — Production Risk Review

Classify all remaining issues:

#### Critical — Must Block Deployment
- Build failure
- Broken navigation or core pages missing
- Fatal runtime error in any Function
- CSP blocking legitimate functionality
- Secrets committed to repository or exposed in `_site/`
- HIGH/CRITICAL CVEs in dependencies
- Broken forms with no functional fallback
- Severe accessibility failure (site unusable without assistive technology)
- Invalid Netlify configuration causing deploy failure
- Missing HTTPS or active mixed content

#### High — Strongly Recommended Before Launch
- Missing page metadata (title, description, canonical)
- Broken structured data / JSON-LD
- Significant Core Web Vitals regression (LCP > 4s, CLS > 0.25)
- CSP violations in browser console
- Missing SRI hashes on third-party scripts
- Honeypot absent from public forms
- Environment variables referencing non-existent Netlify variables
- CloudCannon editability broken for primary content fields

#### Medium — Post-Launch Acceptable
- Minor Lighthouse deductions (< 5 point drop)
- Small accessibility improvements (non-blocking)
- Non-critical redirect optimizations
- Minor CMS UI improvements

#### Low — Nice to Have
- Additional structured data schemas
- Further image compression gains
- Minor code cleanup

---

### Step 8.2 — Deployment Decision

**Deployment is BLOCKED if any Critical issues exist.**

Output:
```
STATUS: NOT READY FOR PRODUCTION
```

**Deployment is APPROVED only when**:
- Build passes with `JEKYLL_ENV=production`
- No Critical issues exist
- Security headers validated
- No secrets exposed
- Dependency audit passed

Output:
```
STATUS: READY FOR PRODUCTION
```

---

### Step 8.3 — Final Report Format

Generate comprehensive deployment report:

```yaml
deployment_report:

  status: READY FOR PRODUCTION | NOT READY FOR PRODUCTION

  build:
    jekyll_build: pass | fail
    jekyll_env: production
    tailwind_purge: pass | fail
    exit_code: 0

  security:
    http_headers: pass
    csp_configured: true
    csp_violations: 0
    hsts_configured: true
    mixed_content: none
    secrets_in_repo: none
    secrets_in_site_output: none
    dependency_cves_critical: 0
    dependency_cves_high: 0

  forms:
    forms_detected: [list]
    honeypot_present: true
    submission_tested: true

  functions:
    functions_detected: [list]
    input_validation: pass
    error_handling: pass
    cors_configured: true

  apis:
    integrations_detected: [list]
    keys_in_env_vars: true
    fallback_ui_present: true

  cloudcannon:
    config_valid: true
    seo_editable: true
    content_editable: true

  seo:
    all_pages_have_title: true
    all_pages_have_description: true
    sitemap_valid: true
    robots_valid: true
    structured_data_valid: true

  geo:
    llms_txt_present: true
    llms_full_txt_present: true
    robots_txt_configured: true
    schema_added: true

  accessibility:
    wcag_target: 2.1 AA
    single_h1_per_page: true
    images_have_alt: true
    keyboard_navigable: true

  performance:
    lcp: ___s
    inp: ___ms
    cls: ___
    ttfb: ___ms

  lighthouse:
    performance: ___
    accessibility: ___
    best_practices: ___
    seo: ___

  netlify:
    build_config_valid: true
    redirects_tested: true
    custom_404_present: true
    https_active: true

  risks:
    critical: []
    high: []
    medium: []
    low: []

  recommendations: []
```

---

### Step 8.4 — Success Condition

A project is production-ready only when ALL of the following are true:

- [ ] Production Jekyll build succeeds (`JEKYLL_ENV=production`, exit code 0)
- [ ] TailwindCSS purge correct, no classes incorrectly stripped
- [ ] HTTP security headers configured (X-Frame-Options, CSP, HSTS, X-Content-Type-Options, Referrer-Policy, Permissions-Policy)
- [ ] CSP validated with zero console violations
- [ ] No secrets in repository or site output
- [ ] Dependency audit passes (zero HIGH/CRITICAL CVEs)
- [ ] All forms have bot protection and tested end-to-end
- [ ] All Netlify Functions validated, input-validated, and error-handled
- [ ] All third-party API integrations secured and resilient
- [ ] CloudCannon CMS editability confirmed for all content, SEO, and media fields
- [ ] GEO implementation complete (llms.txt, robots.txt, schema, FAQ)
- [ ] SEO metadata complete on all indexable pages
- [ ] Sitemap and robots.txt valid and production-configured
- [ ] Accessibility: WCAG 2.1 AA, single H1, keyboard navigable, all images alt-tagged
- [ ] Core Web Vitals within acceptable targets
- [ ] Netlify configuration valid with tested redirects and custom 404
- [ ] HTTPS active with no mixed content
- [ ] Preview/staging deployments blocked from search indexing
- [ ] Cache invalidation verified — users receive latest deployment automatically

**Until all conditions are met: continue auditing, fixing, validating, and reporting.**

---

## Troubleshooting

### Common Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| Build fails with `jekyll-postcss` error | Tailwind not installed or configured | Run `npm install`, verify `postcss.config.js` |
| Tailwind classes missing in production | Content paths don't cover new files | Add paths to `tailwind.config.js` content array |
| CSP blocks legitimate scripts | Missing source in CSP directive | Add domain to CSP `script-src` |
| Forms not appearing in Netlify dashboard | Missing `data-netlify="true"` or `form-name` | Verify form attributes match Netlify requirements |
| `llms.txt` not accessible at root | Jekyll not copying plain text files | Verify file has no YAML front matter |
| Schema validation fails in Google test | Empty fields or incorrect `@type` | Remove empty fields, verify `@type` matches business |
| Old assets served after deploy | Cache headers too aggressive | Verify `Cache-Control: max-age=0` on HTML |
| Environment variables undefined in functions | Not set in Netlify UI for production scope | Set variables in Netlify dashboard → Production |
| CLS poor after font loading | Font not using `font-display: swap` | Add `font-display: swap` to `@font-face` |
| Lighthouse performance score < 90 | Large images, render-blocking resources | Optimize images, defer scripts, preload LCP image |

---

## Best Practices

### GEO Best Practices
- Content depth: Pages of 1,000–3,000 words with 10+ headings see higher AI citation
- Specificity: Pages with real data, definitions, and comparisons see 50%+ higher AI citation
- Only 15% of retrieved pages get cited — clean structure helps pass the second filter
- JSON-LD primarily benefits Bing/Copilot and Google; ChatGPT and Claude read it as plain text
- ChatGPT cites fewer sources but goes deeper (5× per-citation impact); Perplexity casts wider

### Security Best Practices
- Principle of least privilege for all API keys and CMS user roles
- Secure defaults — deny all, allow specific
- OWASP Top 10 alignment for all security decisions
- Never store secrets in version control — environment variables only
- Input validation on every function, form, and API endpoint

### Performance Best Practices
- Preload LCP image, defer non-critical JavaScript
- Use modern image formats (WebP, AVIF) with fallbacks
- Inline critical CSS, preload fonts with `font-display: swap`
- Long cache for fingerprinted assets, no-cache for HTML
- Monitor Core Web Vitals in production (use Netlify Analytics or Google CrUX)

### CMS Best Practices
- All content editable via CloudCannon — no hardcoded text in layouts
- SEO fields (title, description, OG image) editable per page
- Image alt text editable per image
- Publishing workflow configured for content review

---

## Future Improvements

1. **Automated GEO monitoring**: Track AI citation rates and referral traffic over time
2. **Real User Monitoring (RUM)**: Implement Web Vitals tracking in production for field data
3. **Automated visual regression testing**: Compare screenshots before/after deployment
4. **Advanced caching**: Implement stale-while-revalidate for API responses
5. **Image CDN**: Use Netlify Image CDN for on-the-fly image transformation
6. **AI content freshness**: Implement `lastmod` in sitemap and content freshness signals
7. **A/B testing**: Use Netlify Edge Functions for canary deployments
8. **Automated accessibility testing**: Integrate axe-core or Pa11y into CI pipeline

---

## Appendix

### A. GitHub Pages Plugin Compatibility

If deploying via `github-pages` gem (no custom Actions), only these plugins are allowed:

**Allowed on GitHub Pages**:
`jekyll-sitemap`, `jekyll-feed`, `jekyll-seo-tag`, `jekyll-redirect-from`, `jekyll-paginate`, `jekyll-mentions`, `jekyll-gist`, `jekyll-coffeescript`, `jekyll-commonmark-ghpages`

**Not allowed without Actions**:
`jekyll-postcss`, `jekyll-assets`, most third-party gems

If the project uses `jekyll-postcss` for Tailwind but deploys to GitHub Pages via the gem — this is a broken setup. Migrate to GitHub Actions workflow.

---

### B. Key URLs for Testing and Submission

| Resource | URL |
|----------|-----|
| Google Rich Results Test | https://search.google.com/test/rich-results |
| PageSpeed Insights | https://pagespeed.web.dev |
| WebAIM Contrast Checker | https://webaim.org/resources/contrastchecker/ |
| SRI Hash Generator | https://www.srihash.org |
| LLMs.txt Directory | https://directory.llmstxt.cloud |
| LLMs.txt Site Directory | https://llmstxt.site |
| Bing IndexNow | https://www.bing.com/indexnow |

---

### C. Research References

- [GEO: Generative Engine Optimization — Princeton & IIT Delhi, KDD 2024](https://arxiv.org/abs/2311.09735)
- [llms.txt Standard Specification](https://llmstxt.org)
- [Jekyll Plugin Whitelist — GitHub Pages](https://pages.github.com/versions/)
- [jekyll-seo-tag Documentation](https://github.com/jekyll/jekyll-seo-tag)
- [jekyll-sitemap Documentation](https://github.com/jekyll/jekyll-sitemap)
- [Tailwind CSS Content Configuration](https://tailwindcss.com/docs/content-configuration)
- [IndexNow Protocol](https://www.indexnow.org/documentation)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Web Content Accessibility Guidelines (WCAG) 2.1](https://www.w3.org/TR/WCAG21/)

---

### D. Quick Reference: Commands

```bash
# Phase 0: Discovery
cat _config.yml
cat package.json
cat tailwind.config.js
ls -la _layouts/ _includes/ _posts/ _data/

# Phase 2: Production Build
JEKYLL_ENV=production bundle exec jekyll build
ls -lh _site/assets/css/

# Phase 3: Security Audit
bundle audit check --update
npm audit --audit-level=high
git grep -i "api_key\|secret\|password\|token" -- '*.yml' '*.js' '*.json' '*.toml'
grep -r "http://" _site/ --include="*.html" --include="*.css" --include="*.js"

# Phase 4: Performance Audit
npx lighthouse https://[domain] --output html --output-path ./lighthouse.html

# Phase 5: Cache Verification
curl -I https://[domain]

# Phase 6: CloudCannon
cat cloudcannon.config.yml

# Phase 7: Netlify
cat netlify.toml
curl -I https://[domain]/old-path
```

---