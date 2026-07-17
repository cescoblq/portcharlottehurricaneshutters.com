# TODO before portcharlottehurricaneshutters.com goes live

Site builds clean (`hugo --minify`, 8 content pages + 404 = 10 pages total, 0 errors —
only harmless Hugo deprecation warnings about `languageCode`/`.Site.Data`, same as the
other US network sites). Theme (`airspace-hugo`) ships its own Sass/JS asset pipeline
(Bootstrap + Slick + Shuffle + Google Maps) meant for a portfolio/agency site — not
needed here, so this site bypasses it: `layouts/_default/baseof.html` links static
copies of only Bootstrap + Font Awesome CSS/JS (copied into `static/plugins/`, no Hugo
asset-pipeline/Sass dependency), plus a small custom stylesheet at `static/css/site.css`.
If `themes/airspace-hugo` is ever updated, no action needed on this decoupling.

Everything below is what's still missing before this can actually receive and pay for traffic.

## 1. Turnstile widget (anti-bot)
- Create the Turnstile widget for `portcharlottehurricaneshutters.com` at dash.cloudflare.com → Turnstile.
- Replace `TURNSTILE_SITEKEY_TODO` in `layouts/partials/lead-form.html` (`data-sitekey` attribute) with the real site key.
- Deploy the managed siteverify Cloudflare Worker (same pattern as the other US/ES/NL sites — see the `turnstile-spin` skill) and replace `TURNSTILE_WORKER_URL_TODO` in the same file's `<script>` block with the deployed Worker URL.

## 2. TrustedForm (ActiveProspect) — TCPA consent certificate
- **The ActiveProspect account does not exist yet.** François needs to sign up at activeprospect.com.
- Once created, paste the standard adapter snippet where marked in `layouts/partials/lead-form.html` (clearly commented, above the form). It auto-populates the hidden `xxTrustedFormCertUrl` field already present in the form.
- Until this is done, that hidden field will always submit empty — most lead buyers (78% per `recherche/53-carto-debouches-usa.md` §4) require this certificate before accepting/paying for a lead, so **this blocks monetization**, not just the pilot build.
- Budget ~$0.30-0.55/lead once active (see same source).

## 3. n8n webhook
- Webhook `niches-leads-hurricane-shutters-portcharlotte-us` does **not exist yet** in n8n (byteblast VPS). Create it, matching the field names sent by `functions/api/submit.js` → the lead form.
- Route to the same Google Sheet pattern used by other niches (1 tab per niche) or a new tab — François's call.

## 4. DNS + hosting
- Domain `portcharlottehurricaneshutters.com` still needs to be pointed to Cloudflare (nameserver change at the registrar, then zone setup — same API procedure as documented in `project-cloudflare-automatisation-dns.md`).
- Create the Cloudflare Pages project, connect it to this site's git repo (not yet created — see §6), set build command `hugo --minify` and pin Hugo v0.164.0 extended (or newer) via `HUGO_VERSION` env var. No Hugo Modules/Go toolchain needed for this site (unlike venicehurricaneshutters.com) — the theme's Sass pipeline was deliberately bypassed, see note above.

## 5. Debouché / lead buyer confirmation — NOT YET DONE
Per `recherche/53-carto-debouches-usa.md`: no acheteur a confirmé publiquement la couverture hurricane shutters en Floride. Priority contacts (not yet made):
1. Inquirly (inquirly.com/affiliates)
2. 33 Mile Radius (1-888-594-8381)
3. Fallback: direct sale to already-ranked local PMEs if no marketplace confirms coverage.

## 6. Git repo
- No GitHub repo created for this site yet. `git init` was run locally (1 commit) but no remote exists — create `cescoblq/portcharlottehurricaneshutters-com` (or similar) when ready to deploy.

## 7. Photography
- `static/images/lead/placeholder-hurricane-shutters.svg` is a text-labeled placeholder graphic, not a real project photo — swap for a real (licensed/stock) hurricane shutters installation photo before launch.

## 8. Content review
- All regulatory/program claims (Wind-Borne Debris Region, My Safe Florida Home eligibility, insurance credit percentages, Hurricane Ian statistics, Charlotte County permit rules) are sourced in `data/citypack.json` but were written in July 2026 — MSFH funding/eligibility rules change between cycles, so **re-verify current MSFH status before launch** if more than a few weeks pass before going live.

## 9. Scope discipline vs. venicehurricaneshutters.com
- This site is deliberately Port Charlotte/Charlotte County-anchored (hurricane shutters only, no impact-windows promotion — local search volume for impact windows is near-zero here). It does not duplicate or overlap venicehurricaneshutters.com's Venice/Sarasota County content or angle; keep it that way in any future content edits.
