# GitHub Pages Deployment Guide

This MkDocs site is configured for automatic deployment to GitHub Pages using GitHub Actions.

## Setup Instructions

### 1. Enable GitHub Pages

1. Go to your repository on GitHub: `https://github.com/dataidea-science/dataidea-R`
2. Navigate to **Settings** → **Pages**
3. Under **Source**, select **GitHub Actions**
4. Save the settings

### 2. Automatic Deployment

The site will automatically deploy when you push to the `master` branch. The GitHub Actions workflow (`.github/workflows/ci.yml`) will:

1. Build the MkDocs site
2. Deploy it to the `gh-pages` branch
3. Make it available at: `https://dataidea-science.github.io/dataidea-R/`

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
2. Verify GitHub Pages is enabled and set to use GitHub Actions
3. Wait a few minutes for GitHub Pages to update (can take up to 10 minutes)

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
