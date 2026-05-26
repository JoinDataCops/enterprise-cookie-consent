# Enterprise cookie consent

If you're at the renewal table for an enterprise CMP this quarter, the question your auditor cares about isn't 'do we have a banner'. It's 'can we prove no cookie fired before consent and no CAPI call left the perimeter after withdrawal, on a per-event log a regulator can read'. That sentence is two clicks away from the September 2025 CNIL fine pages and is what most banner-only platforms can't deliver.

The fines tell the story. CNIL fined Google EUR 325M and Shein EUR 150M in September 2025 for the same failure pattern. Cookies set on arrival before consent. 'Refuse all' that didn't actually stop new cookies. Downstream reads after the user revoked consent. American Express Carte France EUR 1.5M for advertising cookies placed before consent and still read after withdrawal. None of those failures are banner failures. The banners rendered. The regulator wasn't fooled.

The enterprise market is also in active disruption. OneTrust raised its minimum ACV to about $10K/year for Q2 2026 and started migrating SMB cookie-only customers off the platform, not down. Cookiebot (Usercentrics) doubled base Premium pricing from EUR 15 to EUR 30 per domain per month in August 2025 and auto-upgraded existing 1-3 domain accounts to a Medium tier. Didomi acquired Sourcepoint and Addingwell, backed by Marlin Equity's $83M majority stake. So an unusual share of mid-market and enterprise CMP buyers are at the renewal table this year, looking at higher prices and consolidated vendors with a fining environment that just doubled in seriousness.

The gap on the SERP: top-ranking pages are vendor PLPs comparing banner UIs. None of them frame consent as a three-layer problem. None of them give buyers the seven failure modes auditors actually check.

This post is the layered model. The seven failure modes. The honest field of vendors. And the renewal-table question worth asking.

---

## Quick stuff people keep asking

**What is enterprise cookie consent?** The end-to-end pipeline: a banner UI plus tag-firing enforcement in the browser plus server-side enforcement on outbound CAPI/S2S calls plus a per-event audit log. Most platforms ship layer 1 only.

**Is cookie consent legally required?** In the EU under GDPR/ePrivacy, yes for non-essential cookies. In California under CCPA/CPRA, opt-out signals are required. In Brazil under LGPD, similar opt-in posture. UK PECR mirrors GDPR for cookies. As of 2026, privacy regulations cover roughly 80% of the global population.

**Do I need cookie consent for CCPA?** CCPA is opt-out (Do Not Sell/Share). The cookie banner does need to surface that mechanism, and CCPA-only sites still need a Global Privacy Control signal handler. So functionally yes, the implementation is just different from GDPR opt-in.

**What is Google Consent Mode v2?** A protocol for telling Google's tags whether users have granted consent to ad and analytics storage. Tags adjust behavior based on the consent state. Without v2, Google Ads Smart Bidding and remarketing degrade for EU traffic.

**What happens if I don't have cookie consent?** GDPR fines totalled around EUR 2.92B in 2024. CNIL alone issued 83 sanctions worth ~EUR 486.8M in 2025, with cookie/ePrivacy violations a primary target. The 2025 reset suggests enforcement is now a budget line item, not a hypothetical risk.

---

## The three-layer model

Most enterprise CMP procurement evaluates layer 1 only. The 2025 fines targeted layers 2 and 3.

**Layer 1: the banner UI.** The user-facing dialog that captures consent state. This is where every CMP shines because it's the visible artifact. CookieYes, Cookiebot, OneTrust, Termly, Osano, Usercentrics all do this competently.

**Layer 2: tag-firing enforcement in the browser.** When consent is denied, do tags actually not fire? GTM has a consent state mechanism, GA4 has a Consent Mode v2 mode, Meta Pixel has a consent-aware mode. But all three rely on the page implementing them correctly. If your tag manager is misconfigured, the banner says 'denied' and the tag still fires. That's the SHEIN pattern.

**Layer 3: server-side enforcement on CAPI/S2S egress.** When a user denies consent, do server-side calls to Meta CAPI, Google Ads CAPI, TikTok Events API, LinkedIn Insight CAPI also stop? Most CMPs don't even know server-side calls exist. They live on the front end. So a user clicks Reject, the banner state updates, GTM honors it, the GA4 tag suppresses. And the marketing team's standalone server-side CAPI integration, sitting in their CDP or sGTM, fires anyway because nothing told it to stop.

The Google EUR 325M fine and the Shein EUR 150M fine were not 'we didn't have a banner'. They were 'the banner state didn't propagate through the entire data egress pipeline'. Layer 3 problems.

---

## The seven failure modes auditors actually check

The pattern across CNIL decisions and the broader 2025-2026 audit findings is consistent. The seven failure modes you should be checking your own stack against:

1. **Banner not blocking.** The banner renders but cookies set on arrival anyway, before any user interaction. Direct violation. Shein pattern.

