# Site Map — website-au-v3

Static HTML build (one file per page). Designed to be ported into Webflow with CMS-backed Programs and Schools. See [README.md](README.md) for the full Webflow migration guide; this document focuses on the URL structure and page-by-page intent.

## URL structure

```
/                            index.html                Home
/programs                    programs.html             Programs index (all 21, filterable)
/programs/mba-miami          programs-mba-miami.html   MBA — reference implementation
/programs/<slug>             programs-template.html    CMS template (other 20 programs)
/schools/business            schools-business.html     School of Business (template for 6)
/admissions                  admissions.html           Admissions: steps, requirements, tuition, form, FAQ
/international               international.html        F-1/I-20, English pathways, countries, FAQ
/about                       about.html                Mission, accreditation, by the numbers
/contact                     contact.html              Campus, admissions, international contacts
```

## Page intent

| URL | Page title | Highlights |
|---|---|---|
| `/` | Atlantis University — Accredited Degree Programs in Miami, FL | Hero · Authority strip · Featured programs · Schools grid · Audiences · Dark CTA · FAQ |
| `/programs` | Degree Programs in Miami | Filter bar (School/Level/Format) · Compact program cards · Centered CTA |
| `/programs/mba-miami` | MBA in Miami — Hybrid Master of Business Administration | Hero + facts sidebar · Why MBA · Overview · Key details grid · Why AU · Mid CTA · Career outcomes · Curriculum · Why Miami · International · Tuition · FAQ · Comparison · Related programs · Final CTA |
| `/admissions` | Admissions | 5-step "How to apply" · Requirements · Tuition table · Request info form · FAQ · International callout |
| `/international` | International Students — F-1 Visa, I-20 & English Pathways | 3 pillars · Country chips (20) · FAQ · Continue-to-admissions |
| `/about` | About Atlantis University — Miami, Florida | Mission · Accreditation · By the numbers (4 stats) |
| `/contact` | Contact | Campus · Admissions · International (3-column) |
| `/schools/business` | School of Business — Degrees in Miami | Hero · Programs list · Careers · Final CTA (template for all 6 schools) |

## Shared resources

- `assets/shared.css` — design tokens (Fraunces + DM Sans, school accent colors) and base components, loaded by every page.
- `assets/_shared-partials.html` — header, footer, and global JSON-LD snippets (CollegeOrUniversity, WebSite) used across pages.

## Primary navigation

Programs · Admissions · International · About · Contact · "Apply now" CTA · "Request info" text link.

## SEO / structured data per page

| Page | JSON-LD types |
|---|---|
| `/` | FAQPage, WebSite + SearchAction (global), CollegeOrUniversity (global) |
| `/programs` | ItemList, BreadcrumbList |
| `/programs/mba-miami` (and template) | EducationalOccupationalProgram, BreadcrumbList, FAQPage |
| `/schools/business` (and template) | ItemList, BreadcrumbList |
| `/admissions` | HowTo (5 steps), BreadcrumbList, FAQPage |
| `/international` | BreadcrumbList, FAQPage |
| `/about` | BreadcrumbList |
| `/contact` | ContactPage, BreadcrumbList |
| Global (Project Head) | CollegeOrUniversity, WebSite |

## Roadmap (not yet built)

- Modalidades page (online / hybrid / on-campus comparison)
- Knowledge Hub / blog
- Spanish version under `/es/`

See `README.md` for component naming, CMS field maps, and per-page Webflow prompts.
