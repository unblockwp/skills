---
name: unblock-dynamic-data
description: Connects pages and templates built with Unblock to WordPress data, through the Unblock MCP tools. It covers lists of posts, products or custom post types, custom fields from ACF, Meta Box, Pods or JetEngine, terms, pagination, conditions and fallbacks, and the templates of a site (blog, archives, single posts, search). Use it when the user wants content that comes from the database instead of fixed text, a blog or archive layout, related posts, or a page fed by custom fields. Requires the Unblock MCP server.
license: MIT
compatibility: Requires an MCP client connected to a WordPress site running Unblock 1.0 or later.
metadata:
  version: "0.1.0"
---

# Unblock dynamic data

Content that comes from WordPress stays right when the user adds a post, changes a price or removes
an image. Write the Twig once, then check that it renders what you expect on real data: a wrong
expression gives an empty value, never an error message.

## Start

1. Call `unblock-get-instructions` once per session if you have not yet, in the same turn as your
   first read. It lists the Twig Unblock supports, with its filters, functions and data providers.
   What it does not list does not exist here, even if Twig or Timber has it: no `{% else %}`, no
   `include`, no macros, no `|e`.
2. Know the data before writing:
   - The posts of each type and their values: `unblock-list-posts`, with `fields: ["meta"]` for the
     custom fields.
   - The custom fields of a post type or a taxonomy: `unblock/list-custom-fields`. Each field gives
     its `read` expression: write that one. Never guess a field name.
   - The terms of a taxonomy: `unblock/list-terms`.
   - The templates of the theme: `unblock-list-templates`.
   - The content model: `unblock/list-post-types`, `unblock/list-taxonomies` and
     `unblock/list-field-groups`, each item with the field plugin keeping it.
3. When the project's kit has a `sitemap.md` (in the folder the session runs in, or a subfolder
   named after the site), read the page's "Dynamic content".
4. When the post type, the taxonomy or the fields the page needs do not exist:
   - Use one that fits first, from the content model above.
   - Otherwise, show the user what you will create or change (names, field types, choices) and wait
     for their agreement. The update tools replace what they change.
   - Make them through the site's field plugin, with the tools `unblock-get-instructions` names for
     it. When it names none, give the user the same list to create by hand.
   - With no field plugin on the site, propose the one `unblock-get-instructions` recommends, and
     install it only once the user approves.
   - Until they exist, say what the page shows, and build the dynamic part once they do.

   Then write the posts with their values in `meta`, as `unblock-get-instructions` says.

Tools are named here as your client calls them. A tool in its list goes by its MCP name,
`unblock-list-posts`, which your client may prefix. A name with a slash, `unblock/list-media`, is an
ability it does not list: run it through `execute-ability` with that name.

For a decision between a few options or an approval, use your client's tool for questions with
choices when it has one, after explaining the options in text. Ask open questions in text.

## 1. Choose where the data goes

- **A page** shows a selection: the three latest posts on the home page, the team, the next events.
  Write a loop on a query: `get_posts({...})`.
- **A template** shows whatever WordPress is displaying. Work on it with `template_id`; a write to it
  shows on the site at once, so pass `live: true` only once the user agreed:
  - The blog, archives and search (`home`, `archive`, `category`, `tag`, `search`, the archive of a
    post type): loop the main query with `{% for item in posts %}`. A `get_posts` there would ignore
    the category or the page of results the visitor asked for.
  - Single posts (`single`, or the single template of a post type): `post` is the post shown.
  - A template the theme lacks: create it with `unblock/create-template`, then build it.
  - A template as the theme ships it becomes the user's copy at the first write.
    `unblock/delete-template` brings the theme's version back.
- Static content stays static: a grid of features, pricing cards or steps are HTML, not a
  `get_items` loop.

## 2. Write the queries

- Only the arguments the section needs: the post type, the number, the order, a taxonomy or a field.
- Name the alias after what it holds (`event`, `member`, `product`), not `item` or `post`: it reads
  better and never hides the page's own `post`. Nested loops get a different alias at each level.
- A query used more than once goes in a variable, defined before its first use:
  `{% set events = get_posts({...}) %}`. Then loop it with `{% for event in events %}` and read
  `events.found_posts` or `events.pagination`.