2. **Tags fire pre-consent.** GTM containers, ad pixels, analytics tags execute before the user grants consent. Common when third-party tags are added to the page header without consent gating.

3. **CAPI bypass.** Server-side conversion APIs (Meta, Google Ads, TikTok, LinkedIn) fire on every event regardless of front-end consent state. The front end says 'no analytics' and the backend pushes the conversion anyway.

4. **Multi-domain drift.** Consent state is captured on www.brand.com and the same user later visits brand.shop or eu.brand.com without the consent state propagating. Different domain, different consent state, regulator reads it as separate non-consented data collection.

5. **Regional mis-routing.** EU traffic served by a US-region datacenter, log data crossing borders without an SCC or adequacy decision. The CMP renders EU-appropriate UI but the data lives elsewhere.

6. **No audit log.** When a regulator asks 'show me consent state for user X at event Y', the platform can show the banner state at the session level but cannot show per-event proof of which tags ran and which CAPI calls left the perimeter. This is increasingly the failure mode CNIL flags.

7. **No signal validation.** Bots and headless traffic create consent records too. Without bot filtering on the consent layer, fraudulent traffic generates apparent 'consent given' records that pollute the audit log. When the regulator audits a window of 'consent given' events, half were bots.

A banner-only CMP can address #1 and partially #2. The other five require infrastructure beyond the banner.

---

## The vendor field, honestly

**1. OneTrust**

The Good: The enterprise default. Procurement teams recognize the name. Mature audit features. Wide regulatory coverage (GDPR, CCPA, LGPD, PIPL, etc.). Strong DSAR module.

Frustrations: Minimum ACV raised to about $10K/year effective Q2 2026. SMB cookie-only customers being migrated off, not down. Implementation famously slow, often 6 to 12 weeks before green dashboards. Banner-and-policy first product; layer 3 (CAPI egress enforcement) is mostly outside scope.

Wish List: A real mid-market product. Faster implementation. Native CAPI egress enforcement.

Value for Money: **6.5/10.** Right answer for a regulated $1B+ enterprise where procurement-grade name recognition matters. Wrong answer for almost everyone else in 2026.

Pricing: ~$10K/year minimum ACV from Q2 2026.

---

**2. Cookiebot (Usercentrics)**

The Good: TCF 2.2 certified. Strong consent scanning. Mature Google partnership. Multi-language support.

Frustrations: Premium base pricing doubled from EUR 15 to EUR 30/domain/month in August 2025. Auto-upgraded existing 1-3 domain accounts to Medium. Per-domain pricing scales harshly for multi-site enterprises. Banner-only category, doesn't natively enforce CAPI/S2S egress.

Wish List: Bundle multi-domain pricing. Layer 3 enforcement.

Value for Money: **6/10.** Good banner. The 2025 price hike turned it from a fair deal into a renewal-table question.

Pricing: From EUR 30/domain/month for Premium after August 2025.

---

**3. Didomi (now including Sourcepoint and Addingwell)**

The Good: Strong publisher and ad-tech footprint. TCF 2.2 certified. The Sourcepoint acquisition added enterprise publisher capabilities. The Addingwell acquisition added sGTM hosting. Theoretically the most integrated CMP+sGTM combo in 2026.

Frustrations: Three product lines under one roof, with consolidation friction. PE-backed (Marlin Equity, $83M majority stake), so customers should expect aggressive monetization. Roadmap clarity is a fair question to ask in any procurement conversation.

Wish List: Clearer product unification. Stable pricing posture.

Value for Money: **7/10** for publishers. **6.5/10** for general enterprise.

Pricing: Quote-based, mid-market and up.

---

**4. Osano**

The Good: Compliance-first brand. Generous free tier for SMB. Strong DSAR and data discovery features. Reasonable enterprise pricing relative to OneTrust.

Frustrations: Banner-and-policy category, doesn't natively enforce CAPI/S2S egress. Layer 3 not in scope by design.

Wish List: Server-side enforcement.

Value for Money: **7/10.** Good if compliance reporting and DSAR are the primary lens.

Pricing: Free tier; paid plans starting around $99/mo and climbing into mid-market.

---

**5. Termly**

The Good: Friendly UX. Combined CMP plus policy generator. Good mid-market price point.

Frustrations: Smaller IAB and enterprise footprint. Not the answer for a regulated multi-region enterprise.

Wish List: Layer 3 enforcement. Bigger compliance pedigree.

Value for Money: **6/10** for mid-market. **5/10** for true enterprise.

Pricing: Tiered, mid-market plans in the $30 to $200/mo range.

---

**6. Securiti.ai**

The Good: Broader privacy platform with consent as one component. AI-driven data discovery. Strong for regulated enterprises that want a unified privacy stack.

Frustrations: Heavyweight platform. Implementation complexity. Pricing in the enterprise tier.

