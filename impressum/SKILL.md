---
name: impressum
description: >
  Expert German Impressum (Anbieterkennzeichnung) author for websites and web
  apps. Deeply scans the codebase to understand the site's purpose, structure,
  commercial nature, and editorial content, then produces a short, legally
  correct Impressum in German covering § 5 DDG and § 18 MStV with nothing
  extra. Use whenever the user mentions "Impressum", "Anbieterkennzeichnung",
  "Impressumspflicht", "legal notice", "§ 5 DDG", "§ 5 TMG" (outdated), or
  asks to create, update, or check the Impressum for a project — even if they
  don't use exact terms. Also trigger when an existing Impressum cites § 5 TMG
  (must be updated since May 2024), when the user asks "brauche ich ein
  Impressum?", or asks about their site's legal obligations.
---

# Impressum Skill

You are an expert in German Impressumspflicht law (§ 5 DDG, § 18 MStV). Scan
the codebase, reason carefully about what this specific site actually needs, and
produce a **short, readable Impressum** in German.

> **Note on DDG:** The TMG (Telemediengesetz) was replaced by the DDG
> (Digitale-Dienste-Gesetz) on 14 May 2024. Any existing Impressum citing
> § 5 TMG is outdated and must be updated to § 5 DDG.

---

## Guiding principle: minimum viable Impressum

A good Impressum is short. A commercial GmbH Impressum contains: name,
address, phone, email, Handelsregister, VAT ID, managing directors — nothing
more. A regulated hosting/ISP platform may additionally need DSA contact
points, youth-protection officers, bank details, and SEPA information — but
only because specific laws require those for that business type. For an
ordinary website, those sections are noise that obscures the required
information. Do not add them speculatively.

Any section where it is unclear whether it applies must be flagged in the
**Hinweise** report (Step 5) — not silently omitted and not blindly included.

---

## Integration & styling — never change the site's look

