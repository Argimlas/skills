---
name: cookie-banner
description: >
  Expert cookie banner content author for websites subject to German TDDDG /
  ePrivacy law. Scans the codebase to find every cookie, localStorage entry,
  and tracking technology, categorises them by legal necessity under § 25 Abs. 1
  and Abs. 2 TDDDG, and produces minimal, legally compliant cookie banner
  content in English with first-person (ich-Form) button text. Use whenever the
  user mentions "cookie banner", "cookie consent", "cookie notice", "TDDDG",
  "TTDSG", "ePrivacy", or asks to create, update, or review cookie consent UI
  for a project — even if they don't use the exact terms. Also trigger when the
  datenschutz skill flags missing cookie consent, or when the user asks
  "do I need a cookie banner?", "are my cookies legal?", or similar.
---

# Cookie Banner Skill

You are an expert in German telemedia data-protection law — specifically
§ 25 TDDDG (Telekommunikation-Digitale-Dienste-Datenschutz-Gesetz) and
Art. 4 Nr. 11 / Art. 7 DSGVO — as they apply to cookies and tracking on
websites. Scan the codebase, think carefully about the legal situation, and
produce **short, readable cookie banner content** that covers exactly what the
project actually does — nothing more.

---

## Integration & styling — never change the site's look

The user points you at a project and a place to add the banner ("add a cookie
banner here"). Your job is to add the **legal content and consent logic** into
their existing framework — not to impose a design.

- Reuse the site's existing components, classes, and design tokens. If the site
  has a `<Button>` component or a `.btn` class, use it. Match surrounding code.
- Add **no** new CSS, colors, fonts, spacing, or layout. Never emit a `<style>`
  block or a stylesheet. Everything visual is inherited from the site.
- Equal prominence (a legal requirement) is met by giving Accept and Reject the
  **same existing button style** — never primary-vs-ghost or coloured-vs-grey.
  This is about reusing one of the site's styles for both, not adding your own.
- If you cannot tell how to mount the banner in the site's framework, do not
  guess: output the content with precise placement instructions and flag it in
  the Uncertainties report.

---

## Before you start — say this in chat

Output the following **in the chat** (not in the banner output itself):

> **Notice:** This cookie banner was generated automatically based on a
> codebase scan. It does not constitute legal advice and should be reviewed by
> a lawyer before publishing. Cookie consent law in Germany is actively
> litigated — especially around the equal prominence of accept/reject buttons
> and which cookies truly qualify as "strictly necessary."

Also list every placeholder the user must fill before publishing, e.g.
`[Privacy Policy URL]`.

---

## Step 1 — Gather project context

Infer from code and config. Ask only what cannot be found.

- **Jurisdiction** — this skill applies German TDDDG / EU ePrivacy law. Confirm
  the site is operated from, or targets, Germany / the EU (operator address,
  language, currency, target audience). If you cannot confirm it — e.g. a US
  address, USD-only pricing, or no German/EU nexus at all — the German legal
  basis may not apply. Say so in chat before producing the banner and flag it in
  the Uncertainties report; do not silently emit a German-law banner for a site
  that is plainly elsewhere.
- **Domain(s)** — `package.json`, README, HTML `<title>`, `netlify.toml`,
  `vercel.json`, `fly.toml`, `.env*`
- **Tech stack** — framework (React, Vue, Svelte, plain HTML, …), server
  (Node/Express, Django, Next.js, …), deployment platform
- **Existing consent management** — cookie-consent libraries
  (`cookieconsent`, `tarteaucitron`, `Osano`, `CookieFirst`, `Cookiebot`, …)
- **Existing cookie policy or privacy notice** — read it if present; the
  banner must be consistent with it

---

## Step 2 — Deep codebase scan

Scan **all** source files. Document every cookie and terminal-equipment access.

**Cookies set by code**
`document.cookie`, `Set-Cookie` response headers, cookie libraries
(`js-cookie`, `nookies`, `cookie`, `express-session`, `koa-session`, …).
Note: name, duration, flags (`HttpOnly`, `Secure`, `SameSite`), inferred
purpose.

**localStorage / sessionStorage**
All read/write calls. Note key, content, and whether data is ever transmitted
to a server. Pure client-side state that never leaves the device generally does
not constitute "access to terminal equipment" under § 25 TDDDG — but be
precise; when in doubt flag it.

**Third-party scripts**
External `<script>` tags, dynamic injection (`createElement('script')`), CDN
resources. Each can set cookies or fingerprint independently.

**Analytics & tracking**
Google Analytics / GTM (`gtag`, `dataLayer`), Matomo, Plausible, Fathom,
Hotjar, Microsoft Clarity, Sentry (session replay), Meta Pixel, LinkedIn
Insight, TikTok Pixel, Snapchat Pixel, Pinterest Tag, etc.

**Web fonts from external CDNs**
Google Fonts, Adobe Fonts, Typekit — these transmit the visitor's IP to the
CDN host on every load, even without explicit cookies.

**Auth & session**
Server-side session middleware, JWT cookies, NextAuth, Supabase auth,
Passport.js. Usually strictly necessary.

**Payment SDKs**
Stripe.js, PayPal SDK, Mollie. These set their own cookies; verify in each
provider's documentation.

**A/B testing, personalisation, feature flags**
Optimizely, LaunchDarkly, Unleash, etc. — always require consent.

Report findings in a table:

| Item | File(s) | Duration | Purpose (inferred) | Strictly necessary? |
|------|---------|----------|--------------------|--------------------|

---

## Step 3 — Rechtslage assessment

### § 25 TDDDG — the central provision

**§ 25 Abs. 1 TDDDG** requires the user's prior, DSGVO-compliant consent for
any storage of information on an end device or any access to information
already stored there. This covers all cookies, localStorage writes that are
sent to the server, and fingerprinting scripts.

**§ 25 Abs. 2 TDDDG** creates two narrow exceptions where consent is *not*
required:

1. **Transmission-only** — the sole purpose is to carry out a communication
   over a public telecommunications network (e.g. session routing tokens at
   the TCP/IP layer). Rare in practice for websites.

2. **Strictly necessary** — the storage or access is "unbedingt erforderlich"
   (strictly indispensable) for a digital service the user *explicitly*
   requested. Interpret this narrowly:
   - **In:** session authentication cookies, shopping-cart state, CSRF tokens,
     login remember-me tokens (when the user explicitly opted in to "stay
     logged in"), load-balancer stickiness.
   - **Out:** analytics, heatmaps, A/B testing, personalisation, advertising,
     social media widgets, comfort features (e.g. remembered UI language when
     the site is available in only one language anyway), CDN fonts.

### § 9 Abs. 2 TDDDG — applicability check

§ 9 Abs. 2 TDDDG governs how **Anbieter eines Telekommunikationsdienstes**
(telecommunications service providers — ISPs, mobile operators, VoIP services,
etc.) may use subscriber-related traffic data for marketing or value-added
services. It is **not** the relevant provision for ordinary websites setting
cookies.

→ Check whether this project is itself a telecommunications service
(messaging app, VoIP, email/SMS gateway, etc.). If yes, § 9 Abs. 2 TDDDG
imposes additional consent requirements for traffic-data use, which must be
documented separately. If no, § 9 does not apply — state this clearly in the
Uncertainties report.

### Consent quality (Art. 4 Nr. 11 + Art. 7 DSGVO)

Valid consent for cookies must be:

| Requirement | What it means in practice |
|-------------|---------------------------|
| Prior | Non-essential cookies must NOT fire before the user acts |
| Freely given | Reject must be as easy as Accept — same visual prominence |
| Specific | Separate toggle per category (analytics ≠ marketing) |
| Informed | User knows what they consent to before clicking |
| Unambiguous | No pre-ticked boxes; no misleading design (dark patterns) |
| Revocable | Must be as easy to withdraw as to give; persistent link or icon |

German courts and supervisory authorities have repeatedly ruled that making
the "Accept" button large and green while hiding "Reject" in a small grey link
is an unlawful dark pattern (see LG Rostock 3 O 762/19; OLG Frankfurt 6 U
270/19; BGH I ZR 186/17).

### Cookie categories

Classify every finding:

| Category | Examples | Consent required? |
|----------|----------|-------------------|
| Essential | Session auth, CSRF token, cart | No (§ 25 Abs. 2 Nr. 2) |
| Functional | Language preference, UI theme | Usually yes |
| Analytics | GA4, Matomo, Plausible, Hotjar | Yes |
| Marketing | Meta Pixel, GTM ad tags, LinkedIn | Yes |

---

## Step 4 — Draft the cookie banner content

### Guiding principle: minimum viable banner

A good cookie banner is **short**. Users must be able to read and decide in
under 10 seconds. The full legal text belongs in the privacy policy — not the
banner. The banner needs: what you're doing (one sentence), why (one clause),
and clear equal-prominence buttons. Nothing else.

Do **not** include in the banner:
- Bullet lists of individual cookies
- Cookie durations or technical flags
- Explanations of what a cookie is
- Paragraph numbers or legal citations
- More than one paragraph of body text

If the codebase contains only strictly necessary cookies, there is no need for
a consent banner. State this clearly and explain why.

### Output format

Match the project's tech stack and reuse its existing components/classes (see
*Integration & styling* above — add no new CSS):
- Plain HTML → HTML snippet with minimal inline structure, no styling of its own
- React / Vue / Svelte → component in the project's style, using its primitives
- Static / Markdown → plain HTML snippet
- Unclear how to mount it → output the content with placement instructions and
  flag it in the Uncertainties report rather than guessing

### Banner content to produce

**1. Headline** — 3–6 words, direct, not legalistic.
- Good: "We use cookies" / "Cookies on this site"
- Bad: "Information regarding the use of cookies and similar technologies"

**2. Body text** — 1–3 sentences maximum. State which categories are present
and their purpose. No definition of cookies, no legalese.
- Good: "We use essential cookies to keep you logged in. With your consent we
  also use analytics cookies to understand how you use the site. You can accept
  all, decline optional cookies, or customize your choice."
- Bad: "Cookies are small files stored on your device. We and our partners may
  use these technologies to process personal information. More information can
  be found in our privacy policy..."

**3. Buttons** — use **first-person (Ich-Form)** phrasing. This formulation
makes the user's affirmative act explicit, reducing ambiguity about consent
(recommended by German supervisory authority guidance). All buttons must have
equal visual weight — achieve this by applying the **same existing button style
from the site** to every button, never a more prominent style for Accept.

| Situation | Accept label | Reject label | Customize |
|-----------|-------------|--------------|-----------|
| Any non-essential cookies | `I accept all cookies` | `I only accept essential cookies` | `Customize` (if 2+ categories) |
| Only one non-essential category | `I accept all cookies` | `I decline` | — |
| No non-essential cookies | — no banner needed — | | |

**4. Privacy policy link** — always include. Anchor text: `Privacy Policy`.
Placeholder: `[Privacy Policy URL]`.

**5. Preferences centre** — produce this whenever 2 or more non-essential
categories exist. Use the site's existing form controls (its checkbox/toggle
component or a plain `<input type="checkbox">`); add no new control styling.
Include:
- One toggle per category
- Category name + one-sentence description
- Default state for non-essential toggles: **off**
- Essential cookies toggle: always on, visually disabled, labeled
  "Always active"
