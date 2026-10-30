# Unblock skills

Skills for AI agents that build WordPress sites with [Unblock](https://unblockwp.com) over MCP: plan
the site, set a design system, build pages that look designed by a person, then check that they are
accessible, fast and easy to find.

Each skill starts by calling `unblock-get-instructions`, which holds the rules of Unblock's tools,
and adds the method on top.

## Skills

**Plan and build**

| Skill | Does |
|-------|------|
| [`unblock-site-brief`](skills/unblock-site-brief/SKILL.md) | Interviews the user and writes the project kit (brief, sitemap, voice, AGENTS.md), then waits for confirmation before anything is built |
| [`unblock-design-system`](skills/unblock-design-system/SKILL.md) | Draws a visual direction from the site's subject, then writes the tokens, fonts and base styles every page shares |
| [`unblock-build-page`](skills/unblock-build-page/SKILL.md) | Plans a page from its purpose, writes real copy, composes it in one pass on the design system, reviews it before writing, then checks it |
| [`unblock-content`](skills/unblock-content/SKILL.md) | Writes and rewrites a page's text in the site's voice: headlines, body, buttons, forms, alt text, title and excerpt |
| [`unblock-dynamic-data`](skills/unblock-dynamic-data/SKILL.md) | Connects pages and templates to WordPress data: post lists, custom fields, terms, pagination, empty states, blog and archive templates |
| [`unblock-from-figma`](skills/unblock-from-figma/SKILL.md) | Turns a Figma design into responsive Unblock sections, its variables into tokens, its images into the media library |

**Check**

| Skill | Does |
|-------|------|
| [`unblock-audit-accessibility`](skills/unblock-audit-accessibility/SKILL.md) | Reviews pages against WCAG 2.2 AA |
| [`unblock-audit-performance`](skills/unblock-audit-performance/SKILL.md) | Measures the Core Web Vitals from a trace and finds their causes |
| [`unblock-audit-seo`](skills/unblock-audit-seo/SKILL.md) | Reviews indexing, titles, headings, links, images and structured data |
| [`unblock-audit-code`](skills/unblock-audit-code/SKILL.md) | Reviews the markup, the design tokens, the CSS selectors, the scripts and the Twig |

The audits change nothing: run them on a read-only connection when you can. Each one reports its
findings with their evidence (measured, observed or inferred), their fix, and the checks only a
person can do.

On a new site: brief, design system, then each page, then the audits.

## Planning kit

The templates the brief fills are in
[`skills/unblock-site-brief/assets/`](skills/unblock-site-brief/assets/): `brief.md`, `sitemap.md`,
`voice.md`, `design.md` and `AGENTS.md`. You can also fill them by hand.

## Checklists

Each audit has its checklist, which a person can also use as it is:
[accessibility](skills/unblock-audit-accessibility/references/checklist.md),
[performance](skills/unblock-audit-performance/references/checklist.md),
[SEO](skills/unblock-audit-seo/references/checklist.md),
[code](skills/unblock-audit-code/references/checklist.md).
Before going live, run the [pre-launch checklist](checklists/pre-launch.md).

## Prompts

Ready-to-use prompts for each job, paired with their skill: [prompts](prompts/README.md).

## Install

With the skills installer:

```sh
npx skills add unblockwp/skills
```

As a Claude Code plugin:

```text
/plugin marketplace add unblockwp/skills
/plugin install unblock-skills@unblock
```

Clients without skills support: paste the `SKILL.md` you need into the conversation or the project
instructions.

## Running design sessions

- Design in its own session: the brief, the design system and the pages with their content as
  text. Connect the dynamic data and run the audits in another session. A long session that mixes
  design, code and audits drifts toward a generic page.
- A browser tool is optional. With one (Chrome DevTools MCP takes full-page screenshots), the agent
  looks at each page once and fixes what is broken. Without one, you are the eye: the directions and
  each page land on a draft whose link the agent gives you.
- Answer the questions about references with screenshots or links rather than adjectives.
- Start with the model's reasoning effort at medium, and use a fast output mode when your client
  has one: in the sessions we measured, most of the time went to the model's reasoning, not to the
  tools.
- A new site has three stops for you: the brief, the art direction, the draft of the first page. The
  skills decide the rest with their defaults and say what they chose.

## Writing the skills

A skill says what to do, in which order, and when to ask the user. `unblock-get-instructions`, served
by the plugin and versioned with it, says which tool does it and how. Name a tool in a skill only
when the method needs it, and never copy the guide's rules for a plugin or a tool: when Unblock
changes, the guide changes with it, and the skills stay right for every version.

## Requirements

An MCP client connected to a WordPress site running Unblock 1.0 or later. A browser tool is
optional for design, and needed for the visual, keyboard and performance audits.

## License

MIT
