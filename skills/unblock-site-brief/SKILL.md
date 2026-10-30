---
name: unblock-site-brief
description: Plans a WordPress site before anything is built with Unblock. It interviews the user, reads what the site already has through the Unblock MCP tools, and writes the project kit (brief.md, sitemap.md, voice.md, AGENTS.md) that the design and page skills read, then waits for the user's confirmation. Use it at the start of a new site or a redesign, when the user describes a project, or when a page is requested and no brief exists.
license: MIT
compatibility: Works best with an MCP client connected to a WordPress site running Unblock 1.0 or later, and with access to the project's files. Without files, the kit is given in the reply.
metadata:
  version: "0.1.0"
---

# Unblock site brief

The brief decides what gets built and for whom. A page built on a clear brief needs fewer rounds, a
page built on a guess gets rebuilt. This skill writes the brief with the user, then stops: nothing is
built until they confirm.

## Start

1. When the Unblock MCP server is connected, read what the site already has, with
   `unblock-get-instructions` in the same turn if you have not called it this session:
   - `unblock/get-site-settings`: the title, the tagline, the front page.
   - `unblock-list-posts`: the existing pages, then the posts.
   - `unblock-list-templates`: the templates and template parts of the active theme, whose name
     starts each template id (`theme//slug`).
   - `unblock-get-css`: the tokens, if a design system exists.
   - `unblock/list-plugins`: the plugins installed, a field plugin among them (ACF, Pods, Meta Box,
     JetEngine), and an SEO plugin (Yoast SEO, Rank Math, SEOPress, All in One SEO).
   - `unblock/list-post-types`, `unblock/list-taxonomies` and `unblock/list-field-groups`: the
     content model, each item with the field plugin keeping it.
   - `discover-abilities`: what the site's other plugins let you do (Meta Box's own abilities, or
     the SEO plugin's, for example).

   Without the server, plan from the conversation only, and say so.
