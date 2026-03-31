---
name: geo-seo-audit
description: SEO + GEO audit and implementation workflow — Core Web Vitals, HTML structure, semantic writing, E-E-A-T, content intent, Wikipedia/Wikidata, JSON-LD schema, meta tags, crawl budget, robots.txt, sitemap, IndexNow, topic cluster architecture, link building, brand signals, conversion optimization, analytics, internationalization, platform presence, defensive review
---
# SEO + GEO Audit

This skill covers both traditional SEO and Generative Engine Optimization (GEO) — ensuring advocacy content ranks in search engines AND appears in AI answer systems (ChatGPT, Perplexity, Google AI Overviews, Claude, Gemini, Bing Copilot).

**The core insight:** Only 17-32% of AI Overview citations come from pages ranking in the organic top 10. Domain Authority correlates with AI citations at r=0.18; topical authority (r=0.40) and branded web mentions (r=0.664) are the real predictors. 80% of URLs cited by AI assistants do not rank in Google's top search results. This means SEO and GEO have different — but overlapping — optimization paths.

## When to Use

- When building or reviewing any public-facing advocacy website
- Before launching content that needs to be discoverable by search engines or AI systems
- When diagnosing why a site is not appearing in Google results or AI responses
- When implementing a content hub or topic cluster for an advocacy campaign
- As a pre-launch checklist for any new Open Paws platform page

## Process

### Step 1: Audit Core Web Vitals

Measure using real field data from Google Search Console (CrUX), not lab tools — Google ranks on field data. Current thresholds (as of March 2026):

| Metric | Good | Needs Improvement | Poor |
|--------|------|-------------------|------|
| LCP | ≤ 2.5s | 2.5–4.0s | > 4.0s |
| INP | ≤ 200ms | 200–500ms | > 500ms |
| CLS | ≤ 0.1 | 0.1–0.25 | > 0.25 |

Check: Is LCP element preloaded with `<link rel="preload">`? Are explicit `width` and `height` on every image/video/iframe (prevents CLS)? Is TTFB under 200ms? Are JavaScript long tasks (>50ms) broken up with `scheduler.yield()` or `setTimeout`? Is `font-display: swap` used for web fonts? Are below-fold images lazy-loaded? Are modern image formats (WebP/AVIF) used?

**INP fix priority order:** (1) audit third-party scripts, (2) lazy-load non-critical features, (3) optimize event handlers with `scheduler.yield()`, (4) reduce JavaScript payload with React Server Components, (5) virtual lists and CSS containment.

**Next.js rendering check:**

| Strategy | TTFB | Best For |
|---|---|---|
| SSG (default cached fetch) | Sub-100ms | Static content — blogs, docs |
| ISR (`next: { revalidate: 60 }`) | Fast, cached | Periodically updated content |
| SSR (`cache: 'no-store'`) | 200ms+ | User-specific real-time data |
| PPR (experimental) | Fast shell + streamed | Mixed static/dynamic pages |

Sites with INP above 200ms average -0.8 position drops. Pages with LCP above 3 seconds experience 23% more traffic loss. 43% of sites still fail the 200ms INP threshold — the most common CWV failure.

### Step 2: Audit Technical SEO

**Rendering:**
- Verify SSR or SSG — client-side-only rendering increases crawl cost and is invisible to AI crawlers
- Test with Google's URL Inspection tool to verify rendered content matches expectations
- AI crawlers (GPTBot, ClaudeBot, PerplexityBot) generally do not execute JavaScript

**Mobile-first:**
- Audit using Googlebot Smartphone user agent, not desktop
- Google uses mobile scores as the primary ranking signal for all results
- 53% of mobile visitors abandon sites taking more than 3 seconds to load

**Crawl budget:**
- Check for faceted navigation URL explosion (filter/sort parameter combinations)
- Verify robots.txt blocks low-value parameter URLs, internal search, admin pages
- Check for redirect chains (sequential redirects waste crawl budget)
- Verify XML sitemap contains only canonical, indexable URLs with accurate `<lastmod>` dates
- Audit pages with zero organic traffic over 12 months — candidates for consolidation, noindex, or removal
- Verify HTTP status codes: 200 for live content, 301 for permanent redirects, 404 for missing, 410 for intentionally removed

