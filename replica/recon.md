# Recon map: CARFAX Europe, Spain (web)

Scope: the consumer flow for vehicle history reports. A buyer enters a plate or VIN, gets free basic data, pays for a full report, and reads it online or as a PDF. Also in scope: report packs and credits, the account area, and the marketing pages that feed the search box.
For: assumed to be the user's own product, a vehicle history service for the Spanish used-car market. Confirm this before `/replica-architect`.
Date: 2026-10-23

Method: public pages only, read as text. No account was used, no screenshots were taken, and no JS bundles or network calls were inspected. The support portal (Salesforce) did not render without JS, so the help-center rows are thin. The best next source is a walkthrough in the user's own browser of the sample report and of one real purchase.

## Sources

| # | source | URL | notes |
| --- | --- | --- | --- |
| 1 | marketing site (ES home) | https://www.carfax.eu/es | hero search, report sections, trust claims, FAQ explainer, footer |
| 2 | product page | https://www.carfax.eu/es/informe-vehiculo | report sections, purchase steps, "Comprobación rápida" summary, history timeline |
| 3 | pricing page | https://www.carfax.eu/es/precios | private/business tabs, 2 visible packs, "Más vendido" badge, credits valid 4 years. Prices are rendered by JS and were not captured |
| 4 | pricing, B2B tab | https://www.carfax.eu/es/precios?b2bTab=true | business packs |
| 5 | free VIN decoder | https://www.carfax.eu/es/comprobar-bastidor-vehiculo | what the free tier shows: make, model, manufacturer, origin, registration count, import flag |
| 6 | mileage topic page | https://www.carfax.eu/es/comprobar-el-kilometraje | mileage section: last reading, evolution chart, assessment, "Explicaciones" toggle |
| 7 | topic pages (damage, owners, ITV, registration, charges, import, theft) | https://www.carfax.eu/es/comprobar-danos-coche, /es/historial-de-propietarios, /es/inspecciones-tecnicas-del-vehiculo, /es/comprobar-fecha-matriculacion, /es/comprobar-coches-embargados, /es/comprobar-importacion-coche-gratis, /es/comprobar-si-un-coche-es-robado | one SEO landing page per report section |
| 8 | business page | https://www.carfax.eu/es/business | dealers, marketplaces, banks and insurers. Contact form, no self-serve |
| 9 | support center | https://support.carfax.eu/s/?language=es | needs JS. Contact form: https://support.carfax.eu/s/contactsupport?language=es |
| 10 | login | https://www.carfax.eu/es/account/login | account exists; needed for packs |
| 11 | terms / privacy | https://www.carfax.eu/es/terms-conditions-hub, https://www.carfax.eu/es/privacy-hub | read before using any account for research |
| 12 | third-party price notes (unofficial) | https://sensorautomotriz.com/preguntas-frecuentes/que-precio-tiene-un-informe-carfax | 1 report 19.99–39.99 €, 3 for 49.99 €, 5 for 74.99 €. Possibly out of date; the official home page says "from 29.99 €" |
| 13 | press release | https://www.carfax.eu/press/carfax-launches-in-germany-more-transparency-and-safety-when-buying-a-used-car | report is keyed on the 17-character VIN |

No public API docs. No app store listing was found for a Spanish consumer app, so this is web only.

## Core loop

A used-car buyer types a plate, sees what the car is and the price, pays about 30 €, and reads a report that flags damage, mileage tampering and legal charges before they buy.

## Screens

