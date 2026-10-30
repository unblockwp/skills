# Code checklist

**[code]** from the CSS, the blocks and the rendered HTML, **[browser]** with a browser tool,
**[human]** by a person.

## HTML

- [ ] **[code]** Sections are `section` elements with a heading; content uses `article`, `nav`,
  `ul`, `figure` where they fit, not `div` everywhere.
- [ ] **[code]** Links are `<a href>`, actions are `<button>`; no interactive element inside another.
- [ ] **[code]** No duplicate `id` on a page.
- [ ] **[code]** Top-level layouts follow the section and container structure of
  `unblock-get-instructions`.
- [ ] **[code]** Components and landmarks have a `data-unbk-name`.
- [ ] **[code]** No inline `style` with more than one property; no inline event handler (`onclick`),
  which a strict content security policy blocks.

## Design tokens

- [ ] **[code]** Every color, space, size, radius, shadow and gradient in a component selector uses
  `var()`.
- [ ] **[code]** Token names describe a role, never a component (`--color-surface`, not
  `--color-hero-bg`).
- [ ] **[code]** No two tokens with the same value and the same role; ten color tokens at most.
- [ ] **[code]** Colors in `:root` are written in HSL.
- [ ] **[code]** Tokens sit in the Tokens collection; `@keyframes` in Animations.

## Selectors

- [ ] **[code]** Class selectors with low specificity; no `!important` outside the reduced-motion
  rule.
- [ ] **[code]** Class names say what a component is (`.opening-hours`), not where it sits
  (`.section-2`).
- [ ] **[code]** No shared selector restyled for one page; variations use a modifier class.
- [ ] **[code]** No two selectors that do the same job under different names.
- [ ] **[code]** No duplicate property in a rule.
- [ ] **[code]** `scope:global` only on rules that need it: base styles, classes added by scripts,
  classes used only inside a condition.
- [ ] **[code]** Media queries use the site's breakpoints.
- [ ] **[code]** CSS features are Baseline widely available, or have a fallback.

## Scripts

- [ ] **[code]** Vanilla JavaScript, no jQuery, no imports, not wrapped by hand (Unblock wraps it).
- [ ] **[code]** The content shows and works without scripts; a reveal effect hides through a utility
  class marked `editor:false`, never on the component's own class.
- [ ] **[code]** Scripts select inside their component and handle the keyboard along with clicks.
- [ ] **[browser]** No error in the console.

## Twig and data

- [ ] **[code]** Only the Twig `unblock-get-instructions` lists: no `{% else %}`, `include`, macros or
  unknown filters.
- [ ] **[code]** Empty values are handled with `{% if %}`, `{% if not %}` or a default.
- [ ] **[code]** Custom fields are read with the expression `unblock/list-custom-fields` gives.
- [ ] **[code]** No content written by hand that should come from the database, and no presentational
  grid built as a `get_items` loop.
- [ ] **[code]** The rendered HTML has no empty `href`, `src` or text where an expression should give
  one.

## Images, fonts, forms

- [ ] **[code]** Images come from the media library (`data-unbk-attachment`), with `srcset`,
  `sizes`, `width` and `height`; no image linked from another site; no placeholder left.
- [ ] **[code]** Fonts are installed on the site and referenced through their tokens.
- [ ] **[code]** Forms that collect data use `data-unbk-form`, with no `action` attribute.

## Network

- [ ] **[browser]** No 404, no failed request, no mixed content.

## Handover

- [ ] **[human]** Someone who did not build the site can find a component by its name and change a
  color, a font or a space from the tokens alone.
