# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static marketing website for **SAIGON CLEAN**, a premium commercial cleaning company serving Ho Chi Minh City. No build step — all HTML, CSS, and JS are embedded in single-file pages.

The repository also contains `hcmc_cleaning_leads.csv`, a B2B leads database scraped from Google Maps (columns: No, Business Name, Category, Address, Phone, Website, Rating, Reviews, Google Maps URL, Search Query). This is used to populate the Clients section and for sales outreach.

## Tech Stack

- **Framework**: Static HTML, no build step
- **Styling**: Embedded CSS (custom properties + manual grid)
- **JavaScript**: Vanilla JS, embedded — currently only a scroll listener for the nav
- **Fonts**: Cormorant Garamond (serif display, italic headings) + Inter (body/UI) via Google Fonts CDN
- **Hosting**: Netlify

## File Structure

```
/D:/SaiGonClean/SaiGonClean-agency/
├── index.html          # Main landing page (Nav, hero , clients, included, services, contact, Get a quote)
├── services.html       # Detailed services page (Sevices Details, Pricing, Process, Inclusions Rail, FAQ, CTA Banner, Footer)
├── request.html          # Request page (Nav, hero, quote form, contact strip, live chat button, foooter)
├── contact.html        # Contact page (form, Nav, Hero, direct contact, feedback mailbox, footer )
├── netlify.toml        # Netlify configuration
├── .gitignore          # Git ignore rules
├── CLAUDE.md           # This file
└── .netlify/           # Netlify local state (gitignored)
```

## Deployment

```bash
netlify deploy --prod   # production
netlify deploy          # preview
```

Requires Netlify CLI (`npm install -g netlify-cli`) and `netlify login`.

## Design System

### Colors

| Token | Hex | Usage |
|-------|-----|-------|
| `--bg` | `#F7F4EE` | Page background (warm cream) |
| `--bg2` | `#111111` | Dark sections (Stats, Pull Quote) |
| `--text` | `#0a0a0a` | Body text |
| `--muted` | `rgba(0,0,0,0.42)` | Secondary text |
| `--accent` | `#1F7A50` | Forest green — labels, links, form focus, star ratings |
| `--border` | `rgba(0,0,0,0.09)` | Dividers and card borders |
| `--card` | `#FFFFFF` | Card backgrounds |
| Gold | `#D4AF37` | Logo, form labels, contact info labels |
| Lime | `#c8ff57` | Card tags and stat numbers on dark sections |
| Nav CTA bg | `#c8ff57` | "Get a Quote" button |

### Typography

- **Display headings**: Cormorant Garamond, `font-weight: 300`, `font-style: italic`, `line-height: 1.04`
- **Body / UI**: Inter, weights 300–700
- **Labels / tags**: Inter, `font-size: 10–11px`, `font-weight: 600–700`, `letter-spacing: 0.35–0.42em`, `text-transform: uppercase`

### Logo

`Saigon ◆ Clean` — "Saigon" in gold (`#D4AF37`), "◆" as a small separator gem, "Clean" in white (nav) or `#0a0a0a` (footer). Paired with a gold gradient rule and sub-label "Ho Chi Minh City · Est. 2020".

### Layout

- Max content width: no explicit max — full-bleed sections
- Card grid: 12-column CSS grid (`grid-template-columns: repeat(12, 1fr)`) with asymmetric column spans
- Services grid rows: `520px 400px` desktop; collapses to `1fr 1fr` at 1024px, `1fr` at 640px
- Clients grid: `repeat(3, 1fr)` → `1fr 1fr` → `1fr`

## Git Workflow

### Branch Naming

- `feature/services-page`
- `feature/request-page`
- `feature/contact-page`

### Merging Feature Branches

```bash
git status && git branch -a
git merge feature/services-page -m "Merge feature/services-page: description"
git push origin main
```

### IMPORTANT: Handling index.html Conflicts

Feature branches often use `index.html` as their main file during development. When merging, this can **overwrite the main landing page**.

**After merging, always verify:**
1. `index.html` contains the **main landing page** (title: "SAIGON CLEAN — Premium Commercial Cleaning · Ho Chi Minh City")
2. Feature content is in its **dedicated file** (e.g., `services.html`, not `index.html`)

