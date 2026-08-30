# Hold Prosecutors Accountable, Inc. — website

A static site. No build step, no dependencies. `index.html` contains the markup,
styles, and script in one file so it can be edited without tooling and deployed
as-is.

```
index.html      the entire site (all pages, styles, script)
logo.png        full lockup, shown on the home page
logo-mark.png   shield mark, header and touch icon
favicon.svg     browser tab mark
robots.txt      search engine directive
```

Both `<img>` tags fall back to `favicon.svg` if the PNGs are ever missing, so a
bad deploy shows the simplified vector mark rather than a broken image.

## Pages

The site has eight views in one file, switched client-side by the URL hash. Each
one is linkable and works with the back button:

```
#/               home
#/mission        mission and purpose
#/accountability pillar one — prosecutorial accountability
#/defense        pillar two — the right to a fair defense
#/reentry        pillar three — housing, banking, life after release
#/research       research agenda and publications
#/help           get help
#/donate         donate
```

With JavaScript off, every view renders stacked as one long readable page, so
nothing is hidden from a reader or a crawler.

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

- `[EMAIL]` — general inbox, appears several times including `mailto:` links
- `[PRESS EMAIL]` — can be the same address
- `[PHONE]` — main phone number
- `[STREET]` / `[CITY, STATE ZIP]` — mailing address
- `[PENDING]` — EIN, once the IRS issues the determination letter. Also used on
  the footer's "Financials and filings" line until there is something to link to
- `[DONATION LINK]` — see below
- `[FOUNDING STORY]` — the callout at the bottom of the mission page

Two more, neither in brackets:

- **The home page photograph.** On the "Why we exist" section there is a
  placeholder frame with the caption *"Photograph — a courthouse, a meeting, the
  people this work serves."* Replace the `<figcaption>` with
  `<img src="/photo-home.jpg" alt="...">` and drop the file in the repo root.
  The frame is styled to crop the image to fill.
- **The publications list.** The research page shows a dashed "first
  publications are in preparation" panel. Replace that block with the list of
  reports when the first one ships; the comment above it marks the spot.

Also worth doing: replace `og:title` / `og:description` if the framing changes.
`og:image` points at `/logo.png`; a 1200×630 photograph or title card previews
better in social and email once one exists.

## Taking donations

The donate page has a working amount picker (monthly/one-time, six presets), but
the site does not process payments — the button links out. Options, roughly in
order of least work:

- **Every.org** — free for 501(c)(3)s, handles receipts, no monthly cost.
- **Givebutter** — free platform, tips-optional model, good for campaigns.
- **Stripe** — a Payment Link or Stripe Checkout gives full control; you handle
  receipting and acknowledgment letters yourself.

Whichever you pick, you get a URL that goes in `[DONATION LINK]` (it appears
once, in the script near the bottom of `index.html`). When that value is a real
`https://` URL, the picker appends `?amount=50&frequency=monthly` so the chosen
amount carries over — check the parameter names your processor expects and
adjust the `href()` function if they differ. Don't build a custom checkout for
this; the compliance surface isn't worth it.

## Contact form

There isn't one; the links are `mailto:`. That is deliberate for launch — it's
zero maintenance and nothing to secure. If a form becomes necessary, Formspree
or a Vercel serverless function are the two straightforward paths. Anything
collecting personal accounts from formerly incarcerated people should have a
short privacy statement attached about what happens to what they send.

## Design notes

- Type: **Playfair Display** (headlines) and **Source Sans 3** (body), loaded
  from Google Fonts with system fallbacks.
- Color: navy `#16233F` and deep navy `#0E1729`, gold `#C9A24B`, cream `#F6F3EC`
  on white — taken from the logo. Gold is used for emphasis and for the current
  nav item; it is never used for body text.
- The header is sticky on desktop and collapses to a Menu button below 1080px.
- The banner across the top routes people who need help to `#/help` before
  anything asks them for money.
- The "Get help" situations are `<details>` blocks holding real plain-language
  information, each closing with a line making clear it is not legal advice.
  Expand the content there before anything else — it is the page people in
  trouble actually arrive on.
- Responsive to mobile, keyboard focus is visible, `prefers-reduced-motion` is
  honored, and the page degrades to readable plain HTML with JavaScript off.

## Three things to check with counsel

1. **Naming individuals.** If the site ever names a specific prosecutor
   alongside an allegation, that copy should be reviewed before it ships.
   Sourced, documented, and carefully worded is the standard; the organization's
   name makes it a likelier target than most.
2. **Lobbying limits.** The copy says the organization advocates "within the
   limitations applicable to organizations exempt under Section 501(c)(3)" and
   explicitly disclaims candidate support. Keep that framing as the site grows,
   and track lobbying expenditures if direct legislative advocacy picks up.
3. **The Get Help content.** It is general procedural information, not advice,
   and it is written to stay on that side of the line. Worth a read by counsel
   anyway before launch, since it is the page most likely to be relied on.
