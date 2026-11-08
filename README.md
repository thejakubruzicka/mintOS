# MintOS26 website

A static, single-page site for MintOS26 and MintDE. Plain HTML, CSS and JavaScript, with no build step and no dependencies. It works on GitHub Pages as-is.

```
website/
├── index.html              the page
├── 404.html                "page not found" page for GitHub Pages
├── .nojekyll               tells GitHub Pages to serve files untouched
├── css/style.css           all styles (dark by default, follows light mode)
├── js/main.js              menu, screenshot tabs, copy buttons, demo Terminal
└── assets/
    ├── favicon.png, apple-touch-icon.png
    ├── icons/              MintDE app icons, resized to 128 px
    └── screenshots/        JPEGs made from test-evidence/ and docs/evidence/
```

## Preview it locally

From the repository root:

```sh
python3 -m http.server 8000 --directory website
```

Then open http://localhost:8000.

## Before you publish

1. **Put in your GitHub username.** Every GitHub link uses the placeholder `YOUR-GITHUB-USERNAME`. Find and replace it across `website/` (in VS Code: Find in Files). It appears in `index.html` only.
2. **Check the repository name.** Links assume the repo is called `MintOS26`. If you name it something else, also update the `/MintOS26/` paths in `404.html`.
3. **Check the wallpaper.** The screenshots show the wallpaper from `assets/wallpaper-*.png`. It appears to be Apple's macOS High Sierra wallpaper. If you don't have the rights to share it, use your own wallpaper in MintOS, retake the screenshots and replace the files in `assets/screenshots/` using the same names.

## Publish on GitHub Pages

### Option A: same repository as the OS (recommended)

The workflow in `.github/workflows/deploy-website.yml` publishes this folder automatically.

1. Push the repository to GitHub, with `main` as the default branch.
2. On GitHub, open **Settings → Pages**, and under **Build and deployment → Source** pick **GitHub Actions**.
3. Push any change under `website/` to `main`, or run the workflow by hand from the **Actions** tab.

The site appears at `https://YOUR-GITHUB-USERNAME.github.io/MintOS26/`.

### Option B: a separate repository

1. Create a new repository, for example `MintOS26-website`.
2. Copy everything **inside** `website/` (including `.nojekyll`) to the root of that repository and push it.
3. In **Settings → Pages**, pick **Deploy from a branch**, then `main` and `/ (root)`.

If you use a different repository name, update the `/MintOS26/` paths in `404.html` to match.

## Updating for a new release

- **Version badge:** the `eyebrow` line near the top of `index.html`.
- **Releases:** add a new `<li>` at the top of the `timeline` list and move the `is-latest` class and the `latest` badge to it.
- **Status:** update the three lists in the `status` section.
- **Demo Terminal:** the `ver` reply and other strings in `js/main.js` are copied from `kernel/user/desktop.c`. Keep them in sync.
- **Build step:** the disk image name in the QEMU command contains the version number.
- **Screenshots:** replace files in `assets/screenshots/`. On macOS you can shrink a PNG into a web-sized JPEG with:

  ```sh
  sips -s format jpeg -s formatOptions 80 -Z 1600 input.png --out website/assets/screenshots/name.jpg
  ```

  If a new image has a different size, update its `width` and `height` in `index.html`.
