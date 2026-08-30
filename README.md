# Hold Prosecutors Accountable, Inc. — website

A single-page static site. No build step, no dependencies. `index.html` contains
the markup, styles, and script in one file so it can be edited without tooling
and deployed as-is.

```
index.html     the entire site
favicon.svg    browser tab mark
robots.txt     search engine directive
```

## Deploy

1. On GitHub, create a repo (e.g. `hpa-site`). Public or private both work.
2. Put these files in the repo root and push.
3. On Vercel: **Add New → Project → Import** the repo. Framework preset:
   **Other**. Leave build command and output directory blank. **Deploy**.
4. Live in about 20 seconds at `hpa-site.vercel.app`.

Every push to `main` redeploys automatically. Pull requests get their own
preview URL, which is a good way to review copy changes before they go live.

### Custom domain

Buy the domain (Namecheap, Cloudflare, Porkbun — all fine). In Vercel:
**Project → Settings → Domains → Add**, then follow the DNS records it shows
you. HTTPS is issued automatically. Point both `example.org` and
`www.example.org` at it and let Vercel handle the redirect.

## Fill in before launch

Everything in `[SQUARE BRACKETS]` is a placeholder. Search the file for `[` and
replace:

- `[YEAR]` — year founded
- `[CITY, STATE]` — appears three times (hero, footer, contact)
- `[PENDING]` — EIN, once the IRS issues the determination letter
- `[EMAIL]` — general inbox, appears five times
- `[PRESS EMAIL]` — can be the same address
- `[STREET]` / `[CITY, STATE ZIP]` — mailing address
- `[DONATION LINK]` — see below
- `[Replace with your first project or report.]` — four times, one per focus area

Also worth doing: replace `og:title` / `og:description` if the framing changes,
and add an `og:image` (1200×630 PNG) once there's a logo or photograph, so
links shared on social and in email preview properly.

## Taking donations

The site currently links out rather than processing payments. Options, roughly
in order of least work:

- **Every.org** — free for 501(c)(3)s, handles receipts, no monthly cost.
- **Givebutter** — free platform, tips-optional model, good for campaigns.
- **Stripe** — a Payment Link or Stripe Checkout gives full control; you handle
  receipting and acknowledgment letters yourself.

Whichever you pick, you get a URL that goes in `[DONATION LINK]`. Don't build a
custom checkout for this — the compliance surface isn't worth it.

## Contact form

There isn't one; the links are `mailto:`. That is deliberate for launch — it's
zero maintenance and nothing to secure. If a form becomes necessary, Formspree
or a Vercel serverless function are the two straightforward paths. Anything
collecting personal accounts from formerly incarcerated people should have a
short privacy statement attached about what happens to what they send.

## Design notes

- Type: **Libre Caslon Display** (headlines), **Public Sans** (body, the
  typeface of the US Web Design System), **Courier Prime** (labels and rail).
  Caslon and Courier are both native to American legal printing.
- Color: paper `#EDEEEA`, ink `#16181A`, oxblood `#6B2226` used only for
  emphasis. Light rather than dark on purpose — transparency is the subject.
- The numbered rail down the left edge is a court-transcript line gutter,
  resetting at 25 like a real transcript page. The number nearest the middle of
  the viewport highlights as you scroll. It hides below 1000px.
- Responsive to mobile, keyboard focus is visible, and it degrades to a
  perfectly readable page with JavaScript off.

## Two things to check with counsel

1. **Naming individuals.** If the site ever names a specific prosecutor
   alongside an allegation, that copy should be reviewed before it ships.
   Sourced, documented, and carefully worded is the standard; the organization's
   name makes it a likelier target than most.
2. **Lobbying limits.** The copy says "advocate ... within the limits that apply
   to charitable organizations" and explicitly disclaims candidate support. Keep
   that framing as the site grows, and track lobbying expenditures if direct
   legislative advocacy picks up.
