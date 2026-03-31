---
paths:
  - "**/*.html"
  - "**/robots.txt"
  - "**/sitemap.xml"
  - "**/llms.txt"
  - "**/head/**"
  - "**/seo/**"
  - "**/meta/**"
  - "**/schema/**"
  - "**/structured-data/**"
  - "**/layout.*"
  - "**/Layout.*"
  - "**/BaseHead.*"
  - "**/Head.*"
---
# SEO + GEO Rules for Animal Advocacy Websites

Websites built for animal advocacy serve two discovery channels: traditional search engines and AI answer systems (ChatGPT, Perplexity, Google AI Overviews, Claude, Gemini, Bing Copilot). The game has shifted from optimizing for keyword matching to optimizing for **intent satisfaction** (does your content completely solve the user's problem?), **entity authority** (does Google recognize your brand as a trusted entity in its knowledge graph?), and **technical excellence** (can crawlers efficiently process your site?). Approximately 60% of searches end without a click — appearing in search results is no longer enough; you need to be the source Google trusts enough to synthesize into AI-generated answers.

**How AI citation works:** Google generates an answer first, then scores content against it using embedding distance. Only 17-32% of AI Overview citations come from pages ranking in the organic top 10 — lower-authority pages can win with the right structure (source: Authoritas AI Overviews study, 2024). Domain Authority correlates with AI citations at only r=0.18; topical authority (r=0.40) and branded web mentions (r=0.664) are the real predictors (source: Kalicube GEO correlation study, 2025). 80% of URLs cited by AI assistants do not rank in Google's top search results for the same queries (source: Semrush AI citation analysis, 2024).

---

## HTML Structure

Every page needs exactly one `<h1>` tag containing the primary topic. Use a logical heading hierarchy (`h1 > h2 > h3`), never skipping levels. Phrase `<h2>` headings as questions when the section answers something — question-based headings improve Featured Snippet selection, People Also Ask appearance, and produce 7× more AI citations for smaller sites. The first paragraph after any heading must directly answer that question in 40-60 words. AI systems pull from the first 30% of content 44% of the time — lead with the answer.

Keep paragraphs to 2-4 sentences (40-60 words). Structure content as self-contained 120-180 word modules — this generates 70% more ChatGPT citations than unstructured prose.

Use semantic HTML correctly: `<article>`, `<section>`, `<nav>`, `<aside>`, `<header>`, `<footer>`, `<main>`. Add `lang` attribute to `<html>`. Every `<img>` must have a descriptive `alt` attribute. Every `<a>` must have meaningful anchor text — never "click here".

Prefer these semantic elements for structured data:

- `<table>` for comparison data (32.5% of AI-cited content uses tables)
- `<ol>` and `<ul>` for lists (78% of AI answers include list formats)
- `<blockquote cite="...">` for expert quotations (+28-40% AI visibility)
- `<time datetime="YYYY-MM-DD">` for dates
- `<dfn>` for term definitions
- `<abbr title="full term">` on first use
- Add `id` attributes to all `<h2>` and `<h3>` elements

**Do NOT:**
- Hide content behind JavaScript-only rendering — AI crawlers often cannot execute JS, and relying on JS rendering increases crawl cost for search engines. All critical content must be in the initial HTML response (SSR or pre-rendered).
- Use `display:none` or `visibility:hidden` on content to be indexed.
- Rely on infinite scroll — use paginated `<a>` links.
- Use iframes for primary content.
- Keyword-stuff — stuffing decreases AI visibility by 10% and triggers spam detection.

---

## Semantic Writing for AI Retrieval

AI citation systems retrieve at the sentence and paragraph level, not the page level. Every piece of content must be written to survive extraction from context.

**Entity salience — use active voice:** Always make the primary entity the grammatical subject. "Open Paws documented 34% adoption growth in 2025" gives Open Paws a salience score of 0.74. The passive equivalent ("adoption growth of 34% was documented in 2025") drops the entity to 0.11. AI systems weight entities by their grammatical prominence — this has measurable effects on whether content is cited.

**Atomic claims:** Write every sentence as a self-contained semantic triple (subject + verb + object with explicit context). Eliminate vague pronouns — every sentence must make sense in isolation. "It increased significantly" is never acceptable; "Factory farm enforcement actions increased 34% between 2023 and 2025 according to USDA's annual report" is a citable claim.

**Proper noun density:** AI-cited text averages 20.6% proper nouns versus 5-8% in standard English. Name the organization, the researcher, the report, the year. Vague attribution ("experts say") never gets cited; specific attribution ("according to Compassion in World Farming's 2025 annual report") does.

**Content density sweet spot:** Pages under 5,000 characters get approximately 66% of their content used by AI retrieval. Pages over 20,000 characters get only 12%. Keep individual topic pages focused and information-dense rather than exhaustive. Gemini allocates roughly 380 words per webpage per query — you're competing for a fixed slice.

**Opening structure:** 71% of AI-cited paragraphs contain four lines or fewer. Open every major section with a 40-60 word declarative statement containing the answer. Information buried deep in paragraphs is rarely retrieved.

---

## Content Strategy

### Search intent — match before writing

Google evaluates intent at the level of the specific problem depth, decision stage, and expected outcome. Before writing any page, study the top 5 results for the target keyword to understand what format Google currently considers the best match. Writing excellent content that targets the wrong intent format does not rank.

| Intent type | Expected format |
|-------------|----------------|
| Informational | How-to guides, tutorials, explainers, definitions |
| Commercial investigation | Comparison articles, review roundups, "best of" lists |
| Transactional | Product pages, pricing pages, application forms |
| Navigational | Homepage, specific brand/product pages |

### Google's Helpful Content System

Since the March 2024 core update, the Helpful Content System is integrated directly into core ranking. It evaluates whether content was created primarily for users or primarily for search engines.

**Content that performs well:**
- Solves a specific problem clearly and completely in one visit
- Demonstrates genuine first-hand knowledge and experience
- Includes accurate, up-to-date information with specific attribution
- Would be useful if search engines didn't exist

**Content that gets demoted:**
- Created primarily to attract traffic, not to help users
- Uses AI to mass-produce content without adding original insight or editorial judgment
- Summarizes what others have said without adding genuine expertise
- Thin content that doesn't satisfy the intent completely

Google's position on AI content (Mueller, November 2025): "Our systems don't care if content is created by AI or humans. What matters is whether it's helpful for users." However, since June 2025, Google issues manual actions for "scaled content abuse" targeting mass-published AI content. Unedited AI drafts bounce 18% higher and retain visitors 31% less than human-refined versions. Use AI as a tool in a human-led editorial process — add genuine expertise, original data, and editorial judgment.

### E-E-A-T signals (Experience, Expertise, Authoritativeness, Trustworthiness)

E-E-A-T is not a direct ranking signal but describes what Google's quality raters evaluate. In 2026, it applies to all content, not just YMYL topics. Content with proper author metadata gets cited 40% more by AI systems.

**How to demonstrate E-E-A-T on every content page:**
- **Experience:** Original data, case studies, screenshots, specific processes you've actually run, unique organizational knowledge
- **Expertise:** Detailed author bio with verifiable credentials, consistent coverage of the topic, citations from specific credible sources with dates
- **Authoritativeness:** Third-party coverage, industry recognition, organizational partnerships, links from relevant authoritative sources
- **Trustworthiness:** Clear contact information, privacy policy, transparent organizational identity, HTTPS, cited and accurate claims

Every content page must have: visible author name and credentials, link to author profile page with Person schema (photo, bio, credentials, links to external authoritative profiles).

### Content freshness — what counts and what doesn't

76% of the most-cited AI content was updated within 30 days. Perplexity gives a 3.4× citation advantage to content updated within 30 days. These freshness signals count:
- Genuinely new information added to existing content
- Updated statistics and dated references
- Visible "Last Updated: [date]" using `<time datetime="YYYY-MM-DD">`
- Accurate `dateModified` in Article schema, synchronized with the visible date

Google detects "date freshness hacking" — changing dates without changing content — and this triggers penalties. Only update dates when content actually changes.

### Original research and proprietary data

The most powerful content strategy for both SEO and AI citation. Pages with original statistics see 30-40% higher AI visibility. Original or proprietary data generates 4.31× more AI citations per URL. Companies publishing proprietary data earn 45% more AI citations than those relying on standard content.

For Open Paws: publish annual impact reports, platform usage benchmarks, program outcome data, and advocacy industry adoption statistics as dedicated, permanently-addressable pages.

### First-mover advantage in data voids

For topics with few authoritative sources, publishing first creates a durable citation position. One documented case: content on an obscure topic was cited by ChatGPT, Gemini, AI Overviews, and Grok within 24 hours. A replication attempt two days later failed — the first mover had captured the position. Identify emerging advocacy topics and publish before competitors.

---

## E-E-A-T: Author Pages and Person Schema

Every author who publishes on the site needs a dedicated profile page with:
- `@type: Person` schema with `name`, `url`, `jobTitle`, `description`, `image`, `sameAs` (LinkedIn, relevant external profiles)
- Photo, bio, credentials, and verifiable external presence
- Links to all content by that author

Author pages are linked from every article. This creates a trust chain: Article schema → author `@id` → Person schema → `sameAs` external profiles → third-party corroboration. Without this chain, content is treated as anonymous and cited 40% less.

---

## Structured Data (JSON-LD)

Implement JSON-LD schema in the `<head>` of every page. Sites with structured data achieve 41% AI citation rates vs 15% without. Only 12.4% of websites implement it.

### Organization + WebSite schema (every page)

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@graph": [
    {
      "@type": "Organization",
      "@id": "https://yoursite.com/#organization",
      "name": "Organization Name",
      "url": "https://yoursite.com",
      "logo": { "@type": "ImageObject", "url": "https://yoursite.com/logo.png" },
      "sameAs": [
        "https://www.linkedin.com/company/your-org",
        "https://twitter.com/your-org",
        "https://github.com/your-org",
        "https://en.wikipedia.org/wiki/Your_Org",
        "https://www.wikidata.org/wiki/QXXXXXXX"
      ],
      "description": "One-sentence description"
    },
    {
      "@type": "WebSite",
      "@id": "https://yoursite.com/#website",
      "url": "https://yoursite.com",
      "name": "Site Name",
      "publisher": { "@id": "https://yoursite.com/#organization" }
    }
  ]
}
</script>
```

### Article schema (every content page)

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "Page title",
  "author": {
    "@type": "Person",
    "name": "Author Name",
    "url": "https://yoursite.com/team/author-name",
    "jobTitle": "Their role"
  },
  "publisher": { "@id": "https://yoursite.com/#organization" },
  "datePublished": "2026-01-15T08:00:00Z",
  "dateModified": "2026-03-20T10:30:00Z",
  "image": "https://yoursite.com/images/article-image.jpg",
  "description": "150-160 char meta description"
}
</script>
```