**If a feature branch overwrote index.html:**
```bash
# 1. Save the feature content to its proper file
cp index.html services.html  # or about.html, contact.html

# 2. Restore the original landing page from before the merge
git show <commit-before-merge>:index.html > index.html

# 3. Commit the fix
git add index.html services.html
git commit -m "fix: Restore landing page, move services to services.html"
```

**To find the original index.html:**
```bash
# View commit history
git log --oneline

# Show index.html from a specific commit
git show f87222a:index.html | head -20
```

## Page Structure

### index.html (Main Landing Page)
1. **Nav** — Fixed top bar: logo (Saigon ◆ Clean wordmark + "Ho Chi Minh City · Est. 2020" sub-label), nav links (Services, Clients, Included, Contact), "Get a Quote" CTA button. Transitions to dark/blurred background on scroll via `.scrolled` class toggled by JS.
2. **Hero** — Full-viewport background photo (office interior). Large italic serif headline "Spotless Spaces, Lasting Impressions". Sub-label strip: Offices · Hotels · Serviced Apartments · Coworking Spaces. Two CTAs: primary "Get a Free Quote" (links to `#contact`) and secondary "Our Services" (links to `#services`). Scroll indicator at bottom.
3. **Quote Bar** — Single italic social-proof line ("Trusted by 100+ premium offices…") + phone number as a button CTA.
4. **Services** (`#services`) — Section intro (label, display headline, descriptor paragraph) + asymmetric 12-column editorial card grid (5 cards with hover zoom): Office & Corporate Spaces, Hotels & Serviced Apartments, Shared Workspaces, Luxury Apartments & Condos, Property Management.
5. **Stats** — Full-width dark band, 4 columns: 100+ Active Clients · 4.8★ Average Rating · 24h First Booking · 100% Satisfaction Guarantee.
6. **Pull Quote** — Dark section. Large centered italic brand-promise quote. CTA "Start with a Free Quote".
7. **What's Included** (`#included`) — Section header + horizontally scrollable icon rail with 10 standard inclusions: Dusting & Sweeping, Floor Mopping, Surface Sanitizing, Bathroom Deep Clean, Window Cleaning, Waste Removal, Streak-Free Finish, Eco-Safe Products, Insured Staff, Checklist Report.
8. **Clients** (`#clients`) — Section header with "Become a Client" CTA + 3-column grid of 12 HCMC client cards (category tag, business name, address, star rating, review count, phone, "Visit Website" link). Data sourced from `hcmc_cleaning_leads.csv`. Footer row: "Showing 12 of 100 clients" + "Enquire About Partnership" CTA.
9. **Contact** (`#contact`) — Two-column split: left panel (label, display headline, description, contact info rows: Phone, Email, Service Area, Hours); right panel (quote request form: Name, Business Name, Phone, Address, Service Type dropdown, Message textarea, submit button).
10. **Footer** — Logo wordmark, nav links (Services, Clients, Included, Contact), copyright line.

---

### services.html (Detailed Services)
1. **Nav** — Same as landing page.
2. **Hero** — Full-viewport or half-height photo. Headline: "Five-Star Cleaning for Saigon's Finest Spaces". Brief descriptor. CTA "Get a Free Quote".
3. **Services Detail** — One section per service type, each with: photo, tag, headline, detailed description paragraph, bullet list of inclusions.
   - Office & Corporate Spaces
   - Hotels & Serviced Apartments
   - Coworking & Shared Workspaces
   - Luxury Apartments & Condos
   - Property Management & Turnover
4. **Pricing** (`.pricing-section`) — Three tiers (Essentials, Professional, Enterprise), each with `.pricing-name`, `.pricing-amount`, `.pricing-features` list, and a CTA button.
5. **Process** — "How It Works" — numbered 4-step flow: Enquire → Site Visit & Quote → Schedule First Clean → Ongoing Service.
6. **Inclusions Rail** — Same horizontal scroll rail as landing page (`#included`).
7. **FAQ** — Stacked Q&A: minimum contract terms, insurance, product supply, after-hours availability, cancellation policy.
8. **CTA Banner** — Full-width dark band: headline "Ready to get started?" + button linking to `request.html`.
9. **Footer** — Same as landing page.

---

