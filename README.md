# Atlantis University — Full Site Build

**Webflow + Relume MCP build guide**

This is the complete site rebuilt from the Lovable prototype. Visual structure matches the prototype 1:1; SEO, JSON-LD, semantic HTML, and CMS-ready architecture are added on top.

---

## What's in this delivery

```
atlantis-site/
├── index.html                    # Home
├── programs.html                 # Programs index (filterable list of all 21)
├── programs-mba-miami.html       # MBA — reference implementation for all program pages
├── programs-template.html        # Generic template (binds to CMS Programs collection)
├── schools-business.html         # School page (reference for all 6 schools)
├── admissions.html               # Admissions: 5-step process, requirements, tuition, form, FAQ
├── international.html            # F-1/I-20, English pathways, countries, FAQ
├── about.html                    # Mission, accreditation, by the numbers
├── contact.html                  # Campus, admissions, international contacts
├── assets/
│   ├── shared.css                # Design tokens + base components (used by every page)
│   └── _shared-partials.html     # Header, footer, global JSON-LD (documented snippets)
└── README.md                     # This file
```

Open any `.html` in a browser to see the page rendered exactly like the Lovable prototype. The class names are deliberately BEM-style (`.card__title`, `.school-card__bar`, `.faq__item`) so Relume and the Webflow MCP can recognize repeated patterns and turn them into reusable components and combo classes.

---

## 1) Webflow setup (do this first, in order)

### 1.1 Add Google Fonts
Project Settings → Fonts → Add Google Font:
- **Fraunces** — weights 400, 500, 600
- **DM Sans** — weights 400, 500, 600, 700

### 1.2 Create Style Variables
Settings → Style Variables. One Webflow variable per CSS custom property in `assets/shared.css`:

| Variable | Value | Type |
|---|---|---|
| `font-display` | `"Fraunces", ui-serif, Georgia, serif` | Font family |
| `font-sans` | `"DM Sans", ui-sans-serif, system-ui, sans-serif` | Font family |
| `color-background` | `#ffffff` | Color |
| `color-foreground` | `#0d1117` | Color |
| `color-muted-foreground` | `#5b6573` | Color |
| `color-sand` | `#f6f3ec` | Color |
| `color-border` | `#e7e3da` | Color |
| `school-business` | `#b0223a` | Color |
| `school-tech` | `#2752c2` | Color |
| `school-health` | `#1f7a4a` | Color |
| `school-eng` | `#c95416` | Color |
| `school-edu` | `#d4a017` | Color |
| `school-lang` | `#5b3aa6` | Color |
| `container-max` | `80rem` | Size |
| `radius-card` | `1rem` | Size |
| `radius-pill` | `9999px` | Size |

### 1.3 Body and heading defaults
Body Tag (All Pages):
- Font family: `var(--font-sans)`
- Color: `var(--color-foreground)`
- Background: `var(--color-background)`
- Line height: `1.6`

All H1, H2, H3, H4:
- Font family: `var(--font-display)`
- Font weight: `500`
- Letter spacing: `-0.02em`

### 1.4 Project-wide custom code
Project Settings → Custom Code → Head Code. Paste the **CollegeOrUniversity** and **WebSite** JSON-LD blocks from `assets/_shared-partials.html`. These apply globally and don't need to be repeated per page.

---

## 2) CMS Collections

Create these **before** wiring the pages. The pages will then bind to collection items.

### `Programs` (21 items)
| Field | Type |
|---|---|
| `name` | Plain text |
| `slug` | Slug |
| `short-name` | Plain text (e.g. "MBA") |
| `school` | Reference → Schools |
| `credential` | Plain text |
| `format` | Plain text or multi-reference |
| `level` | Option (Certificate, Diploma, Associate, Bachelor's, Master's) |
| `duration` | Plain text |
| `credits` | Number |
| `lead-paragraph` | Plain text long |
| `overview-text` | Rich text |
| `is-featured` | Switch |
| `meta-title` | Plain text |
| `meta-description` | Plain text long |
| `accent-color-var` | Plain text (e.g. `--school-business`) |
| `outcomes` | Multi-reference → Career Outcomes |
| `highlights` | Multi-reference → Program Highlights |
| `why-points` | Multi-reference → Why Points |
| `curriculum-blocks` | Multi-reference → Curriculum Blocks |
| `faqs` | Multi-reference → FAQs (filtered by program) |
| `related-programs` | Multi-reference → Programs (max 4) |