2. Look for an existing kit in the working folder and in a subfolder named after the site:
   `brief.md`, `sitemap.md`, `voice.md`, `design.md`, `AGENTS.md`. When it exists, update it and ask
   only what changed. When there is none, decide where it goes before writing anything (see "Where
   the kit goes").
3. The templates of each file are in `assets/`, next to this skill. Keep their headings.

Tools are named here as your client calls them. A tool in its list goes by its MCP name,
`unblock-list-posts`, which your client may prefix. A name with a slash, `unblock/list-media`, is an
ability it does not list: run it through `execute-ability` with that name.

For a decision between a few options or an approval, use your client's tool for questions with
choices when it has one, after explaining the options in text. Ask open questions in text.

## 1. Interview

Send one message with six questions at most, the ones everything else depends on:

1. What the business or project is, in the user's words, and where it operates.
2. Who the site is for (one to three audiences), and what each one comes to do.
3. The one action that matters most (book, buy, call, sign up, visit), and how the user will count
   it.
4. The pages they think they need, or the old site this one replaces.
5. Sites they like and what they like in each one, even outside their trade (links or screenshots
   say more than adjectives), and whether you should look for references yourself, which takes a
   few minutes and is off unless they ask.
6. Fixed points and limits: a logo, brand colors, fonts, photos (and without them: temporary photos
   from a free library, images the client generates, placeholder images, or a type-led design with
   no photos), languages, legal needs, a deadline.

The design step reads these answers from the kit and does not ask them again.

Take whatever the user already has instead of answers: an old site, a brochure, a pitch, notes. Read
it first, and do not ask what it already answers.

Then stop asking. Draft the kit from what you have, mark every guess, and let the user correct it.
A visible guess the user corrects is faster than a long questionnaire.

Mark a guess `[Assumption]` and an unknown `[To confirm: what is needed]`. When the user asks you to
invent content (a demo, a test site), mark each invented value `[Invented]`: pages use it as given,
and it is listed for replacement before launch.

## 2. Draft brief.md

Keep the kit short: facts and decisions, not prose. About a hundred lines for `brief.md`, a few
lines per page in `sitemap.md`.

- **Summary:** two or three sentences anyone in the business could repeat.
- **Goal:** one, with a number and a date ("30 bookings a month from the site by March").
- **Audiences:** for each one, who they are, the question they arrive with, what holds them back,
  and what convinces them.
- **Offer:** what is sold or told, prices or ranges when they are public, and what makes it
  different, in facts rather than adjectives.
- **Proof:** what exists and can be shown: reviews, clients, press, figures. Real items only, listed
  so that no page invents one.
- **References:** each site with what to take from it and what to leave.
- **Scope:** what this version includes, and what waits.
- **Constraints:** the accessibility target (WCAG 2.2 AA unless told otherwise), languages, legal
  needs (privacy, cookies, terms, rules of the sector), hosting, deadline.
- **Success criteria:** how the user will decide the site is done and working.

## 3. Draft sitemap.md

- **Each page:** its name and slug, its type (a page, the blog, an archive, the template of single
  posts), its purpose (the audience and the question it answers), its primary action, its sections in
  a few words, its dynamic content (post type, fields, taxonomy) and its status (to build, exists, to
  redo).
- **Shared parts:** the header and its navigation, the footer and its contents, the 404 page, search
  when the site needs it.
- **A small first version:** the pages the goal needs. The others go under "Later".
- **Existing pages** from `unblock-list-posts`: keep, redo or remove each one, decided with the user.
- **Dynamic content:** when a page lists items (posts, products, team members, events), name the
  post type and the fields. When the site has custom fields, read them with
  `unblock/list-custom-fields` instead of guessing their names. When they do not exist yet, say how
  they will be created (step 4) and what the page shows until then.

## 4. List the setup

Write a "Setup" section in `brief.md`: what must be in place before building, and who does it.

**You can do it, once the user confirms:**

- The site's title, tagline, front page and timezone: `unblock/update-site-settings`.
- The site's language, once its language pack is installed: the same tool.
- Plugins the user approved, installed from WordPress.org. Record them under "Plugins approved";
  install no other. With custom fields needed and no field plugin on the site, propose the one
  `unblock-get-instructions` recommends.
- Post types, taxonomies and custom fields, through the site's field plugin, with the tools
  `unblock-get-instructions` names for it.
- Pages, templates, CSS, fonts and media: the Unblock tools.

**The user does it:** no tool can.

- Install what the tools refuse or cannot: a plugin WordPress will not install from here (a host
  that allows no file changes, a multisite where a network admin installs), a theme, a language pack.
  In WordPress, choosing a language under Settings, General downloads its pack.
- Create post types and fields by hand when `unblock-get-instructions` names no tool for the site's
  field plugin: give the exact list (names, field types, choices).
- Open to Unblock, in its privacy settings, a post type kept off the public site whose posts feed
  other pages (the shop's details, for example).
- With Rank Math, run its setup wizard; with SEOPress, turn on its abilities for a page's social texts
  (see "The SEO plugin").
- Anything outside WordPress: the domain, the email accounts, the hosting.

**The theme.** On a new site, recommend the Unblock theme when the active theme is a default
WordPress theme or brings a strong design of its own. It is blank, so every style comes from the
design system and nothing in the theme has to be traced or overridden. Installing it is a user step
(downloaded from their Unblock account, uploaded under Appearance, Themes). Ask before the design
system starts. When the user keeps their theme, note it in `brief.md`.

**The SEO plugin.** Unblock works with Yoast SEO, Rank Math, SEOPress and All in One SEO. When the
site has one, keep it. When it has none, ask the user which one to install, or none, with what each
lets you do. The choice is theirs. With any of the four, you write each page's SEO title and meta
description with `unblock/update-seo`, on a local site as on a live one. Beyond them:

- **Yoast SEO:** its own abilities set who the site represents (an organization or a person), and on a
  site in production only, a page's social texts. Nothing to do on the site.
- **SEOPress:** its own abilities, a page's social texts among them, once the user turns on, in SEO,
  Advanced, "Expose abilities to AI agents and external tools". Until then you see none of them.
- **Rank Math:** its own abilities set the home page and the site-wide settings. The user runs its
  setup wizard once, connecting a free account or skipping that step: until then Rank Math prints no
  SEO tag on the site.
- **All in One SEO:** its own abilities set a page's focus keyphrase, robots and social texts, and the
  rules of robots.txt. Nothing to do on the site. On a page it never saved, such as one made over MCP,
  they answer "Post not found" until `unblock/update-seo` writes its title or description. Its
  analysis in the editor (TruSEO) finds no words on an Unblock page: its score says nothing of it.
- **None:** WordPress prints the page title, but no meta description and no social tags.

Check with `discover-abilities` which abilities the site gives you. Record the choice in `brief.md`
and install it like any plugin the user approved.

## 5. Draft voice.md

- **We are, we are not:** three to five pairs ("expert, not technical").
- **How we write:** "you" and "we", formality, contractions, sentence length, the case of headings.
- **Tone by context:** the home page, the offer pages, forms and error messages, legal pages.
- **Words:** the words of the trade to use, the words to avoid (jargon, clichés).
- **Examples:** two or three flat sentences rewritten in the voice, taken from the user's own
  material when possible.

When the user has no idea of a voice, derive it from the way they talk in the interview and from the
audience, and mark it `[Assumption]`.

## 6. Write AGENTS.md

Keep it under 20 lines. It tells the next session where things are, nothing more:

- The project in one sentence, the site's URL, the name of the MCP server in the client.
- Where the kit lives, and that it is read before building.
- The order of work: brief, design system, pages.
- A "Rules" list left empty. The user adds a rule when an agent repeats a mistake.

Do not fill it with general advice: what the kit or `unblock-get-instructions` already says does not
belong there. When the client reads another context file (`CLAUDE.md`, `GEMINI.md`, a rules folder),
create it in the kit's folder with one line that points to `AGENTS.md`. Never change a context file
you did not write: it may belong to another project.

`design.md` is written by `unblock-design-system`. Note in `brief.md` the fixed points it must
respect (logo, brand colors, fonts).

## 7. Confirm

Show a summary that fits on one screen: the goal, the audiences, the pages with their primary action,
the setup steps left to the user, the assumptions and the open questions. Ask the user to confirm or
correct it, and update the files until they confirm. Their confirmation also approves the setup the
brief lists (the plugins to install, the pages to trash) and the header and footer built with the
first page, which show on the site at once, as a template has no draft: do not ask for each again.

This is the first of three stops in a new site: the brief, the art direction, the draft of the first
page. Between them, decide with the defaults the skills give and say what you chose.

Before the confirmation, do not create a page, write CSS, install a font or change a setting.

After it, the next step is the design system (`unblock-design-system`), unless `unblock-get-css`
shows one and the user wants to keep it.

## Where the kit goes

- **By default, a new folder named after the site**, inside the folder the session runs in
  (`fournil-rouge/`): the kit never mixes with other files. Say where you wrote it.
- **The current folder**, when it already holds this site's kit.
- **Another folder**, or **no files** (the kit stays in your replies, and a later session will need
  it again), when the user asks for it.

When the folder the session runs in holds another project (code, a repository, another site's kit),
ask where the kit goes before writing anything there. Once the kit is written, tell the user to start
the next sessions in its folder, so their client reads `AGENTS.md`.

Without files on the client's side (claude.ai, ChatGPT and other hosted clients), give each file in
the reply as a Markdown block, and tell the user to add them to the knowledge of their project in the
client, so later sessions read them.

## Report

End with:

- The files written, and where.
- The setup steps left to the user.
- The assumptions and open questions left.
- The next step.