Wish List: A focused CMP product without the rest of the platform.

Value for Money: **7/10** for enterprises that want the broader platform.

Pricing: Enterprise, quote-based.

---

**7. iubenda**

The Good: Strong policy and CMP combo. Friendly DIY-to-enterprise ramp. Italian compliance pedigree.

Frustrations: Banner-and-policy category. Layer 3 not in scope.

Wish List: Server-side enforcement.

Value for Money: **6.5/10** for SMB and mid-market.

Pricing: Tiered, accessible.

---

**8. Quantcast Choice**

The Good: TCF 2.2 certified, IAB founding member, free for many publishers.

Frustrations: Publisher-tilted. Less suited for non-ad-tech enterprises.

Wish List: Broader enterprise positioning.

Value for Money: **6.5/10** for publishers, lower outside that bracket.

Pricing: Free for many cases.

---

**9. DataCops**

The Good: First-party CMP runs on a CNAME on your own subdomain (datacops.yourdomain.com), so consent state lives on first-party storage that survives ITP and ad blockers. Bundled with first-party analytics, server-side Meta and Google CAPI with consent-state enforcement at the egress layer (so denied consent stops the matching CAPI call from leaving the perimeter), and bot filtering so bot-generated consent records don't pollute the audit log. Per-event log captures consent state to tag decision to egress decision. TCF 2.2 certified. Multi-domain included on paid tiers, billed flat. Setup is one script tag and one CNAME, live in 5 to 30 minutes.

Frustrations: Brand new compared to OneTrust or Cookiebot. SOC 2 Type II is in progress, not active. Google Consent Mode v2 certification is in progress on the certification track. SSO/SAML is planned, not active. Fewer pre-built one-click CMS integrations than enterprise CDPs. White-label is on the Talk-to-Sales tier, not on Growth or Business.

Wish List: SOC 2 finished. The DSAR API plus downstream deletion to Meta and Google (currently on the planned roadmap, honestly disclosed). SSO/SAML active.

Value for Money: **8.5/10** for mid-market and enterprise buyers who want layers 1+2+3 plus a per-event audit log on one bill. The honesty about what's in progress versus active is itself a procurement-grade differentiator (most enterprise CMP vendors imply certifications they haven't earned yet).

Pricing: Enterprise tier is Talk to Sales, includes single-tenant isolated runtime, dedicated IP reputation database, custom DPA, EU/US data residency, HubSpot integration, migration engineer, 99.9% uptime SLA. Growth $7.99/mo and Business $49/mo cover mid-market with multi-domain bundled.

---

## So what should you actually use?

Want the procurement-grade enterprise nameplate, willing to spend ~$10K/year minimum and 6-12 weeks of implementation? OneTrust.

Want a banner-only CMP plus policy generator at mid-market price, comfortable with banner being layer 1 only? Cookiebot, Osano, Termly, iubenda.

Want a publisher-strong CMP with sGTM under the same roof? Didomi, with the Sourcepoint and Addingwell capabilities factored in.

Want a unified privacy platform where CMP is one module among many? Securiti.ai.

Want the layer 3 enforcement (CAPI/S2S egress with consent state) plus the per-event audit log a regulator can read, at SMB and mid-market pricing, on a CNAME you control? DataCops.

Want a free-tier publisher CMP for ad-tech use cases? Quantcast Choice.

---

## The mistake I see people make

They treat enterprise CMP procurement as a 'pick the banner' exercise. They tab between OneTrust, Cookiebot, and Osano comparing UI customization options. Then a year later their auditor asks for per-event proof that no CAPI call left the perimeter for users who denied consent, and the platform can show banner state but not egress state. Fine.

The second mistake: assuming Google Consent Mode v2 is a server-side problem. It isn't. Consent Mode v2 is a front-end signaling protocol. Whether your server-side stack honors the signal depends on whether your sGTM, your CDP, or your standalone CAPI integration was wired up to read the consent flag and gate the call. None of that is the CMP's job natively.

The third mistake: ignoring bot-generated consent. Without filtering bots out of the consent layer, a meaningful share of 'consent given' records in your audit log are bots. When the regulator audits a window of consent records, half don't represent real humans. That's a layer-1 cleanliness problem that requires bot detection, not a banner problem.

---

## Now your turn

Which layer is your current CMP missing? Layer 1 banner state, layer 2 tag enforcement, or layer 3 CAPI egress enforcement? And does your audit log give you per-event proof, or session-level state? The honest answer is usually 'we have layer 1 and partial layer 2, no layer 3, and our audit log is session-level'. Which is fine until a regulator asks for the layer 3 data and the per-event proof. Drop where you are and what's at the renewal table this quarter.

---

Research by [DataCops](https://www.joindatacops.com) — first-party tracking, consent infrastructure, fraud prevention, and server-side CAPI for Meta, Google, TikTok, and LinkedIn.
