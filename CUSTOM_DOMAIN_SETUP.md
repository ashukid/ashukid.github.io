# Setting Up Your Custom Domain (Namecheap) for GitHub Pages

This guide will help you connect your Namecheap domain to your GitHub Pages blog (LazyInq).

## Prerequisites

- Domain purchased from Namecheap
- GitHub repository set up (ashukid/ashukid.github.io)
- Access to Namecheap account
- Access to GitHub account

---

## Step 1: Configure DNS in Namecheap

You need to add DNS records in Namecheap to point your domain to GitHub Pages.

### Option A: Using Apex Domain (e.g., yourdomain.com)

1. **Log in to Namecheap** and go to your domain list
2. Click **Manage** next to your domain
3. Go to the **Advanced DNS** tab
4. Add the following **A Records** (point to GitHub Pages IP addresses):

   | Type | Host | Value | TTL |
   |------|------|-------|-----|
   | A Record | @ | 185.199.108.153 | Automatic |
   | A Record | @ | 185.199.109.153 | Automatic |
   | A Record | @ | 185.199.110.153 | Automatic |
   | A Record | @ | 185.199.111.153 | Automatic |

   **Note:** GitHub uses multiple IP addresses for redundancy. Add all 4.

5. Click the **Save All Changes** button

### Option B: Using WWW Subdomain (e.g., www.yourdomain.com)

If you prefer to use `www.yourdomain.com`:

1. Go to **Advanced DNS** tab in Namecheap
2. Add a **CNAME Record**:

   | Type | Host | Value | TTL |
   |------|------|-------|-----|
   | CNAME Record | www | ashukid.github.io | Automatic |

3. Click **Save All Changes**

### Option C: Both Apex and WWW (Recommended)

You can set up both! This allows people to access your site with or without www:
- `yourdomain.com` → Use A Records (Option A)
- `www.yourdomain.com` → Use CNAME Record (Option B)

---

## Step 2: Create CNAME File in Your Repository

GitHub Pages needs to know about your custom domain.

1. **Create a file named `CNAME`** (all caps, no extension) in your repository root
2. **Add your domain** to this file (one domain per line):

   ```
   yourdomain.com
   www.yourdomain.com
   ```

   Replace `yourdomain.com` with your actual domain.

3. **Commit and push** this file to your repository

---

## Step 3: Configure GitHub Pages Settings

1. Go to your GitHub repository: `https://github.com/ashukid/ashukid.github.io`
2. Click on **Settings** (top menu)
3. Scroll down to **Pages** in the left sidebar
4. Under **Custom domain**, enter your domain:
   - Enter: `yourdomain.com` (or `www.yourdomain.com` if using www)
5. GitHub will automatically check the DNS configuration
6. **Optional:** Check **Enforce HTTPS** (wait until DNS propagates first, then enable)

---

## Step 4: Update Jekyll Configuration

Update your `_config.yml` file to use your custom domain:

```yaml
url: "https://yourdomain.com"
baseurl: ""
```

Replace `yourdomain.com` with your actual domain.

---

## Step 5: Wait for DNS Propagation

DNS changes can take **24-48 hours** to fully propagate, though often it's much faster (1-2 hours).

You can check if your DNS is ready:
- Visit: https://www.whatsmydns.net
- Enter your domain and check if the A records point to GitHub's IPs

---

## Step 6: Enable HTTPS (After DNS Propagation)

Once your domain is working:

1. Go back to GitHub Pages settings
2. Check the box for **Enforce HTTPS**
3. GitHub will automatically provision an SSL certificate for your domain

**Note:** This may take a few minutes to a few hours after DNS propagation.

---

## Troubleshooting

### Domain Not Working After 48 Hours

1. **Check DNS records:**
   - Use `dig yourdomain.com` or `nslookup yourdomain.com` in terminal
   - Verify A records point to GitHub's IPs

2. **Verify CNAME file:**
   - Check that `CNAME` file exists in repository root
   - Ensure it contains only your domain name

3. **Check GitHub Pages settings:**
   - Domain is entered correctly
   - Repository is set to GitHub Pages

4. **Clear browser cache:**
   - Sometimes browsers cache old DNS entries

### Mixed Content Warnings

If you see HTTPS warnings:
- Wait for GitHub to provision SSL certificate
- Ensure "Enforce HTTPS" is enabled in GitHub Pages settings
- Check that all your URLs use `https://` in `_config.yml`

### WWW vs Non-WWW

- GitHub Pages works with both
- You can redirect one to the other in your Jekyll configuration if needed
- It's recommended to pick one and stick with it

---

## Quick Checklist

- [ ] Added A Records (or CNAME) in Namecheap DNS settings
- [ ] Created `CNAME` file in repository root with your domain
- [ ] Updated `_config.yml` with your custom domain URL
- [ ] Configured custom domain in GitHub Pages settings
- [ ] Committed and pushed changes to GitHub
- [ ] Waited for DNS propagation (1-48 hours)
- [ ] Enabled HTTPS in GitHub Pages settings
- [ ] Tested domain in browser

---

## Need Help?

- **Namecheap Support:** https://www.namecheap.com/support/
- **GitHub Pages Docs:** https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site
- **DNS Propagation Checker:** https://www.whatsmydns.net

---

**Note:** Replace `yourdomain.com` with your actual domain name throughout this guide!

