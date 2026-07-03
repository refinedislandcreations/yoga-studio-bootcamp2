# Mission

Act as a senior Jekyll, SEO, Core Web Vitals, and technical search optimization engineer.

Do **not** explain what you will do. Execute the work directly across the entire repository.

Your goal is to achieve the following:

## Required Success Criteria

### PageSpeed Insights

* Performance: **100**
* Accessibility: **100**
* Best Practices: **100**
* SEO: **100**
* Agentic Browsing: **3/3**

### Google Rich Results Test

The homepage and every eligible page must produce **valid rich results** instead of:

> No items detected.

If a page is not eligible for any rich result type, explain why and implement the appropriate supported structured data where applicable.

---

# Repository Audit

Perform a complete audit of the entire Jekyll project.

Inspect every:

* layout
* include
* page
* post
* collection
* plugin
* configuration
* asset
* JavaScript
* CSS
* Liquid template
* generated HTML
* sitemap
* robots.txt
* RSS
* manifest
* metadata
* structured data

Do not assume previous implementations are correct.

Remove duplicate, conflicting, deprecated, or ineffective SEO implementations.

---

# Structured Data

Validate every JSON-LD block.

Ensure there are:

* no syntax errors
* no invalid properties
* no duplicate entities
* no conflicting IDs
* no orphan objects
* no nested mistakes

Implement appropriate Schema.org types where applicable.

Examples include:

* Organization
* WebSite
* WebPage
* BreadcrumbList
* Article
* BlogPosting
* FAQPage
* HowTo
* Person
* ImageObject
* VideoObject
* CollectionPage
* SearchAction
* ItemList
* ProfilePage
* ContactPage
* AboutPage

Only use schema types that are actually supported and appropriate for each page.

Every entity should have:

* @id
* url
* name
* description
* image
* inLanguage
* publisher
* mainEntityOfPage
* isPartOf

where appropriate.

Connect entities correctly using @id references.

---

# Rich Results

Ensure pages qualify for Google's Rich Results where eligible.

Implement and validate:

* Article
* Breadcrumb
* FAQ
* HowTo
* Profile
* Video
* Product (only if applicable)
* Recipe (only if applicable)

Do not generate fake content merely to trigger rich results.

Only implement rich result types that accurately reflect the page.

After implementation, validate against Google's Rich Results requirements.

---

# Metadata

Audit all metadata.

Ensure:

* unique title
* unique meta description
* canonical URL
* robots directives
* Open Graph
* Twitter Cards
* theme-color
* viewport
* charset
* hreflang
* language
* author
* publisher

Generate missing metadata automatically from front matter where possible.

---

# Core Web Vitals

Optimize for perfect Lighthouse scores.

Improve:

* LCP
* CLS
* INP
* TTFB
* FCP
* Speed Index
* Total Blocking Time

Eliminate unnecessary rendering work.

---

# Performance

Optimize:

* critical CSS
* CSS size
* JavaScript execution
* unused CSS
* unused JavaScript
* render-blocking resources
* image loading
* font loading
* caching
* preconnect
* dns-prefetch
* preload
* modulepreload
* compression
* lazy loading
* responsive images
* SVG optimization

Reduce:

* DOM size
* layout shifts
* network requests
* duplicate assets

---

# Accessibility

Achieve Lighthouse Accessibility 100.

Verify:

* heading hierarchy
* landmarks
* labels
* alt text
* focus states
* keyboard navigation
* contrast
* ARIA usage
* semantic HTML

---

# SEO

Achieve Lighthouse SEO 100.

Verify:

* crawlability
* canonical consistency
* sitemap.xml
* robots.txt
* structured data
* internal linking
* anchor text
* pagination
* indexability
* duplicate metadata
* broken links
* image metadata

---

# Agentic Browsing

Identify why Lighthouse reports only **1/2 Agentic Browsing**.

Determine the exact missing requirement.

Implement whatever is required to achieve **3/3** while remaining compliant with current Google recommendations.

Do not guess.

Verify against the latest Lighthouse implementation.

---

# GEO (Generative Engine Optimization)

Review every page for AI discoverability.

Improve:

* entity consistency
* semantic HTML
* structured content
* topical completeness
* FAQ where appropriate
* author information
* citations
* content hierarchy
* machine-readable metadata
* descriptive headings

Ensure the site is optimized for AI assistants and search engines without keyword stuffing.

---

# Jekyll

Follow Jekyll best practices.

Avoid:

* duplicated Liquid logic
* unnecessary includes
* excessive loops
* redundant builds
* invalid front matter

Keep templates maintainable.

---

# Validation

Run and verify:

* Jekyll build
* HTML validation
* JSON-LD validation
* structured data validation
* Lighthouse
* PageSpeed
* Rich Results eligibility

---

# Final Deliverables

Provide:

1. Every file changed.
2. Exact code modifications.
3. Why each change was made.
4. Performance improvements.
5. SEO improvements.
6. Structured data improvements.
7. Rich Results improvements.
8. Agentic Browsing improvements.
9. Remaining limitations (if any).
10. A checklist confirming all success criteria have been met.

Do not stop until every possible optimization has been completed. If a requested score cannot reach 100 due to external factors (hosting, third-party resources, browser variance, or Lighthouse limitations), identify the exact blocker with supporting evidence and implement the best possible mitigation instead of claiming success.
