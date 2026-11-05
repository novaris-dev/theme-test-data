# Novaris Theme Test Data

Sample content for testing Novaris themes, similar to the WordPress theme unit test data. Every post, page and category here exists to test one thing a theme has to handle: long titles, missing images, every Markdown element, right-to-left text, pagination and so on.

It is a test fixture, not a theme or a package. Nothing here is installed into a theme or included in a theme's release.

## Using it

The folders match a site's `user/` folder:

```
content/   → user/content
media/     → user/media
```

### With `NOVARIS_USER_PATH` (Novaris Framework 1.0.7 or newer)

Clone this repository next to the site, then add one line to the site's `.env`:

```
NOVARIS_USER_PATH=../theme-test-data
```

The site now reads its content and media from this folder instead of `user/`, so `novaris export` (or `npm run dev` in a theme) builds the test content. Remove the line to go back to the site's own content. Nothing is copied, and the site's `user/` folder is not touched.

Resized images are written into `media/` next to the originals; `.gitignore` keeps them out of commits.

### By copying the files

With older versions of the framework, copy both folders into a **copy** of the site (never your real content), then run `novaris export` or open the site in a browser:

```bash
rm -rf user/content user/media
cp -r path/to/theme-test-data/content user/content
cp -r path/to/theme-test-data/media user/media
vendor/bin/novaris export
```

In GitHub Actions, check this repository out at a tag and copy it in the same way:

```yaml
- uses: actions/checkout@v4
  with:
    repository: novaris-dev/theme-test-data
    ref: develop # or a release tag
    path: theme-test-data
- run: |
    rm -rf user/content user/media
    cp -r theme-test-data/content user/content
    cp -r theme-test-data/media user/media
```

The content uses the content types most Novaris themes define, so the site's `config/content.php` should have:

- `post` in `_posts`, with the `blog` prefix, date archives, and the single path `{year}/{month}/{day}/{name}`
- `category` in `_categories`, as a taxonomy of posts

## What to check

Run the export first. It should finish without errors or PHP warnings, and every link should point at a page that exists. Then look through the pages below, mainly for layout problems such as text overflowing, broken images or empty headings.

### Posts

| Post | Tests |
|------|-------|
| Markdown Reference | Every Markdown element: headings 1–6, emphasis, strikethrough, links, nested lists, task lists, blockquotes, code blocks, tables (including a very wide one), definition lists, images, footnotes, horizontal rules, attributes and raw HTML. A `<script>` tag that should be escaped |
| Gallery | Six images in three columns, so every row is full |
| Gallery: Captions, Links and Mixed Shapes | Seven images of different shapes with captions, so the last row has one image, plus a gallery of images that link to the full-size file |
| Gallery: Columns and Edge Cases | Two- and four-column galleries, a one-image gallery, a gallery with a missing image, and images smaller than a column |
| Long title | A title long enough to wrap in lists, on the post, and in the document title |
| No title | No `title` in the front matter. Headings and links to it should not be empty |
| Special characters | Quotes, `<Tags>`, `&`, accents, emoji and symbols in the title. They should show as written and not turn into HTML |
| Thumbnail: Landscape, Tall Portrait, Very Wide, Square | Thumbnails in different shapes (1600 × 900, 600 × 1600, 2400 × 400, 800 × 800) |
| Thumbnail: Smaller Than the Image Sizes | A 120 × 80 thumbnail, smaller than any image size a theme defines |
| Thumbnail: Missing File | A thumbnail that points at a file that doesn't exist. No broken image, no error |
| No Thumbnail | A post without a thumbnail next to ones that have one |
| A Very Long Post | Fifteen sections, for reading width and spacing |
| A One-Line Post, An Empty Post | Very short and empty content |
| Long words and URLs | A title, a word and a URL with no spaces. They should wrap, not make the page scroll sideways |
| A Post in Many Categories | Eleven categories, for how the category list wraps |
| A Post With No Category | No `category` in the front matter |
| A Post in a Category With No File | A category with no file in `_categories` |
| A Post by an Author With No Profile | An author with no profile |
| Japanese, Arabic | Non-Latin text, and right-to-left text mixed with English |
| Starts With a List, Starts With Code, Only an Image | Excerpts built from content that doesn't start with a paragraph |
| A Hidden Post | `visibility: hidden`. It should not appear anywhere |
| Same Day: First Post, Second Post | Two posts on the same day, for the day archive |
| Archive Post 1–6, The Oldest Post | Posts from 2022 to 2024, so there are year and month archives and the blog needs several pages |

There are 37 posts in all (36 visible), so a blog showing 10 posts per page has four pages.

### Categories

| Category | Tests |
|----------|-------|
| General | Used by most posts, so its archive has several pages |
| One Post | Exactly one post |
| Gallery | The gallery posts |
| Empty Category, No Description | No posts. Their archives, and links to them, should still work |
| A Category With a Remarkably Long Name… | A name long enough to wrap |
| Uncategorized, Category 1–8 | Fill out the many-categories post and the category list |

### Pages

| Page | Tests |
|------|-------|
| Home (`index.md`) | The home page, with links into the test content |
| About | An ordinary page |
| Empty Page | A page with a title and no content |
| Gallery Page | A captioned gallery on a page, which often uses a wider template than posts |
| Page Without Title | A page with content and no title |
| Parent Page → Child Page → Grandchild Page | Nested pages, the deepest one as a single file rather than a folder |

### Galleries

Novaris has no gallery shortcode or block. A gallery is a `div` with a `gallery` class and a `gallery-columns-{n}` class, with one image per line and a blank line between them:

```markdown
<div class="gallery gallery-columns-3">

![First image](/user/media/gallery-01.jpg "Optional caption")

![Second image](/user/media/gallery-02.jpg)

</div>
```

Each image becomes a `<figure>`, with its title as the `<figcaption>`. A theme styles `.gallery` and the column classes it supports, usually as a grid. These posts check two, three and four columns.

### Media

`landscape.jpg`, `portrait.jpg`, `wide.jpg`, `square.jpg` and `small.jpg`, plus `gallery-01.jpg` to `gallery-09.jpg` in landscape, portrait, square and wide shapes. Each is labeled with its number or name and its size, so it's easy to spot when a theme crops or scales it.

## Adding test cases

When a theme breaks on something these files don't cover, add a post or page for it and a line to the tables above. Keep each file focused on one thing, and keep images small.

Use the `develop` branch for changes. Tag a release (`0.1.0`, `0.2.0` …) when themes should pick up new content, so a theme's tests only change when it moves to a newer tag.
