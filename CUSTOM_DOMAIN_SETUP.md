# Custom Domain Setup for GitHub Pages

## Changes Made

### 1. Updated `mkdocs.yml`
- Changed `site_url` from `https://dataidea-science.github.io/dataidea-R/` to `https://r.dataidea.org/`
- Added explicit `use_directory_urls: true` configuration

### 2. Updated GitHub Actions Workflow
- Added step to copy CNAME file to the site directory
- Set `cname: r.dataidea.org` in the deployment action

## Why This Fixes the 404 Errors

When using a custom domain with GitHub Pages, the `site_url` in `mkdocs.yml` must match your custom domain. This ensures that:

1. **All internal links are generated correctly** - MkDocs uses the `site_url` to generate absolute URLs and canonical links
2. **Navigation works properly** - The Material theme uses the site URL to construct navigation links
3. **Assets load correctly** - CSS, JavaScript, and images reference paths relative to the site URL

## Next Steps

1. **Commit and push the changes:**
   ```bash
   git add mkdocs.yml .github/workflows/ci.yml
   git commit -m "Configure for custom domain r.dataidea.org"
   git push origin master
   ```

2. **Wait for GitHub Actions to deploy** (check the Actions tab)

3. **Verify DNS settings** - Ensure your DNS is configured correctly:
   - For apex domain: A records pointing to GitHub Pages IPs
   - For subdomain: CNAME record pointing to `dataidea-science.github.io`

4. **Check GitHub Pages settings:**
   - Go to Settings → Pages
   - Verify "Custom domain" shows `r.dataidea.org`
   - Ensure "Enforce HTTPS" is enabled (after DNS propagates)

5. **Clear browser cache** - The 404s might be cached, so try:
   - Hard refresh (Ctrl+Shift+R or Cmd+Shift+R)
   - Incognito/Private browsing mode
   - Clear browser cache

## Troubleshooting

If pages still return 404 after deployment:

1. **Check the deployed site structure:**
   - Visit `https://r.dataidea.org/r-fundamentals/getting-started/`
   - Check if the file exists at that path

2. **Verify CNAME is deployed:**
   - Check the `gh-pages` branch has `CNAME` file in root
   - The file should contain: `r.dataidea.org`

3. **Check GitHub Pages build logs:**
   - Go to Actions tab
   - Check if the build completed successfully
   - Look for any warnings or errors

4. **Test with GitHub Pages URL:**
   - Temporarily test: `https://dataidea-science.github.io/dataidea-R/r-fundamentals/getting-started/`
   - If this works but custom domain doesn't, it's a DNS/domain configuration issue

5. **Wait for DNS propagation:**
   - DNS changes can take up to 48 hours
   - Use `dig r.dataidea.org` or `nslookup r.dataidea.org` to check DNS

## Additional Notes

- The CNAME file in your repository root will be automatically copied to the deployed site
- GitHub Pages will automatically create/update the CNAME file in the `gh-pages` branch
- After DNS propagates, GitHub will automatically enable HTTPS for your custom domain
