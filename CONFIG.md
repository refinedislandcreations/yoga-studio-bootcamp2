# CONFIG.md

## Project: The Yoga Studio
**URL:** `https://the-yoga-studio.refinedislandcreations.com`

### GEO & SEO Implementation

#### robots.txt
Deploy the standard portfolio `robots.txt` file to the site root.
*File content defined in the global AGENTS.md.*

#### Title & Meta
No changes needed. The existing tags are correct and require no updates.

#### Structured Data
Add the `CreativeWork` schema to the `<head>`. This explicitly connects the concept site to the Refined Island Creations portfolio.

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "CreativeWork",
  "name": "The Yoga Studio - Custom-Coded Website by Refined Island Creations",
  "description": "A concept website for a yoga and wellness studio. Designed and built by Refined Island Creations as a portfolio demonstration of custom-coded wellness website design with class scheduling and booking integration.",
  "url": "https://the-yoga-studio.refinedislandcreations.com",
  "creator": {
    "@type": "Organization",
    "name": "Refined Island Creations",
    "url": "https://refinedislandcreations.com"
  },
  "isPartOf": {
    "@type": "CollectionPage",
    "name": "Portfolio - Refined Island Creations",
    "url": "https://refinedislandcreations.com/portfolio.html"
  },
  "about": {
    "@type": "SportsActivityLocation",
    "name": "The Yoga Studio"
  }
}
</script>