# EMD HOA Management SEO Network
## Infrastructure & Technical SEO Playbook

Author: System Architecture Document
Purpose: Infrastructure required to deploy a scalable network of EMD (Exact Match Domain) lead generation sites targeting HOA management queries.

Sources:
Google Search Documentation
https://developers.google.com/search/docs

Bing Webmaster Documentation
https://learn.microsoft.com/en-us/bingwebmaster/

---

# 1. robots.txt

Endpoint:

/robots.txt

Example:

User-agent: *
Allow: /

Sitemap: https://domain.com/sitemap.xml

Flask example:

@app.route("/robots.txt")
def robots():
    site = get_site()
    robots = f\"\"\"
User-agent: *
Allow: /

Sitemap: https://{site.domain}/sitemap.xml
\"\"\"
    return Response(robots, mimetype="text/plain")

Documentation:
https://developers.google.com/search/docs/crawling-indexing/robots/create-robots-txt

---

# 2. XML Sitemap

Endpoint:

/sitemap.xml

Example pages:

/
/services/hoa-management-phoenix
/services/hoa-management-company-phoenix
/services/hoa-property-management-phoenix
/quote

Example generator logic:

@app.route("/sitemap.xml")
def sitemap():

    site = get_site()

    pages = [
        "",
        "quote",
        f"services/hoa-management-{site.city.lower()}",
        f"services/hoa-management-company-{site.city.lower()}",
        f"services/hoa-property-management-{site.city.lower()}"
    ]

    xml = ['<?xml version="1.0" encoding="UTF-8"?>']
    xml.append('<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">')

    for page in pages:
        xml.append("<url>")
        xml.append(f"<loc>https://{site.domain}/{page}</loc>")
        xml.append("</url>")

    xml.append("</urlset>")

Sitemap protocol:
https://www.sitemaps.org/protocol.html

---

# 3. Google Search Console Setup

Verification method recommended:

DNS TXT Record

Example Cloudflare record:

Type: TXT
Name: @
Value: google-site-verification=XXXXX

After verification submit sitemap:

https://domain.com/sitemap.xml

Documentation:
https://support.google.com/webmasters/answer/9008080

---

# 4. Bing Webmaster Tools

Verification options:

DNS TXT
XML verification file
Meta tag

Recommended:

DNS TXT

Submit sitemap:

https://domain.com/sitemap.xml

Documentation:
https://learn.microsoft.com/en-us/bingwebmaster/getting-started/verification

---

# 5. Indexing Acceleration

## Google Indexing API

Endpoint:

https://indexing.googleapis.com/v3/urlNotifications:publish

Payload example:

{
"url": "https://domain.com/",
"type": "URL_UPDATED"
}

Documentation:
https://developers.google.com/search/apis/indexing-api

---

## Bing URL Submission API

Endpoint:

https://ssl.bing.com/webmaster/api.svc/json/SubmitUrlbatch

Example payload:

{
"siteUrl":"https://domain.com",
"urlList":[
"https://domain.com/",
"https://domain.com/services/hoa-management-phoenix"
]
}

Documentation:
https://learn.microsoft.com/en-us/bingwebmaster/url-submission-api/

---

# 6. Canonical Tags

Add to every page:

<link rel="canonical" href="https://domain/current-page">

Flask / Jinja example:

<link rel="canonical" href="https://{{ site.domain }}{{ request.path }}">

Documentation:
https://developers.google.com/search/docs/crawling-indexing/consolidate-duplicate-urls

---

# 7. Structured Data

Recommended schema types:

ProfessionalService
LocalBusiness
Organization

Example:

{
"@context": "https://schema.org",
"@type": "ProfessionalService",
"name": "HOA Management Company Phoenix",
"areaServed": "Phoenix AZ",
"serviceType": "HOA Management",
"url": "https://domain.com"
}

Documentation:
https://schema.org/ProfessionalService

---

# 8. Local SEO Signals

Pages must include geographic references:

City
County
Neighborhoods
Local areas

Example for Phoenix:

Maricopa County
East Valley
Arcadia
Ahwatukee
Desert Ridge

Documentation:
https://developers.google.com/search/docs/fundamentals/local-search

---

# 9. Page Speed Targets

Largest Contentful Paint < 2.5s
CLS < 0.1
TTFB < 800ms

Tools:

PageSpeed Insights
Lighthouse

Documentation:
https://web.dev/vitals/

---

# 10. Internal Link Graph

HOME
 ├── services/hoa-management-city
 ├── services/hoa-management-company-city
 ├── services/hoa-property-management-city
 └── quote

Service pages link to:

home
quote

Documentation:
https://developers.google.com/search/docs/crawling-indexing/links-crawlable

---

# 11. Open Graph Tags

Required:

og:title
og:description
og:url
og:image

---

# 12. Image SEO

Use:

alt tags
city keyword filenames

Example:

hoa-management-phoenix.jpg

Documentation:
https://developers.google.com/search/docs/appearance/google-images

---

# 13. NAP Consistency

Signals:

company name
address
phone number

Formatting must remain consistent across pages.

Documentation:
https://support.google.com/business/answer/3038177

---

# 14. Additional Keyword Pages

Recommended expansion:

/hoa-management-cost-city
/hoa-management-services-city
/hoa-property-management-city
/hoa-management-companies-city

Captures long tail keywords.

---

# 15. Crawl Budget Strategy

If deploying hundreds of domains:

separate sitemap per domain
fast server response
small site structure

Documentation:
https://developers.google.com/search/docs/crawling-indexing/large-site-managing-crawl-budget

---

# 16. Technical SEO Checklist

robots.txt
sitemap.xml
canonical tags
structured data
internal links
fast load speed
meta titles
meta descriptions
image alt text

---

# 17. Backlink Seeding

Examples:

industry directories
local directories
press releases
citation sites

Documentation:
https://developers.google.com/search/docs/fundamentals/seo-starter-guide

---

# 18. Automation Scripts Needed

domain provisioning
cloudflare dns automation
google search console verification
bing verification
sitemap submission
indexing api submission

---

# 19. City Content Blocks

Store in database to prevent duplicate content.

Example:

city_intro
city_neighborhoods
city_hoa_density

---

# 20. Admin Dashboard Requirements

create new site
manage domains
view leads
export leads
trigger indexing
manage content
