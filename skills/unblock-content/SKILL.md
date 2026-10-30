---
name: unblock-content
description: Writes and rewrites the text of pages built with Unblock, through the Unblock MCP tools, in the site's voice. It covers headlines, body copy, buttons, form labels and messages, alt text, page titles and excerpts, and the SEO title and meta description through the site's SEO plugin. Use it when the user asks to write, rewrite, shorten, proofread or change the tone of a page or a section, or to fill the [To confirm] markers a build left. It changes text only, never the layout or the styles. Requires the Unblock MCP server.
license: MIT
compatibility: Requires an MCP client connected to a WordPress site running Unblock 1.0 or later. A browser tool is optional and used to check how the text fits at phone width.
metadata:
  version: "0.1.0"
---

# Unblock content

Write text that tells the visitor what they get, in the words of the business, and leads them to
the page's one action. Change the words, not the page: the layout and the styles stay as they are.

## Start

1. Call `unblock-get-instructions` once per session if you have not yet, in the same turn as your
   first read. It holds the rules of the blocks and the tools: follow it. This skill only adds the
   writing method.
2. Read the project's kit when your client has files: `voice.md` first, then `brief.md` (audiences,
   offer, proof) and the page's entry in `sitemap.md`, in the folder the session runs in or in a
   subfolder named after the site. When you find none, ask the user where it is.
3. Read the page with `unblock-get-context`: every text, by section, with its `bid`.
4. Write in the site's language: the brief's, or `language` from `unblock/get-site-settings`.

Without `voice.md`, derive the voice from the site's existing copy and from the brief, state it in
three lines before writing, and suggest saving it with `unblock-site-brief`.

Tools are named here as your client calls them. A tool in its list goes by its MCP name,
`unblock-get-context`, which your client may prefix. A name with a slash, `unblock/list-media`, is
an ability it does not list: run it through `execute-ability` with that name.

## 1. Read before writing

For each section, note what it says, what the visitor needs at that point, and what is wrong:

- A vague claim where a fact would do.
- Filler, jargon, or words the audience does not use.
- A promise with no proof next to it.
- A heading that does not say what the section is about.
- A button that does not say what happens.
- The same idea repeated in two sections.
- Text longer than the visitor will read.

Read only the headings and the buttons, in order. They should tell the page's story on their own.

## 2. Agree on the changes

- A small change (a headline, a button, one paragraph): apply it.
- More than one section: show the before and after of the headlines and buttons first, and wait for
  the user's agreement, unless they asked you to apply directly.
- Text that needs another structure (a list instead of a paragraph, a missing section, a block of
  another plugin to replace): say so and propose it. Apply it with `unblock-replace-blocks`, keeping
  every `bid`, only once the user agrees, or hand it over to `unblock-build-page`.

## 3. Write

**Order of a page:** what the visitor gets, then how it works, then the proof, then the action.

**Headlines**

- Say what the business does or what the visitor gets, in the visitor's terms.
- Ten words or fewer for the main one. A clear headline beats a clever one.
- No question as a heading unless it is the visitor's real question (a FAQ).

**Body**

- One idea per paragraph, two to four sentences.
- Short sentences, with some longer ones so the text does not sound clipped.
- Concrete nouns, real numbers from the brief, examples from the trade.
- Active voice. Talk to the visitor ("you") more than about the business ("we").

**Proof**

- Only what `brief.md` lists or the user gives: reviews, clients, figures, awards, with their source.
- A quote keeps the words and the name the user gave. Never polish a real quote.

**Buttons and links**

- A verb and what it gets: "Book a table", "Download the menu". Never "Learn more", "Click here" or
  "Submit" alone.
- The same action keeps the same words everywhere on the site.
- A link's text makes sense read alone.
- A button label fits on one line on a phone.

**Forms**

- A label says what to enter. Help text sits before the field it explains.
- An error message says what went wrong and how to fix it, without blame.
- Required fields are marked in words, not by color alone.

**Avoid**

- Inflated words: pivotal, game-changing, revolutionary, transformative, world-class.
- Filler adjectives: seamless, vibrant, cutting-edge, robust, innovative, breathtaking.
- Stock verbs: unlock, elevate, empower, leverage, streamline, delve.
- Stock openers: "Welcome to", "In today's fast-paced world", "Whether you're X or Y".
- Lists of three by reflex ("fast, simple and powerful"), and "not just X, it's Y".
- Vague sources ("experts say"), hedging ("it's worth noting"), exclamation marks, emoji.
- "Serves as", "stands as" where "is" works.

**Accessible text**

- Plain words. Spell out an abbreviation the first time it appears.
- No instruction that depends on shape, color or position ("the green button on the right").
- A phrase in another language gets a `lang` attribute on its element
  (`unblock-update-block-attributes`).

**Alt text**

- Say what the image shows that matters on this page, in one sentence. No "image of".
- A decorative image keeps `alt=""`.
- For a featured image, the theme reads the alt text from the media library: update it there with
  `unblock/update-media`.

**Title and excerpt**

- The page title says what the page is, with the words visitors use for it.
- The excerpt sums up the page in one sentence.
- The meta description tells a reader, in one or two sentences, what they get on the page. Write it
  when the site has an SEO plugin. When a page has none, the plugin builds one from the page's fixed
  texts, never from its loops, its conditions or a text holding an expression.
- Write for people. Never repeat a term to rank, and never fill a page with text nobody needs.

**Length**

Keep about the length of the text you replace unless the user asks otherwise: the layout was built
around it. In languages that run longer than English, such as French or German, shorten rather than
let buttons and headlines wrap.

**Facts you cannot state**

Legal terms, health, financial or safety claims, prices and guarantees come from the user. When one
is missing, write `[To confirm: what is needed]`, and list it in your report. Fill a `[To confirm]`
marker only with a fact the user or the brief gives.

## 4. Apply

- The text of a block: `unblock-update-block-content` with its `bid`. Keep the inline markup
  (`<strong>`, `<em>`, `<a href>`) and every Twig expression (`{{ … }}`) exactly as they are.
- An attribute (`alt`, `aria-label`, `title`): `unblock-update-block-attributes`.
- The page's title or excerpt: `unblock-update-post`. Change a slug only if the user agrees: it
  breaks the links to the old address.
- The SEO title and meta description: `unblock/get-seo` reads them, then `unblock/update-seo` writes
  them (`execute-ability`), the same way with Yoast SEO, Rank Math, SEOPress and All in One SEO.
  Plain text, or the plugin's variables (`%%sitename%%`, `#site_title`) when the user uses them; an
  empty string gives one back to the plugin's default. With no SEO plugin, these tools are absent:
  WordPress prints the page title and no description. With All in One SEO, write them before using
  its own abilities on the page: they answer "Post not found" for a page it never saved.
- The text of a block of another plugin cannot be changed in place. Propose to replace that block
  with Unblock elements (see step 2).
- With `post_id`, pass the `content_hash` each write returns to the next one.
- A write to a published or private page, or to a template, is refused until you pass `live: true`,
  once the user agreed. Before a redesign, offer a draft copy (`unblock/copy-post`) or an editor
  session the user opens on the page.

## 5. Check

- Read the page back with `unblock/get-rendered-html`: the new text is there, every Twig expression
  still renders, the headings are in order.
- Run the "Avoid" list on every text you wrote.
- Read the headings and buttons alone, in order, once more.
- With a browser tool, look at the page at 390px wide: buttons on one line, main headline on three
  lines at most.

## Report

End with:

- The changes by section, with the before and after of each headline and button.
- Every `[To confirm: …]` left in the page.
- What a person must review: legal wording, claims, quotes.
- What you could not check.