### `Schools` (6 items)
| Field | Type |
|---|---|
| `name` | Plain text (e.g. "School of Business") |
| `slug` | Slug |
| `tagline` | Plain text (e.g. "Business") |
| `description` | Plain text long |
| `lead-paragraph` | Plain text long |
| `program-count` | Number |
| `accent-color-var` | Plain text |

### `FAQs`
| Field | Type |
|---|---|
| `question` | Plain text |
| `answer` | Rich text |
| `page` | Option (Home, MBA, Admissions, Programs, International, About, etc.) |
| `program` | Reference → Programs (optional) |
| `order` | Number |

### `Career Outcomes`, `Program Highlights`, `Why Points`, `Curriculum Blocks`
Small reference collections (each with a `title` and `description` field — Curriculum Blocks also has `credits` and a `courses` rich text field).

---

## 3) Components (Webflow Component naming)

Use **these exact names** so the MCP can reuse them across pages.

| HTML pattern | Webflow Component name | Reused on |
|---|---|---|
| `<header class="site-header">` | `Site Header` | Every page |
| `<footer class="site-footer">` | `Site Footer` | Every page |
| Home `<section class="hero">` | `Hero — Editorial Lead + Facts` | Home |
| `<section class="page-hero">` | `Page Hero — Eyebrow + Title + Lead` | Programs, Admissions, International, About, Contact, Schools |
| Home authority strip | `Authority Strip — 3 Cards` | Home |
| Featured Programs | `Program Card Grid — 2 Col` | Home |
| `.card` (program) | `Program Card` | Home, Schools |
| `.school-card` | `School Card` | Home |
| Schools grid | `Schools Grid — 3 Col` | Home |
| Audiences | `Audience Tiles — 3 Col` | Home, About |
| `.finalcta` (dark band) | `CTA Band — Dark` | Home, MBA, Programs, Schools |
| `.finalcta--centered` | `CTA Band — Centered` | MBA, Programs, Schools |
| `.faq` | `FAQ — Two Column List` | Every page with FAQ (CMS-bound) |
| `.pcard` (compact) | `Program Card — Compact` | Programs index, Schools |
| `.steps` (5-step) | `Steps — 5 Numbered Cols` | Admissions |
| `.factcard` | `Fact Card — Sand Sidebar` | MBA, all programs |
| `.keydetails` | `Key Details — 8-cell Grid` | MBA, all programs |
| `.curr-block` | `Curriculum Block` | MBA, all programs |
| `.role-card` | `Role Card` | MBA, all programs |
| `.compare-card` | `Comparison Card` | MBA, all programs |
| `.related-card` | `Related Program Card` | MBA, all programs |
| `.mid-cta` | `Mid CTA — Dark Pill` | MBA, all programs |
| `.intl-list` | `International Bullet List` | MBA, all programs |
| `.tuition-list` | `Tuition Notes — Divided List` | MBA, Admissions |

Buttons (`btn--primary`, `btn--secondary`, `btn--ghost`, `btn--inverse`, `btn--inverse-outline`, `btn--sm`) should each be a **combo class** on a base `Button` element — that way swapping styles doesn't require rebuilding.

---

## 4) MCP prompts (run in order)

These assume sections 1, 2, and 3 are done.

### Prompt A — Header + Footer
```
Read /atlantis-site/assets/_shared-partials.html. Build two Webflow components:
1. "Site Header" from the <header class="site-header"> markup.
   - Brand text "Atlantis University" using --font-display
   - 5-item nav (Programs, Admissions, International, About, Contact)
   - Right side: "Request info" text link + "Apply now" filled pill (btn--primary) + search icon
   - Sticky on scroll, backdrop-filter blur, 1px bottom border
   - Add aria-current="page" support so each page can mark its active link
2. "Site Footer" from <footer class="site-footer">.
   - 3-column grid on desktop (2fr / 1fr / 1fr)
   - Address must remain in <address> tag for SEO
   - Bottom bar with copyright + accreditation
Apply globally.
```

