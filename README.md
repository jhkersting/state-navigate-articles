# State Navigate articles

This repository hosts `https://articles.statenavigate.com` through GitHub Pages.
Authors write and publish in WordPress at `statenavigate.org`. GitHub Actions
fetches published posts hourly and builds the full site with the canonical
renderer in `jhkersting/state-navigate`. Shared CSS, scripts, navigation, and logo
assets resolve from `https://projects.statenavigate.com/`.

The HTML retains each WordPress permalink path, with the new article origin for
canonical metadata, related articles and links between migrated posts. Images,
audio and existing interactive iframe URLs retain their original hosts.

## Initial activation

1. Publish the renderer, URL helper and renderer tests in `jhkersting/state-navigate`;
   hold the main site's link migration until the new domain works.
2. Create `jhkersting/state-navigate-articles` from this local repository.
3. Enable GitHub Pages with the GitHub Actions build source and set its custom
   domain to `articles.statenavigate.com`.
4. Add the `articles` DNS CNAME for `statenavigate.com`, targeting
   `jhkersting.github.io`; verify HTTPS before redirecting readers.
5. Run **Refresh and deploy WordPress articles** and verify the site.
6. Publish the main site's article-link migration and configure WordPress 301
   redirects using the generated `redirects.json`. Redirect only known post
   paths: keep REST APIs, media uploads, editor, authors and taxonomy pages on WordPress.

Each build generates a fresh deployment artifact. Failed fetches or invalid
article content stop deployment and leave the previous site intact. Rebuilding
the full artifact removes unpublished or deleted posts from the next release.
The source checkout is read-only; Actions does not commit generated files.
`SOURCE_REPO_SSH_KEY` is a dedicated read-only deploy key for the private
`state-navigate` repository, not an account-wide token.

`manifest.json` records WordPress sources, public URLs, modification timestamps,
output paths, hashes and warnings. `sitemap.xml` lists published article URLs.
No credentials or WordPress draft content are shipped in the site artifact.