**Security headers (set via Next.js middleware):**
- CSP with nonces for scripts/styles
- `Strict-Transport-Security: max-age=63072000; includeSubDomains; preload`
- `X-Content-Type-Options: nosniff`
- `X-Frame-Options: DENY`
- `Referrer-Policy: strict-origin-when-cross-origin`
- `Permissions-Policy: camera=(), microphone=(), geolocation=()`
- `Cross-Origin-Opener-Policy: same-origin`
- Remove `X-Powered-By` header

**Supply chain:**
- Pin exact dependency versions, use `npm ci` in CI pipelines
- Scan with Socket.dev or Snyk
- Enable 2FA on npm accounts

### Step 3: Audit HTML Structure

Check each page for:
1. Exactly one `<h1>` tag containing the primary topic
2. Logical heading hierarchy (`h1 > h2 > h3`) with no skipped levels
3. `<h2>` headings phrased as questions where the section answers something (7× citation impact for smaller sites; also improves Featured Snippet and PAA selection)
4. First paragraph after each heading: direct 40-60 word answer
5. Paragraphs of 2-4 sentences (40-60 words); content sections of 120-180 words
6. Semantic elements: `<article>`, `<section>`, `<nav>`, `<aside>`, `<header>`, `<footer>`, `<main>`
7. `lang` attribute on `<html>` tag
8. Descriptive `alt` text on every `<img>`; descriptive file names with keywords
9. Meaningful anchor text on every `<a>` — never "click here"
10. `<table>` for comparison data; `<ol>` and `<ul>` for lists; `<blockquote cite="...">` for quotations
11. `<time datetime="YYYY-MM-DD">` for dates; `<dfn>` for definitions; `<abbr title="...">` for acronyms
12. `id` attributes on all `<h2>` and `<h3>` elements

Flag any content rendered exclusively by JavaScript.

### Step 4: Audit Content Intent and Search Positioning

Before auditing content quality, verify content targets the right intent format. Study the top 5 results for each target keyword. Content targeting the wrong intent format does not rank regardless of other optimizations.

| Intent type | Expected format to match |
|-------------|--------------------------|
| Informational | How-to guides, tutorials, explainers, definitions |
| Commercial investigation | Comparison articles, review roundups, "best of" lists |
| Transactional | Product/service pages, pricing pages, application forms |
| Navigational | Homepage, specific brand/product pages |

Also check: Does the content completely satisfy user intent in one visit (Helpful Content System standard)? Does it demonstrate genuine first-hand knowledge and experience? Does it include specific, dated citations rather than vague attribution?

### Step 5: Audit Semantic Writing Quality

AI citation systems retrieve at the sentence and paragraph level. Check:

**Entity salience:** Is the primary entity the grammatical subject in active voice? Active voice gives subject salience 0.74; passive voice drops it to 0.11. Check for passive constructions and fix them.

**Atomic claims:** Are sentences self-contained semantic triples? Eliminate vague pronouns — every sentence must make sense in isolation.

**Proper noun density:** AI-cited text averages 20.6% proper nouns versus 5-8% standard. Check whether content names organizations, researchers, reports, and years specifically rather than using vague attribution.

**Content density:** Pages under 5,000 characters get approximately 66% of content used; pages over 20,000 characters get only 12%. Check whether pages are focused and dense.

**Opening structure:** Every major section should open with a 40-60 word declarative answer. Information buried deep in paragraphs is rarely retrieved by AI systems.

### Step 6: Audit E-E-A-T Signals

Check for Experience, Expertise, Authoritativeness, Trustworthiness on every content page:
- Original data, case studies, screenshots, specific processes (Experience)
- Detailed author bio with verifiable credentials (Expertise)
- Specific citations from credible sources with names, organizations, and dates (Expertise)
- Third-party coverage, organizational partnerships, industry recognition (Authoritativeness)
- Clear contact information, privacy policy, transparent organizational identity (Trustworthiness)