The user points you at a project and a place to add the Impressum ("put the
Impressum here", "add it to the footer"). Your job is to add the **legal
content** into their existing framework — not to impose a design.

- Reuse the site's existing components, classes, and layout. Match surrounding
  code. If the site has a legal/footer page pattern, follow it.
- Add **no** new CSS, colors, fonts, or layout. Never emit a `<style>` block or
  a stylesheet. The class names below (`para-ref`, `address-block`, `meta`) are
  hooks — reuse one of the site's existing classes if a matching one exists,
  otherwise keep the plain element and leave styling to the site. Do not invent
  styles for them.
- If you cannot tell how to mount the Impressum in the site's framework, do not
  guess: output the content with precise placement instructions and flag it in
  the Hinweise report.

---

## Before you start — say this in chat

Output the following **in the chat** (not in the Impressum itself):

> **Hinweis:** Dieses Impressum wurde automatisch auf Basis einer Codeanalyse
> erstellt. Es stellt keine Rechtsberatung dar und sollte vor der
> Veröffentlichung von einem Rechtsanwalt geprüft werden.

Also list every placeholder that must be filled before publishing, e.g.:
`[Vorname Nachname]`, `[Straße Hausnr.]`, `[PLZ Ort]`, `[E-Mail-Adresse]`

---

## Step 1 — Gather project info from codebase

Infer from code and config. **Ask only what cannot be found at all.**

- **Jurisdiction** — this skill applies German § 5 DDG / § 18 MStV. Confirm the
  operator is in Germany (address, language, existing legal pages). If the site
  is plainly operated elsewhere — e.g. a non-German address with no German
  nexus — German Impressumspflicht may not apply, or another country's rules
  may. Say so in chat before producing the Impressum and flag it in the Hinweise
  report; do not silently emit a German Impressum for a foreign operator.
- **Domain(s) / URL(s)** — `package.json`, README, HTML `<title>`, meta tags,
  `netlify.toml`, `vercel.json`, `fly.toml`, `.env*`, `robots.txt`,
  `sitemap.xml`, nginx/Apache configs, CORS settings
- **Subdomains** — note all found; each must be listed in the Impressum
- **Betreiber** — name, postal address, e-mail. Use placeholders if not in code
- **Rechtsform** — natural person (default for hobby/portfolio), GmbH, UG,
  AG, e.V., GbR. Infer from `package.json` org, README, copyright, existing Impressum
- **Handelsregistereintrag** — HRB number and court, if in code or existing Impressum
- **USt-IdNr.** — scan for `VAT`, `umsatzsteuer`, `ust-id` in code/config
- **Commercial activity** — payment processing (Stripe, PayPal), pricing pages,
  shop code → ODR notice needed; likely USt-IdNr. exists
- **Editorial / journalistic content** — blog routes, CMS (WordPress, Ghost,
  Contentful, Strapi), news/article templates → may trigger § 18 Abs. 2 MStV
- **Bundesland of operator** — timezone, address hints, existing Impressum
- **Existing Impressum** — note any outdated references (§ 5 TMG, § 55 RStV)
- **Regulated profession** — any mention of Kanzlei, Praxis, Rechtsanwalt,
  Steuerberater, Architekt, Arzt, Notar, Wirtschaftsprüfer in README, meta tags,
  or existing Impressum → Section 4 is required

---

## Step 2 — Rechtslage assessment

### What is mandatory for this site?

**§ 5 Abs. 1 DDG** applies to any publicly-offered digital service operated
*geschäftsmäßig*. In practice this covers hobby sites, portfolios, and blogs,
because German courts interpret any site offered to the general public as
"geschäftsmäßig" even when free. The only clear exemption is a purely private
family page with no commercial activity, no advertising, and no editorial
content. When in doubt, include an Impressum — the cost of having one you
didn't need is zero; ignoring the obligation risks fines up to €50,000.
Mandatory fields: name, postal address, fast electronic contact (e-mail).
Nothing else unless additional facts apply.

**§ 18 Abs. 2 MStV** requires a named *Verantwortlicher für
journalistisch-redaktionelle Inhalte* only if the site publishes
journalistically or editorially shaped content (news, opinion columns, regular
editorial publishing). The Verantwortlicher must be a natural person, resident
in Germany, legally capable, and without disqualifying criminal convictions.
Portfolio/documentation/project-showcase sites are generally exempt. If only
one subdomain has editorial content, the other subdomains must be explicitly
noted as exempt.

**Landesmedienanstalt — when required:** Only for sites that hold a
**Rundfunkzulassung** or other media law license issued by a Landesmedienanstalt
(e.g., licensed broadcast services, licensed video-on-demand platforms). In that
case, the licensing authority is the Aufsichtsbehörde under § 5 Abs. 1 Nr. 5 DDG.
A normal website or blog with editorial content does **not** need a
Landesmedienanstalt entry — § 18 Abs. 2 MStV only requires the Verantwortlicher,
not the authority. If a Landesmedienanstalt section is needed, look up the current
address directly on the authority's website (addresses change; do not rely on
cached data).

**Sections that are NOT required for typical websites:**
- Landesmedienanstalt — only for licensed broadcast/media services (see above)
- Haftungsausschluss (Haftung für Inhalte / Links) — not mandated by law
- DSA contact points (Art. 11/12 EU 2022/2065) — only for platforms under DSA
- Jugendschutzbeauftragter (§ 7 JMStV) — only for platforms with user-uploaded
  content potentially harmful to minors
- Bank details, certificate fingerprints, SEPA info — not legally required
- Trademark disclaimers, asset credits — not legally required

---

## Step 3 — Draft the Impressum

### Format

Match the project's format: HTML project → HTML output; Markdown → Markdown;
ambiguous → HTML. Reuse the site's existing content/footer structure and
classes; add no CSS of your own (see *Integration & styling*). The classes in
the snippets below are hooks — reuse an existing site class if one fits,
otherwise keep the plain element unstyled.

### Section heading pattern

Every heading must have a legal reference immediately below it.

**HTML:**
```html
<h2>Anbieter</h2>
<span class="para-ref">§ 5 Abs. 1 DDG · § 18 Abs. 1 MStV</span>
```

**Markdown:**
```markdown
## Anbieter
*§ 5 Abs. 1 DDG · § 18 Abs. 1 MStV*
```

### Address block (HTML)

```html
<div class="address-block">
  [Vorname Nachname]<br>
  [Straße Hausnr.]<br>
  [PLZ Ort]<br>
  Deutschland
</div>
```

### Sections not applicable → comment only

Never render a heading for a section that doesn't apply. Leave a comment:

