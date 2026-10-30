# Review

Run reviews on a read-only connection when you can: the audit skills change nothing anyway.

## Accessibility audit

**Skill:** `unblock-audit-accessibility`. **You get:** a report against WCAG 2.2 AA, each finding with
its evidence, and the checks only a person can do.

Weak:

```
Is my site accessible?
```

Strong:

```
Use the unblock-audit-accessibility skill.
Audit {the home page, the contact page and one blog post} against WCAG 2.2 AA, on the front end,
logged out. Use the browser tool for keyboard, contrast and zoom.
Compute every contrast ratio, mark each finding as measured, observed or inferred, and change
nothing.
```

The weak version invites a yes or a no. The strong one sets the scope, the standard and the evidence
expected, which keeps the AI from inventing findings or calling the site compliant.

## Performance audit

**Skill:** `unblock-audit-performance`. **You get:** LCP, INP and CLS per page from a trace, and the
cause of each problem.

```
Use the unblock-audit-performance skill.
Measure {pages} on an emulated mid-range phone, logged out, and tell me the settings you used.
Find the LCP element from the trace, and say for each problem whether it comes from the page, the
theme, a plugin or the hosting. Change nothing.
```

## SEO audit

**Skill:** `unblock-audit-seo`. **You get:** what stops pages from being found or understood, and
where to fix it.

```
Use the unblock-audit-seo skill.
Audit every page of the sitemap: indexing, titles, descriptions, headings, links, images and
structured data. Tell me which plugin writes the head, and what to fix there or in the pages.
Change nothing.
```

## Code review before handover

**Skill:** `unblock-audit-code`. **You get:** the state of the design system and of the markup, for
whoever maintains the site next.

```
Use the unblock-audit-code skill.
Review the site's CSS and every page built with Unblock before I hand the site over.
List raw values that should be tokens, classes restyled for one page, scripts that break without
JavaScript and Twig that can render empty. Change nothing.
```
