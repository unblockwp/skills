---
name: unblock-audit-seo
description: Reviews how a WordPress site built with Unblock can be found and understood by search engines, through the Unblock MCP tools and an optional browser tool or web fetch. It covers titles, headings, indexing, links, images, structured data and content quality, and reports each finding with its evidence and its fix. Review only, it changes nothing. Use it when the user asks for an SEO audit, why a page does not show in search, or a check before launch.
license: MIT
compatibility: Requires an MCP client connected to a WordPress site running Unblock 1.0 or later; a read-only connection is enough. A browser tool or a web fetch tool is needed to read the head of each page, the robots file and the sitemap.
metadata:
  version: "0.1.0"
---

# Unblock SEO audit

Check that each page can be found, that search engines understand what it is about, and that it is
worth showing. Review only: change nothing, whatever the connection allows.

## Start

1. Call `unblock-get-instructions` once per session if you have not yet, in the same turn as your
   first read.
2. Agree on the scope. By default: every page of the sitemap, one post, the blog and one archive.
3. Find out which plugin writes the `<head>`: an SEO plugin, the theme, or WordPress alone. Many
   findings are fixed there, not in Unblock.

Unblock's tools are named here as your client calls them. A tool in its list goes by its MCP name,
`unblock-list-posts`, which your client may prefix. A name with a slash, `unblock/list-media`, is an
ability it does not list: run it through `execute-ability` with that name.

## Tools

- `unblock-list-posts`: the pages and posts, with their title, slug, status and excerpt.
- `unblock/get-rendered-html` with `template: true` (or with the page's `url`): the body of the page
  as the visitor gets it, headings and links included.
- `unblock/get-site-settings`: the site's title, tagline and language.
- `unblock/list-media`: the alt text the media library keeps.
- A browser tool or a web fetch tool: the `<head>` of each page (title, meta description, canonical,
  robots, Open Graph, JSON-LD), `/robots.txt`, the XML sitemap (`/wp-sitemap.xml` with WordPress
  alone, the SEO plugin's own otherwise), and Lighthouse's SEO category when the tool runs it.
- `unblock/get-seo`, through `execute-ability`: the SEO title and meta description a page was given,
  as stored, with Yoast SEO, Rank Math, SEOPress or All in One SEO. For its robots, the plugin's own
  abilities when it gives them: `rank-math/get-post-seo-meta`, `aioseo-posts/seo-data-get`,
  `yoast-seo/get-post-seo-data` on a site in production, `seopress/get-post-title-description` once
  the user exposed SEOPress's abilities. All in One SEO answers "Post not found" for a page it never
  saved: it holds nothing for it, and the page follows its post type's defaults. Not a fault of the
  page.

Do not take an SEO plugin's content analysis or score for an Unblock page: they read the saved
blocks, not the page. `rank-math/analyze-post-content` finds no words, Yoast's scores
(`yoast-seo/get-seo-scores`) are the ones its editor saved, none for a page never opened there, and
All in One SEO's TruSEO score (`aioseo-posts/list-truseo-score`) counts no words of Unblock blocks.
Read the content with `unblock/get-rendered-html`.

## Run the checklist

Run [`references/checklist.md`](references/checklist.md) on each page in scope. **[code]** items come
from the rendered HTML and the head, **[browser]** items need a browser tool or a fetch, **[human]**
items go to the "needs a person" list.

## Facts to apply

- Google writes its own snippet when the meta description does not fit the query: a description is a
  suggestion, not a ranking factor.
- More than one `h1` is valid HTML and not a search penalty. One `h1` per page is the rule of this
  site for accessibility and clarity, so report extra ones under accessibility, not as an SEO error.
- Title and description lengths are guides for how they display, not limits.
- With Yoast SEO, Rank Math, SEOPress or All in One SEO, a page with no description of its own gets
  one from its fixed texts, never from its loops, its conditions or a text holding an expression: a
  page built mostly from loops gets a thin one or none.
- Structured data must match what the page shows. Google stopped showing FAQ rich results on
  7 May 2026: FAQ markup no longer earns one.
- Google does not use `llms.txt`.
- Pages produced in bulk to rank, with little value for a reader, go against Google's spam policies,
  whoever or whatever wrote them.

## Rules for findings

- **Evidence:** **measured** (a tool returned it: a status code, a fetched tag), **observed** (seen in
  the HTML or the head), **inferred** (read in code or settings, not confirmed on the live page).
- **Never promise a ranking.** Say what a fix changes for search engines and how to see it in Search
  Console.
- **One finding per cause.** A title template wrong on every post is one finding, with the list of
  pages.
- **Say where the cause lives:** the page's content in Unblock, the post's settings (title, slug,
  excerpt), the SEO plugin, the theme, or WordPress settings. Report what is outside Unblock as out of
  scope, with who can fix it.

## Report

Start with a **verdict**: whether the site can be indexed, the main issues, the pages checked, and
what could not be checked.

Then each **finding**, the most severe first:

- **Severity:** Critical (pages cannot be indexed or found), High (pages indexed but misunderstood or
  duplicated), Medium (a weaker result in search), Low (a polish).
- **Topic:** indexing, titles, headings, links, images, structured data, content.
- **Where:** the pages, the component by its `data-unbk-name` when it is in the content.
- **Evidence:** what was found, and whether it was measured, observed or inferred.
- **Fix:** what to change, and where (the block, the post's settings, the SEO plugin, the theme).
- **Recheck:** how to confirm it, on the live page or in Search Console.

End with **not checked, needs a person** and **out of scope**.

## After the report

Fix only when the user asks, with a connection that can write: the content through `unblock-content`
or `unblock-build-page`, the title, slug and excerpt with `unblock-update-post`, the SEO title and
meta description with `unblock/update-seo`, as `unblock-content` does. A slug change breaks the
links to the old address: change it only with the user's agreement, and with a redirect.

Taking a post type off the public site (to drop thin pages from search) also stops Unblock from
reading its posts, until the user opens it in Unblock's privacy settings. Say so before the change,
and check the pages that list it afterwards.
