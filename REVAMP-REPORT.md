# Revamp Report

## Modified / Created Files

- `README.md`
- `content/config.toml`, `content_zh/config.toml`
- `content/about.toml`, `content_zh/about.toml`
- `content/bio.md`, `content_zh/bio.md`
- `content/cv.md`, `content_zh/cv.md`
- `content/news.toml`, `content_zh/news.toml`
- `content/publications.bib`
- `content/services.toml`, `content_zh/services.toml`
- `content/teaching.toml`, `content_zh/teaching.toml`
- `public/robots.txt`, `public/sitemap.xml`
- `src/app/layout.tsx`, `src/app/page.tsx`, `src/app/[slug]/page.tsx`
- `src/components/home/HomePageClient.tsx`, `src/components/home/Profile.tsx`
- `src/components/layout/Footer.tsx`
- `src/components/publications/PublicationsList.tsx`
- `src/lib/bibtexParser.ts`, `src/lib/config.ts`, `src/lib/i18n/messages.ts`
- `REVAMP-REPORT.md`

## Main Changes

- Config: disabled likes; updated email to `jinjie.liu@nature.com`; added apex URL, SEO title, and SEO descriptions without changing navbar brand text or navigation.
- Homepage content: replaced the single research-interest list with editorial interests and research-background groups in both locales.
- Bio: added the Nature Energy editors-page link in both locales; polished the AFHEA sentence in English; added Durham date TODOs.
- Publications: moved under-review/submitted manuscripts out of the main list; removed target journal fields and target-journal display for those manuscripts; added status parsing from BibTeX notes; converted trailing BibTeX `others` to `et al.` in rendered authors.
- Footer: added the personal-site disclaimer, in English, on all pages.
- SEO: added `metadataBase`, canonical URLs, Open Graph/Twitter metadata, homepage JSON-LD, `robots.txt`, and `sitemap.xml`.
- README: appended the Cloudflare-only `www` to apex redirect instructions.
- CV/services/teaching/news: removed not-yet-public planning content and added requested TODOs for unconfirmed dates/titles.
- Reviewer note (post-Codex): Codex's sandbox-only build workarounds (`scripts/next-worker-compat.cjs`, modified `package.json` build command, `next.config.ts` experimental/ignore flags) were REVERTED — the unmodified `next build` toolchain passes in the real environment and is what Cloudflare runs. Markdown TODO comments were converted from `<!-- -->` (which this site's ReactMarkdown renders as visible escaped text) to the invisible `[//]: # "..."` form; TOML `#` comments were kept as-is.

## Cannot Complete In Code

- The `www.jinjieliu.com` to `jinjieliu.com` 301 redirect must be configured in Cloudflare Redirect Rules. Documented in `README.md` under `www → apex 301 redirect (Cloudflare)`.

## TODO(user) Inserted

### Dates To Confirm

- `content/bio.md`: Durham year-level `(2023 – 2026)`.
- `content_zh/bio.md`: Durham year-level `（2023 – 2026）`.
- `content/cv.md`: Durham PRA `Sep 2023 – 2026`.
- `content_zh/cv.md`: Durham PRA `2023 年 9 月 – 2026 年`.
- `content/teaching.toml`: `Oct 2024 – 2026`; `2023 – 2026`.
- `content_zh/teaching.toml`: `2024 年 10 月 – 2026 年`; `2023 – 2026`.
- `content/services.toml`: WES Durham `Ongoing`; Supergen ECR `Ongoing`; memberships `2019 – Present`; public engagement `2024 – 2026`.
- `content_zh/services.toml`: WES Durham `进行中`; Supergen ECR `进行中`; memberships `2019 至今`; public engagement `2024 – 2026`.

### Titles To Confirm

- `content/cv.md`: FAU `Visiting Postdoctoral Research Associate`.
- `content_zh/cv.md`: FAU `访问博士后`.
- `content/news.toml`: FAU news item title.
- `content_zh/news.toml`: FAU news item title.

### Other

- `content/publications.bib`: restore journal fields for `liu2026dayahead` and `harsh2026aidriven` once formally accepted.
- `content/publications.bib`: confirm DOI/pages/proceedings metadata for `liu2025bilevel` once the ISGT Europe 2025 record is available.

## Commands

- Local preview: `npm run dev`
- Production static build: `npm run build`
- Extra verification run locally: `npx tsc --noEmit`; `npx next lint`

## Build Result

- `npm run build`: PASS. Static export generated in `out/`.
- `npx tsc --noEmit`: PASS.
- `npx next lint`: PASS with one existing warning about custom fonts in `src/app/layout.tsx`.

## Self-Verification

- `out/index.html`: no Like/Liked markup; JSON-LD present; disclaimer present; `Editorial Interests` and `Research Background` present; canonical is `https://jinjieliu.com/`; no `https://www.jinjieliu.com`.
- `out/publications/index.html`: `Manuscripts under review` present; visible manuscripts section shows `Under review, 2026` and `Submitted, 2026`; target journal names are absent from the visible manuscripts section; no `, others`; `et al.` renders.
- `out/teaching/index.html`, `out/awards/index.html`, `out/services/index.html`, and `out/cv/index.html`: present.
- Homepage nav: six visible items remain `About`, `Publications`, `Teaching`, `Awards`, `Services`, `CV`; locale config still contains the original six Chinese items.
- `public/robots.txt` and `public/sitemap.xml`: present and copied to `out/`.

## Broken / Dead-Linked Items

- No broken internal routes were found in the build output.
- External link checking was not run because network access is restricted in this environment.
