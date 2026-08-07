# Project: Zeqian Yu — Personal Academic Website

Context and decisions log so any future session can continue without re-deriving everything.
Last updated: 2026-07-04.

**Status: LIVE.** The site is deployed and reachable at https://zeqianyu.com (secure, padlock). Hosting on GitHub Pages, fronted by the Cloudflare proxy, with Cloudflare Web Analytics running.

## Owner & goal

- Zeqian Yu (余泽谦), master's student in economics.
- Currently: Master in Economics (Empirical Applications and Policies), UPV/EHU (Bilbao).
- Incoming 2026–2028: MRes in Economic Analysis, Universidad Carlos III de Madrid (UC3M) — path to PhD.
- Career target: PhD → professor or central-bank researcher. The site is aimed at an **academic** audience.
- Contact email on site: zeqian.yu@alumnos.uc3m.es. Personal email: ninjazack@duck.com. Cloudflare/GitHub email: Zeqianyu@gmail.com.

## Repository / folders

- **Website repo (this folder):** `/Users/zeqianyu/Documents/Personal Website` — the git repo, pushed to GitHub.
- **CV sources (archive):** `/Users/zeqianyu/Desktop/CV` — LaTeX `.tex` + rendered `.pdf`, dated filenames (e.g. `CV_Zeqian_Yu_edu_20260704.tex`). Master copy of the CV lives here.
- **Certificates:** `/Users/zeqianyu/Desktop/Certifications` — PDFs of training/workshop certificates. These are the SOURCE OF TRUTH for the CV's workshops section; do not invent workshop content, read the certs.

## Site structure

- `index.html` — Home. Hero: circular portrait (`assets/profile_picture.jpg`), name "Zeqian Yu", Chinese name 余泽谦, a small "Welcome" heading, a two-sentence intro, interest chips, and a single "Contact" button (mailto). Browser tab `<title>` is just "Zeqian Yu" (user removed the earlier "| Economist"). Research/CV pages use "Research | Zeqian Yu" / "CV | Zeqian Yu".
- `research.html` — Research. Plain academic listing: "Research" heading → "Working papers" section → one entry (the HSR/EV thesis). Built to grow as publications arrive (copy the `<article class="pub">` block).
- `cv.html` — CV. "CV" heading + a download banner linking `files/Zeqian_Yu_CV.pdf` + an education timeline.
- `styles.css` — the whole design system (see below).
- `assets/` — `favicon.svg`, `profile_picture.jpg` (and a large original PNG, gitignored).
- `files/Zeqian_Yu_CV.pdf` — the published CV. **Stable filename on purpose** so the site link never changes; update by overwriting this file.
- `CNAME` — contains `zeqianyu.com`.
- `.nojekyll` — present, so GitHub serves files as-is (no Jekyll). `.md` files are NOT rendered as pages.

## Design system & conventions

- Style: **modern minimal, light theme only** (no dark mode — user chose this).
- Accent: navy/blue gradient (`--accent: #1f5f8b`, `--accent-strong: #12466b`).
- Font: Inter (Google Fonts). Sticky blurred nav. Subtle load-in animations that respect `prefers-reduced-motion`.

### User preferences observed (apply to site AND CV)

- **No em-dashes anywhere.** Rephrase with commas / restructure. (En-dashes in numeric ranges were also mostly removed; when in doubt avoid dashes.)
- Concise, plain writing. No "bluffing" / overcomplicated academic padding. Prefer short, readable sentences.
- Things the user explicitly removed from the site: the "View research"/"View CV" hero buttons, footer nav links, the hero tagline "Studying the forces that move economies and markets," the status pill "Economics · Incoming MRes, UC3M," the GitHub icon, and the CV download icon in the hero.
- Kept: the Chinese name on the homepage.
- Contact is a single labeled "Contact" button (not bare icons).
- Homepage interest chips (order matters): **Causal inference** (first), Macroeconomics, International finance, Machine learning. (Monetary policy was removed.)

## Hosting (LIVE SETUP — do not reconfigure without reason)

- **GitHub repo:** `github.com/ninjazack/ninjazack.github.io` (public user site).
- **GitHub Pages:** Source = Deploy from a branch, branch `main`, folder `/ (root)`.
- **Custom domain:** `zeqianyu.com` (matches CNAME file).
- **DNS on Cloudflare** (domain registered/managed there — account `Zeqianyu@gmail.com`). Records:
  - `A  @  185.199.108.153`
  - `A  @  185.199.109.153`
  - `A  @  185.199.110.153`
  - `A  @  185.199.111.153`
  - `CNAME  www  ninjazack.github.io`
