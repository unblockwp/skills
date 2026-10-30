# Prompts

Prompts to copy into your AI client, grouped by job, each paired with the skill it runs. Replace
every `{placeholder}` with your own words.

| Job | File |
|-----|------|
| Plan the site | [plan.md](plan.md) |
| Set the look | [style.md](style.md) |
| Build pages | [build.md](build.md) |
| Write the text | [content.md](content.md) |
| Connect data | [data.md](data.md) |
| Review | [review.md](review.md) |
| Launch | [ship.md](ship.md) |

## What makes them work

- **They name the skill.** An agent does not always load a skill on its own. Writing "Use the
  unblock-build-page skill" makes sure it does.
- **They give facts, not adjectives.** "Six mountain guides in Chamonix, starts at 4 a.m., known
  for ski touring" gives the AI something to design from. "Modern and clean" gives it nothing.
- **They say what must not change.** A page, a section, the colors: name what stays.
- **They ask for a plan before a build** when the job is large, and for a report at the end.
- **One job per prompt.** Plan, then style, then build, then review.

The first prompt of each file comes in two versions, a weak one and a strong one, to show the
difference.