### FAQPage schema (pages with Q&A content)

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "What is the question?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Direct, complete answer in 40-80 words."
      }
    }
  ]
}
</script>
```

Also implement when applicable: **HowTo**, **BreadcrumbList**, **SoftwareApplication**, **Event**, **Dataset**, **Person**, **VideoObject** (for video content), **LocalBusiness** (for location-based entities). Always JSON-LD (not Microdata). Use `@id` to connect entities. Keep `dateModified` accurate. Validate at https://validator.schema.org/.

---

## Meta Tags

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Primary Keyword — Brand Name</title><!-- 50-60 chars, keywords first -->
  <meta name="description" content="150-160 chars. Direct answer + one statistic.">
  <link rel="canonical" href="https://yoursite.com/this-page">
  <meta property="og:title" content="Page Title">
  <meta property="og:description" content="Description">
  <meta property="og:type" content="article">
  <meta property="og:url" content="https://yoursite.com/this-page">
  <meta property="og:image" content="https://yoursite.com/images/og-image.jpg">
  <meta property="og:site_name" content="Site Name">
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:title" content="Page Title">
  <meta name="twitter:description" content="Description">
  <meta name="twitter:image" content="https://yoursite.com/images/twitter-image.jpg">
  <meta property="article:published_time" content="2026-01-15T08:00:00Z">
  <meta property="article:modified_time" content="2026-03-20T10:30:00Z">
</head>
```