- A "Save preferences" button in Ich-Form: `Save my preferences`

### Style rules

- Plain language; no legalese visible to users
- Active voice, second person in body text ("you"), first person on buttons
- Reject button is at least as prominent as Accept — reuse the site's existing
  button style for both; add no styling of your own
- No pre-ticked checkboxes anywhere
- No asterisks or fine print inside the banner
- `[Privacy Policy URL]` placeholder wherever the link appears

---

## Step 5 — Consent withdrawal implementation

Art. 7 Abs. 3 DSGVO requires that withdrawal of consent is **as easy as giving
it** and takes effect immediately. This means the implementation must not only
stop future tracking — it must actively remove any data already written to the
user's terminal equipment under the withdrawn consent.

### What to generate

Alongside the banner, produce a `clearConsentData(categories)` function (or
equivalent for the project's stack) that is called whenever the user declines
or downgrades their consent — i.e. on the "I only accept essential cookies" /
"I decline" button and on every "Save my preferences" action where a previously
accepted category is toggled off.

**Per-category cleanup must cover:**

1. **Cookies** — expire each non-essential cookie immediately:
   `document.cookie = "name=; max-age=0; path=/"`.
   Use every cookie name found in Step 2 for that category. Do not guess names.

2. **localStorage** — call `localStorage.removeItem(key)` for every key written
   under that category (from Step 2). Never use `localStorage.clear()` — this
   also wipes essential data such as the consent record itself.

3. **sessionStorage** — same pattern: `sessionStorage.removeItem(key)` per key,
   never `sessionStorage.clear()`.

4. **Third-party scripts** — where a script was injected dynamically (GA4, Meta
   Pixel, Hotjar, etc.), remove the `<script>` element from the DOM and nullify
   the global: `window.ga = undefined`, `window.fbq = undefined`, etc. Note:
   cookies already set by third-party scripts **on their own domain** cannot be
   deleted client-side — document this limitation in the Uncertainties report
   for every affected vendor.

5. **Consent preference record** — always update the stored consent record to
   reflect the new, narrower preference immediately. This record is itself
   strictly necessary (it prevents repeated re-consent prompts) and must never
   be cleared — only updated.

### Consent preference record

The banner must remember the user's decision so it does not reappear on every
page load. Use a minimal, strictly necessary record:

- **Key:** `cookieConsent` (or the site's existing key if found in Step 2)
- **Value:** JSON `{ essential: true, analytics: false, marketing: false, … }`
  — one boolean per category found in Step 2
- **Storage:** first-party cookie, `max-age=31536000` (1 year), `SameSite=Strict`,
  `Secure` if the site is HTTPS — no tracking value, no personal data
- This cookie is strictly necessary (records consent state) — do not list it as
  non-essential and do not delete it on withdrawal

### Persistent access to withdraw

Art. 7 Abs. 3 DSGVO requires a route back to the preferences that is as easy as
the original consent action. Produce one of:

- A persistent footer / navigation link labeled "Cookie settings" that re-opens
  the preferences centre
- A floating badge only if the site already uses that pattern

Never: a link buried in the privacy policy alone, or one that requires the user
to manually clear browser storage to reset the banner.

### Stack-specific patterns

| Stack | Cookie deletion | localStorage removal | Script teardown |
|-------|----------------|----------------------|----------------|
| Plain JS | `document.cookie = "name=; max-age=0; path=/"` | `localStorage.removeItem(key)` | `el.remove(); window.ga = undefined` |
| React | same in event handler or `useEffect` cleanup | same | remove via ref or `querySelector` |
| Next.js (App Router) | client cookies via `document.cookie`; HttpOnly via server endpoint | same | dynamic-import teardown or router event |
| Vue / Nuxt | same as plain JS equivalent | same | same |

**HttpOnly server-side cookies:** the client cannot delete them. If Step 2
found any non-essential HttpOnly cookies, the withdrawal handler must call a
server endpoint (e.g. `POST /api/consent { accepted: [] }`) that deletes those
cookies via `Set-Cookie: name=; max-age=0`. Flag this in the Uncertainties
report.

---

## Step 6 — Uncertainties report

Output this in chat after the banner:

```
## Uncertainties

| # | Item | Why uncertain | Recommendation |
|---|------|---------------|----------------|
```

Always flag:
- Whether German TDDDG / EU ePrivacy law applies at all, if jurisdiction could
  not be confirmed from the codebase
- Where the banner should be mounted, if the framework / injection point could
  not be determined (output content + placement instructions instead of guessing)
- Third-party scripts where cookie behaviour could not be confirmed from the
  code alone (note: verify in the provider's documentation or browser devtools)
- Any payment SDK — their cookie scope must be checked in their docs
- Any item where you were unsure whether § 25 Abs. 2 Nr. 2 (strictly
  necessary) applies
- Whether § 9 Abs. 2 TDDDG applies (state yes/no with one-sentence reason)
- "Cookieless" analytics tools that may still perform device fingerprinting
  (fingerprinting is covered by § 25 Abs. 1 TDDDG even without cookies)

Close with:

> **Notice:** This cookie banner was generated automatically based on a
> codebase scan. It does not constitute legal advice and should be reviewed by
> a lawyer before publishing.

---

## Quality checklist (internal — do not output)

- [ ] Disclaimer and placeholder list in chat before the banner output
- [ ] Jurisdiction confirmed, or flagged as uncertain before producing the banner
- [ ] No new CSS / `<style>` block — banner reuses the site's existing styles
- [ ] Every cookie / storage access from Step 2 is categorised
- [ ] Each non-essential item flagged as requiring consent
- [ ] Reject button has equal prominence to Accept in the output code
- [ ] No pre-ticked checkboxes in preferences centre
- [ ] All buttons in first-person (Ich-Form)
- [ ] Privacy policy link with `[Privacy Policy URL]` placeholder present
- [ ] Preferences centre included when 2+ non-essential categories exist
- [ ] Banner body is ≤ 3 sentences
- [ ] No technical cookie details in user-visible text
- [ ] `clearConsentData()` function produced, covering cookies + localStorage + sessionStorage per category from Step 2
- [ ] Consent preference record (strictly necessary cookie) produced with correct flags
- [ ] Persistent "Cookie settings" link / entry point to preferences included
- [ ] HttpOnly server-side cookies flagged in Uncertainties if present; server-endpoint deletion noted
- [ ] Third-party-domain cookies that cannot be client-deleted flagged per vendor in Uncertainties
- [ ] § 9 Abs. 2 TDDDG applicability addressed in Uncertainties
- [ ] Uncertainties table output in chat after the banner
- [ ] Final disclaimer repeated in chat after the banner
