---
name: unblock-build-page
description: Builds or rebuilds a page of a WordPress site with Unblock, through the Unblock MCP tools. It plans the page from its purpose, writes real copy, composes the whole page in one pass on the site's design tokens and existing components, reviews it before writing, checks the markup and accessibility, and looks at it once at phone and desktop widths when a browser tool is connected. Use it when the user asks for a new page (home, about, services, landing, contact) or to redo a page or a section. Requires the Unblock MCP server.
license: MIT
compatibility: Requires an MCP client connected to a WordPress site running Unblock 1.0 or later. A browser tool is optional: with one, the agent looks at the page once at phone and desktop widths; without one, the user reviews the draft.
metadata:
  version: "0.1.0"
---

# Unblock build page

Build a page that does one job for one audience, reads like the brand wrote it, and looks like it
belongs to the site. Plan first, compose the whole page in one pass, then check what the visitor
will get.

Aim for a page a designer would sign, not a safe template. Be ambitious where it counts, in one or
two places, and keep the rest calm.

## Start

1. Call `unblock-get-instructions` once per session if you have not yet, in the same turn as your
   first read or `unblock-create-post`. It holds the rules of the blocks, the CSS, Twig and the
   tools: follow it. This skill only adds the method.
2. Read the site's CSS: its tokens, fonts, breakpoints and the classes that already exist. For a new
   page, `unblock-create-post` returns it (step 1): create the draft first and do not read it again.
   For an existing page, `unblock-get-css`. With no tokens in `:root`, set up the design system
   first (`unblock-design-system`), or ask the user whether to build on the theme's styles.
3. Read the project's kit when your client has files: `brief.md`, `sitemap.md` (this page's entry),
   `design.md`, `voice.md`, in the folder the session runs in or in a subfolder named after the site.
   When you find none, ask the user where it is before working without it. They come before your
   own ideas. The composition and the imagery of `design.md` decide how the page is laid out; with
   no art direction recorded, ask the user for one (`unblock-design-system`) before planning.
4. The examples in `unblock-get-instructions` show the syntax, not a style. Do not copy their
   gradient hero, their centered layout or their copy.
5. Compose the look first, with the page's content written as text. Loops, fields and conditions
   come after, with `unblock-dynamic-data`, best in a fresh session: a long session that mixes
   design, code and audits drifts toward a generic page.

Tools are named here as your client calls them. A tool in its list goes by its MCP name,
`unblock-get-css`, which your client may prefix. A name with a slash, `unblock/list-media`, is an
ability it does not list: run it through `execute-ability` with that name.

For a decision between a few options or an approval, use your client's tool for questions with
choices when it has one, after explaining the options in text. Ask open questions in text.

## 1. Find the target