- **Proxy is now ON (orange cloud) for all five records.** It was grey-cloud (DNS only) during initial setup so GitHub could issue its HTTPS cert; once that was active we switched to proxied to enable Cloudflare Web Analytics + CDN/DDoS protection.
- **SSL/TLS mode = Full** (SSL/TLS → Overview). This is REQUIRED with the proxy on — never "Flexible" (causes redirect loops). To revert to the pre-proxy state, flip the five records back to grey cloud.
- **Always Use HTTPS = On** (SSL/TLS → Edge Certificates) so `http://` auto-redirects to `https://`.
- **HTTPS cert:** with the proxy on, Cloudflare presents its edge cert to visitors and connects to GitHub (origin) over HTTPS. GitHub's own "Enforce HTTPS" is not needed and was left as-is.
- **Analytics:** Cloudflare Web Analytics, "Automatic setup" (works because traffic is proxied). View at Cloudflare → Analytics → Web analytics → zeqianyu.com. No script in the site code.
- **Caching caveat:** because traffic is proxied, edits can take a few extra minutes to show for visitors; there's a "Purge Cache" button in Cloudflare (Caching) if needed.
- **KNOWN GITHUB FLAKINESS:** the `pages build and deployment` action intermittently has `build` succeed but `deploy` fail ("Deployment failed, try again later") or get stuck on "Queued" forever. This is a GitHub-side issue, unrelated to the site/Cloudflare. **Reliable fix:** push a fresh commit — an empty one works: `git commit --allow-empty -m "redeploy" && git push`. (Re-running the failed job tends to get stuck; a fresh push supersedes stuck runs and deploys cleanly.)

## Maintenance workflow

Update loop (Terminal):
```
cd "/Users/zeqianyu/Documents/Personal Website"
git add -A
git commit -m "describe change"
git push
```
Remote and `main` are already set, so plain `git push` works. GitHub auto-rebuilds; live in ~1 min. Hard-refresh (Cmd+Shift+R) to bypass cache (Cloudflare cache may add a short delay). Auth: HTTPS with a **Personal Access Token** (classic, `repo` scope), username `ninjazack` — the token is now saved in macOS Keychain, so pushes no longer prompt. If a deploy fails or sticks on "Queued," push an empty commit to redeploy (see the GitHub flakiness note under Hosting).

### Updating the CV

1. Edit the CV in `/Users/zeqianyu/Desktop/CV` (edit `.tex`, re-render with `pdflatex`, delete `.aux/.log/.out` after).
2. Overwrite the published copy, keeping the exact name:
   `cp "/Users/zeqianyu/Desktop/CV/CV_Zeqian_Yu_edu_<date>.pdf" "/Users/zeqianyu/Documents/Personal Website/files/Zeqian_Yu_CV.pdf"`
3. Push (three git commands above). No HTML edit needed — the link is fixed.

## CV content conventions (in the Desktop/CV .tex)

Section order: **Education → Research Experience & Course Projects → Workshops, Seminars & Conferences → Relevant Professional Experience → Honors & Awards → Other Information.** Two-page target.

- Education bachelor line reads "Bachelor of Economics Sciences in Finance." MRes line does **not** say "(Path to PhD)."
- No em-dashes; no "out-of-sample"; no code/variable names (e.g. not `Math_Score`, not `R(1-3)`).
- ML project bullet lists models explicitly: Ridge, LASSO, Elastic Net, Decision Tree, Bagging, Random Forest, Gradient Boosting — framed to show ML competence.
- The title-analysis project is named "Web Scraping and Sentiment Analysis of Research Publications."
- Two Data Science Summer Schools listed (Hertie/DS3): 2025 = fundamentals (R, Python, applied calculus, linear algebra, statistics & probability, AI agents/experimental design); 2022 = de-duplicated to its unique advanced ML/deep-learning (computer vision, NLP). Course lists come from the DS3 site (ds3.ai), not invented.
- Two EURAXESS Science Slam entries (2023 and 2021), "Beijing, China (Remote)," descriptions taken from the actual certificates; both note the top-level Diamond certificate.
- Public Speaking & Investor Pitching credited to **Nicolaus Copernicus University** (2022).
- CRIC role labeled "E-House (China) Holdings Limited, CRIC" (not "CRIC Division"). Professional-experience bullets adapted from the older 0318 CV, kept to 3 simple bullets.
- Santander Open Academy entry does NOT list the sub-course universities.
- Technical Skills include ArcGIS (alongside R, Stata, MATLAB beginner, LaTeX).
- Honors & Awards: "Tuition Waiver, MRes in Economic Analysis — Universidad Carlos III de Madrid, 2026." (Confirm if it was merit-based before adding that word.)
- A right/left-aligned "Last updated: <date>" line at the very bottom.

## Open / possible future work

- CV upgrades discussed but NOT yet added: website URL (zeqianyu.com) + optional ORCID/Google Scholar in the CV header; a one-line "Research Interests"; a References/referees section (supervisor Prof. Javier Gardeazabal); add Python to skills if true; a Teaching section if any TA experience.
- Research page is intentionally minimal now; user will add publications over time.
- Research entries are currently coursework projects (plus the thesis); section is titled "Research Experience & Course Projects" for honesty.

## Done (go-live milestones)

- Site pushed to GitHub, Pages enabled, custom domain `zeqianyu.com` verified.
- Cloudflare DNS configured; HTTPS cert issued; site went live and secure.
- Switched Cloudflare to proxied (orange cloud) + SSL Full + Always Use HTTPS.
- Cloudflare Web Analytics enabled.
- Site copy fixes applied live: homepage `<title>` → "Zeqian Yu"; cv.html education entry → "Bachelor of Economics Sciences in Finance".
