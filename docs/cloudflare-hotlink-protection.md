# Cloudflare Custom Rule: Hotlink Protection

Block requests to site assets from external referrers to prevent hotlinking and unauthorized embedding of content.

## Setup Instructions

1. Log in to your Cloudflare dashboard
2. Select your domain (ontap.coffee)
3. Navigate to **Security** → **WAF** → **Custom rules**
4. Click **Create rule**

## Rule Configuration

**Rule name:** `Block Hotlinking`

**Field:** `Referer`

**Operator:** `does not contain`

**Value:** `ontap.coffee`

### Expression (Advanced)

```
(http.referer ne "" and not http.referer contains "ontap.coffee" and not http.referer contains "github.io")
```

This expression:
- Allows requests with no referer (direct visits, bookmarks, etc.)
- Allows requests from ontap.coffee
- Allows requests from github.io (for GitHub Pages hosting)
- Blocks all other external referrers

### Action

Select: **Block**

## Alternative: Protect Only Static Assets

If you only want to protect images and static assets:

```
(http.request.uri.path contains "/images/" and http.referer ne "" and not http.referer contains "ontap.coffee" and not http.referer contains "github.io")
```

## Testing

After enabling the rule:
1. Visit your site directly - should work
2. Try embedding an image on another site - should be blocked
3. Check Cloudflare **Security** → **Events** to see blocked requests

## Notes

- Rules may take a few minutes to propagate
- Monitor the Security Events log for false positives
- Adjust the expression if you need to whitelist additional domains (e.g., social media preview crawlers)

### Whitelist Search Engines & Social Media

To allow search engines and social media platforms to access your content for previews:

```
(http.referer ne "" and not http.referer contains "ontap.coffee" and not http.referer contains "github.io" and not http.referer contains "google" and not http.referer contains "facebook" and not http.referer contains "twitter" and not http.referer contains "linkedin")
```
