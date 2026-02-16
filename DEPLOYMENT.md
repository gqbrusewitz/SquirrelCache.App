# Cloudflare Deployment Guide

This guide covers everything needed to deploy SquirrelCache to Cloudflare Pages.

## Prerequisites

- Cloudflare account at https://dash.cloudflare.com
- GitHub repository (this project)
- A domain (optional - Cloudflare Pages provides a `.pages.dev` subdomain)

## Setup Instructions

### 1. Create a Cloudflare Pages Project

1. Log in to [Cloudflare Dashboard](https://dash.cloudflare.com)
2. Go to **Pages** section
3. Click **Create a project**
4. Select **Connect to Git**
5. Authorize GitHub and select this repository
6. Fill in project details:
   - **Project name**: `squirrelcache`
   - **Production branch**: `main`
   - **Build command**: (leave empty - no build needed)
   - **Build output directory**: `.`
7. Click **Save and Deploy**

### 2. Set GitHub Secrets

For automatic deployments via GitHub Actions, add these secrets to your GitHub repository:

1. Go to repository **Settings** → **Secrets and variables** → **Actions**
2. Add the following secrets:

#### `CLOUDFLARE_API_TOKEN`
- Go to Cloudflare Dashboard → **Account** → **API Tokens**
- Click **Create Token**
- Use the **Edit Cloudflare Workers** template OR create custom with:
  - **Permissions**:
    - Account → Cloudflare Pages → Edit
  - **Account Resources**: Include your account
- Copy the token and add to GitHub secrets

#### `CLOUDFLARE_ACCOUNT_ID`
- Found in Cloudflare Dashboard → **Account** → **Settings**
- Copy your Account ID and add to GitHub secrets

### 3. Configure Your Domain (Optional)

To use a custom domain:

1. In Cloudflare Dashboard → **Pages** → **squirrelcache**
2. Go to **Custom domains** tab
3. Click **Set up custom domain**
4. Enter your domain (e.g., `squirrelcache.com`)
5. Follow nameserver instructions if needed

### 4. Environment Variables (Optional)

If your site needs environment variables:

1. In Cloudflare Pages project settings
2. Add environment variables under **Settings** → **Environment variables**
3. Set values for both Preview and Production

## Deployment

### Automatic Deployment
- Any push to `main` branch triggers automatic deployment
- Preview deployments available for pull requests

### Manual Deployment
```bash
npm install -g wrangler
wrangler pages deploy . --project-name=squirrelcache
```

## File Structure

- `index.html` - Homepage
- `privacy.html` - Privacy policy
- `docs/index.html` - Documentation
- `css/` - Stylesheets
- `js/` - JavaScript files
- `assets/` - Images and media
- `_redirects` - URL routing rules
- `wrangler.toml` - Cloudflare configuration

## Troubleshooting

### Deployment Fails
- Check GitHub Actions logs in repository
- Verify Cloudflare API token has correct permissions
- Ensure `CLOUDFLARE_ACCOUNT_ID` is correct

### Assets Not Loading
- Check `_redirects` file for correct routing
- Verify asset paths in HTML are relative (e.g., `./assets/`)
- Ensure file extensions are lowercase

### Custom Domain Not Working
- Verify nameservers point to Cloudflare
- Allow 15-30 minutes for DNS propagation
- Check SSL certificate status in Cloudflare Dashboard

## Advanced Features

### Caching Headers
Edit `_redirects` to add cache headers:
```
/*
  Cache-Control: public, max-age=31536000

/index.html
  Cache-Control: public, max-age=3600
```

### Analytics
Enable in Cloudflare Dashboard → **Pages** → **Analytics**

### Redirects
Edit `_redirects` file to add URL redirects and rewrites

## Support

For Cloudflare Pages documentation: https://developers.cloudflare.com/pages/

For issues or questions, check the repository issues section.
