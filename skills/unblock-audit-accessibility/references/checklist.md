# Accessibility checklist

WCAG 2.2, level AA. **[code]** from the rendered HTML and the CSS, **[browser]** with a browser tool,
**[human]** by a person.

## Structure

- [ ] **[code]** One `h1` per page, headings in order with no level skipped. *1.3.1, 2.4.6*
- [ ] **[code]** Landmarks: `header`, `nav`, `main`, `footer`; several `nav` each have a label.
  *1.3.1*
- [ ] **[code]** Lists are `ul` or `ol`; data tables have `th` and a caption. *1.3.1*
- [ ] **[code]** `lang` on `html` matches the content; a phrase in another language has its own
  `lang`. *3.1.1, 3.1.2*
- [ ] **[code]** Each page has a unique, descriptive `title`. *2.4.2*
- [ ] **[browser]** A skip link reaches the main content. *2.4.1*

## Images and media

- [ ] **[code]** Every `img` has an `alt`: what the image means here, or `alt=""` when decorative.
  *1.1.1*
- [ ] **[code]** Decorative SVG icons have `aria-hidden="true"`; meaningful ones have a name. *1.1.1*
- [ ] **[code]** No text inside images, logos aside. *1.4.5*
- [ ] **[code]** Every `iframe` has a `title`. *4.1.2*
- [ ] **[human]** Videos have captions, audio has a transcript. *1.2.2, 1.2.1*

## Links and controls

- [ ] **[code]** A link's text makes sense out of context. *2.4.4*
- [ ] **[code]** Links are `<a href>`, actions are `<button>`; no click handler on a `div` or a
  `span`. *2.1.1, 4.1.2*
- [ ] **[code]** The accessible name of a control contains its visible label. *2.5.3*
- [ ] **[browser]** Targets are at least 24 by 24 CSS pixels, or spaced enough. *2.5.8*

## Keyboard and focus

- [ ] **[browser]** Every interactive element can be reached and used with the keyboard, in a
  logical order. *2.1.1, 2.4.3*
- [ ] **[browser]** The focus is visible on every element, and a sticky header never hides it.
  *2.4.7, 2.4.11*
- [ ] **[browser]** No keyboard trap; a dialog keeps the focus inside while open and gives it back on
  close. *2.1.2*
- [ ] **[code]** No positive `tabindex`. *2.4.3*

## Color and contrast

- [ ] **[browser]** Text contrast at least 4.5:1, 3:1 for large text, computed. *1.4.3*
- [ ] **[browser]** Controls, focus outlines and meaningful graphics at least 3:1 against what is
  next to them. *1.4.11*
- [ ] **[code]** Information is not given by color alone; links in running text are underlined or
  marked otherwise. *1.4.1*

## Layout and zoom

- [ ] **[browser]** At 320 CSS pixels wide, no horizontal scroll and nothing cut off. *1.4.10*
- [ ] **[browser]** Text zoomed to 200% stays readable. *1.4.4*
- [ ] **[browser]** Larger line, letter and word spacing breaks nothing. *1.4.12*
- [ ] **[code]** The page works in both orientations. *1.3.4*

## Motion

- [ ] **[code]** `prefers-reduced-motion` turns off non-essential motion. *2.3.3, as good practice*
- [ ] **[code]** Anything that moves for more than 5 seconds can be paused. *2.2.2*
- [ ] **[code]** Nothing flashes more than three times a second. *2.3.1*

## Forms

- [ ] **[code]** Every field has a label; required fields say so in text. *1.3.1, 3.3.2*
- [ ] **[code]** Personal data fields have `autocomplete`. *1.3.5*
- [ ] **[browser]** Errors are named in text, tied to their field, with a way to fix them.
  *3.3.1, 3.3.3*
- [ ] **[code]** Logging in needs no puzzle or memory test without an alternative. *3.3.8*

## Dynamic content

- [ ] **[code]** Status messages are announced without moving the focus (`aria-live`). *4.1.3*
- [ ] **[code]** Accordions, tabs, menus and toggles expose their state (`aria-expanded`,
  `aria-selected`, `aria-current`). *4.1.2*

## People

- [ ] **[human]** The main task done with a screen reader (VoiceOver, NVDA), from the home page to
  the primary action.
- [ ] **[human]** Navigation and repeated components stay in the same place and keep the same names.
  *3.2.3, 3.2.4*
- [ ] **[human]** Help (contact, chat, phone) sits in the same place on every page. *3.2.6*
