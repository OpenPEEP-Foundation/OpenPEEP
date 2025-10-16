# Setting Up GitHub Pages for OpenPEEP Viewer

This guide explains how to host the OpenPEEP Validator & Viewer tool on **GitHub Pages** (free hosting by GitHub).

---

## Why GitHub Pages?

✅ **Free hosting** - no cost, no server maintenance  
✅ **HTTPS by default** - secure, no CORS issues  
✅ **Automatic updates** - push to repo, site updates  
✅ **Custom domain** - can use your own domain  
✅ **Perfect for file loading** - can load schemas from `/schemas/` directory without CORS errors  
✅ **Public accessibility** - share with stakeholders, developers, consultees

---

## Quick Setup (5 Minutes)

### Step 1: Enable GitHub Pages

1. Go to your GitHub repository
2. Click **Settings** (top menu)
3. Scroll down to **Pages** (left sidebar)
4. Under **Source**, select:
   - Branch: `main` (or your default branch)
   - Folder: `/ (root)`
5. Click **Save**

**That's it!** GitHub will build your site.

---

### Step 2: Wait for Deployment (1-2 minutes)

GitHub will display:
```
✅ Your site is live at https://yourusername.github.io/openpeep-standard/
```

---

### Step 3: Access the Viewer

Your viewer will be available at:
```
https://yourusername.github.io/openpeep-standard/tools/viewer/
```

**Bookmark this URL** and share with your team!

---

## Full Validation Now Works!

Once hosted on GitHub Pages, the viewer will:

✅ **Load schemas** from `/schemas/` directory (no CORS errors)  
✅ **Full AJV validation** (all required fields, types, formats, enums)  
✅ **Detailed error messages** (missing fields, wrong types, etc.)  
✅ **Fix suggestions** (how to correct each error)  
✅ **Format validation** (UUIDs, dates, emails)  
✅ **Enum validation** (only allowed values accepted)  
✅ **Constraint validation** (min/max length, array sizes, etc.)

---

## Testing After Setup

### 1. Visit Your Viewer
```
https://yourusername.github.io/openpeep-standard/tools/viewer/
```

### 2. Test with Valid Example

**Paste this URL** in your browser to download a valid PEEP:
```
https://yourusername.github.io/openpeep-standard/examples/bundles/bundle_01_peep.json
```

**Or paste JSON directly** into the viewer.

**Expected result:**
```
✅ Validation Passed
Schema: PEEP v1.0.0
✅ All required fields present
✅ All field types correct
✅ No validation errors
```

**Then:** Beautifully formatted PEEP displayed below.

---

### 3. Test with Invalid Example

**Paste this broken JSON:**
```json
{
  "schemaVersion": "1",
  "evacuationStrategy": "Simultaneous Evacuation"
}
```

**Expected result:**
```
❌ Validation Failed
Schema: PEEP v1.0.0
5 error(s) found:

  Error 1: Missing required field: "peepId"
  Path: /
  💡 Fix: Add a UUID v4: "peepId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

  Error 2: Missing required field: "personProfile"
  Path: /
  💡 Fix: Add person profile object

  [etc...]
```

**Perfect for development!**

---

## Custom Domain (Optional)

If you want a custom domain (e.g., `viewer.openpeep.org`):

### Step 1: Add CNAME Record

In your DNS settings (where you manage your domain):
```
Type:  CNAME
Name:  viewer
Value: yourusername.github.io
```

### Step 2: Configure in GitHub

1. Go to **Settings → Pages**
2. Under **Custom domain**, enter: `viewer.openpeep.org`
3. Click **Save**
4. Wait for DNS check (✅ appears when ready)

**Now accessible at:**
```
https://viewer.openpeep.org/
```

---

## Updating the Viewer

**Workflow:**

1. Make changes to `tools/viewer/index.html`
2. Commit and push to GitHub:
   ```bash
   git add tools/viewer/index.html
   git commit -m "feat: improve validator error messages"
   git push origin main
   ```
3. GitHub Pages **automatically rebuilds** (30-60 seconds)
4. Refresh `https://yourusername.github.io/openpeep-standard/tools/viewer/`
5. See your changes live!

**No manual deployment needed!**

---

## Sharing with Others

**Share this URL:**
```
https://yourusername.github.io/openpeep-standard/tools/viewer/
```

**Use cases:**
- ✅ Public consultation (show working examples)
- ✅ Developer onboarding (test their JSON)
- ✅ Stakeholder demos (validate PEEPs)
- ✅ Compliance audits (check required fields)
- ✅ Training (see how JSON becomes readable PEEP)

---

## Security & Privacy

**Still privacy-preserving even when hosted:**

✅ **Client-side only** - JavaScript runs in user's browser  
✅ **No server processing** - JSON never sent to GitHub  
✅ **No logging** - GitHub Pages doesn't log POST data (we don't send any)  
✅ **No tracking** - no analytics in the viewer  
✅ **Safe for sensitive data** - validate real PEEPs without privacy concerns

**Your JSON stays in your browser. Always.**

---

## Troubleshooting

### "404 - Site not found"

**Wait 2-3 minutes** - GitHub Pages takes time to build.  
**Check:** Settings → Pages shows green "✅ Your site is published"

### "Schema not loaded" warning

**If on GitHub Pages and seeing this:**
- Check schema files exist in `/schemas/` directory
- Check paths in `index.html` are correct (`../../schemas/...`)
- Check GitHub Pages is serving from root (not a subdirectory)

### "CORS error" in browser console

**This should NOT happen on GitHub Pages** (same origin).  
If it does: check GitHub Pages is enabled and site is live.

---

## Advanced: GitHub Actions (Auto-Deploy)

For advanced users, you can add a GitHub Action to:
- Auto-validate all examples on commit
- Generate static HTML renders of all examples
- Deploy to separate `gh-pages` branch

See `.github/workflows/` for examples (to be created if needed).

---

## Next Steps

1. **Enable GitHub Pages** (Settings → Pages → Source: main, folder: /)
2. **Wait for deployment** (1-2 minutes)
3. **Access viewer** at `https://yourusername.github.io/openpeep-standard/tools/viewer/`
4. **Test with examples** (drag & drop from `/examples/bundles/`)
5. **Share URL** with team/stakeholders

---

**Questions?**

- GitHub Pages docs: https://docs.github.com/en/pages
- Issues: https://github.com/OpenPEEP-Foundation/OpenPEEP/issues

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

