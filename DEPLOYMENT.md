# GitHub Pages Deployment Guide

This MkDocs site is configured for automatic deployment to GitHub Pages using GitHub Actions.

## Setup Instructions

### 1. Enable GitHub Pages

1. Go to your repository on GitHub (e.g. `https://github.com/dataidea-science/dataidea-R` or `.../r-science`)
2. Navigate to **Settings** → **Pages**
3. Under **Source**, select **Deploy from a branch** (not "GitHub Actions")
4. Under **Branch**, choose **gh-pages** and **/ (root)**
5. Save the settings

**Important:** This workflow uses `peaceiris/actions-gh-pages`, which pushes the built site to the `gh-pages` branch. GitHub Pages must be set to serve from that branch. If you select "GitHub Actions" instead, the site will not be served and you will get 404.

### 2. Automatic Deployment

The site will automatically deploy when you push to the `master` or `main` branch. The GitHub Actions workflow (`.github/workflows/ci.yml`) will:

1. Build the MkDocs site
2. Push the built site to the `gh-pages` branch
3. Make it available at:
   - **Custom domain:** `https://r.dataidea.org/` (if CNAME/DNS is set)
   - **Default:** `https://<owner>.github.io/<repo>/` (e.g. `dataidea-science.github.io/r-science/` or `.../dataidea-R/` depending on your repo name)

### 3. Manual Deployment (Local)

If you want to test the build locally before pushing:

```bash
# Install dependencies
pip install mkdocs mkdocs-material

# Build the site
mkdocs build

# Preview locally
mkdocs serve
```

### 4. Updating Content

Simply edit the markdown files in the `docs/` directory and push to `master`:

```bash
git add .
git commit -m "Update documentation"
git push origin master
```

The GitHub Actions workflow will automatically build and deploy the updated site.

## Configuration

- **Site URL**: Configured in `mkdocs.yml` as `site_url`
- **Repository**: `dataidea-science/dataidea-R`
- **Theme**: Material for MkDocs
- **Build Output**: `site/` directory (ignored by git)

## Troubleshooting

### Build Fails

1. Check GitHub Actions logs: Go to **Actions** tab in your repository
2. Verify `mkdocs.yml` syntax is correct
3. Ensure all referenced files exist in `docs/`

### Site Not Updating

1. Check if GitHub Actions workflow ran successfully
2. Verify GitHub Pages is set to **Deploy from a branch** → **gh-pages** (not "GitHub Actions")
3. Wait a few minutes for GitHub Pages to update (can take up to 10 minutes)

### Getting 404

1. **Check Pages source:** Settings → Pages → Source must be **Deploy from a branch** with branch **gh-pages**. If it is "GitHub Actions", change it to deploy from the `gh-pages` branch.
2. **Use the correct URL:** The site is served at `https://<owner>.github.io/<repo>/` where `<repo>` is your repository name (e.g. `r-science` or `dataidea-R`). Ensure you are not missing the repo name or using a different name.
3. **Custom domain:** If using `r.dataidea.org`, ensure DNS has a CNAME to `<owner>.github.io` and that the custom domain is set in Settings → Pages.
4. **First deployment:** After the first push, you may need to go to Settings → Pages and manually select the `gh-pages` branch once before the site appears.

### Local Build Issues

```bash
# Update dependencies
pip install --upgrade mkdocs mkdocs-material

# Clean build
rm -rf site/
mkdocs build
```

## Additional Resources

- [MkDocs Documentation](https://www.mkdocs.org/)
- [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
