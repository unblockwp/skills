---
name: unblock-design-system
description: Creates or changes the design system of a WordPress site built with Unblock, through the Unblock MCP tools. It draws a visual direction from the site's subject, then writes the design tokens in :root (colors, type, spacing, radius, shadows), the fonts and the base styles every page shares. Use it before the first page of a site is built, when the site's CSS has no tokens yet, or when the user asks for a new look, a palette, fonts or a style direction. Requires the Unblock MCP server.
license: MIT
compatibility: Requires an MCP client connected to a WordPress site running Unblock 1.0 or later. A browser tool is optional: the directions are rendered on a draft the user opens.
metadata:
  version: "0.1.0"
---

# Unblock design system

Give the site one visual system before any page is built. Pages then read every color, size and
space from tokens. The look of the whole site changes with a few values, and it stays coherent when
the user or another AI session adds a page later.

Aim for a look a designer would sign: one that belongs to this subject and to its time, not a safe
template. Be ambitious where it counts, in one or two places (type at a large scale, a field of
color, a striking image, a layout move), and keep the rest calm.

## Start

1. Call `unblock-get-instructions` once per session if you have not yet, in the same turn as your
   first read. It holds the token naming, the CSS directives and the rules of the tools: follow it.
   This skill only adds the design method.
2. Read what exists:
   - `unblock-get-css`: the variables, fonts, breakpoints, global selectors and classes.
   - `unblock/get-global-styles`: the theme's palette, fonts and content width.
   - The active theme, whose name starts each template id of `unblock-list-templates`
     (`theme//slug`).
3. Check the theme. The Unblock theme is blank: every style then comes from your tokens. Another
   theme adds its own fonts, colors, spacing, header, footer and templates around them.
   - On a new site with a default WordPress theme, or a theme with a strong design of its own,
     recommend the Unblock theme before writing tokens, and say why in one sentence. The user
     installs it (from their Unblock account, under Appearance, Themes): no tool can.
   - When the user keeps their theme, say what it will still style, read its global styles, and plan
     to align them in step 7.
4. Pick the mode:
   - No tokens in `:root`: run every step below.
   - Tokens exist and the user asks for a page: the system is there, build the page
     (`unblock-build-page`).
   - Tokens exist and the user asks for a new look or a change: go to "Change an existing system".
5. The examples in `unblock-get-instructions` show the syntax, not a style. Do not reuse their
   values, their gradient hero or their centered layout.

Tools are named here as your client calls them. A tool in its list goes by its MCP name,
`unblock-get-css`, which your client may prefix. A name with a slash, `unblock/list-media`, is an
ability it does not list: run it through `execute-ability` with that name.

For a decision between a few options or an approval, use your client's tool for questions with
choices when it has one, after explaining the options in text. Ask open questions in text.

## 1. Gather the brief

You need:

- The subject, in concrete terms: what is sold, made or told, where, by whom.
- The audience, and what they come to do.
- The one action the site wants from a visitor.
- Fixed points: a logo, brand colors, fonts already licensed, a style guide.
- References the user likes, and what to take from each one.
- Constraints: the accessibility target (WCAG 2.2 AA unless told otherwise), languages, dark mode.

Read them from the project's kit first (`brief.md`, `design.md`, `AGENTS.md`) when your client has
files: in the folder the session runs in, or in a subfolder named after the site. When you find
none, ask the user where it is, or whether there is one, before working without it. Then read the
conversation. Ask only for what is missing, in one message, five questions at most. Never change a
logo or a brand color the user gave without asking.

**The user's ideas.** Read from `brief.md` or the conversation what the user already said: sites
they like, colors or fonts they like or refuse, a mood, their photos and how pages show the subject
without them (temporary photos from a free library, images your client generates, placeholder
images, or a type-led design with a drawn motif), and whether you should look for references. Ask
only what is missing, in one message. When the imagery is left open, take temporary photos and say
so. Record the imagery in `design.md`.

**References.** A design grounded in real sites the user likes beats one invented from words. Look
for references only when the user asked you to, and keep it to about five minutes:

- With a web tool, browse curated galleries by sector and by style: Awwwards (categories such as
  Food & Drink, Hotel / Restaurant, Fashion, Architecture) and Siteinspire (subjects and styles such
  as Typographic, Minimal, Grid Layout). Look at the first screen of each site, no deeper, and pick
  4 to 6 sites of the same trade or of the feel the brief asks for.
- Show each one with its link and, in one line, what it could give this site (a layout, a use of
  type, a way of showing products). Ask the user to keep two or three.
- Gallery sites often rely on heavy motion and effects. Take their art direction, not their effects.
- Without a web tool, ask the user to name two or three sites they like, even outside their trade.

Note the references kept, and what to take from each, in `brief.md` or `design.md`.

## 2. Ground the design in the subject

Before choosing any color, write 6 to 10 facts that belong to this subject and to no other:
materials, places, tools of the trade, its words, its season or hours, what the audience sees around
it. For a mountain guide office: rope and granite, a hand-drawn route on a topo, altitudes, a 4 a.m.
start. For a ceramic studio: raw clay, glaze test tiles, kiln temperatures, fingerprints in the rim.