Meta description: 150-160 chars, direct factual answer to the primary query, one specific statistic, never duplicated across pages. Title: `Primary Keyword — Brand Name`, 50-60 chars, keywords first, unique per page. For time-sensitive content, include the year in the title.

---

## Core Web Vitals

Google's Core Web Vitals are confirmed ranking factors measured via real Chrome user data (CrUX) at the 75th percentile. INP officially replaced FID in March 2024.

**Current thresholds (as of March 2026):**

| Metric | Good | Needs Improvement | Poor |
|--------|------|-------------------|------|
| LCP (Largest Contentful Paint) | ≤ 2.5s | 2.5–4.0s | > 4.0s |
| INP (Interaction to Next Paint) | ≤ 200ms (target ≤ 150ms) | 200–500ms | > 500ms |
| CLS (Cumulative Layout Shift) | ≤ 0.1 | 0.1–0.25 | > 0.25 |

**Ranking impact:** Pages at position 1 show a 10% higher CWV pass rate than position 9. Sites with INP above 200ms saw an average ranking drop of 0.8 positions; above 500ms dropped 2-4 positions. Pages with LCP above 3 seconds experienced 23% more traffic loss than faster competitors. 43% of sites still fail the 200ms INP threshold in 2026.

**INP — the hardest metric to fix.** INP requires restructuring how JavaScript executes. The primary technique is `scheduler.yield()` (Chrome-native, with `setTimeout` fallback), which sends remaining work to the front of the task queue while letting the browser handle user input:

