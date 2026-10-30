---
name: unblock-audit-performance
description: Reviews the loading speed of a WordPress site built with Unblock against the Core Web Vitals (LCP, INP, CLS), with a browser trace and the Unblock MCP tools, and reports each finding with its measure, its cause and its fix. Review only, it changes nothing. Use it when the user asks why a page is slow, for a performance or Core Web Vitals audit, or for a check before launch.
license: MIT
compatibility: Requires an MCP client connected to a WordPress site running Unblock 1.0 or later; a read-only connection is enough. A browser tool that records performance traces (such as Chrome DevTools MCP) is needed for any measure.
metadata:
  version: "0.1.0"
---

# Unblock performance audit

Measure how fast the page gets to the visitor, find what slows it, and say who can fix it. Review
only: change nothing, whatever the connection allows.

## Start

1. Call `unblock-get-instructions` once per session if you have not yet, in the same turn as your
   first read: it tells you how Unblock writes images, fonts, scripts and CSS.
2. Agree on the scope. By default: the home page, the page of the main task, and one page of each
   template that matters (a post, the blog, an archive).
3. Measure the front end as a visitor gets it: logged out, with the page cache the site uses.

Unblock's tools are named here as your client calls them. A tool in its list goes by its MCP name,
`unblock-get-css`, which your client may prefix. A name with a slash, `unblock/list-media`, is an
ability it does not list: run it through `execute-ability` with that name.

## Targets

At the 75th percentile of real visits: **LCP** 2.5 s or less, **INP** 200 ms or less, **CLS** 0.1 or
less. Server response (TTFB) under 0.8 s.

A lab trace is a test on one machine, not real visits. Field data exists only for pages with enough
traffic. A new or local site has none: say so, and report lab numbers as lab numbers.

## Measure

With the browser tool, on each page:

1. Emulate a mid-range phone: a slower CPU (about 4 times) and a mobile network. Say which settings
   you used.
2. Record a trace of a fresh load (reload, stop when the page settles).
3. Read its insights: the LCP element and the breakdown of its time (server response, load delay,
   load duration, render delay), render-blocking requests, layout shifts and their causes, long
   tasks.
4. List the network requests: their weight, their origin (the site, a plugin, a third party), and any
   error or 404.
5. Repeat on desktop when the user cares about it.

When the site has real traffic and your client can reach the PageSpeed Insights API, read the field
data of the page and of its origin.

Lighthouse's categories in a browser tool may leave performance out: use the trace for performance.

## Read the cause in Unblock

Then find where each problem comes from, with `unblock/get-rendered-html` and `unblock-get-css`:

- **The LCP image:** an `<img>` in the HTML or a CSS background, `loading`, `fetchpriority`,
  `srcset`, `sizes`, `width` and `height`, its file weight against its displayed size.
- **Images further down:** lazy, sized, served from the media library.
- **Fonts:** how many families and faces are active (`unblock/list-fonts`), and whether they are
  served from the site or from another origin. A face nothing uses can be turned off with
  `unblock/update-fonts`.
- **CSS:** Unblock prints only the CSS a page uses, but rules marked `scope:global` load on every
  page. List the large ones.
- **Scripts:** block scripts on the page, and the scripts of the theme and other plugins.
- **Layout shifts:** images without dimensions, late fonts, banners or embeds inserted above content.

Run [`references/checklist.md`](references/checklist.md) for the rest. **[code]** items come from the
markup and CSS, **[browser]** items from the trace and the network, **[human]** items go to the
"needs a person" list.

## Rules for findings

- **Never guess the LCP element.** It comes from the trace. Without a trace, there is no LCP finding,
  only "inferred" candidates.
- **Evidence:** **Measured** (from a trace or the network), **observed** (seen in the markup or the
  CSS), **inferred** (read in code, not measured).
- **No gain promised from lab numbers.** Say what a fix should change and how to measure it. Real
  visits confirm it weeks later.
- **An insight with no measurable effect is noted, not recommended.**
- **Say where the cause lives:** Unblock content, the theme, a plugin, a third party, the hosting
  (server response, cache). Report what is outside Unblock as out of scope, with who can fix it.

## Report

Start with a **verdict**: the three metrics per page in a short table (lab or field, with the
settings), the main cause, and what could not be measured.

Then each **finding**, the largest effect first:

- **Severity:** Critical (a metric far over its target), High (a metric over its target), Medium (a
  metric close to its limit, or a large waste), Low (a small waste).
- **Metric:** LCP, INP, CLS or TTFB, and the measure.
- **Where:** the pages, the element or the request, the component by its `data-unbk-name`.
- **Evidence:** what was found, and whether it was measured, observed or inferred.
- **Fix:** what to change, and where (the block, the image, the CSS rule, the theme, the plugin, the
  hosting).
- **Recheck:** the trace or the measure that confirms the fix.

End with **not checked, needs a person** and **out of scope**.

## After the report

Fix only when the user asks, with a connection that can write, through `unblock-build-page` or the
block tools for what Unblock holds. Measure again after each fix, with the same settings.
