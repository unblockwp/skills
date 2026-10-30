# Connect data

## Design the blog

**Skill:** `unblock-dynamic-data`. **You get:** the blog, archive and single post templates, with
pagination and empty states, checked on real posts.

Weak:

```
Make a blog.
```

Strong:

```
Use the unblock-dynamic-data skill.
Build the blog templates on the site's design system: the posts page and the category archive with
{number} posts per page and pagination, and the single post with its author, date, category and
three related posts.
Handle posts without a featured image and long titles. Check them on real posts and report the
cases you could not check.
```

## List a custom post type

**Skill:** `unblock-dynamic-data`. **You get:** a section that lists items with their custom fields.

```
Use the unblock-dynamic-data skill.
On the {page name} page, list the {post type, such as team members or events} with
{the fields to show}. Read the field names from the site, do not guess them.
Order them by {order}. Show a message when there are none, and handle empty fields.
```

## Show related content

**Skill:** `unblock-dynamic-data`. **You get:** a related section on single posts that leaves out the
post being read.

```
Use the unblock-dynamic-data skill.
At the end of the single {post type} template, show {number} related items from the same
{category or taxonomy}, without the current one. When there are none, show {fallback}.
```