```typescript
function yieldToMain(): Promise<void> {
  if ('scheduler' in window && 'yield' in (window as any).scheduler) {
    return (window as any).scheduler.yield();
  }
  return new Promise(resolve => setTimeout(resolve, 0));
}
// In long event handlers: await yieldToMain(); then doExpensiveWork()
```

For Next.js: use React Server Components to reduce client JS, `useTransition` for non-urgent state updates, and `next/dynamic` for heavy components. Audit third-party scripts first — highest impact, lowest effort.

**Rendering strategies in Next.js App Router:**

| Strategy | TTFB | Best For |
|---|---|---|
| SSG (default cached fetch) | Sub-100ms from CDN | Static content — blogs, docs, marketing pages |
| ISR (`next: { revalidate: 60 }`) | Fast, cached + background revalidate | Periodically updated content |
| SSR (`cache: 'no-store'`) | 200ms+ per request | User-specific real-time data — dashboards |
| PPR (experimental, Suspense) | Fast static shell + streamed dynamic | Mixed pages with some dynamic content |

**Implementation:**
- TTFB < 200ms (rendering cannot start until HTML arrives)
- Preload the LCP element: `<link rel="preload" as="image" href="hero.webp">`
- Inline critical CSS, defer non-critical CSS
- Add explicit `width` and `height` attributes to every image, video, iframe, and ad slot (prevents CLS)
- Use `font-display: swap` for web fonts
- Break JavaScript long tasks (>50ms) with `yieldToMain()` during interactions
- Implement `loading="lazy"` for below-fold images
- Use modern image formats (WebP/AVIF) — 25-50% smaller than JPEG/PNG; AVIF 50% smaller than JPEG
- Keep total page weight < 1MB (18% of pages over 1MB are abandoned by AI crawlers)
- Test with real field data (Search Console), not just lab tools — Google ranks on field data

---

## Image Optimization

- Descriptive file names with keywords: `animal-welfare-audit-checklist-2026.webp` not `IMG_4827.jpg`
- Meaningful alt text describing the image content (both SEO and accessibility)
- Modern formats: WebP/AVIF with `<picture>` fallbacks for older browsers
- Responsive images using `srcset` at multiple viewport sizes
- `loading="lazy"` for below-fold images
- Explicit `width` and `height` on every image, video, and embed (prevents CLS)

```html
<picture>
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="Descriptive text" width="800" height="600" loading="lazy">
</picture>
```

---

## Technical SEO

### Rendering and JavaScript

Require SSR or static site generation (SSG). Client-side-only rendering is a strategic error: it increases crawl cost, AI crawlers (GPTBot, ClaudeBot, PerplexityBot) generally do not execute JavaScript, and relying on JS rendering increases the risk of critical content being missed. All critical content must be in the initial HTML response.

### Mobile-first indexing