| ID | screen | route / how to reach | purpose | key components | states seen |
| --- | --- | --- | --- | --- | --- |
| S01 | Home | `/es` | Pitch, plus the main plate search | header nav with mega-menu, hero with plate input, feature cards, tabbed "what we check", testimonial carousel, stats with footnotes, FAQ accordion, footer | filled. Inferred: input validation error |
| S02 | Free VIN decoder | `/es/comprobar-bastidor-vehiculo` | Free basic data, and an upsell to the report | plate/VIN input, result card, explainer content | filled. Inferred: not found |
| S03 | Lookup result / offer | after submitting a plate on S01/S02 (route unknown) | Shows the car's basic data and the final price, and starts the purchase | vehicle summary card, price, buy CTA, "use a credit" option (inferred) | inferred: found, not found, invalid plate, loading |
| S04 | Checkout | after S03 | Email, then payment | email input, payment method selector (card, Apple Pay, Google Pay, PayPal), order summary | inferred: payment failed, processing |
| S05 | Report | opens right after payment; also from the email link and S10 | The product | quick-check summary with flags; sections for damage, mileage (chart), owners, ITV, registration, charges, import, theft; history timeline; "Explicaciones" toggle; PDF download | filled, partial data (no records in a section). Inferred: expired after 30 days |
| S06 | Pricing | `/es/precios` | Single report vs packs | private/business tabs, pack cards, "Más vendido" badge, plate input, payment logos | filled |
| S07 | Topic landing (x8) | `/es/comprobar-danos-coche` etc. | SEO; explains one report section with a plate search | plate input, sample-report visuals, explainer | filled |
| S08 | Sample report | link "Ver un ejemplo de informe CARFAX" (URL not captured) | Shows the product before purchase | same as S05 on fictional data | filled |
| S09 | Log in / sign up | `/es/account/login` | Account for packs and history | email/password form (inferred), reset password (inferred) | inferred: error, reset sent |
| S10 | Account: my reports and credits | after login (route unknown) | Bought reports, credit balance, invoices | reports list, credit counter, invoice list | inferred: empty, filled |
| S11 | Business | `/es/business` | B2B pitch, lead capture | sector cards, stats, contact CTA to support form | filled |
| S12 | Support / contact | support.carfax.eu | Help articles, contact, refund request | category list, article, contact form | not seen (JS) |
| S13 | Legal and cookie banner | `/es/terms-conditions-hub`, `/es/privacy-hub`, `/es/informacion-legal` | GDPR, terms | cookie consent modal, long-form pages | filled |
| S14 | Blog | `/es/blog` | Content marketing | article list, article page | filled |

Everything marked "inferred" is a state the buyer is likely to hit, but nobody has looked at it yet. Confirm these in a live walkthrough.

## Flows

```
F01 Buyer checks a car and buys one report (guest)   <- core loop
    S01 -> S03 -> S04 -> S05 (+ email with link and PDF)
    happy path clicks: ~5 (type plate, submit, buy, enter email, pay)
    edge: invalid plate format; plate not found; vehicle found but few records
          (price lower, or "few data" warning?); payment declined; email typo,
          so the buyer never gets the link; buyer closes the tab before the report loads

F02 Buyer gets free basic data, then upgrades
    S02 -> (free result) -> S03 -> S04 -> S05
    happy path clicks: ~6
    edge: VIN with I/O/Q; foreign plate; VIN decodes but has no history

F03 Buyer buys a pack, then spends credits
    S06 -> S09 (sign up) -> S04 -> S10 ; later S01 -> S03 (use credit) -> S05
    happy path clicks: ~6 to buy, ~3 per later report
    edge: credits expired (4 years); 0 credits left; same car checked twice
          (charge again or reopen the old report?)

F04 Buyer reopens a report
    email link or S09 -> S10 -> S05
    happy path clicks: 1 from the email, ~3 from the account
    edge: link older than 30 days; guest purchase with no account

F05 Buyer reads a sample report before buying
    S01 or S07 -> S08 -> S01 search
    happy path clicks: 2

F06 Buyer asks for a refund (30-day guarantee)
    S12 contact form (or email) -> manual review -> refund to the original payment method
    edge: business customer (not eligible); report already used

F07 Business asks for a demo
    S11 -> S12 business contact form
    happy path clicks: 2
```

Number to beat: plate to report in about 5 clicks, with no account.

## Components

| component | variants | states | used on |
| --- | --- | --- | --- |
| Plate/VIN input | hero (large), inline, header-compact | empty, typing, invalid, submitting | S01 S02 S03 S06 S07 |
| Button | primary CTA, secondary, link | default, hover, focus, disabled, loading | all |
| Header nav | desktop mega-menu, mobile drawer | open, closed | all |
| Country/language switcher | dropdown | open, closed | all |
| Feature card | icon + title + text | default | S01 S07 |
| Tabs | content tabs, pricing audience tabs | active, inactive | S01 S06 |
| Pricing card | single, pack, "best seller" highlighted | default, highlighted | S06 |
| Testimonial carousel | | autoplay, paused | S01 |
| FAQ accordion | | open, closed | S01 S02 S05 |
| Vehicle summary card | | found, not found | S03 S05 |
| Payment method selector | card, Apple Pay, Google Pay, PayPal | selected, error | S04 |
| Report quick-check | flag list (ok, warning, alert) | data, no data | S05 S08 |
| Report section | collapsible, with "Explicaciones" toggle | has records, no records | S05 S08 |
| Mileage chart | line chart with readings, average and predicted points | consistent, inconsistent | S05 S08 |
| History timeline | vertical, typed events (ITV, service, odometer, owner change, accident, theft, import) | default | S05 S08 |
| Status badge | ok, warning, alert | | S05 S10 |
| Credit counter | | 0, n, expiring | S10 S03 |
| Data table/list | reports, invoices | empty, filled | S10 |
| Cookie consent modal | first visit, settings | | all |
| Toast/alert | success, error | | S04 S09 S10 |
| Footer | with payment logos and certification badge | | all |

## Inferred data model

