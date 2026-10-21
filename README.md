# upward-content

Content source for **Insights** on upwardtech.nl: the articles, comparison and
roundup pages, and news that GTM writes for the Upward Technologies workspace.

This repo is content only. Rendering, SEO and the site build live in
`upwardtechnologies/website`. Nothing here is written by hand: GTM publishes it.

## Layout

```
blog/<slug>.json            one file per article (blog posts and AEO pages)
news/<slug>.json            one file per news item
site/homepage-posts.json    which posts the homepage features (GTM, Posts & Content, Homepage)
```

The filename without `.json` must equal the post's `slug`. The contract is the
same as `cryptool-io/cto-content`, so GTM publishes here exactly as it does there.

## A post

| Field          | Required | Notes |
|----------------|----------|-------|
| `slug`         | yes      | lowercase letters, numbers and single hyphens; equals the filename |
| `title`        | yes      | |
| `date`         | yes      | ISO 8601, UTC |
| `content_html` | yes      | semantic HTML; no `<script>`, `<style>` or inline styles |
| `type`         | no       | `blog` or `news`, must match the folder |
| `updated`      | no       | ISO 8601 |
| `author`       | no       | |
| `excerpt_html` | no       | short teaser, also the meta description |
| `hero_image`   | no       | absolute `https://` URL (`cover` is read as a fallback) |
| `tags`         | no       | string array; the first tag is the category |
| `list_items`   | no       | roundup pages: the products compared, in order |

## How it reaches the site

```
GTM  --PUT blog/<slug>.json-->  this repo
                                  |  notify-site.yml pings https://upwardtech.nl/api/insights/refresh
                                  v
upwardtech.nl reads this repo (public, no key), checks every post, renders
/en/insights/<slug> and /en/insights/news/<slug>, the sitemap, the RSS feed
and llms.txt. Without the ping it picks the change up within five minutes.
```

A post that fails the checks is left out of the site and logged; the rest of
the site stays up. `validate-content.yml` runs the same checks on every push,
so a bad file shows as a failed check here too.
