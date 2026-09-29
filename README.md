# The Thought Pattern — static site + Pages CMS

This version keeps the site fully static/GitHub Pages friendly while allowing content to be edited in Pages CMS.

## What was fixed

The original HTML contained the JSON files, but never loaded them. Pages CMS correctly writes changes to GitHub, but a static HTML file cannot automatically know that a JSON file changed.

This version:

- Loads `content/landing.json` into `index.html`.
- Loads `content/projects/index.json` to build the Work grid.
- Loads `content/projects/{slug}.json` into the single reusable `work-details.html` template.
- Supports project galleries, quotes, program features, impact stats and challenges.
- Uses relative asset paths at runtime so Pages CMS `/assets/...` paths also work on GitHub Pages project URLs.
- Keeps the original HTML as a visual fallback if JSON cannot be loaded.

## Pages CMS structure

`.pages.yml` now contains:

- `landing-page` → `content/landing.json`
- `projects-index` → `content/projects/index.json`
- `work-projects` → `content/projects/*.json`

The project collection excludes `index.json`, so the manifest is not treated as a project entry.

## How to update the site

### Landing page

In Pages CMS, edit **Landing Page**. Changes are written to:

`content/landing.json`

The homepage reads this file every time it loads.

### Existing projects

Edit any file in **Work Projects**. For example:

`content/projects/little-planet.json`

The URL is:

`work-details.html?project=little-planet`

You can use the same URL pattern for every project slug.

### Add a new project without coding

1. In Pages CMS open **Work Projects**.
2. Create a new project.
3. Set the `slug`, for example `new-client`.
4. Add the hero image, gallery images and project content.
5. Open **Project Listing** in Pages CMS.
6. Add the new project's `slug`, title, thumbnail and categories to the list.
7. Commit/save the changes in Pages CMS.
8. Wait for GitHub Pages to finish deploying.

The homepage will then show the project and link to:

`work-details.html?project=new-client`

No HTML file needs to be created for the new project.

## Important GitHub Pages check

Pages CMS writes changes to GitHub; it does not deploy the website itself. Your GitHub Pages deployment must publish the same branch that Pages CMS is editing.

If Pages CMS shows the new JSON in GitHub but the live website does not change, check:

1. GitHub Pages → Settings → Pages.
2. Confirm the deployment source/branch is the branch Pages CMS is committing to.
3. Confirm the Pages deployment has completed successfully.
4. Open the JSON file on the deployed site, for example:
   `content/projects/little-planet.json`
5. Hard refresh the browser.

The site adds a cache-busting query to JSON requests, so browser caching should not prevent updated JSON from being read once the new GitHub Pages deployment is live.

## Project JSON model

Each project can contain:

- `slug`
- `title`
- `meta`
- `categories`
- `hero`
- `lead`
- `programTitle`
- `programIntro`
- `programFeatures[]`
- `impactTitle`
- `impact[]`
- `challengesTitle`
- `challengesIntro`
- `challenges[]`
- `accent`
- `gradient`
- `wordmark`
- `gallery[]`
- `quotes.quote1`
- `quotes.quote2`

The Pages CMS editor exposes these fields through `.pages.yml`.

## Local testing

Because the site uses `fetch()` for JSON, do not test it by double-clicking `index.html` or `work-details.html` with a `file://` URL. Use a local HTTP server, for example:

```bash
python3 -m http.server 8000
```

Then open:

`http://localhost:8000/`

GitHub Pages already serves the files over HTTP, so no server-side application is required.