### Prompt B — Home
```
Read /atlantis-site/index.html and create the home page in Webflow:
1. Hero ("Hero — Editorial Lead + Facts"): 8/4 grid on desktop, single col mobile.
   Eyebrow with horizontal rule, H1 max-width 18ch, lead paragraph, three CTAs,
   sand "At a glance" facts card on the right. Keep dl/dt/dd semantics.
2. Authority Strip: sand background, 3 columns, each with 1px black top border,
   eyebrow "AUTHORITY", H3, paragraph.
3. Featured Programs: 2-col grid of "Program Card" components. Bind to
   Programs CMS filtered by is-featured=true, limit 4. Each card has a 4px
   accent-color top bar (from CMS), school eyebrow (inherits accent),
   H3, dl with Credential and Format, description, career outcome chips,
   Apply now btn--sm + View details text link.
4. Schools Grid: 3-col grid of "School Card" components bound to Schools CMS.
   Card has 4px accent top bar, tagline eyebrow, H3, description, footer
   row (program count + Explore arrow).
5. Mid-section CTA banner: sand pill with copy + 2 buttons.
6. Audience Tiles: 3 columns, each with 1px black top border, H3, paragraph,
   arrow link.
7. CTA Band Dark: foreground bg, white text, two CTAs (btn--inverse + btn--inverse-outline).
8. FAQ: sand bg, 2-col rows (question 18rem / answer flex). Bind to FAQs CMS
   filtered by page=Home, sorted by order.

Page settings: title "Atlantis University — Accredited Degree Programs in Miami, FL".
Add the FAQPage JSON-LD from index.html to Page Settings > Head Code.
```

### Prompt C — Programs index
```
Build /programs from /atlantis-site/programs.html:
1. Page Hero with eyebrow "Academics", H1, two-paragraph lead, two CTAs.
2. Sticky filter bar (top: 4rem to sit below header). Three select dropdowns
   (School, Degree level, Format) + count + "Clear filters" button.
   Wire filtering via Webflow's CMS filter or with custom JS reading
   data-school/data-level/data-format attributes on each card.
3. Program grid: 3 cols desktop, 2 cols tablet, 1 col mobile. Each "Program
   Card — Compact" (.pcard class) bound to Programs CMS. Card has 4px accent
   top bar, school name + level pill row, H3, credential, description,
   format/duration dl, View details link.
4. Centered Final CTA dark band.

Page Settings: add the ItemList JSON-LD from programs.html (lists all 21
programs as a structured collection) plus the BreadcrumbList JSON-LD.
```

### Prompt D — MBA (template for all program pages)
```
Build /programs/mba-miami from /atlantis-site/programs-mba-miami.html.
This is the most complex page and serves as the reference for every other
program page. Expected sections in this exact order:

1. Breadcrumb (Home / Programs / MBA in Miami)
2. Program Hero: 8/4 split. Left side has 3px red accent bar, school
   eyebrow link, large H1, lead paragraph, 4 highlight cards (2x2),
   two CTAs, authority footnote. Right side is a sticky "Program Facts"
   sand card with 6 dl rows.
3. Two stacked twocol blocks (4/8): "Why MBA" and "Program overview"
   with heading on left, body paragraphs on right.
4. Key Details: 8-cell sand grid (2 cols desktop, 1 mobile) with all
   program facts as dl pairs.
5. Why Atlantis: 6 cards in a 3-col grid with title + paragraph.
6. Mid-CTA: dark pill banner with H3 + 2 inverse buttons.
7. Career Outcomes: 6 role cards in a 3-col grid. Each card has a red
   "ROLE" eyebrow, role title, employer context paragraph.
8. Curriculum (sand bg): twocol layout with heading on left, three
   curriculum blocks on right. Each block has H3 (with credit count),
   description, and a 2-col grid of course pills.
9. Why Miami: 4 cards in a 2-col grid.
10. International support (sand bg): twocol with heading + bullet list
    of 6 SEVP/F-1 features.
11. Tuition: twocol with heading + divided list of 6 financing notes.
12. FAQ: 10 questions, two-column rows.
13. Comparison: "ADVANTAGE" eyebrow cards in a 3-col grid (5 advantages).
14. Related programs (sand bg): 4-card grid linking to other programs.
15. Centered Final CTA dark band.

All red accent elements use var(--school-business). For other programs,
swap to var(--school-tech) / --school-health / etc. via the body
--accent variable.

Page Settings: add EducationalOccupationalProgram JSON-LD,
BreadcrumbList JSON-LD, and FAQPage JSON-LD from programs-mba-miami.html.
```

