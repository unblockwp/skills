# Pre-launch checklist

Run it on the site as visitors will get it, logged out. **[code]** an AI can check it from the
markup, **[browser]** an AI with a browser tool can check it, **[human]** a person checks it.

The four audits go deeper: `unblock-audit-accessibility`, `unblock-audit-performance`,
`unblock-audit-seo` and `unblock-audit-code`, each with its full checklist.

## Content

- [ ] **[code]** No `[To confirm: …]`, `[Assumption]`, lorem ipsum or placeholder image left, and
  every `[Invented]` value of the brief replaced with the real one.
- [ ] **[human]** Prices, opening hours, addresses, names and quotes checked by the business.
- [ ] **[human]** Legal pages in place: legal notice, privacy policy, cookie policy, terms of sale
  when the site sells.
- [ ] **[code]** Test posts and pages removed or unpublished.
- [ ] **[code]** Temporary photos (marked as placeholders in the media library) replaced with the
  business's own, or kept on purpose with their license checked.
- [ ] **[code]** The 404 page and the search results page are designed.

## Accessibility

- [ ] **[code]** One `h1` per page, headings in order, an `alt` on every image.
- [ ] **[browser]** Text contrast at least 4.5:1, computed.
- [ ] **[browser]** The main task done with the keyboard only, with the focus always visible.
- [ ] **[browser]** No horizontal scroll at 320 CSS pixels wide.

## Performance

- [ ] **[browser]** LCP 2.5 s or less and CLS 0.1 or less on a phone, in a trace of the home page and
  of the main task's page.
- [ ] **[code]** The main image at the top of each page is not lazy and has `fetchpriority="high"`.
- [ ] **[browser]** The page cache is on.

## Search

- [ ] **[human]** WordPress's "Discourage search engines" setting is off.
- [ ] **[code]** Each page has a unique title and a meta description.
- [ ] **[browser]** `/robots.txt` and the XML sitemap answer, and list the right pages.
- [ ] **[human]** Search Console set up, sitemap sent.

## Forms and email

- [ ] **[human]** Each form sent once for real; the message arrives where it should, and the visitor
  sees a confirmation.
- [ ] **[human]** The site's emails leave from an address and a mail service that do not land in spam.

## Code

- [ ] **[browser]** No console error, no 404, no mixed content.
- [ ] **[code]** No raw color or size in component selectors: everything goes through the tokens.

## Site

- [ ] **[human]** HTTPS on every address, with the version without `www` (or with it) redirecting to
  the other.
- [ ] **[human]** Site title, tagline, site icon and the front page set in WordPress.
- [ ] **[code]** Only the plugins approved in `brief.md` are installed and active
  (`unblock/list-plugins`), and only the font faces the design uses are active
  (`unblock/list-fonts`).
- [ ] **[human]** Analytics and cookie consent in place where the law asks for it.
- [ ] **[human]** A backup taken, and a way to restore it.
- [ ] **[human]** The user who will edit the site knows how to change a text, an image and a color.