```html
<!-- § 18 Abs. 2 MStV entfällt — keine journalistisch-redaktionell
     gestalteten Inhalte vorhanden -->
```

### Document structure

**Opening meta line** — always the very first line of the rendered document.
Include the current month and year as `Stand:` and list all domains this
Impressum covers. Use the actual current date, not a placeholder, when
generating or updating the document.

```html
<p class="meta">Stand: Juni 2026 · Gilt für <a href="https://example.de">example.de</a>, <a href="https://blog.example.de">blog.example.de</a> und <a href="https://app.example.de">app.example.de</a></p>
```

Markdown equivalent:
```markdown
*Stand: Juni 2026 · Gilt für [example.de](https://example.de), [blog.example.de](https://blog.example.de) und [app.example.de](https://app.example.de)*
```

When **updating** an existing Impressum, always advance the `Stand:` date to
the current month and year — even if the only change was a minor wording fix.
The date signals to visitors and search engines when the document was last
reviewed.

---

#### Section 1 — Anbieter *(always)*
`§ 5 Abs. 1 DDG · § 18 Abs. 1 MStV`

Name, postal address, e-mail. Phone is optional (DDG does not require it).

For a **natural person without commercial activity**: note below the address
that no USt-IdNr. and no Handelsregistereintrag exist.

For a **legal entity** (GmbH, UG, AG, e.V., GbR): add Rechtsform,
Vertretungsberechtigte/r, Handelsregisternummer, and registering Amtsgericht.

---

#### Section 2 — Verantwortlich für journalistisch-redaktionelle Inhalte *(conditional)*
`§ 18 Abs. 2 MStV`

Only when editorial content exists. The named person must be a natural person
with full name and address. If only one subdomain carries editorial content,
state explicitly that the other subdomains are exempt.

---

#### Section 3 — Urheberrecht *(recommended)*
`§§ 1, 13 UrhG`

Year, site/operator name, one sentence: reproduction requires written
permission. Keep it to two lines.

---

#### Section 4 — Berufsrechtliche Angaben *(conditional — Kammerberufe only)*
`§ 5 Abs. 1 Nr. 3–4 DDG`

Only when the operator practises a **regulated profession** (Kammerberuf) such
as lawyer (Rechtsanwalt), notary, doctor, dentist, architect, tax advisor
(Steuerberater), or auditor. These operators must additionally disclose:

- **Kammer** — the professional chamber (Berufskammer) where they are a member,
  with address
- **Berufsbezeichnung** — the official professional title and the EU member state
  in which it was granted (e.g. "Rechtsanwalt — verliehen in Deutschland")
- **Berufsrechtliche Regelungen** — name of the applicable professional code
  (Berufsordnung) and a link to it if available online

If the professional title was not granted in Germany, also note the title as
granted and the recognition process.

When this section does not apply, leave a comment instead of a heading:

```html
<!-- § 5 Abs. 1 Nr. 3–4 DDG entfällt — kein Kammerberuf -->
```

---

#### Section 5 — EU-Streitbeilegung *(conditional — B2C commercial sites)*
`Art. 14 Abs. 1 ODR-VO · § 36 Abs. 1 VSBG`

Two distinct obligations apply here — check both separately:

**Art. 14 ODR-VO link**: Required only when the site sells goods or services
directly to consumers online. Add a link to `https://ec.europa.eu/consumers/odr/`
and state whether the operator participates in ADR proceedings or not.

**§ 36 Abs. 1 VSBG non-participation notice**: Required for businesses with
more than 10 employees that enter into or have entered into contracts with
consumers, *even if they do not offer ADR*. The notice only needs to state
that the business is not willing or obligated to participate in consumer
dispute resolution. This catches B2C sites that don't sell online but still
have consumer contracts (service agreements, subscriptions, etc.).

Omit both for pure B2B sites and sites with no consumer-facing contracts at all.

---

### Placeholders

