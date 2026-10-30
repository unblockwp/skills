---
name: unblock-audit-accessibility
description: Reviews the accessibility of a WordPress site built with Unblock against WCAG 2.2 AA, through the Unblock MCP tools and an optional browser tool, and reports each finding with its evidence and its fix. Review only, it changes nothing. Use it when the user asks for an accessibility audit, a WCAG check, a review of a page or a template, or a check before launch.
license: MIT
compatibility: Requires an MCP client connected to a WordPress site running Unblock 1.0 or later; a read-only connection is enough. A browser tool (such as Chrome DevTools MCP) is needed for the keyboard, contrast and layout checks.
metadata:
  version: "0.1.0"
---

# Unblock accessibility audit

Find what stops people from using the site, prove each finding, and say how to fix it. Review only:
change nothing, whatever the connection allows.

## Start

1. Call `unblock-get-instructions` once per session if you have not yet, in the same turn as your
   first read: it tells you how Unblock writes blocks, names components and stores CSS.
2. Agree on the scope with the user. By default: the home page, one page of each template (a page, a
   post, the blog, an archive, search, the 404) and the pages of the main task (contact, booking,
   checkout).
3. Audit the site as a visitor gets it: the front end, logged out.

Unblock's tools are named here as your client calls them. A tool in its list goes by its MCP name,
`unblock-get-css`, which your client may prefix. A name with a slash, `unblock/list-media`, is an
ability it does not list: run it through `execute-ability` with that name.

## Tools

- `unblock/get-rendered-html` with `template: true` (or with the page's `url`): the whole page as the
  visitor gets it, header and footer included.
- `unblock-get-context`: the blocks of a page, with the `data-unbk-name` of each component.
- `unblock-get-css`: the tokens, the focus styles, the media queries and the motion rules.
- `unblock/list-media`: the alt text the media library keeps.
- A browser tool: the accessibility tree, screenshots at several widths, the keyboard (Tab, Enter,
  Escape), scripts that read computed styles, and Lighthouse's accessibility category when it runs it.

## Run the checklist

Run [`references/checklist.md`](references/checklist.md) on each page in scope. Each item says how it
can be checked:

- **[code]** from the rendered HTML and the CSS.
- **[browser]** with a browser tool. Without one, the item is "not checked". Code can at most make it
  "inferred".
- **[human]** by a person. It goes to the "needs a person" list, never into a verdict.

## Rules for findings

- **Evidence:** each finding says how it is known. **Measured:** a tool computed it (a contrast from
  computed styles, a target size from its box). **Observed:** seen in the markup, the accessibility
  tree or a screenshot. **Inferred:** read in the code, not confirmed in a browser.
- **Never state a contrast ratio you did not compute.** Never guess what a screen reader announces:
  read the accessibility tree.
- **Never say the site "is WCAG compliant" or "conforms".** An automated or AI review covers part of
  WCAG, and a Lighthouse score of 100 is not conformance.
- **One finding per cause, not per instance.** Group by component, named by its `data-unbk-name`,
  and list the pages where it appears.
- **Say where the cause lives:** Unblock content (fixable through MCP), the theme, another plugin, or
  WordPress core. Report what is outside Unblock as out of scope, with who can fix it.
- **Check the intent before flagging.** `alt=""` on a decorative image is right. A `<main>` or a skip
  link missing from the content is the theme's job, not the page's.

## Report

Start with a **verdict**: one paragraph with the main barriers, how many pages were checked, and what
could not be checked.

Then each **finding**, the most severe first:

- **Severity:** Critical (a task cannot be done, such as a form unusable with the keyboard), High
  (a task is very hard for some people), Medium (a barrier with a workaround), Low (friction).
- **Standard:** the WCAG 2.2 success criterion, number and name (1.4.3 Contrast (Minimum)).
- **Where:** the pages, the component by its `data-unbk-name`, the selector.
- **Evidence:** what was found, and whether it was measured, observed or inferred.
- **Fix:** what to change, and where (the block, the CSS rule, the theme, the plugin).
- **Recheck:** how to confirm the fix.

End with:

- **Not checked, needs a person:** the [human] items, and the [browser] items when no browser tool was
  available.
- **Out of scope:** findings from the theme, plugins or core.

## After the report

Fix only when the user asks. Fixing needs a connection that can write, and goes through the page and
content skills (`unblock-build-page`, `unblock-content`) or the block tools. Recheck each fixed
finding and update the report.
