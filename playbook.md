# HOA Management EMD Programmatic SEO Network
## Full Technical Playbook + Build Specification

Purpose:
Build a scalable network of Exact Match Domain (EMD) lead generation sites targeting HOA management queries across U.S. cities using a single Flask backend.

Primary keyword model:

hoa management company {city}
hoa management companies {city}
hoa property management {city}

Example:

hoamanagementcophoenix.com
hoamanagementcomesa.com
hoamanagementcochandler.com

Sources:
Google Search documentation
https://developers.google.com/search/docs

Bing Webmaster documentation
https://learn.microsoft.com/en-us/bingwebmaster/

Sitemap protocol
https://www.sitemaps.org/protocol.html

Schema structured data
https://schema.org

---

# 1. System Overview

The system deploys many domains with one backend application.

Each domain represents a localized SEO landing site targeting HOA management queries in a specific city.

Example:

hoamanagementcophoenix.com
hoamanagementcomesa.com
hoamanagementcoscottsdale.com

All domains resolve to the same server.

Routing logic determines which city site to render.

Architecture:

Internet
│
Cloudflare DNS
│
Flask Application
│
Domain Resolver Middleware
│
City Site Configuration
│
Template Rendering

---

# 2. Technology Stack

Backend

Python
Flask
Gunicorn

Frontend

Jinja2 templates
Minimal CSS
Minimal JavaScript

Infrastructure

DigitalOcean / VPS
Cloudflare DNS
PostgreSQL or SQLite

SEO Systems

Google Search Console
Bing Webmaster Tools
Google Indexing API
Bing URL Submission API

---

# 3. Domain Strategy

Each city receives a dedicated EMD domain.

Naming convention:

hoamanagementco{city}.com

Examples:

hoamanagementcophoenix.com
hoamanagementcomesa.com
hoamanagementcochandler.com
hoamanagementcoscottsdale.com

Cloudflare DNS configuration:

Type: A
Name: @
IP: SERVER_IP

Optional:

www CNAME → @

---

# 4. Database Schema

Table: sites

id
domain
city
state
primary_keyword
meta_title
meta_description
phone
email
created_at
active

Example record:

domain: hoamanagementcophoenix.com
city: Phoenix
state: AZ
primary_keyword: HOA Management Company Phoenix

---

Table: leads

id
site_id
name
email
phone
hoa_name
units
message
created_at
ip
utm_source
utm_campaign

---

Table: conversions

id
site_id
lead_id
conversion_time
page

---

# 5. Flask Application Structure

hoa_seo_network
│
├── app.py
├── models.py
├── create_site.py
├── requirements.txt
│
├── templates
│   ├── layout.html
│   ├── home.html
│   ├── services.html
│   ├── quote.html
│   └── success.html
│
├── static
│   ├── css
│   ├── js
│   └── images
│
└── utils
    ├── domain_resolver.py
    ├── sitemap.py
    └── robots.py

---

# 6. Domain Routing Middleware

Example logic:

from flask import request, abort
from models import Site

def get_site():
    domain = request.host.lower()
    site = Site.query.filter_by(domain=domain).first()

    if not site:
        abort(404)

    return site

---

# 7. URL Structure

/
/services/<keyword>-<city>
/quote
/success

Example:

hoamanagementcophoenix.com/
hoamanagementcophoenix.com/services/hoa-management-phoenix
hoamanagementcophoenix.com/services/hoa-management-company-phoenix
hoamanagementcophoenix.com/quote
hoamanagementcophoenix.com/success

---

# 8. Homepage Template

Example title:

HOA Management Company Phoenix | HOA Property Management

Example content:

<h1>{{ site.primary_keyword }}</h1>

If you are searching for a professional HOA management company in {{ site.city }}, our team provides full service association management for homeowners associations and condominium communities.

---

# 9. Services Pages

/services/hoa-management-phoenix
/services/hoa-management-company-phoenix
/services/hoa-property-management-phoenix

