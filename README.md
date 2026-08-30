# Hold Prosecutors Accountable, Inc. — website

A static site. No build step, no dependencies. `index.html` contains the markup,
styles, and script in one file so it can be edited without tooling and deployed
as-is.

```
index.html      the entire site (all pages, styles, script)
content.json    text edited through the editor; overrides index.html
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

## Editing the text without touching the code

There is a password-protected editor at **`/#/admin`**. It is not linked from
anywhere on the site; you get there by typing the address. The default password
is **`hpa-admin`** — change it before launch (see below).

Signing in drops you back on the site with editing switched on and a bar across
the top. From there:

- **Click any words to change them.** A small box opens where they sit; type,
  and the page updates under you, in the real design, at the real size. Use the
  page menu in the bar to move around while editing is on — clicking a link
  edits the link's wording instead of following it.
- **Save from any page.** The gold **Save** button in the bar publishes
  everything you have changed, wherever you happen to be. `⌘S` / `Ctrl-S` does
  the same. It needs the one-time token setup described below.
- **All text…** opens the full panel: every line on the site in a list, grouped
  by page, searchable, filterable down to just what you changed. Useful when
  hunting for a phrase is easier than finding it on the page.

Alongside the wording, the editor also holds the browser-tab title for each
page, the sentence search engines show under the site name, and the donation
link.

### Nothing is live until you save

Edits are held in your own browser and are visible only to you. Saving writes
them to `content.json` in this repository, which redeploys the site. Two ways,
both on the panel's Save card:

1. **Let the page do it.** Paste a GitHub [fine-grained personal access
   token](https://github.com/settings/personal-access-tokens) scoped to this
   repository with **Contents: read and write**, once. After that the **Save**
   button in the bar commits `content.json` from any page and Vercel redeploys —
   live in about a minute.
2. **By hand.** Download `content.json` (or copy it) and commit the file to the
   repository root yourself. Nothing else is needed.

If you close the browser mid-edit, the draft is still there when you come back.
"Discard my changes" throws it away; "Back to the original" on any single line
restores the wording in `index.html`.

### How the override works

`index.html` stays the source of truth. Each line of text is keyed by a hash of
its original wording, and `content.json` maps those keys to replacements. So
rewording a line in `index.html` retires the override that used to sit on it,
and the file wins — an edit in the code is never silently undone by an old
override. Deleting `content.json` puts every word back to what the markup says.

Text written by the script rather than the markup is not editable this way: the
sentence under the donation amounts, the label on the Give button, and the
copyright year. The donation amounts themselves are editable, as settings.

### About the password

Change it from the editor's Password card — it takes effect for everyone once
you publish. To set it in the code instead, replace `BUILTIN_HASH` in
`index.html` with the SHA-256 of `hpa.editor.v1:` plus your password:

```
printf 'hpa.editor.v1:your-new-password' | shasum -a 256
```

Be clear-eyed about what that password does. It is checked in the browser, and
the page's source is public, so it keeps the editor out of visitors' way — it is
not a lock on anything. What actually protects the live site is the GitHub
token: without one, an edit never leaves the browser it was typed in. Give the
password out freely enough; hand out tokens carefully, and issue one per person
so a single one can be revoked.

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

Everything in `[SQUARE BRACKETS]` is a placeholder. Either edit them at
`/#/admin` and publish, or search `index.html` for `[` and replace:

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

**Do not collect card numbers on this site.** Every option below hands the donor
to a hosted checkout page run by the processor, so card data never touches this
site and never passes through anything the organization has to secure. That is
the right answer for a small nonprofit: it keeps PCI scope at effectively zero,
and the processor issues the receipt.

### What to use

**Start with Stripe.** Create *Payment Links* in the Stripe dashboard — no code,
no backend, and it works before the IRS determination letter arrives, which
Every.org and Givebutter's nonprofit onboarding generally do not. Standard rate
is 2.9% + 30¢; once the determination letter is in hand, email
`nonprofit@stripe.com` with the EIN and letter to ask for the nonprofit rate of
2.2% + 30¢ (it requires that at least 80% of volume be tax-deductible
donations). Stripe pays out to the organization's bank account, handles Apple
Pay and Google Pay automatically, and manages the recurring charges for monthly
donors. What it does not do is write acknowledgment letters — that stays a
back-office job.

**Add Every.org once the 501(c)(3) is recognized**, if the receipting and the
long tail of giving methods are worth more than the control. It is free to the
nonprofit, issues tax receipts itself, and accepts donor-advised funds, stock,
and crypto — three things that are real work to take otherwise. **Givebutter** is
the comparable alternative, with better campaign pages and a tips-optional model.

Whatever you pick, keep the button on this page pointing out to it. Do not build
a custom checkout; the compliance surface is not worth it for donation volume
this site will see.

### Wiring it up

Everything below is set in the editor, under *Tab titles and settings* — no code
changes.

**Stripe Payment Links** take no amount parameter in the URL, so each amount is
its own link. Create one link per amount (monthly links use a recurring price,
one-time links a one-off price), plus one "customers choose what to pay" link
for the Other button, and paste them into **A link for each amount**, one per
line:

```
monthly 10  https://buy.stripe.com/aaa
monthly 25  https://buy.stripe.com/bbb
monthly 50  https://buy.stripe.com/ccc
once 25     https://buy.stripe.com/ddd
once 50     https://buy.stripe.com/eee
other       https://buy.stripe.com/fff
```

**Every.org, Givebutter, Donorbox** and most others take the amount in the URL,
so one link is enough. Put it in **Donation link** — for Every.org that is
`https://www.every.org/your-slug#donate` — and the page appends the chosen
amount and frequency. The parameter names are settings too, because processors
disagree about them; the defaults (`amount`, `frequency`, `MONTHLY`, `ONCE`)
are Every.org's. Check your processor's documentation and adjust.

Either way, **Monthly amounts** and **One-time amounts** set the buttons
themselves — plain comma-separated numbers. Any amount with no matching direct
link falls back to the Donation link, so the two styles can be mixed.

### Before you take the first dollar

- The EIN and the `[PENDING]` markers on the donate page and in the footer need
  to be real before you solicit anything.
- Most states require registration before soliciting charitable donations from
  their residents, and a website that asks the whole country counts. Ask counsel
  which registrations apply — this is the item most new organizations miss.

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
4. **Charitable solicitation registration.** Asking for donations on a public
   website is soliciting in most states that require registration. Sort out
   which ones apply before the donate page goes live.