Google uses mobile scores as the primary ranking signal for all results, including desktop. Over 60% of searches occur on mobile; 53% of mobile visitors abandon sites taking more than 3 seconds to load. Test with the Googlebot Smartphone user agent, not desktop.

### Crawl budget

Google allocates crawl resources based on perceived site value. Faceted navigation is the most common crawl budget killer — a site with 1,000 products can generate 1,000,000 low-value URLs through filter combinations. Manage crawl budget by:
- Blocking low-value parameter URLs, internal search results, and admin pages in robots.txt
- Fixing redirect chains (multiple sequential redirects waste crawl budget)
- Keeping XML sitemaps clean — only canonical, indexable URLs with accurate `<lastmod>` dates
- Auditing and pruning pages with zero organic traffic over 12 months (concentrate link equity on high-value content)
- Returning proper HTTP status codes: 200 for live content, 301 for permanent moves, 404 for missing, 410 for intentionally removed

### Security headers

Implement all of these — they are expected signals in 2026. For Next.js, set via `middleware.ts`:
- `Content-Security-Policy` (CSP with nonces for scripts/styles) — prevents XSS
- `Strict-Transport-Security: max-age=63072000; includeSubDomains; preload` (HSTS)
- `X-Content-Type-Options: nosniff`
- `X-Frame-Options: DENY`
- `Referrer-Policy: strict-origin-when-cross-origin`
- `Permissions-Policy: camera=(), microphone=(), geolocation=()` — disable powerful features by default
- `Cross-Origin-Opener-Policy: same-origin` — enables cross-origin isolation when paired with COEP
- Fix all mixed content (HTTP resources on HTTPS pages)
- Remove `X-Powered-By` header (information disclosure)

### Supply chain security

Pin exact dependency versions, use `npm ci` (never `npm install` in CI), and scan with Socket.dev or Snyk. The 2025 npm supply chain attacks (chalk/debug compromise affected packages with 2.6 billion combined weekly downloads) showed that well-maintained packages are vulnerable. Verify every dependency. Enable 2FA on npm accounts.

---

## Robots.txt

```txt
User-agent: Googlebot
Allow: /

User-agent: Bingbot
Allow: /

# AI citation crawlers — allow for AI answer visibility
User-agent: OAI-SearchBot
Allow: /

User-agent: ChatGPT-User
Allow: /

User-agent: PerplexityBot
Allow: /

User-agent: ClaudeBot
Allow: /

User-agent: Claude-SearchBot
Allow: /

User-agent: Applebot
Allow: /

User-agent: Amazonbot
Allow: /

# AI training crawlers — block if not consenting to training use
User-agent: GPTBot
Disallow: /

User-agent: CCBot
Disallow: /

User-agent: Google-Extended
Disallow: /

# Block scraper bots
User-agent: AhrefsBot
Disallow: /

User-agent: SemrushBot
Disallow: /

Sitemap: https://yoursite.com/sitemap.xml
```

Critical: blocking `Googlebot` blocks both Google Search AND AI Overviews — there is no way to allow one without the other. There are now 226+ identified AI crawlers (last verified 2026-03-01; source: DarkVisitors); review quarterly. Some AI agents use standard browser user-agent strings and ignore robots.txt — treat this as best-effort.

---

## XML Sitemap

Include only canonical, indexable URLs. `<lastmod>` must reflect actual update dates — never fake it. Reference in robots.txt. Submit to Google Search Console and Bing Webmaster Tools. Regenerate automatically when content changes. Maximum 50,000 URLs per file, 50MB uncompressed.

---

## IndexNow

IndexNow notifies Bing (which feeds ChatGPT) instantly when content is published or updated. Place a key file at `https://yoursite.com/{key}.txt` and ping on every publish:

```txt
GET https://api.indexnow.org/indexnow?url=https%3A%2F%2Fyoursite.com%2Fnew-page&key=YOUR_KEY
```

Integrate into CI/CD pipeline or CMS publish hooks. Supported by Bing, Yandex, Seznam, Naver. Rate limit: 10,000 URLs/day.

---

## Site Architecture

URL rules: descriptive hyphenated lowercase under 75 characters, primary keyword in URL, max 3 levels deep. Canonical tags on every page. 301 redirects for URL changes — never leave broken links. Breadcrumb navigation with BreadcrumbList schema.