Sections:

Financial management
Board meeting support
Vendor management
Maintenance coordination
Reserve planning
Insurance claim support

---

# 10. Quote Page

Fields:

Name
Email
Phone
Community Name
Number of Units
Message

POST endpoint:

/api/lead

Example:

@app.route("/api/lead", methods=["POST"])
def lead():
    site = get_site()

    lead = Lead(
        site_id = site.id,
        name = request.form["name"],
        email = request.form["email"],
        phone = request.form["phone"],
        message = request.form["message"]
    )

    db.session.add(lead)
    db.session.commit()

    return redirect("/success")

---

# 11. Success Page

Purpose:

conversion tracking
analytics events
pixel tracking

---

# 12. robots.txt

Endpoint:

/robots.txt

Example:

User-agent: *
Allow: /

Sitemap: https://domain.com/sitemap.xml

---

# 13. XML Sitemap

Endpoint:

/sitemap.xml

Example pages:

/
/services/hoa-management-phoenix
/services/hoa-management-company-phoenix
/services/hoa-property-management-phoenix
/quote

---

# 14. Canonical Tags

<link rel="canonical" href="https://{{ site.domain }}{{ request.path }}">

Documentation:
https://developers.google.com/search/docs/crawling-indexing/consolidate-duplicate-urls

---

# 15. Structured Data

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

# 16. Local SEO Signals

Include geographic references:

Maricopa County
East Valley
Arcadia
Ahwatukee
Desert Ridge

---

# 17. Page Speed Targets

Largest Contentful Paint < 2.5s
CLS < 0.1
TTFB < 800ms

Tools:

Google Lighthouse
PageSpeed Insights

https://web.dev/vitals/

---

# 18. Internal Linking

HOME
 ├── services pages
 └── quote

Service pages link to:

home
quote

---

# 19. Image SEO

Use alt attributes and city-based filenames.

Example:

hoa-management-phoenix.jpg

---

# 20. NAP Consistency

Example:

Heywood HOA Management
Phoenix AZ
480-XXX-XXXX

https://support.google.com/business/answer/3038177

---

# 21. Google Search Console

Verify domain via DNS TXT.

Submit sitemap:

https://domain.com/sitemap.xml

https://support.google.com/webmasters/answer/9008080

---

# 22. Bing Webmaster Tools

Verification:

DNS TXT
XML file
Meta tag

Submit sitemap.

https://learn.microsoft.com/en-us/bingwebmaster/

---

# 23. Indexing APIs

Google:

https://indexing.googleapis.com/v3/urlNotifications:publish

Payload:

{
"url": "https://domain.com/",
"type": "URL_UPDATED"
}

Bing:

https://ssl.bing.com/webmaster/api.svc/json/SubmitUrlbatch

---

# 24. Backlink Seeding

Initial backlinks from:

business directories
local citations
press releases
industry directories

https://developers.google.com/search/docs/fundamentals/seo-starter-guide

---

# 25. Automation Scripts

Automation tasks:

register domain
create cloudflare dns record
insert site record
submit sitemap
submit indexing api

Example CLI:

python create_site.py --city "Phoenix" --state "AZ" --domain hoamanagementcophoenix.com

---

# 26. Admin Dashboard

create site
manage domains
view leads
export leads
trigger indexing
manage content

---

# 27. Future SEO Pages

/hoa-management-cost-{city}
/hoa-management-services-{city}
/hoa-management-companies-{city}
/hoa-board-responsibilities-{city}
/hoa-reserve-study-{city}

---

# 28. Scaling Strategy

Initial:

Top 100 cities

Future:

Top 500 cities

Domains:

500+

All served by one backend.

---

# 29. Expected Outcome

System deploys hundreds of city-specific HOA management lead generation sites.

Each domain acts as:

local SEO landing page
lead capture site
location authority signal

Infrastructure controlled by:

single Flask backend
single database
single deployment