- **New page:** `unblock-create-post` makes a draft and returns its `post_id`, the `content_hash` of
  the first write, `template` (what the page's template prints around the content) and `css` (the
  site's CSS): build on them without reading the page or the CSS again.
- **Existing page:** find it with `unblock-list-posts`, then read it with `unblock-get-context`.
  A write to a published or private page, or to a template, is refused until you pass `live: true`,
  once the user agreed. Before a redesign, offer a draft copy (`unblock/copy-post`) or an editor
  session the user opens on the page.
- **Who holds the `h1`:** `template.h1`, from `unblock-create-post` for a new page, or from
  `unblock-get-context` with `template: true` the first time you read an existing one. When the
  template prints the post title as the `h1`, the content starts at `h2` and the post title must be
  the page's real heading. When it does not, the first section holds the only `h1`. A template
  without a title, if the theme has one, can be set with `unblock-update-post`, with the user's
  agreement.
- The header and the footer belong to template parts (`template_id`, from `unblock-list-templates`).
  A template has no draft: a write to it shows at once. On a new site, build them with the first
  page and `live: true`, as the brief's confirmation approved. Otherwise change them only when the
  user asks.

## 2. Plan the page

Write the plan in a few lines and go on to the build: the user judges it on the draft, the next stop.
Wait for their agreement first only when they asked to see the plan, or when the page has no entry in
`sitemap.md`:

- **Job:** the visitor this page is for, the question they arrive with, and the one action the page
  asks for. Every section serves that action.
- **Sections:** usually 4 to 6, in order. For each one: its job in one sentence, its content, its
  layout in a few words.
- **Signature:** where the signature element of the design system appears. Once or twice on the page,
  not in every section.
- **Images:** what each one shows, how it is cropped, and where it comes from (see "Images" in
  step 5).
- **Missing facts:** what you need from the user and do not have (prices, opening hours, names,
  quotes, figures).

Give the page a rhythm: change scale, width and density from one section to the next, and put the
boldest move in one or two places.

## 3. Write the copy

- Real content from the brief and the user. No lorem ipsum.
- Never invent facts: testimonials, client names or logos, figures, prices, awards, addresses. Take
  them from the Proof and Offer of `brief.md` or from the user. Values the brief marks `[Invented]`
  (a demo) are used as given and listed in your report. When a fact is missing, write
  `[To confirm: what is needed]` in its place and list it in your report.
- Follow `voice.md` when it exists. Otherwise: plain words, short sentences, concrete nouns, the
  vocabulary of the trade.
- The headline says what the visitor gets or what the business does, not a slogan. No "Welcome to",
  no "Unlock", "Elevate", "Seamless", "Cutting-edge", "Next level".
- Buttons say the action and its result ("Book a table", "Get the price list"), not "Learn more",
  "Get started" or "Click here".
- One primary action. It can appear more than once, with the same words.

## 4. Compose the page

Compose the page as a whole, the way the direction in `design.md` describes it. When the subject
justifies it, use what the web does well today: type set very large or variable, an asymmetric or
broken grid, a full-bleed photo or a cinematic opening, large fields of saturated color, texture, an
image that crosses the edge of its section. Pick one or two such moves and let the rest stay calm.

Give each section the form its content asks for: a list when it is a list, a table when it compares,
a large photo when the place or the product sells, a text column when the page explains.

Then check the page against these defaults. Each one is fine when the brief gives a reason for it;
otherwise take its alternative:

1. A centered hero with a headline, a subtitle and two buttons over a gradient. Instead: an opening
   built on the subject, an image, a word set large, the product.
2. Every item in a rounded card with an icon, a title and two lines. Instead: a list, a table, or
   one large feature.
3. Every section with the same height, padding and heading. Instead: scale, width and density that
   change.
4. A small, widely spaced uppercase label above every heading, or steps numbered 01, 02, 03 by
   default. Instead: hierarchy from size and weight.
5. The model's house looks: a cream ground with a serif display and a terracotta accent; near-black
   with one acid accent; italic accent words; monospace labels; pill buttons everywhere. Instead:
   the direction recorded in `design.md`.
6. Figures, quotes or logos the user never gave. Instead: real content, or `[To confirm]`.
7. Emoji or a generic icon set as decoration. Instead: a motif drawn from the subject, or nothing.
8. The same fade-in on every section. Instead: one orchestrated moment, or none.

**Review before writing.** Most clients cannot see the page: get it right here. Read your HTML and
CSS as a designer reads a mockup:

- The plan: every section, image and action is there, in order.
- The list above, on the whole page.
- Every grid and flex layout at each breakpoint of `unblock-get-css`: what stacks, what shrinks,
  what could overflow.
- The longest word of each large heading, at its phone size, fits 390px less the side padding.
- Every text and ground pair passes contrast (step 6).

## 5. Build

Compose the whole page in one `unblock-insert-blocks` call: every section, its HTML and all its CSS,
the tokens the design needs included, as `unblock-get-instructions` asks for a new page. With
`post_id`, pass the `content_hash` the write returns to the next one.

**Look once, when you can.** With a browser tool, take one full-page screenshot at 390px and one at
1440px, in the same turn, and fix what is broken: overflow, overlap, an image or a font that does
not load, a section that strays from the plan. Fix by rewriting whole sections, not by nudging
values. Without a browser tool, go on: the review before writing and the markup check of step 6
stand in for it.

A design critique by a subagent with a fresh context runs only when the user asks for one: it
judges the screenshots against the direction, design quality and originality first, and names the
three worst problems. Otherwise the user's review of the draft is the critique.

Make independent calls together, in one turn: image uploads, font installs, screenshots.

Then give the user the draft's preview link, with your screenshots when you took them, and wait for
their go before the details (dynamic data, other pages). This is the third stop of a new site. Work
section by section only to redo one section.

**CSS**

- Every value from a token. When the design needs a value no token has, add a token by role (see
  `unblock-get-instructions`), never a raw value in a component.
- Classes are site-wide: a rule you send replaces that selector's declarations on every page. Reuse
  a class `unblock-get-css` lists when it fits as it is. For a variation, add a modifier class;
  never resend a shared selector with changes for this page.
- Before naming a new class, check it is free with `unblock-get-css` and `where.search`. Name it by
  what it is (`.menu-board`, `.opening-hours`), not by where it sits (`.section-2`, `.home-block`).
- A class you stop using keeps its rule stored. List it in your report and propose deleting it;
  delete it only once the user agrees, as `unblock-get-instructions` says.
- A class that matches no block in the editor needs `/* unbk scope:global:next */` before its rule
  to load on the page: a class added by a script, or a class used only inside a `{% if %}` branch.
- Lay out with grid and flex, sizes with `clamp()` and the tokens, breakpoints from
  `unblock-get-css`. Use CSS that is Baseline widely available.

**Images**

- Use the user's photos first: the media library (`unblock/list-media`) or files they give. Then the
  temporary photos the design step uploaded. Search for a new one only for a photo the page needs
  and the library lacks. To add an image, upload it from its URL (`unblock/upload-media`), never
  link to another site.
- Without the user's photos, use the stand-ins `design.md` records. When it records none, ask the
  user which way to go before building:
  - **Temporary photos** from a free library (Unsplash, not its paid Unsplash+ images; Pexels),
    chosen to match the art direction and the subject. Show them with their links, upload the ones
    the user keeps, and mark each as a placeholder in its title and description, with its source,
    so it is found and replaced before launch.
  - **Generated images**, when your client can make them: uploaded with `unblock/upload-media` and
    marked as placeholders the same way. They stand in for the real photos and are replaced before
    launch: a generated cake or shop must never pass for the business's own.
  - **A type-led page** with a drawn motif (an inline SVG taken from the subject), as `design.md`
    describes.
  - **Placeholder images**, as `unblock-get-instructions` describes them, at the size and ratio of
    the final photo, with an `alt` that says what the real photo will show, each listed in your
    report as `[To confirm: photo of …]`.
- Every image the plan lists is in the page, as a photo or its stand-in: never drop an image slot
  because the photo is missing. A page with no image is the user's choice, never a default.
- Write `<img src srcset sizes width height alt data-unbk-attachment>` with the values the tool
  returns. The width and height prevent layout shift.
- The main image at the top of the page: an `<img>`, not a CSS background, with
  `fetchpriority="high"` and no `loading="lazy"`. The images further down get `loading="lazy"`.
- `alt` says what the image shows that matters here. A decorative image gets `alt=""`.
- No text inside images.

**Motion and scripts**

- Motion when the direction calls for it: one orchestrated entrance or scroll moment, on
  `transform` and `opacity`. Under `prefers-reduced-motion`, a fade replaces the movement. When
  content reveals on scroll, use the reveal pattern of `unblock-get-instructions`.
- Native elements before scripts: `<details>` for disclosure, `<dialog>` for modals, `popover` for
  menus. A script only when HTML cannot do it.

**Dynamic content**

- Before reading a custom field, list the fields with `unblock/list-custom-fields`. Never guess a
  field name.
- Plan for empty and long values: a post with no featured image, a title on three lines, an empty
  field. Twig has no `{% else %}`: write the empty case as its own `{% if not … %}`.

## 6. Check

Run every check once, fix, and report what is left.

**Markup**, with `unblock/get-rendered-html` and `template: true`:

- One `h1` on the whole page, headings in order with no level skipped.
- Every image of the plan is there: count the `img` against the plan.
- Every `img` has `alt`, `width` and `height`; only the main image is not lazy.
- Every link has an `href` and a text that makes sense out of context.
- No empty value from a Twig expression (a typo gives an empty value, never an error).
- Every `[To confirm: …]` is in your report.

**Accessibility**

- Compute the contrast of each new text and ground pair with the WCAG 2.2 formula (4.5:1 for text,
  3:1 for large text and controls). Never estimate a ratio.
- Targets of at least 24 by 24 CSS pixels, 44 by 44 for the primary action on phones.
- Every interactive element reachable with the keyboard, with a visible focus.

Lighthouse and the checks at every width belong to the audits (`unblock-audit-accessibility`,
`unblock-audit-performance`).

## Report

End with:

- The page, its status (draft or published) and its sections in one line each.
- Every `[To confirm: …]` left in the page, and every temporary photo with its source.
- The checks run and their results, contrast ratios included.
- The SEO title and meta description: written with `unblock/update-seo` when the site has an SEO
  plugin, else the page title WordPress prints (see `unblock-content`).
- What you could not check, and what needs a person (reading with a screen reader, real content,
  the final images).

Publish only when the user asks.
