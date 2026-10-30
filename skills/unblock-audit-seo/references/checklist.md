# SEO checklist

**[code]** from the rendered HTML and the head, **[browser]** with a browser tool or a fetch,
**[human]** by a person.

## Indexing

- [ ] **[browser]** Each page answers 200 at its canonical address; old addresses redirect with a
  301.
- [ ] **[code]** No `noindex` on a page meant for search, in the meta robots or the HTTP header.
- [ ] **[browser]** `/robots.txt` blocks nothing meant for search.
- [ ] **[browser]** The XML sitemap lists the public pages and posts, and no draft, test or thin page.
- [ ] **[code]** Each page has a canonical pointing to itself, or to the page it duplicates.
- [ ] **[human]** WordPress's "Discourage search engines" setting is off at launch.

## Titles and descriptions

- [ ] **[code]** Each page has a unique `title` that says what it is, with the words visitors use.
- [ ] **[code]** Each page has a meta description that sums it up for a reader.
- [ ] **[code]** The slug is short, readable, and in the page's language.

## Headings and content

- [ ] **[code]** The main heading says what the page is about; section headings say what each section
  holds.
- [ ] **[code]** The text a visitor needs is in the HTML, not only in images or loaded by a script.
- [ ] **[human]** Each page has a purpose of its own, and no two pages target the same need.
- [ ] **[human]** The content was reviewed by a person who knows the subject.

## Links

- [ ] **[code]** Link texts describe their target.
- [ ] **[code]** Every page in the sitemap is linked from the navigation or from another page.
- [ ] **[browser]** No broken internal link.

## Images

- [ ] **[code]** Images that carry meaning have an `alt` that describes them.
- [ ] **[code]** Images have `width`, `height` and `srcset`, from the media library.

## Structured data

- [ ] **[code]** JSON-LD, when present, matches what the page shows (organization, local business,
  article, product, breadcrumbs).
- [ ] **[code]** No FAQ markup added for rich results: Google stopped showing them on 7 May 2026.

## Sharing

- [ ] **[code]** Open Graph title, description and image on the pages people share.

## Languages

- [ ] **[code]** `lang` on `html` matches each page's language.
- [ ] **[code]** On a multilingual site, `hreflang` links each page to its translations.

## People

- [ ] **[human]** Search Console is set up for the site, with the sitemap sent.
- [ ] **[human]** The business's details (name, address, phone) match its other listings.