Hub-and-spoke topic cluster model — increases AI citation rates from 12% to 41%, bidirectional internal linking increases citation probability by 2.7×:
- **Pillar page**: 2,000-4,000 words on a broad topic targeting head keywords
- **Cluster pages**: 8-15 detailed articles each targeting a specific subtopic
- **Bidirectional links**: every cluster page links to the pillar; the pillar links to every cluster page

Internal linking rules: no important page more than 3 clicks from homepage. Use descriptive anchor text — never "read more". Distribute link equity strategically toward high-value pages. Audit for orphan pages (no internal links pointing to them).

---

## Wikipedia and Wikidata

This is the single highest-leverage off-site action. Wikipedia accounts for 47.9% of ChatGPT's top-10 cited sources. Having a Wikipedia page creates a stable Wikidata Q-ID that AI systems use as a truth anchor — when multiple sources conflict, the version corroborated by Wikipedia prevails.

Wikidata serves 11 million queries daily across 119 million entities. Google's Knowledge Graph, Amazon Alexa, Apple Siri, and Microsoft all integrate Wikidata. Companies have gained Knowledge Panels within 7 days of creating a Wikidata entry.

For any organization this site represents:
- Verify Wikipedia article exists, is accurate, cited, and current
- Verify Wikidata entry is complete: organization type, founding date, location, founders, official website, social media profiles
- Add Wikidata Q-ID to Organization schema `sameAs` array (`https://www.wikidata.org/wiki/Q...`)
- Add Wikipedia URL to Organization schema `sameAs` array
- Build an entity web: organization → key tools → key people → related organizations → policy areas
- Ensure site structured data is consistent with Wikipedia and Wikidata — inconsistency reduces AI confidence

**Wikipedia COI (mandatory):** Never directly edit your own organization's Wikipedia article. Disclose affiliation on the Talk page. Propose edits through Talk-page requests or neutral editors. Use only independent, reliable sources. Follow Wikipedia's Conflict of Interest and Notability guidelines.

---

## Authoritative Platform Publishing and Brand Signals

85% of AI brand mentions come from third-party pages. Brand search volume is the strongest single predictor of AI citations (r=0.334). Brand mentions now account for 55% of off-page ranking weight (up from ~20% in 2012); backlinks account for 45%. Brands appearing on 4+ platforms are 2.8× more likely to appear in AI responses.

**Platform trust hierarchy:**
- AI Overviews are 3× more likely to cite .gov sources
- YouTube accounts for ~23.3% of AI citations across platforms
- Wikipedia accounts for ~18.4%
- Reddit accounts for up to 46.5% of Perplexity's top citations

**Practical actions:**
- Publish thorough, helpful content in relevant subreddits — Reddit citations in AI Overviews surged 450% in three months. Authentic participation only; AI systems have visibility into Reddit's moderation pipeline.
- Create and maintain YouTube content with transcripts enabled (transcripts become crawlable text)
- Maintain active LinkedIn organization page with substantive posts
- Publish documentation, datasets, and reports on GitHub as Markdown
- Submit data to Our World in Data and relevant academic/government databases
- Monitor unlinked brand mentions (60% of online mentions are unlinked) and convert high-authority ones to backlinks — close rates typically above 30%

---

## Link Building

Backlinks remain the #2 ranking factor but their nature has changed. Topical relevance between linking and linked domains (r=0.4) now outweighs raw domain authority. Natural anchor text distribution: ~40-50% branded, ~20-30% generic, ~15-25% partial match, ~5-10% exact match. Sites with anchor text diversity below 30% saw average ranking drops of 15 positions in the March 2026 spam update.

**What works:**
- **Digital PR:** Original research, industry surveys, data-driven reports that journalists cover editorially. Content built on original data sees 156% more link acquisition than generic how-to content.
- **Unlinked brand mention conversion:** Monitor with Google Alerts or Ahrefs Alerts. Outreach to convert to links.
- **Broken link building:** Find broken links on high-authority pages, create superior replacement content, outreach.

**What was devalued in the March 2026 spam update:** sponsored guest posts on generalist high-DA news sites, niche edit placements on aged thin domains, PBN links. Google's SpamBrain detects these reliably now.

---

## Conversion Optimization

