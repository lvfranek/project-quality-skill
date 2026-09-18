# Legal (Germany): Impressum, Datenschutz, Cookies

**Important:** This is a completeness and consistency check, not legal advice. Don't write legal texts from scratch. Check whether the required pages exist, whether they match what the site actually does, and point out gaps. For the final wording, the user should use a generator (e.g. e-recht24, datenschutz-generator.de) or ask a lawyer. Legal pages are written in German (an English version can be added).

## Step 1: Data-flow inventory (the basis for everything else)
List every service that receives personal data — **an IP address counts**. Search the code, `package.json`, env variables and the browser's network tab:
- Hosting (Vercel, Netlify, …) → server logs
- Backend/auth/database (Supabase, Firebase) → which region?
- Fonts, CDNs, icon libraries loaded from external servers
- Embeds (YouTube, Google Maps, Calendly, …)
- Analytics, error tracking (Sentry, …), email/contact-form services
- Cookies, localStorage, sessionStorage — what is stored and why

Output: table *service | what data | purpose | where (EU/US) | needed for the site to work?*

## Step 2: Fix what creates problems in the first place
- **Google Fonts and other fonts: self-host them.** Loading them from Google's servers without consent was ruled unlawful (LG München I, 2022).
- Replace or remove external resources you don't need.
- Embeds (YouTube, Maps): two-click solution (load only after the user clicks) or a link instead.
- Supabase project region in the EU (e.g. Frankfurt) if possible.
- Collect only data that is really needed (data minimization).

## Step 3: Impressum (§ 5 DDG)
- Recommended for portfolio projects (a site used to present yourself professionally is usually treated as "geschäftsmäßig"). When in doubt, have one.
- Contents: full name, **address where you can be served** (no P.O. box; a c/o or Impressum service address is possible if the user doesn't want to publish their home address), email address plus a second quick contact option (e.g. contact form or phone).
- Reachable from **every page**, max. two clicks, clearly labeled "Impressum" (footer link).
- Demo projects on their own domain/subdomain: link to the same Impressum.

## Step 4: Datenschutzerklärung (Art. 13 DSGVO)
Check that it contains:
- Controller (name, contact)
- For **each processing from the inventory**: purpose, legal basis (Art. 6 (1) a/b/f DSGVO), recipients/processors, storage duration
- Transfers to third countries (US services: EU-US Data Privacy Framework or other safeguards)
- Data subject rights: access, rectification, erasure, restriction, portability, objection, withdrawal of consent, complaint to a supervisory authority
- Consistency: **every service from the inventory is mentioned, and nothing is mentioned that isn't used** (copy-pasted generator texts often list Google Analytics that doesn't exist)
- Linked on every page ("Datenschutz"), and near forms that collect data (contact, sign-up)

## Step 5: Cookies & consent (§ 25 TDDDG)
- **No banner needed** if only technically necessary storage is used (login session, cart, saved settings like language/theme).
- Analytics, tracking, marketing, external embeds → prior consent needed: "reject" as easy as "accept", nothing pre-selected, nothing loads before consent.
- Best option for a portfolio: no tracking at all, or a cookieless, EU-hosted analytics tool. Then no banner.

## Step 6: Processor agreements (Art. 28 DSGVO)
Hosting and backend providers process data on your behalf. Supabase, Vercel and Netlify offer a DPA (Data Processing Agreement) — tell the user to accept/sign it in the dashboard or via the provider's legal page.

## Step 7: Features with legal relevance
- User accounts: users can delete their account and data (right to erasure). At least a documented way.
- Seed/demo data and screenshots: fake data only, no real people or customer data.

## AGB (terms and conditions)
**Only needed if you offer contracts** (shop, paid service, bookings). Portfolio and demo projects don't need AGB — don't add fake ones. If the demo has public sign-up, a short "Terms of use" note is enough (e.g. "Demo project, data may be deleted at any time, no guarantee of availability").

## Accessibility law (BFSG)
Since 28 June 2025, the Barrierefreiheitsstärkungsgesetz applies to certain consumer-facing services (e.g. online shops). Micro-enterprises (< 10 employees and ≤ €2M turnover) are exempt for services. For portfolio projects it usually doesn't apply — the accessibility phase covers the quality side anyway.

## Report
The inventory table, a checklist (Impressum / Datenschutz / consent / DPAs / account deletion) with status, what was fixed in code (fonts, embeds, footer links), and what the user must do themselves (generator/lawyer, sign DPAs, enter address).