```
Vehicle   id, vin (17, unique), plate (nullable, unique per country), country,
          make, model, manufacturer, first_registration_date, origin_country,
          registrations_count
          evidence: S02 free output, S05 registration section, press release (VIN-keyed)
          confidence: high

VehicleRecord  id, vehicle_id, type (inspection | service | odometer | owner_change |
          accident | damage | theft | total_loss | import | charge), occurred_at,
          country, odometer_km (nullable), source_id, payload (json: damaged_parts,
          repair_cost_estimate, itv_result, owner_use private|commercial|taxi|rental, ...)
          evidence: S05 history timeline description, section pages (S07)
          confidence: high for the types; payload fields are a guess

Source    id, name, country, kind (registry | insurer | workshop | auction | partner)
          evidence: "177,000+ sources", "fuentes seleccionadas"
          confidence: medium; never shown in detail

Report    id, vehicle_id, order_id, user_id (nullable), email, created_at,
          access_expires_at (created_at + 30d), pdf_url, snapshot (json of records
          at purchase), quick_check_flags (json), countries_with_data
          evidence: S05, "30 days online access", "PDF by email"
          confidence: high

MileageAssessment  (derived, not stored, or stored on Report) last_reading_km,
          last_reading_at, predicted_km, avg_km_per_year, status
          (consistent | possible_rollback | possible_error)
          evidence: S07 mileage page
          confidence: medium

User      id, email, password_hash, locale, created_at, type (private | business)
          evidence: S09, packs need an account, private/business tabs
          confidence: high

Product   id, kind (single | pack), reports_count, price_cents, currency, audience
          (private | business), badge
          evidence: S06
          confidence: high

Order     id, user_id (nullable), email, product_id, amount_cents, currency,
          payment_method, status (pending | paid | failed | refunded), vehicle_id
          (single report only), created_at
          evidence: S04, refund policy
          confidence: high

CreditLot id, user_id, order_id, total, remaining, expires_at (+4 years)
          evidence: "créditos válidos durante 4 años"
          confidence: high

RefundRequest id, order_id, reason, status, created_at
          evidence: 30-day guarantee
          confidence: guess (could be just a support ticket)

Lead      id, company, sector (dealer | marketplace | bank | insurer), email, message
          evidence: S11
          confidence: medium
```

Relationships: Vehicle 1-n VehicleRecord; Source 1-n VehicleRecord; Vehicle 1-n Report; Order 1-1 Report (single) or Order 1-1 CreditLot (pack); CreditLot 1-n Report (redeemed); User 0-n Order; User 0-n Report.

Pricing rule: a single report's price depends on `countries_with_data`. With pack credits, the price per report is flat.

## Feature matrix

See `features.csv`: 45 rows. Must: 20, should: 13, could: 12. Three rows are marked skip (the data, the partner logos, the ISO badge).

## Out of scope (cannot or should not be cloned)

- **The vehicle history data itself.** This is the product. CARFAX buys it under long-term agreements with 100k+ sources in 22+ countries. A clone without data is an empty report template. You need your own legal data supply before this is a business. Candidates to research (unverified, check each one's terms): DGT vehicle reports for Spain (official, paid per query), ITV station networks, insurer claim databases (e.g. TIREA in Spain, access restricted), workshop/DMS partners, auction houses, and B2B data resellers. Without one of these, build the product shell on fictional data, which is fine for the sample report.
- **The partner network.** The marketplace integrations (Coches.net, AutoScout24, Wallapop) and the "35+ integrations" are business deals.
- **Brand and trust assets.** The CARFAX name, the logo, the illustrations, the testimonials, the "6 million vehicles" stats and the ISO 27001 badge. Earn your own.
- **Their copy and the sample report content.** Write your own.
- **Using a CARFAX account to build this.** Their terms may forbid using the service to build a competing product. Read the terms (source 11) before doing a logged-in walkthrough; if they forbid it, stay on public pages.

## Size

Screens 14 (about 20 page types counting S07 x8), flows 7, entities 10.

Hard parts:
1. **Data acquisition and normalisation.** Licensing sources, matching records to a VIN across countries, and turning plate into VIN. This is a business problem more than an engineering one.
2. **Report engine.** It snapshots records at purchase and derives the quick-check flags and the mileage consistency check (rollback detection). It renders the same report as a responsive page and as a PDF, and refreshes within 30 days.
3. **Payments and credits.** Dynamic per-vehicle pricing, guest checkout, wallets plus PayPal, 4-year credit lots, refunds, and Spanish VAT invoices.

Size: **M** for the product shell (a few weeks: search, checkout, report on seed data, account, credits, PDF, marketing pages). **XL** if "clone" includes a real data supply, so rescope it: ship the shell first with one licensed source for Spain.
