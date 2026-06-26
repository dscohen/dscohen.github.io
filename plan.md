# Website Refresh & Redesign Plan — dscohen.github.io

## Context
The site is the academicpages (Minimal Mistakes) Jekyll template, last touched Oct 2023, and
is badly out of date: the config still calls Daniel a "Postdoctoral Researcher at Brown," the
homepage bio is a single PhD-era sentence, the **CV page is 100% template placeholder**
("B.S. in GitHub, GitHub University"), the publications list stops at 2022, and the
`_publications/` collection still holds 3 template papers. Daniel is now a Research Scientist
at Dataminr building production LLM/agentic systems. Goal: a more professional, redesigned
site with a real general CV and updated publications.

User decisions: **larger redesign** (not just content) · CV as **updated PDF + rebuilt page** ·
sidebar shows **Scholar + GitHub + LinkedIn + scraper-obfuscated email**.

## Approach
Theme is vendored locally → safe to edit `_sass`, `_includes`, `_config.yml`, `_pages`.
Visual changes are pure SASS/HTML so GitHub Pages compiles them with no plugin risk.

### 1. Visual redesign (professional polish)
- `_sass/_variables.scss`: refined accent/link color (professional indigo/teal), tuned
  heading weight & spacing. Keep the system font stack (no fragile webfont fetch).
- New `_sass/_custom.scss` (imported last in `assets/css/main.scss`): avatar styling
  (rounded, subtle border/shadow), remove the useless "Follow" button, tighten sidebar link
  list, card/hover treatment for publication entries, cleaner section rules and spacing,
  responsive tweaks.
- Downscale `images/profile.png` (currently 4.7 MB — a real perf/professionalism problem) to
  ~600 px web-optimized via `sips`.

### 2. Content / config (`_config.yml`)
- `description` → e.g. "Research Scientist at Dataminr — LLMs, agentic AI, NLP, and
  information retrieval."
- Author block: `employer: Dataminr`, short `bio`, `linkedin: <from user>`,
  `email: dcohenCS@gmail.com`, keep `googlescholar` + `github`. Remove Twitter from display
  (not requested). `location`: per user.

### 3. Email obfuscation (`_includes/author-profile.html`)
- Replace the plain `mailto:` block with a split-attribute link:
  `{% assign p = author.email | split: "@" %}` → `data-user` / `data-domain` assembled into
  the `mailto:` via a tiny inline JS on click. Address never appears whole in static HTML.

### 4. Homepage (`_pages/about.md`)
- Rewrite as a real landing page: one-line headline/tagline, 2 short paragraphs (current role
  at Dataminr + research focus in IR/NLP/LLMs/agentic AI), and a "Selected work" pointer to
  Publications/CV/Scholar. Professional, first-person, concise.

### 5. CV page (`_pages/cv.md`) — rebuild from `~/Documents/cv/myresume.tex` (general CV)
- Real sections: Summary, Technical Skills, Experience (Dataminr → NYU), Education, Selected
  Publications, Awards & Grants, Service. "Download PDF" button at top linking `/files/cv.pdf`.
- Use the general (non-Oura-tailored) resume as the source of truth.

### 6. CV PDF (`files/cv.pdf`)
- Replace with the current general resume PDF (`~/Documents/cv/myresume.pdf`).

### 7. Publications (`_pages/publications.md`)
- Prepend missing 2023–2025 papers (from Daniel's current resume / Scholar):
  ACL Findings 2025 (Explain then Rank); NAACL Findings 2024 (In-Context Example Ordering);
  CIKM 2023 (Predictive Uncertainty-based Bias Mitigation in Ranking); SIGIR 2023 (Lightweight
  Constrained Generation for Query-Focused Summarization).
- Reorganize with year headings, consistent formatting, keep Scholar link, bold best-paper tags.

### 8. Cleanup
- Delete the 3 placeholder files in `_publications/` (template "Paper Title Number 1–3") so
  they can't surface in archives/sitemap.

## Verification
1. If Ruby/bundler available: `bundle exec jekyll build` and scan output for errors; otherwise
   validate Liquid/YAML by inspection.
2. Grep the built/rendered output (or sources) to confirm no remaining placeholder strings
   ("GitHub University", "Paper Title Number", "Postdoctoral Researcher").
3. Confirm obfuscated email renders (no full address in static HTML) and links work.
4. Show Daniel a local preview / screenshots before any commit. Do NOT commit or push without
   explicit approval.

## Open items needing user input
- LinkedIn URL/username (required for sidebar).
- Location to display (or omit).
- OK to downscale profile.png.