**Author attribution audit:** Every content page must have a visible author name, credentials, and link to an author profile page. The author page must have Person schema with `name`, `url`, `jobTitle`, `description`, `image`, and `sameAs` (LinkedIn, external profiles). Content with proper author metadata gets 40% more AI citations.

### Step 7: Audit Wikipedia and Wikidata Presence

Wikipedia accounts for 47.9% of ChatGPT's top-10 cited sources. Organizations visible in Wikidata get Knowledge Panels within 7 days and begin appearing consistently in AI answers.

Check:
1. Does the organization have a Wikipedia article? Is it accurate, cited, current?
2. Does the organization have a Wikidata entry (Q-ID)? Is it complete with: type, founding date, location, founders, official website, social profiles?
3. Is the Wikidata Q-ID in the site's Organization schema `sameAs` array?
4. Is Wikipedia URL in the site's Organization schema `sameAs` array?
5. Does an entity web exist connecting organization → tools → people → related orgs → policy areas?
6. Does the site's structured data match Wikipedia and Wikidata? Inconsistency reduces AI confidence.

If no Wikipedia article exists: document as **High** finding. Check whether sufficient independent secondary coverage exists to support notability per Wikipedia's guidelines. If yes, creating the article should be prioritized over almost any on-site optimization.

**Wikipedia COI policy (mandatory):** Never directly edit your own organization's Wikipedia article. Disclose any affiliation on the Talk page. Propose edits through Talk-page requests or experienced neutral editors. Use only independent, reliable sources — no primary sources or press releases. All contributions must follow Wikipedia's Conflict of Interest and Notability guidelines. Document the disclosure and source list for audit purposes.

### Step 8: Audit Structured Data (JSON-LD)

Sites with structured data achieve 41% AI citation rates vs 15% without. Only 12.4% of websites implement it.

For every page verify:
- **Organization + WebSite schema** on every page: `name`, `url`, `logo`, `sameAs` (LinkedIn, Twitter, GitHub, Wikipedia URL, Wikidata URL), `description`
- **Article schema** on every content page: `headline`, `author` (with `name`, `url`, `jobTitle`), `publisher`, `datePublished`, `dateModified`, `image`, `description`
- **FAQPage schema** on any page with Q&A content
- **BreadcrumbList schema** on all non-homepage pages
- **Person schema** on all author/team profile pages
- Additional applicable types: HowTo, SoftwareApplication, Event, Dataset, VideoObject, LocalBusiness

Validate all schema at https://validator.schema.org/ and Google's Rich Results Test. `dateModified` must reflect actual update dates.

### Step 9: Audit Meta Tags and Head Elements

For every page:
- `<title>`: `Primary Keyword — Brand Name`, 50-60 chars, keywords first, unique per page
- `<meta name="description">`: 150-160 chars, direct answer to primary query, one statistic, unique
- `<link rel="canonical">` pointing to canonical URL
- Open Graph tags: `og:title`, `og:description`, `og:type`, `og:url`, `og:image`, `og:site_name`
- Twitter Card: `twitter:card`, `twitter:title`, `twitter:description`, `twitter:image`
- Article timestamps: `article:published_time`, `article:modified_time` (ISO 8601)

### Step 10: Audit Robots.txt

Verify AI citation crawlers are allowed:
- **Must allow** (power AI answers): `OAI-SearchBot`, `ChatGPT-User`, `PerplexityBot`, `ClaudeBot`, `Claude-SearchBot`, `Applebot`, `Amazonbot`
- **May block** (training only): `GPTBot`, `CCBot`, `Google-Extended`, `meta-externalagent`, `Bytespider`
- **Never block**: `Googlebot` (blocks both Search and AI Overviews simultaneously)

Note: 226+ AI crawlers exist. Verify list is current. Some AI agents use standard browser user-agent strings and ignore robots.txt.