For nonprofit donation pages: present 3-4 preset amounts with the middle option pre-selected and pair amounts with impact descriptions ("$40/month provides clean water for 12 families"). Pre-select monthly giving — monthly donors become more valuable than one-time donors within 5.25 months, yet 64% of nonprofits still default to one-time. Single-step forms vastly outperform multi-step (52% drop in completions with multi-step). Eliminating site header navigation during the donation flow produced a documented 195% conversion increase. Embed the form directly on-site; never redirect to a third-party processor.

For all forms: target 3-5 essential fields maximum — each additional field costs ~11% in conversion. Start with easy, non-threatening questions. Make optional fields clearly optional (+25-35% completion). Support browser autofill with correct `autocomplete` attributes (HTML standard; React JSX uses `autoComplete`) (also required for WCAG 2.2 AA SC 1.3.5).

Trust signals with documented impact: charity rating badges increase giving likelihood for 72% of donors; video testimonials are 80-86% more effective than text; specific quantified impact numbers with real names beat generic claims.

**Dark patterns carry legal risk.** The FTC's $2.5 billion settlement with Amazon (September 2025) for a deliberately complex cancellation flow marks the largest dark pattern enforcement action in history. California, Colorado, and 10+ other states explicitly prohibit dark patterns in privacy laws. For advocacy organizations, any perception of manipulation is existential — donor trust is the primary asset.

---

## Analytics

Use **Plausible** ($9/month cloud) or **Umami** (self-hosted, free) as primary analytics — no cookies, no consent banner required, ~1KB script versus GA4's 23KB+. Add GA4 only if you need Google Ads integration or predictive analytics. Proxy analytics scripts through your own domain (Next.js rewrites) to bypass adblockers.

**Tracking AI referral traffic:** AI-generated traffic grew 357% year-over-year to 1.1 billion visits in June 2025. ChatGPT drives 78% of AI traffic but its mobile app drops referrer data entirely (appears as Direct in analytics). Create a custom channel group in GA4 with source matching:

```txt
(chatgpt\.com|chat\.openai\.com|perplexity\.ai|claude\.ai|gemini\.google\.com|copilot\.microsoft\.com)
```

Claude drives <0.17% of volume but has the highest session value at $4.56/visit. Track `donation_completed` (with value), `newsletter_signup`, and `volunteer_form_submit` as GA4 conversions.

Use **GrowthBook** (self-hosted, MIT, free) for A/B testing — it queries your existing data warehouse directly and avoids sending user data to third-party servers.

---

## Internationalization

For multilingual advocacy sites, use **next-intl** (1.8M weekly downloads) with subdirectory URL strategy (`/en/`, `/hi/`, `/ar/`). Subdirectories centralize domain authority onto one deployment. Set `lang` and `dir` on `<html>`:

```tsx
<html lang={locale} dir={locale === 'ar' ? 'rtl' : 'ltr'}>
```

The `lang` attribute determines text-to-speech pronunciation in screen readers. Hreflang tags must be self-referencing and reciprocal on every page — 31% of international sites have broken hreflang. Use `alternates.languages` in Next.js `generateMetadata` to generate hreflang automatically.

Arabic requires all 6 CLDR plural categories — use ICU MessageFormat, not string concatenation. Native CSS logical properties (`padding-inline-start`, `padding-inline-end`, `text-align: start`) handle RTL layout mirroring automatically without separate stylesheets; Tailwind equivalents are `ps-4`, `pe-4`, `text-start`.

For Devanagari and Arabic scripts, use Google's **Noto** family with `unicode-range` in `@font-face` to lazy-load only the glyphs needed for each script. Always `font-display: swap` and WOFF2 format.

---

## llms.txt

Place at `/llms.txt` — a Markdown file describing the site for AI systems.

**Current value is effectively zero.** Google's John Mueller: "No AI system currently uses llms.txt." An 8-month study found zero AI crawler visits. Across Acquia's hosting fleet, llms.txt received 0.001% of traffic — all from SEO audit tools, not AI crawlers. Implement it (low effort) but do not invest significant time. The IETF AIPREF Working Group (co-authored by Google and Mozilla) is the more likely path to a real standard.

---

## Platform-Specific Behavior and Citation Volatility