### Prompt E — Admissions
```
Build /admissions from /atlantis-site/admissions.html:
1. Page Hero with eyebrow "Admissions", H1 "How to apply.", lead.
2. "Application steps" section with id="apply" (for hash linking from CTAs
   across the site). 5-column grid of steps, each with a 1px top border,
   serif step number (01–05), step title, description.
3. Two-column section: Requirements (border-divided list) on the left,
   Tuition (price table) on the right. Tabular numbers for prices.
4. Request Info section with id="request-info". Sand bg. Two-col layout
   with copy on left and form card on right. Form fields: full name,
   email, program select. Submit button uses btn--primary.
5. FAQ — 5 questions.
6. International link callout at the bottom.

Page Settings: add HowTo JSON-LD (5 steps), BreadcrumbList,
and FAQPage from admissions.html. Both #apply and #request-info
sections need scroll-margin-top: 5rem so they don't sit under the
sticky header when linked.
```

### Prompt F — International
```
Build /international from /atlantis-site/international.html:
1. Page Hero "Built for students from across the Americas."
2. 3-column "pillars" section: F-1 student visa, English pathways,
   Bilingual support. Each with 1px black top border, H3, paragraph.
3. Sand "Students currently enrolled from" section with a chip list of
   20 countries (Argentina to Venezuela).
4. FAQ — 6 international-specific questions.
5. Continue to admissions link.

Page Settings: BreadcrumbList + FAQPage JSON-LD.
```

### Prompt G — About
```
Build /about from /atlantis-site/about.html:
1. Page Hero "A small, accredited Miami university with international reach."
2. Two-column Mission / Accreditation section.
3. Sand "By the numbers" section with 4 stat cards (2010, 6, 20+, 1:14)
   in a 4-col grid. Each stat has 1px top border, big serif number,
   small label below.

Page Settings: BreadcrumbList JSON-LD.
```

### Prompt H — Contact
```
Build /contact from /atlantis-site/contact.html:
1. Page Hero "Get in touch."
2. 3-column contact section: Campus (address), Admissions (phone +
   email + hours), International (email + WhatsApp + bilingual note).
   All H2 are uppercase eyebrow style.

Page Settings: ContactPage + BreadcrumbList JSON-LD from contact.html.
```

### Prompt I — Schools (template)
```
Build /schools/business from /atlantis-site/schools-business.html, then
duplicate the template for the other 5 schools (technology, health,
engineering, education, languages), changing only:
- Body --accent variable
- Page hero copy
- Programs list (filtered from Programs CMS by school)
- Careers chip list

Sections:
1. Page Hero with breadcrumb, accent bar, school eyebrow, H1, lead,
   two CTAs.
2. Programs list (2-col grid of compact cards filtered by school).
3. Sand Careers section with eyebrow, H2, description, chip list.
4. Centered Final CTA dark band.

Page Settings: BreadcrumbList + ItemList JSON-LD listing the school's programs.
```

---

## 5) SEO + Page Settings checklist (per page)

For every page, set:
- **Title** — copy from `<title>` in the file
- **Meta description** — copy from `<meta name="description">`
- **Canonical** — copy from `<link rel="canonical">`
- **Open Graph title + description** — copy from `<meta property="og:*">`
- **Page Custom Code Head** — paste any page-specific JSON-LD (Breadcrumb, FAQ, EducationalOccupationalProgram, HowTo, ItemList, ContactPage)

Global head code (one-time): paste the CollegeOrUniversity + WebSite JSON-LD from `assets/_shared-partials.html`.

---

## 6) LLM-first checklist

Validating against the rules from the project context document:

- ✅ Single H1 per page, descriptive and keyword-rich
- ✅ H2/H3 are real questions where applicable (every FAQ section)
- ✅ FAQPage JSON-LD on Home, MBA, Admissions, International (4 of 9 pages)
- ✅ Direct answers in first 100 words of each page
- ✅ Entity consistency — "Atlantis University", "Doral, Miami, FL", "ACCSC-accredited" appear identically across pages and JSON-LD
- ✅ Lists and tables for structured info (At-a-glance dl, Key Details grid, Tuition table, Steps grid, country chips)
- ✅ Internal links use descriptive anchor text — never "click here"
- ✅ EducationalOccupationalProgram structured data on each program page
- ✅ ItemList structured data on Programs index and each School page
- ✅ HowTo structured data on Admissions
- ✅ BreadcrumbList on every internal page
- ✅ CollegeOrUniversity entity defined once with full address, telephone, sameAs

