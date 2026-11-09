# Changelog

All notable changes to the Novaris theme test data are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Added

- Post format test content: one post for each post format (aside, status, quote, link, image, gallery, video, audio, chat), a post with an unknown format, and a Post Formats category.
- Embed test content: three posts covering YouTube and Vimeo links, video and audio files (with a small `video.mp4` and `audio.mp3`), and links that should stay links, plus an Embeds category.
- Gallery test content: three gallery posts (full rows; captions, links and mixed shapes; two and four columns, one image, a missing image and small images), a gallery page, a Gallery category and nine gallery images. The readme explains the gallery markup.
- 34 posts covering titles, thumbnails, every Markdown element, long words and URLs, categories, authors, non-Latin and right-to-left text, excerpts, hidden posts and date archives.
- 14 categories, including one with many posts, one with exactly one, empty ones and one with a long name.
- 7 pages, including an empty page, a page without a title and nested pages.
- 5 test images in different shapes and sizes.
- A readme listing what each file tests and how to use the data with a theme.
- A `.gitignore` for the resized images Novaris writes into `media/`.
- Readme instructions for using the data with `NOVARIS_USER_PATH` (Novaris Framework 1.0.7 or newer).