Citation patterns vary dramatically by platform and change constantly:
- **ChatGPT**: Wikipedia-heavy (47.9% of top-10 citations), converts at 4.4× the rate of search visitors
- **Perplexity**: Reddit-heavy (up to 46.5% of top citations), most vulnerable to external content attacks
- **Google AI Overviews**: 3× more likely to cite .gov, strongly favors known entities with digital credibility

Only 11% of domains are cited by both ChatGPT and Perplexity for the same queries — platform-specific optimization matters. Citation volatility is extreme: 40-60% monthly turnover is normal. A single platform update caused a -52% traffic collapse for some sites. Build multi-platform presence rather than depending on any single system.

Topical authority (r=0.40) is the strongest on-site predictor of AI citations — Domain Authority (r=0.18) explains less than 20% of citation variance.

---

## Defensive Awareness

These techniques are used by competitors but carry severe penalties. Understand them to recognize manipulation and to ensure this site never crosses into prohibited territory.

**Hidden text injection** — invisible instructions embedded in web content (white-on-white text, zero-size fonts, invisible Unicode characters U+E0000 to U+E007F) — explicitly prohibited by Google's spam policies and actively detected. Palo Alto Networks documented 24 layered injection attempts on a single website in March 2026. Google's SpamBrain and tools like PhantomLint compare parsed text against OCR-rendered images. Penalties are domain-wide.

**Agent-aware cloaking** — serving different content to AI crawlers than to human visitors — is explicitly prohibited by all major platforms. Documented cases show it dramatically inflates AI scores while the human-visible version scores poorly. Google treats this as spam; penalties are domain-wide and can include complete deindexing.

**Scaled AI content without human review** caused sites to lose up to 80% of organic traffic overnight in the March 2024 Google core update. Google issues manual actions for "scaled content abuse" since June 2025.

**FTC legal exposure:** "Operation AI Comply" (September 2024) stated "using AI tools to trick, mislead, or defraud people is illegal" with "no AI exemption." For advocacy organizations, the reputational consequences of exposure amplify any platform penalty enormously.

---

## Key Statistics

| Signal | Impact |
|--------|--------|
| Adding statistics to claims | +41% AI visibility |
| Citing credible sources inline | +30-40% AI visibility |
| Expert quotations | +28-40% AI visibility |
| Lower-ranked sites citing sources | +115% AI visibility |
| Keyword stuffing | -10% AI visibility |
| FAQ schema | 41% citation rate vs 15% without |
| Question-based headings | 7× citation impact for smaller sites |
| 120-180 word modular sections | 70% more ChatGPT citations |
| Content over 2,900 words | 59% more likely to be cited |
| Original or proprietary data | 4.31× more citations per URL |
| Author metadata | +40% citations |
| Topic cluster architecture | Citation rate 12% → 41% |
| Fresh content (within 30 days) | 76% of most-cited; 3.4× Perplexity advantage |
| Structured data (schema) | 73% higher AI selection rate |
| Wikipedia/Wikidata presence | Knowledge Panel within 7 days |
| Monthly citation turnover | 40-60% — continuous freshness required |
| Domain Authority vs AI citations | r=0.18 — weak predictor |
| Topical authority vs AI citations | r=0.40 — strongest on-site predictor |
| Brand mentions vs AI citations | r=0.664 — strongest overall signal |
| AI Overview citations from top-10 | Only 17-32% — lower-authority pages can win |
| Sites passing all CWV thresholds | 24% lower bounce rates; 15-30% conversion improvement |
| INP above 200ms | -0.8 average position drop |
| LCP above 3s | 23% more traffic loss vs faster competitors |
| Backlinks vs brand signals | 45% / 55% of off-page ranking weight |
| Original data content | 156% more link acquisition; 45% more AI citations |
| Unlinked brand mention conversion | >30% close rate |
| AI referral traffic growth | 357% YoY to 1.1B visits (June 2025) |
| ChatGPT share of AI traffic | 78% of AI referral visits |
| Pre-selecting monthly giving | Accounts for 31% of online nonprofit revenue |
| Donation form fields ≤4 | 160% more conversions vs longer forms |
| Overlay widgets and lawsuits | 22.6% of sued sites had overlays installed |