### Step 11: Audit XML Sitemap and IndexNow

**Sitemap:**
- Exists at `/sitemap.xml`
- Contains only canonical, indexable URLs
- `<lastmod>` dates reflect actual content update dates (never faked)
- Referenced in robots.txt
- Submitted to Google Search Console and Bing Webmaster Tools
- Auto-regenerates when content changes

**IndexNow:**
- API key file at `https://yoursite.com/{key}.txt`
- Ping fires on every publish/update event
- Integrated in CI/CD pipeline or CMS publish hooks

### Step 12: Audit Site Architecture and Content Freshness

**Architecture:**
- URL structure: descriptive hyphenated lowercase, under 75 characters, max 3 levels deep, canonical tags, 301 redirects for changed URLs
- No important page more than 3 clicks from homepage
- Breadcrumb navigation with BreadcrumbList schema
- Topic cluster structure: pillar pages + 8-15 cluster pages with bidirectional links?
- Orphan pages (no internal links pointing to them)?

**Content freshness:**
- "Last Updated: [date]" visible on every content page using `<time datetime="YYYY-MM-DD">`
- `dateModified` in Article schema synchronized with visible dates
- Content genuinely updated (not just date-changed) — Google detects date freshness hacking
- 76% of most-cited AI content updated within 30 days; Perplexity gives 3.4× advantage to content under 30 days old

**First-mover check:** For emerging advocacy topics with few authoritative sources, is the site publishing before competitors claim citation positions?

### Step 13: Audit Platform Presence and Brand Signals

85% of AI brand mentions come from third-party pages. Brand mentions now account for 55% of off-page ranking weight.

Check:
1. **Reddit:** Active, authentic presence in relevant subreddits? AI systems have visibility into Reddit's moderation pipeline — inauthentic content carries negative weight.
2. **YouTube:** Channel with transcripts enabled?
3. **LinkedIn:** Active organization page with substantive posts?
4. **GitHub:** Documentation, datasets, or reports published as Markdown?
5. **Unlinked brand mentions:** Monitor with Google Alerts or Ahrefs. Convert high-authority mentions to backlinks — close rate typically above 30%.
6. **Cross-platform consistency:** Key facts (founding date, mission, statistics) consistent across site, Wikipedia, Wikidata, LinkedIn, GitHub?

Platform-specific behavior: Only 11% of domains are cited by both ChatGPT and Perplexity for the same queries. Citation volatility is 40-60% monthly. Build multi-platform presence.

### Step 14: Audit Link Profile

Backlinks remain the #2 ranking factor. Topical relevance between linking and linked domains (r=0.4) now outweighs raw domain authority.

Check:
- Natural anchor text distribution: ~40-50% branded, ~20-30% generic, ~15-25% partial match, ~5-10% exact match. Sites below 30% diversity saw -15 average position drops in March 2026 spam update.
- Links in editorial context (body content), not footers/sidebars/author bios
- Top-performing link building tactics: original research/data (156% more links vs generic how-to), digital PR campaigns, unlinked mention conversion
- Any links from PBNs, sponsored guest posts on generalist sites, or niche edits on thin domains — these were specifically devalued in March 2026

### Step 15: Audit Content Patterns

Check whether content templates implement high-citation patterns:

**Citable paragraph:** every major claim follows `[fact]. [statistic with attribution]. [elaboration]. [Source: Org, Date]`

**FAQ block:** `<h2>` with exact question, 40-60 word direct answer immediately following, FAQPage schema. Also improves Featured Snippet and People Also Ask selection.

**Definition block:** `<section id="what-is-term"><h2>What is [Term]?</h2><p><dfn>[Term]</dfn> is [direct definition].</p></section>`

**Original data:** Pages with proprietary data get 4.31× more citations per URL. Check whether any unique organizational data can become dedicated, permanently-addressable pages.

**VideoObject schema:** If any video content exists, verify VideoObject schema with `name`, `description`, `thumbnailUrl`, `uploadDate`, and `contentUrl`.

