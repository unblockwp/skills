---
name: unblock-audit-code
description: Reviews the code quality of a WordPress site built with Unblock, through the Unblock MCP tools and an optional browser tool. It covers semantic HTML, the design tokens and CSS selectors, scripts, Twig expressions, images and forms, and reports each finding with its evidence and its fix. Review only, it changes nothing. Use it when the user asks for a code review of a page or of the site's CSS, before handing a site over, or before launch.
license: MIT
compatibility: Requires an MCP client connected to a WordPress site running Unblock 1.0 or later; a read-only connection is enough. A browser tool is optional and used for console errors and failed requests.
metadata:
  version: "0.1.0"
---

# Unblock code audit

Check that the site is built so it lasts: semantic markup, one design system, CSS that does not fight
itself, scripts that degrade well, data that renders right. Review only: change nothing, whatever the
connection allows.

## Start

1. Call `unblock-get-instructions` once per session if you have not yet, in the same turn as your
   first read. Its rules are the standard this audit checks against: the token naming, the CSS
   directives, the structure of sections, the Twig Unblock supports.
2. Agree on the scope. By default: the site's CSS, and every page and template built with Unblock.

Unblock's tools are named here as your client calls them. A tool in its list goes by its MCP name,
`unblock-get-css`, which your client may prefix. A name with a slash, `unblock/list-media`, is an
ability it does not list: run it through `execute-ability` with that name.

## Tools

- `unblock-get-css`: the tokens, the selectors with their rules (`where.search` for full rules), the
  at-rules, the fonts and the breakpoints.
- `unblock-get-context`: the blocks of each page, with their attributes and scripts.
- `unblock/get-rendered-html`: the page as the visitor gets it, Twig resolved.
- `unblock-list-posts` and `unblock-list-templates`: what to cover.
- A browser tool: console errors, failed requests, mixed content.

## Run the checklist

Run [`references/checklist.md`](references/checklist.md). **[code]** items come from the CSS, the
blocks and the rendered HTML, **[browser]** items need a browser tool, **[human]** items go to the
"needs a person" list.

To find raw values in the CSS, read the rules of the component selectors and list each color, size,
space, radius or shadow written without `var()`. Report them by selector, with the token each one
should use.

## Rules for findings

- **Evidence:** **observed** (seen in the CSS, the blocks or the HTML), **measured** (returned by a
  tool: a console error, a 404), **inferred** (likely from the code, not confirmed).
- **The standard is `unblock-get-instructions` and this checklist**, not taste. A naming style that
  differs from yours is not a finding when it follows the rules.
- **One finding per cause.** A token missing for a value used in ten selectors is one finding, with
  the selectors listed.
- **Say where the cause lives:** Unblock content and CSS (fixable through MCP), the theme, another
  plugin. Report what is outside Unblock as out of scope.
- **Never call a selector unused** unless you checked every page and template that could use it,
  conditions and scripts included.

## Report

Start with a **verdict**: the state of the design system, the main risks for whoever maintains the
site, and what could not be checked.

Then each **finding**, the most severe first:

- **Severity:** Critical (broken output: a script error, an empty Twig value, content hidden without
  scripts), High (a rule broken across the site: raw values, component-named tokens, a shared
  selector changed for one page), Medium (a local break of the rules), Low (tidiness).
- **Rule:** the rule of `unblock-get-instructions` or the checklist item.
- **Where:** the pages, the component by its `data-unbk-name`, the selector.
- **Evidence:** what was found, and whether it was observed, measured or inferred.
- **Fix:** what to change: the token to add, the class to rename, the block to rewrite.
- **Recheck:** how to confirm it.

End with **not checked, needs a person** and **out of scope**.

## After the report

Fix only when the user asks, with a connection that can write, through `unblock-design-system` for
the tokens, `unblock-build-page` for the blocks, or the block tools. Renaming a token or a class
changes every page that uses it: search for them first with `unblock-get-css` and `where.search`.
Deleting a rule needs the user's agreement and the check that no page or template uses it,
conditions and scripts included.