### request.html (Request Free Quote)
1. **Nav** — Same as landing page.
2. **Hero** — Half-height. Headline: "Request Your Free Quote". Sub-copy: "We'll visit your space and send a tailored proposal within 24 hours."
3. **Quote Form** — Full-width or centered two-column form:
   - Contact details: Name, Business Name, Phone Number, Email
   - Property details: Address, District, Property Type (dropdown), Approximate Size (m²)
   - Service details: Service Type (dropdown), Preferred Frequency (Daily / Weekly / Monthly / One-off), Preferred Start Date
   - Additional notes: Message / Special Requirements textarea
   - Submit button: "Send My Request"
4. **What Happens Next** — 3-step reassurance strip: ① We review your request (same day) → ② Site visit & tailored quote (within 24h) → ③ First clean scheduled at your convenience.
5. **Contact Strip** — Inline contact info (phone + email) for users who prefer to call.
6. **Live Chat Button** — Fixed floating button (bottom-right corner): chat bubble icon + "Chat with us" label. Opens an inline chat widget or links to a WhatsApp/Zalo thread. Visible on all scroll positions; collapses to icon-only on mobile.
7. **Footer** — Same as landing page.

---

### contact.html (Contact)
1. **Nav** — Same as landing page.
2. **Hero** — Half-height. Headline: "Get In Touch". Sub-copy: "We're here Mon–Sat, 7:00 AM–8:00 PM."
3. **Contact Split** — Two-column layout:
   - Left: contact info rows (Phone, Email, Service Area: All Districts HCMC, Hours), embedded Google Map of service area.
   - Right: general contact form (Name, Email, Company, Subject dropdown, Message textarea, submit button).
4. **Direct Contact** — Highlight card with phone number as a large click-to-call link and WhatsApp button.
5. **Footer** — Same as landing page.

## Common Updates

### Update client cards
Edit the `.cl-card` blocks in `saigon-clean.html` directly. Source data is in `hcmc_cleaning_leads.csv` — columns map to: `.cl-cat` (Category), `.cl-name` (Business Name), `.cl-addr` (Address), `.cl-stars` + `.cl-reviews` (Rating + Reviews), `.cl-phone` (Phone), `.cl-link` href (Website).

### Update contact details
Search for `+84 904 915 240` and `hello@saigonclean.vn` — they appear in both the Quote Bar and the Contact section.

### Change accent colors
- Forest green labels/accents: `#1F7A50` and `var(--accent)`
- Gold logo/form labels: `#D4AF37`
- Lime card tags / dark-section highlights: `#c8ff57`
### contact.html (Contact)
1. **Hero** — Contact headline
2. **Contact Form** — Name, email, company, message fields
3. **Founder Section** — Direct contact info
4. **CTA** — Calendly booking link

## Interactive Features

- Scroll-triggered reveal animations via Intersection Observer
- Animated counters with easeOutQuart easing
- Mouse-following cursor glow effect (desktop only)
- Scroll progress indicator in header
- Smooth scroll navigation
- Fully responsive down to mobile

## Deployment

### Deploy to Production
```bash
netlify deploy --prod
```

### Preview Deploy
```bash
netlify deploy
```

### Requirements
- Netlify CLI installed (`npm install -g netlify-cli`)
- Authenticated with Netlify (`netlify login`)

## External Dependencies

- **Calendly**: Meeting booking at `calendly.com/leftclick-meeting-30`
- **Google Fonts**: Inter font family via CDN
- **No npm packages** — Zero build dependencies

## Common Tasks

### Update Content
Edit the relevant HTML file directly. All content, styles, and scripts are embedded.

### Change Colors
Search for hex codes:
- Primary green: `#10b981`
- Light green: `#34d399`
- Dark green: `#059669`

### Update Calendly Link
Search for `calendly.com/leftclick-meeting-30` and replace across all files.

### Update Pricing (services.html)
Find the `.pricing-section`. Each tier has:
- `.pricing-name` — tier name
- `.pricing-amount` — price
- `.pricing-features` — feature list

### Add/Remove Services (services.html)
Each service category has a `.services-detail-grid` containing `.service-detail-card` elements.

## Notes

- Keep the single-file architecture — no bundlers or build steps
- Maintain the squared corner aesthetic (no pills)
- Test scroll animations after content changes
- Counter animations trigger on scroll into view
- **index.html is the main landing page** — don't overwrite with feature content