### Step 16: Audit Conversion Optimization and Analytics

**Donation page audit:**
- 3-4 preset amounts with middle pre-selected and impact descriptions?
- Monthly giving pre-selected? (31% of nonprofit online revenue; 64% of orgs still default one-time)
- Single-step form? (multi-step forms see 52% drop in completions)
- Site header navigation removed during donation flow?
- Form embedded on-site, not redirecting to third-party processor?
- `autocomplete` attributes on all form fields? (React JSX: `autoComplete`)

**Analytics:**
- Using cookieless analytics (Plausible or Umami) as primary?
- AI referral traffic tracked with custom channel group? (ChatGPT 78% of AI traffic; mobile app drops referrer)
- Key conversion events marked in GA4: `donation_completed` (with value), `newsletter_signup`, `volunteer_form_submit`?

**Internationalization check (if multilingual):**
- `lang` and `dir` attributes set correctly on `<html>`?
- Hreflang tags self-referencing and reciprocal on every page?
- Subdirectory URL strategy (`/en/`, `/hi/`, `/ar/`) for domain authority consolidation?
- ICU MessageFormat used for plural/gender forms (Arabic requires 6 CLDR plural categories)?

### Step 17: Defensive Review

Check for techniques that would violate platform guidelines:

**Hidden text:** Verify no content is hidden via `display:none`, `visibility:hidden`, white-on-white text, zero-size fonts, negative positioning, or invisible Unicode (U+E0000 to U+E007F). Google's SpamBrain and PhantomLint compare parsed text against OCR-rendered images.

**Agent-aware cloaking:** Verify the server does not serve different content to AI crawlers vs human visitors based on User-Agent detection. Explicitly prohibited; domain-wide penalty risk.

**Scaled AI content:** Verify all published content has meaningful human review. Manual actions issued by Google since June 2025 for "scaled content abuse."

**User-generated content injection:** If the site hosts comments or forum posts, verify they are sanitized before being served to crawlers.

### Findings Report

Document findings by priority:

**Critical (blocks launch)**
- Primary content rendered JavaScript-only
- Missing `<h1>` or broken heading hierarchy
- Missing canonical tags
- robots.txt blocking AI citation crawlers
- Missing HTTPS or expired SSL
- LCP > 4s or page weight > 3MB
- Agent-aware cloaking detected

**High (implement before significant content investment)**
- LCP > 2.5s (failing Good threshold)
- INP > 200ms (43% of sites fail this)
- No Wikipedia article when sufficient notability sources exist
- No Wikidata entry
- Missing structured data (JSON-LD schema)
- Content siloed with no topic cluster architecture
- Pages not SSR or SSG
- Missing `sameAs` links to Wikipedia/Wikidata in Organization schema
- Content not matching search intent format for target keywords

**Medium (implement in next sprint)**
- Question-format headings not used
- Answer-first paragraph pattern not followed
- Missing entity salience (passive voice, vague pronouns, weak proper noun density)
- Content density outside optimal range
- Missing E-E-A-T signals (author attribution, sourcing, experience signals)
- Missing author profile pages with Person schema
- Missing IndexNow integration
- Outdated or inconsistent `dateModified` values
- No Reddit, YouTube, or LinkedIn presence
- Key facts inconsistent across platforms
- Anchor text distribution outside recommended ranges
- Donation page using multi-step form or missing monthly pre-selection
- No AI referral traffic tracking in analytics
- Missing cookieless analytics (relying solely on GA4 with consent banner)
- Missing hreflang tags on multilingual pages

**Low (optimize over time)**
- llms.txt not implemented (low current value but zero-cost)
- Individual content not following citable paragraph pattern
- Missing comparison tables on decision-relevant pages
- No proprietary data or original research pages
- VideoObject schema not implemented for video content
- Security headers incomplete (CSP, Permissions-Policy, COOP)
- Supply chain scanning not in CI pipeline

For each finding: the specific page or component affected, what is missing or incorrect, and the exact implementation needed.