Draw the palette, the type and the signature element from these facts. "Modern", "clean" and
"premium" are not directions: they lead every site to the same look.

## 3. Propose three art directions

Skip this step when the user gave a design (a `design.md`, a brand book, a Figma file, a site to
match). Follow it, and say where it fails the checks of step 4 or step 6.

Otherwise, propose three art directions that differ on at least two axes: light or dark ground,
serif or sans display, dense or airy, sharp or soft, quiet or loud, full-bleed or framed photos. For
each one:

- **Name and intent:** one sentence, tied to the facts of step 2.
- **References:** the kept references it draws from, and what it takes from each.
- **Composition:** how pages are laid out: an editorial page with columns and margins, a poster with
  type at full width, a menu board, a gallery of large photos, a catalogue. Say what changes from one
  section to the next, so the page has a rhythm.
- **Imagery:** what the photos show, how they are cropped, sized and treated (full bleed, framed,
  duotone, cut out), and which sections use them. The imagery the user chose holds for all three
  directions: they differ in how they treat images, not in whether the pages have any. Until the real
  photos exist, every direction uses the stand-ins the user chose. A direction is type-led, with a
  drawn motif (an inline SVG from the subject), only when the user asked for no photos: images are
  what a visitor notices first.
- **Palette:** 4 to 6 colors in HSL, each with its role (ground, text, muted text, accent, a second
  surface).
- **Type:** a display family and a text family, or one family for both, why they fit, and the size
  ratio.
- **Shape and space:** radius, density, borders or shadows, the rhythm of the sections.
- **Signature:** one element a visitor remembers (a typographic treatment, a rule, a photo crop, a
  texture, a block of color). One only.
- **Leaves out:** what this direction refuses.

Give exact values: colors in HSL, font names, radius, the order of the sections, motion durations.
A direction in adjectives turns into the model's default look.

**Photos, once for the site.** When the pages use temporary photos, choose them here, before
rendering: every photo the pages of `sitemap.md` need (the opening, the place, the people, the work),
in one pass on a free library, one search per subject. Pick from the descriptions the library gives,
and open an image only to settle a doubt. Upload them together with `unblock/upload-media`, each
marked as temporary in its title and description with its source, and list them in `design.md`. The
pages take them from the media library. Generated images: the same, once.

**Show them rendered.** Words hide what a direction looks like. Build the first screen of the home
page three times on one draft page, in one write, one per direction: a draft stays private, so no
need to ask first. Each section carries its direction's tokens on a class of its own
(`.direction-a { --color-surface: …; }`), so `:root` stays untouched. Check with
`unblock/list-fonts` that every weight, style and width the directions use has its face. Then give
the user the draft's preview link with the text of each direction: they compare them on their own
screen, which most clients cannot see.

This is the second stop. Ask the user to pick a direction or mix them, and in the same question
whether to delete the draft and its direction classes once the tokens are written. If the user says
"you choose", take the one that fits the brief best and say why in one sentence.

## 4. Guard against the default look

Generated sites converge on a few looks. Check the chosen direction against this list. Each item is
fine when the brief gives a reason for it; otherwise take its alternative:

1. An indigo, violet or blue-to-purple accent, gradients behind headings, blurred glowing shapes.
   Instead: colors taken from the facts of step 2.
2. A face chosen because it is common (Inter, Roboto, Poppins, Montserrat, the system font).
   Instead: a pair chosen for the subject, with a reason.
3. The model's house looks: a cream or off-white ground with a serif display and a terracotta
   accent; near-black with one acid accent; italic accent words in headlines; monospace labels and
   middle dots; pill buttons everywhere. Instead: the ground, type and shapes of the direction.
4. Every surface a rounded card with a soft shadow. Instead: what the content asks for, a list, a
   table, a full-bleed image, open space.
5. A small, widely spaced uppercase label above every heading. Instead: hierarchy from size and
   weight.
6. A page whose only rhythm is bands of the same height with the same heading. Instead: scale, width
   and density that change from one section to the next.
7. Invented figures or quotes, emoji or a generic icon set as decoration. Instead: real content, and
   a motif drawn from the subject.
8. The same fade-in on every section. Instead: one orchestrated moment, or none.

## 5. Write the tokens

Write them in one `unblock-update-css` call with no target, which writes the site's CSS. File the
`:root` block with `/* unbk collection:Tokens:next */`. The values are those of the direction the
user picked on its rendered section. Expect to adjust a few once the first whole page is built:
change values, keep names.

**Colors**

- Role names from `unblock-get-instructions` (`--color-surface`, `--color-text`, `--color-muted`,
  `--color-primary`, `--color-accent`, `--color-border`…), in HSL, 8 to 10 tokens.
- Tint the neutrals with the hue of the palette (a few percent of saturation) instead of pure gray.
- Hover and active states are tokens too (`--color-primary-hover`).
- A dominant color, used in large fields where the direction calls for it, and one sharp accent for
  the primary action.

