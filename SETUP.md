# 📚 GitHub Pages Setup Guide

This guide will help you publish the AI Agent Launchpad documentation to GitHub Pages.

## 🚀 Quick Setup

### 1. Repository Setup

1. **Create a new repository** (or use existing one):
   ```bash
   git clone https://github.com/yourusername/agent-launchpad-infra.git
   cd agent-launchpad-infra
   ```

2. **Copy documentation files**:
   ```bash
   # Copy all docs files to your repository
   cp -r docs/* ./
   # Or if you want to keep docs in a folder:
   # git add docs/
   ```

3. **Update configuration**:
   Edit `_config.yml` and replace:
   - `yourusername` with your GitHub username
   - `agent-launchpad-infra` with your repository name
   - Update URLs and contact information

### 2. Enable GitHub Pages

1. Go to your repository on GitHub
2. Click **Settings** tab
3. Scroll to **Pages** section
4. Under **Source**, select:
   - **Deploy from a branch**
   - **Branch**: `main` (or `master`)
   - **Folder**: `/ (root)` or `/docs` (if you kept files in docs folder)
5. Click **Save**

### 3. Custom Domain (Optional)

If you have a custom domain:

1. Create a `CNAME` file in the root:
   ```bash
   echo "your-domain.com" > CNAME
   ```

2. Configure your DNS:
   - Add a CNAME record pointing to `yourusername.github.io`

## 📝 Customization

### Update Site Information

Edit `_config.yml`:

```yaml
title: "Your Project Name"
description: "Your project description"
url: "https://yourusername.github.io"
baseurl: "/your-repo-name"

author: "Your Name"
twitter:
  username: "your-twitter"
```

### Update Navigation

Edit the navigation section in `_config.yml`:

```yaml
navigation:
  - title: "Home"
    url: "/"
  - title: "Architecture"
    url: "/README"
  - title: "API Reference"
    url: "/api-reference"
  # Add more pages as needed
```

### Add Google Analytics (Optional)

Uncomment and update in `_config.yml`:

```yaml
google_analytics: "UA-XXXXXXXX-X"
```

## 🎨 Styling

### Theme Options

The site uses the Cayman theme. You can change it in `_config.yml`:

```yaml
# Popular themes:
remote_theme: pages-themes/cayman@v0.2.0    # Current
remote_theme: pages-themes/minimal@v0.2.0   # Minimal
remote_theme: pages-themes/architect@v0.2.0 # Architect
remote_theme: pages-themes/midnight@v0.2.0  # Dark theme
```

### Custom CSS

Create `assets/css/style.scss`:

```scss
---
---

@import "{{ site.theme }}";

/* Custom styles */
.highlight {
  background-color: #f8f9fa;
  border-radius: 6px;
  padding: 16px;
}

.mermaid {
  text-align: center;
  background-color: white;
  border-radius: 8px;
  padding: 20px;
  margin: 20px 0;
}
```

## 🔧 Advanced Configuration

### Adding Plugins

Add to `_config.yml`:

```yaml
plugins:
  - jekyll-feed           # RSS feed
  - jekyll-sitemap        # XML sitemap
  - jekyll-seo-tag        # SEO meta tags
  - jekyll-mentions       # @username mentions
  - jekyll-redirect-from  # Page redirects
```

### Environment Variables

For sensitive information, use GitHub Secrets:

1. Go to repository **Settings** > **Secrets**
2. Add secrets like `GOOGLE_ANALYTICS_ID`
3. Reference in `_config.yml`:
   ```yaml
   google_analytics: {{ site.github.environment_variables.GOOGLE_ANALYTICS_ID }}
   ```

## 📱 Mobile Optimization

The Cayman theme is mobile-responsive by default. For additional optimization:

1. **Test on mobile devices**
2. **Optimize images** - use appropriate sizes
3. **Check Mermaid diagrams** - ensure they're readable on small screens

## 🔍 SEO Optimization

### Meta Tags

The site includes `jekyll-seo-tag` plugin. Add to page front matter:

```yaml
---
title: "Page Title"
description: "Page description for search engines"
image: "/assets/images/og-image.png"
---
```

### Structured Data

Add to `_includes/head.html`:

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "name": "{{ site.title }}",
  "description": "{{ site.description }}",
  "url": "{{ site.url }}{{ site.baseurl }}"
}
</script>
```

## 🚀 Deployment

### Automatic Deployment

GitHub Pages automatically rebuilds when you push to the main branch:

```bash
git add .
git commit -m "Update documentation"
git push origin main
```

### Build Status

Check build status:
1. Go to **Actions** tab in your repository
2. Look for "pages build and deployment" workflow
3. Green ✅ = successful deployment
4. Red ❌ = build error (check logs)

## 🐛 Troubleshooting

### Common Issues

1. **Site not updating**:
   - Check GitHub Actions for build errors
   - Clear browser cache
   - Wait a few minutes for CDN propagation

2. **Mermaid diagrams not rendering**:
   - Add to `_includes/head.html`:
     ```html
     <script src="https://cdn.jsdelivr.net/npm/mermaid/dist/mermaid.min.js"></script>
     <script>mermaid.initialize({startOnLoad:true});</script>
     ```

3. **404 errors**:
   - Check file paths and names
   - Ensure `baseurl` is correctly set
   - Verify navigation links

4. **Styling issues**:
   - Check `_config.yml` syntax
   - Validate CSS in custom style files
   - Clear browser cache

### Getting Help

- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [Jekyll Documentation](https://jekyllrb.com/docs/)
- [GitHub Community Forum](https://github.community/)

## ✅ Checklist

Before going live:

- [ ] Update all placeholder URLs and usernames
- [ ] Test all navigation links
- [ ] Verify Mermaid diagrams render correctly
- [ ] Check mobile responsiveness
- [ ] Test on different browsers
- [ ] Set up analytics (if desired)
- [ ] Add custom domain (if applicable)
- [ ] Review and update content

---

🎉 **Your documentation site should now be live at `https://yourusername.github.io/your-repo-name`** 