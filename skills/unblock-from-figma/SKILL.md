---
name: unblock-from-figma
description: Turns a Figma design into WordPress pages built with Unblock, using a Figma MCP server and the Unblock MCP tools together. It maps Figma variables to design tokens, rebuilds each frame as semantic, responsive sections, moves the images to the media library, then compares the result with the design. Use it when the user shares a Figma link or frame and wants it built in WordPress, or wants a site's tokens taken from a Figma file. Requires the Unblock MCP server and a Figma MCP server.
license: MIT
compatibility: Requires an MCP client connected to a WordPress site running Unblock 1.0 or later and to a Figma MCP server (Figma's own, or one using Figma's REST API). A browser tool is optional and used to compare the result with the design.
metadata:
  version: "0.1.0"
---

# Unblock from Figma

A Figma file is a picture of a page at one width, made of frames and text. A web page needs headings,
links, buttons, tokens, a layout that works at every width and states Figma does not draw. Read the
design for its intent and its measures, then build it the Unblock way.

## Start

1. Call `unblock-get-instructions` once per session if you have not yet, in the same turn as your
   first read or `unblock-create-post`, and follow it.
2. Read the site's CSS with `unblock-get-css`: its tokens, fonts, breakpoints and classes.
3. Read the file's structure once, with the Figma server's metadata tool, on the page or frame the
   user gave. Find the frames to build, a mobile version if there is one, and the components.
   When the link does not say which frame to build, list them and ask.
4. Keep Figma calls few: Figma limits them per day according to the plan and seat. One structure
   read, then one design read and one screenshot per section.

Unblock's tools are named here as your client calls them. A tool in its list goes by its MCP name,
`unblock-get-css`, which your client may prefix. A name with a slash, `unblock/list-media`, is an
ability it does not list: run it through `execute-ability` with that name.

## 1. Tokens first

Read the file's variables and styles (the Figma server's variables tool) before any section.

- **The site has no tokens:** create them from the file. Rename each one by its role, following
  `unblock-get-instructions` (`Brand/500` becomes `--color-primary`, `Spacing/24` becomes
  `--spacing-lg`). Colors in HSL. Write them in one `unblock-update-css` call with no target.
- **The site has tokens:** map each Figma value to the existing token it matches. Show the user the
  values that match none, and ask whether to add a token or use the nearest one.
- **The file has no variables:** collect the colors, sizes and spaces the frames use, group the
  values that are close, and propose a scale. Snap a value to the scale when it is within a few
  pixels, and list every value you snapped.
- **Fonts:** Figma names fonts by family and style (`Family:Medium`). Check `unblock/list-fonts`
  first, then install the family by its public name with `unblock/install-fonts` or the `fonts` of
  `unblock-update-css`, and use weights rather than style names. For a font that is not on Google
  Fonts, ask the user for its files and its license, and install it from them (`custom`). Keep only
  the faces the design uses active, with `unblock/update-fonts`.

## 2. Read each section

For each frame, in page order:

- Take its screenshot: it is the truth for the look.
- Read its design context: it gives measures, colors, text and structure. The code it returns
  (often React with Tailwind) is a source of values, not code to translate line by line.
- Copy the text as it is in the design. When the design holds placeholder text, keep it, mark it
  `[To confirm: real copy]`, and propose `unblock-content` afterwards.

## 3. Rebuild it the web way

**Semantics.** Figma has frames and text, the page needs elements. Decide each tag from its role:
the heading levels, `nav`, `ul` for a list, `<a href>` to go somewhere, `<button>` to act, `figure`
and `img` with an `alt`. One `h1` per page; `template` says whether the page's template prints one:
`unblock-create-post` returns it, `unblock-get-context` gives it with `template: true`.

**Layout.**

- Auto layout becomes flex: direction, `gap` from the spacing, alignment from the frame's
  alignment, "fill" as `flex: 1`, "hug" as the content's size, wrap as `flex-wrap`.
- Layout grids and card rows become CSS grid.
- Fixed widths become fluid: `max-width`, percentages, `clamp()`, and the container of
  `unblock-get-instructions`. Absolute positioning only where the design overlaps elements on purpose.

**Clean the file's artefacts.** Drop hidden layers, flipped or rotated wrappers, masks that only crop,
hard line breaks in text, and fixed heights on text boxes.

**Shapes drawn as vectors.** A button background, an underline, a divider or a card drawn as a vector
becomes CSS (background, border, `text-decoration`, a pseudo-element), not an image.

**Repeated frames.** Copied cards that are not components are still one component: one class, written
once.

**Images.**

- Upload each one to the media library with `unblock/upload-media`, from its URL, right after reading
  the section: Figma's image URLs expire.
- An image URL on `localhost` (a Figma server running on the user's machine) cannot be fetched by the
  site. Ask the user to export those images, or use URLs the site can reach.
- Icons become inline SVG: `aria-hidden="true"` when decorative, a `title` when they carry meaning.
- Write `<img src srcset sizes width height alt data-unbk-attachment>` with what the upload returns.

**States Figma does not draw.** Hover, focus, active, disabled, open and closed: derive them from the
tokens (`--color-primary-hover`, the focus style of the site) unless the file has them as variants.

**Responsive.** With a mobile frame, follow it. Without one, decide how each section stacks and how
the type scales, and tell the user it was inferred.

## 4. Build

Build one section per `unblock-insert-blocks` call, with its `html` and `css`, as `unblock-build-page`
does:

- Every value from a token.
- Classes are site-wide: reuse one that fits as it is, check a new name is free with
  `unblock-get-css` and `where.search`, and name it after what it is.
- A class that matches no block in the editor (added by a script, or only inside a `{% if %}`) needs
  `/* unbk scope:global:next */` before its rule.
- On a new page, create a draft first with `unblock-create-post`.

## 5. Compare with the design

With a browser tool, take a screenshot of the built section at the width of the Figma frame and put
it next to the Figma screenshot. List the differences (spacing, sizes, colors, line breaks,
alignment), fix them, and compare again. Stop after two rounds and report what is left.

Then check what Figma cannot show:

- Screenshots at 390px and 768px wide: no overflow, no overlap, readable text.
- Contrast of each text and ground pair, computed with the WCAG 2.2 formula (4.5:1 for text, 3:1 for
  large text and controls). When a color of the design fails, do not change it silently: tell the user
  and propose the nearest value that passes.
- Every interactive element reachable with the keyboard, with a visible focus.

Without a browser tool, say that the look was not compared, and point the user to the draft.

## Report

End with:

- The sections built, and the frame each one comes from.
- The token mapping: each Figma variable or value and the token it became, and the values snapped.
- What was inferred: responsive behavior, states, semantics the design did not show.
- The images uploaded, and the ones the user must provide.
- The differences left with the design, and the contrast pairs that fail.