**Type**

- Families as tokens, `--font-family-heading` and `--font-family-body`, each with a fallback stack.
  One or two families.
- Read the fonts the site has with `unblock/list-fonts` first, and prefer one of them.
- Install a new family with `unblock/install-fonts` or the `fonts` of `unblock-update-css`: a
  Google Font by its name, or a licensed font from files the user gives (`custom`). WordPress then
  serves its files from the site itself.
- A direction that uses widths (condensed or wide cuts, `font-stretch`) needs faces with those
  widths: read what `unblock/install-fonts` says about them before installing.
- An install activates every face of the family. Keep active only the weights and styles the design
  uses, with `unblock/update-fonts`: fewer faces, lighter pages. Deactivate the theme's fonts the
  design does not use the same way, once the global styles no longer set the text or the headings
  with them (the tool warns), and delete with `unblock/delete-fonts` a library font nothing uses.
- Sizes as a scale of 5 or 6 steps, `--font-sm` to `--font-3xl`, built from one ratio: 1.2 for dense
  text, 1.25 to 1.333 for most sites, more for a poster-like direction. Body text at 1rem or more,
  the two largest steps fluid with `clamp()`.
- Line height: about 1.5 to 1.7 for body text, 1.05 to 1.25 for large headings.

**Space, shape, depth**

- Spacing in multiples of 4px (0.25rem), 6 or 7 steps from `--spacing-xs` to `--spacing-3xl`. The
  padding of sections uses the top steps.
- Radius: one to three values that fit the direction. 0 is a valid choice.
- Shadows only if the direction uses depth. Otherwise, borders in `--color-border`.
- Gradient tokens only if the direction uses a gradient. The token table of
  `unblock-get-instructions` lists gradient names as options, not as a requirement.

**Base styles**, in the same call:

- `box-sizing: border-box` on every element, `*, *::before, *::after`. Unblock sets it only inside
  its sections, so the header, the footer and blocks of other plugins keep `content-box`, and the
  same component gets another size there.
- Headings: family, weight, line height, `text-wrap: balance`.
- Body text: `text-wrap: pretty`, and a measure of about 60 to 75 characters on text containers,
  never on the layout.
- Links: color, a visible underline in running text, a hover state.
- `:focus-visible`: an outline with 3:1 contrast against every surface it can sit on.
- `::selection` in the colors of the palette.
- A primary and a secondary button style, unless `unblock-get-css` already lists a button class:
  reuse it rather than adding another.

A rule on a selector that matches no block in the editor (`::selection`) or every element (`*`)
loads on every page only with `/* unbk scope:global:next */` before it. A tag, `:root`, `html` and
`body` are global already.

## 6. Check the result

**Contrast.** List each pair of text and ground the tokens allow: text, muted text and links on each
surface, the button labels on their colors, the focus outline on each surface. Compute each ratio
with the WCAG 2.2 formula: 4.5:1 for text, 3:1 for large text, icons, control borders and the focus
outline. Compute it from the token values, with a short script when your client runs code, and show
the numbers.
Change the lightness until each pair passes. Never estimate a ratio.

**Specimen, on request.** The rendered directions, then the first page, show the system at work.
When the user wants a reference page as well, ask before writing it, create a draft titled "Design
system" with the type scale in real copy, the colors with their names, both buttons, a link inside a
paragraph and a focused button, and give the classes only this page uses a `specimen` prefix: when
the user deletes the page, propose deleting their rules too.

**Default look.** Run the list of step 4 again on the CSS.

## 7. Record the system

The site's CSS is the source of truth: `unblock-get-css` reads it back in any later session.

When your client can write files, write or update `design.md` in the kit's folder: the intent of
the direction, its references, its composition and imagery, the facts of step 2, the signature
element, the tokens with their roles, the fonts, and a short list of do's and don'ts for pages.
With no kit folder yet, ask where to write it, as `unblock-site-brief` does. Without files, end your
reply with the same summary so the user can keep it.

Then align the theme. On a new site with the Unblock theme, set the same fonts and palette in
WordPress's global styles (`unblock/update-global-styles`), so the header, the footer and core blocks
share the look, and say so in your report. When the site keeps another theme, ask first: its own
templates change with them.

## Change an existing system

- Change values and keep names: every page reads the names. A rename needs the user's agreement and
  a search of each selector that uses the old name (`unblock-get-css` with `where.search`).
- A value changed in `:root` changes every page at once. Tell the user before writing it.
- Before removing a token, search for the selectors that use it.
- A renamed or replaced class or token leaves its old rule stored. Propose deleting the rules
  nothing uses any more, and delete them only once the user agrees, as `unblock-get-instructions`
  says: another page may still use one.
- Run the checks of step 6 on what changed.

## Report

End with:

- The direction, in two sentences.
- The tokens written, by group, the fonts installed, and the faces left active.
- Each contrast pair with its ratio.
- The draft to look at, and what you could not check.

Then the pages: `unblock-build-page` composes the first one on these tokens.