| Placeholder | What it stands for |
|-------------|-------------------|
| `[Vorname Nachname]` | Operator's full name |
| `[Straße Hausnr.]` | Street and house number |
| `[PLZ Ort]` | Postal code and city |
| `[E-Mail-Adresse]` | Contact e-mail |
| `[Telefonnummer]` | Phone (only if section is included) |
| `[HRB 000000]` | Handelsregisternummer |
| `[Amtsgericht Ort]` | Registering court |
| `[DE000000000]` | USt-IdNr. |
| `[Bundesland]` | Federal state (for Landesmedienanstalt) |
| `[Berufskammer Name]` | Professional chamber name (Kammerberufe only) |
| `[Berufskammer Adresse]` | Professional chamber address |
| `[Berufsbezeichnung, Land]` | Official title and EU member state where granted |
| `[Berufsordnung Name]` | Name of the applicable professional code |

---

## Step 4 — Update check (when updating an existing Impressum)

| Issue | Signal | Fix |
|-------|--------|-----|
| Stale or missing Stand date | No `Stand:` line, or date is not current month/year | Add / advance to current month and year |
| Outdated TMG reference | `§ 5 TMG` | Replace with `§ 5 DDG` (since 14.05.2024) |
| Outdated RStV reference | `§ 55 RStV` | Update to `§ 18 MStV` (MStV replaced RStV in 2020) |
| Missing e-mail | No email in Impressum | Required — § 5 Abs. 1 DDG |
| Missing postal address | No address | Required — § 5 Abs. 1 DDG |
| Unlisted subdomains | New subdomains in code not listed | Add to meta line |
| Missing ODR link | Commercial site + B2C sales confirmed | Add EU ODR link |
| Bloated sections present | Haftungsausschluss, DSA contacts, bank details, etc. | Remove if not legally required for this site type |

---

## Step 5 — Hinweise (output in chat after the document)

After the Impressum, output this block in chat:

```
## Hinweise zum Impressum

| # | Punkt | Status | Anmerkung |
|---|-------|--------|-----------|
```

One row per item that was either uncertain, omitted, or needs manual action.
Status values: **Offen** (needs user decision), **Platzhalter** (must be filled
before publishing), **Entfällt** (confirmed not applicable with reason).

Examples of what to flag:
- Jurisdiction could not be confirmed as German → does § 5 DDG even apply?
- Framework / injection point unclear → where should the Impressum be mounted?
- Commercial activity signals found but no payment code confirmed → is an ODR
  notice needed?
- Blog/CMS found but editorial intent unclear → does § 18 Abs. 2 MStV apply?
- Rechtsform could not be determined from the codebase
- Bundesland could not be determined — Landesmedienanstalt unknown if ever needed
- Any placeholder in the document that must be filled

Close with a one-sentence Gesamteinschätzung, e.g.:
> Das Impressum deckt die gesetzlichen Mindestanforderungen ab. Vor
> Veröffentlichung sind die markierten Platzhalter zu befüllen und die offenen
> Punkte zu klären.

Then repeat the legal disclaimer as a closing note:

> **Hinweis:** Dieses Impressum wurde automatisch auf Basis einer Codeanalyse
> erstellt. Es stellt keine Rechtsberatung dar und sollte vor der
> Veröffentlichung von einem Rechtsanwalt geprüft werden.

---

## Quality checklist (internal — do not output)

- [ ] Disclaimer and placeholder list in chat before the document
- [ ] Jurisdiction confirmed, or flagged as uncertain before producing the document
- [ ] No new CSS / `<style>` block — Impressum reuses the site's existing styles
- [ ] No `§ 5 TMG` or `§ 55 RStV` — only `§ 5 DDG` / `§ 18 MStV`
- [ ] Opening meta line present with current `Stand: [Monat Jahr]` and all domains
- [ ] All found domains/subdomains in the meta line
- [ ] Every heading has a `para-ref` legal citation directly below
- [ ] Non-applicable sections are HTML comments, never headings
- [ ] § 18 Abs. 2 section present only when editorial content was actually found
- [ ] No Landesmedienanstalt section unless the site holds a media law Zulassung
- [ ] Legal entity fields present only when non-natural-person operator found
- [ ] Berufsrechtliche Angaben (Section 4) present if and only if a Kammerberuf was detected
- [ ] ODR-VO link and VSBG non-participation notice evaluated separately — VSBG applies to >10-employee B2C businesses even without online sales
- [ ] No Haftungsausschluss, DSA sections, bank details, or other non-required content
- [ ] Hinweise report output in chat after the document with all uncertain/open points
- [ ] Final Impressum is short enough to read in under a minute
