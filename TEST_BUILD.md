# Testing the SEO Build Fixes

## What Was Fixed

The initial build errors were caused by:

1. **Hugo 0.42 compatibility issue** in `seo_meta.html`
   - Problem: Variable reassignment with `=` not supported in Hugo 0.42
   - Fixed: Removed variable assignment, use direct conditionals instead

2. **JSON syntax errors** in `structured_data.html`
   - Fixed: Trailing commas in conditional statements
   - Fixed: Proper handling of optional fields (keywords)

3. **Missing HTML entity escaping**
   - Fixed: Added `| htmlEscape` to meta tag content (titles, descriptions)
   - Fixed: Added `| jsonify` to JSON-LD content (properly escapes quotes, special chars)

## How to Test

### Test 1: Run blogdown serve_site

In R/RStudio:
```r
blogdown::serve_site(port = 8080, host = '0.0.0.0')
```

**Expected**: Server starts without errors

### Test 2: Check for Build Errors

If you see errors, run in terminal to see details:
```bash
# If Hugo is in your PATH
hugo server -D

# Or use blogdown which has Hugo bundled
R -e "blogdown::build_site()"
```

**Expected**: No error messages, clean build

### Test 3: Verify SEO Tags in Browser

1. Start the server (Test 1)
2. Open: `http://localhost:8080` or `http://0.0.0.0:8080`
3. View page source (Ctrl+U or Cmd+Option+U)
4. Search for:
   - `meta name="description"` - should exist
   - `@type.*TechArticle` - should exist on blog posts
   - `@type.*Organization` - should exist on homepage

**Expected**: All tags present, properly formatted

### Test 4: Validate JSON-LD

1. View source of any blog post
2. Copy the JSON-LD `<script type="application/ld+json">` content
3. Paste into: https://jsonlint.com/
4. Click "Validate JSON"

**Expected**: Valid JSON, no syntax errors

## Common Issues and Solutions

### Issue: "Error building site: logged X error(s)"

**Cause**: Template syntax error or missing Hugo function

**Solution**:
- Check that all `{{ if }}` have matching `{{ end }}`
- Verify all template variables exist (e.g., `.Site.Params.name`)
- Look for unescaped quotes in strings

### Issue: Titles with quotes break JSON

**Example**: Post title is `Understanding "p-value" in genomics`

**Solution**: Use `| jsonify` filter (already applied in fixed templates)
- Converts: `"Understanding "p-value" in genomics"`
- To: `"Understanding \"p-value\" in genomics"`

### Issue: Trailing commas in JSON

**Example**:
```json
{
  "name": "value",
  "other": "value",  <- Invalid if this is the last property
}
```

**Solution**: Conditional comma placement (already fixed)
```hugo
"url": "{{ .Permalink }}"{{ with .Params.tags }},
"keywords": "{{ delimit . ", " }}"{{ end }}
```

## Verification Checklist

After fixes, verify:

- [ ] `blogdown::serve_site()` starts without errors
- [ ] Homepage loads at http://localhost:8080
- [ ] Blog posts load without errors
- [ ] View source shows meta description tags
- [ ] View source shows JSON-LD structured data
- [ ] JSON-LD validates at jsonlint.com
- [ ] No console errors in browser

## Files Modified in This Fix

1. `layouts/partials/seo_meta.html`
   - Added `| htmlEscape` to meta tag content attributes
   - Ensures titles with quotes, ampersands, etc. are safe

2. `layouts/partials/structured_data.html`
   - Added `| jsonify` to JSON-LD string values
   - Fixed conditional comma placement for keywords
   - Properly handles optional fields (image, tags)

## Next Steps After Successful Test

1. ✅ Verify build works locally
2. ✅ Commit the fixes
3. ✅ Push to GitHub
4. ✅ Netlify will auto-deploy
5. ✅ Verify on production site

## Rollback if Needed

If issues persist:
```bash
# Revert all SEO changes
git checkout HEAD -- layouts/partials/
git checkout HEAD -- static/robots.txt
git checkout HEAD -- config.toml

# Restart server
```

Then report what error you see and we'll fix it.

---

**Last Updated**: January 2025
**Status**: Fixed - Ready for testing