- Values of the current post work inside the arguments: `'post__not_in': [post.id]`.

Common queries:

| Need | Query |
|------|-------|
| Latest posts | `get_posts({'post_type': 'post', 'posts_per_page': 3})` |
| Related posts, on a single template | `get_posts({'post_type': 'post', 'posts_per_page': 3, 'cat': post.category.id, 'post__not_in': [post.id]})` |
| Posts of one category | `get_posts({'post_type': 'post', 'category_name': 'news'})` |
| Items marked by a field | `get_posts({'post_type': 'event', 'meta_query': [{'key': 'featured', 'value': '1'}]})` |
| Categories | `get_terms({'taxonomy': 'category', 'hide_empty': true})` |
| Rows of a repeater field | `get_items({{ post.meta('steps') }})`, with the `read` expression of the field |

## 3. Paginate

- On the blog and archive templates, `{{ posts.pagination }}` prints WordPress's page links, and
  `{{ posts.found_posts }}` the number of results.
- On a custom query, set it in a variable with its `posts_per_page`, then use
  `{{ events.pagination }}`.
- For your own markup, loop `posts.pagination.pages` (each page has `link`, `title`, `current`) and
  use `posts.pagination.prev` and `posts.pagination.next`. Put it in
  `<nav aria-label="Pagination">`, give the current page `aria-current="page"`, and give the previous
  and next links a text, not only an arrow.

## 4. Plan for every value

Any field can be empty, long or unexpected. Write each case:

- **Empty value:** wrap the element in `{% if value %}` so no empty tag or label prints. Write the
  fallback as its own `{% if not value %}`. For a short text, a default inline:
  `{{ event.meta('venue') ?: 'Venue to be announced' }}`.
- **No results:** a loop with no items prints nothing. With the query in a variable, add the message:
  `{% if events.found_posts == 0 %}<p>No events planned yet.</p>{% endif %}`.
- **Images:** wrap them in `{% if event.thumbnail %}`, with `src`, `srcset`, `sizes`, `width`,
  `height` and `alt` from the image, as in the examples of `unblock-get-instructions`. A card's
  image next to its linked title keeps the library's alt text or none, never the title repeated.
- **Long text:** titles on three lines, words that do not break, long URLs. Shorten excerpts with
  `excerpt` or `truncate`; let headings wrap with `text-wrap: balance`, and break long words with
  `overflow-wrap: anywhere`.
- **Dates and numbers:** in the site's language and format, with `date` or `format_date`, and
  `number_format` or `format_currency`.
- **A variable number of items:** lay lists out with
  `repeat(auto-fill, minmax(var(--grid-min), 1fr))`, so one, two or seven items all look intended.

## 5. Style what the editor does not show

The editor renders one item of a loop and one branch of each condition. A class that appears only
where the editor does not render it, such as the no-results message or the fallback of an empty
field, matches no block there. Put `/* unbk scope:global:next */` before its rule so the page loads
it.

Inside `<style>`, `<script>` and `<svg>`, only `{{ }}` expressions work: put the loop or the condition
around the element.

## 6. Check on real data

After each write with Twig, render it with `unblock/get-rendered-html`:

- A page: with its `post_id`.
- The blog, an archive, a category, a search: with its `url` (`/?s=word` for a search).

Check that:

- Every expression gave a value: no empty tag, no `href=""`, no `src=""`.
- The number of items is right, and the page links appear when there are more results than one page.
- The hard cases render well: a post without a featured image, the longest title, a category with one
  post, the second page of results, a field left empty.

A whole list or every value of a post type reading empty often means the post type is kept off the
public site, which Unblock does not read until the user opens it in its privacy settings
(`unblock/list-post-types` says which). Tell the user; never make the post type public to get around
it.

When the site has no post for a case, say that it was not checked. Ask before creating posts to test
or to fill a list: they are content on the user's site. When the user agrees, create them with
`unblock-create-post` and their `meta`, and list them in your report so they are removed before
launch.

## Report

End with:

- What is dynamic on the page or template, and where each piece comes from (query, field, template).
- The cases checked, and on which URL or post.
- The cases not checked.
- The fields the user must fill for the page to look right, and any test posts to remove.
