# Universal Jekyll AI Optimization Instructions

Execute the following steps to achieve 100/100/100/100 and 3/3 Agentic Browsing on PageSpeed Insights for **any Jekyll project**.

## 1. Fix SEO Crawlability (Target: 100 SEO)
Search engines and Lighthouse heavily penalize empty or un-crawlable anchor tags.
- Scan the entire project (`_layouts`, `_includes`, and markdown files) for any placeholder `href="#"` or front matter `url: "#"` attributes.
- Replace all placeholder instances with a valid, crawlable path (e.g., `href="/"` or the actual target page).

## 2. Implement Complete SEO Tags (Target: 100 SEO)
Ensure all pages have proper OpenGraph, Twitter Cards, and Canonical tags.
- Ensure the `jekyll-seo-tag` plugin is installed in `Gemfile` and `_config.yml`.
- Verify that the `{% seo %}` liquid tag is present inside the `<head>` of your main layout file (usually `_layouts/default.html` or `_includes/head.html`). 
- *Note: If the project requires hardcoded custom `<title>` or `<meta name="description">` tags, use `{% seo title=false %}` instead so it doesn't overwrite the custom title.*

## 3. Fix LCP Performance (Target: 100 Performance)
Eliminate Largest Contentful Paint (LCP) delays by forcing the browser to load the largest above-the-fold asset immediately.
- Identify the main hero image or above-the-fold media for the site (this could be in a hero include, the homepage layout, or a component block).
- Add the `fetchpriority="high"` and `loading="eager"` attributes to the `<img>` tag of that critical asset.
- Ensure all other images *below* the fold have `loading="lazy"`.

## 4. Fix Agentic Browsing Discovery (Target: 3/3 Agentic Browsing)
Ensure AI agents can programmatically discover the `llms.txt` and `llms-full.txt` files without guessing the root URL.
- Add the following tags to the main `<head>` layout (e.g., `_layouts/default.html`):
```html
<!-- Agentic Browsing Discovery -->
<link rel="alternate" type="text/markdown" href="/llms.txt" />
<link rel="alternate" type="text/markdown" href="/llms-full.txt" />
```

## 5. Pass Google Rich Results Testing (Target: Rich Snippets)
If you are applying non-visual schemas for GEO (like `CreativeWork` or a basic `Organization`), Google's Rich Results Test tool will ignore them and return a "No items detected" error.
- To satisfy the Google testing tool, append a visually supported schema, such as a `BreadcrumbList`, alongside any custom schemas in your `<head>` layout:
```html
<!-- GEO: Breadcrumb Schema (For Google Rich Results) -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [{
    "@type": "ListItem",
    "position": 1,
    "name": "Home",
    "item": "{{ site.url }}"
  }]
}
</script>
```