Pending (not built yet, mentioned in plan):
- Modalidades page (online vs hybrid vs on-campus)
- Knowledge Hub / blog (biggest SEO leverage still pending)
- Spanish version (`/es/`)

---

## 7) Conversion checklist

- ✅ "Apply now" CTA appears 5+ times per page minimum
- ✅ "Request information" appears 4+ times per page (soft conversion)
- ✅ Hero fineprint reduces friction ("Rolling admissions · No application fee · Bilingual advisors")
- ✅ Authority strip immediately after Home hero answers "is this real?"
- ✅ Audiences section addresses three intents (US / International / Working professional)
- ✅ Final CTA dark band breaks page rhythm before the FAQ
- ✅ Cross-page hash linking (`/admissions#apply`, `/admissions#request-info`) with scroll-margin
- ✅ Form on Admissions page has only 3 fields (name, email, program) — minimum friction

To-do for later:
- Sticky mobile bottom bar with Apply / Request info
- Form should auto-fill `?program=MBA` from URL params
- Logo bar of partner employers between Hero and Authority on Home

---

## 8) Things preserved from the Lovable prototype (NOT changed)

- All section orders
- CTA button placement
- The "At a glance" facts sidebar
- The 3-card authority strip after Home hero
- The 2×2 featured programs layout
- The 3×2 schools grid
- The 3-column audiences section
- The dark final CTA band before FAQ
- The two-column FAQ layout
- Footer 3-column structure
- 5-step Admissions process visual
- Tuition table styling
- Request Info form layout (copy left, form right)
- Country chips on International
- "By the numbers" 4-column layout on About
- 3-column Campus/Admissions/International on Contact
- MBA page section order (hero → why → overview → key details → why AU → mid CTA → outcomes → curriculum → why Miami → international → tuition → FAQ → comparison → related → final CTA)

## 9) What was added (not in original Lovable)

- **FAQPage JSON-LD** on every FAQ section — was missing
- **WebSite + SearchAction** JSON-LD (sitelinks search) — was missing
- **CollegeOrUniversity** JSON-LD with full address — was only partial
- **EducationalOccupationalProgram** JSON-LD on program pages
- **HowTo** JSON-LD on Admissions
- **BreadcrumbList** on every internal page
- **ItemList** on Programs index and School pages
- **ContactPage** schema on Contact
- Real `<dl>/<dt>/<dd>` definition lists where Lovable used divs
- `<address>` tags around postal addresses
- `aria-labelledby` on every section
- `aria-current="page"` on active nav links
- `role="banner"` / `role="contentinfo"` on header/footer
- `scroll-margin-top` on hash-linked sections so they don't hide under sticky header

---

## 10) Open items to confirm before publishing

1. **Phone number** `+1 (305) 555-1234` — placeholder
2. **Emails** `admissions@atlantisuniversity.edu`, `international@atlantis.edu` — confirm
3. **Tuition rates** ($385 / $565 / $2,400 / $320) — verify 2025–26 with finance
4. **Florida CIE License No. 3009** — verify number
5. **Social URLs** in `sameAs` — replace with real handles
6. **OG image** 1200×630 — needs design
7. **Logo file** — upload and update path in JSON-LD
8. **Full program copy for the other 20 programs** — only MBA is fully written; the others have stub copy in programs.html cards. Use the MBA structure and CMS bindings to flesh out each program.

---

## 11) File-to-Webflow page mapping

| Local file | Webflow URL |
|---|---|
| `index.html` | `/` |
| `programs.html` | `/programs` |
| `programs-mba-miami.html` | `/programs/mba-miami` |
| `programs-template.html` | (CMS template for `/programs/[slug]`) |
| `schools-business.html` | `/schools/business` (and template for the other 5) |
| `admissions.html` | `/admissions` |
| `international.html` | `/international` |
| `about.html` | `/about` |
| `contact.html` | `/contact` |
