# Cloudflare Custom Rule: Block Invalid Paths

Block requests to paths that don't exist on the OnTap Coffee site to reduce unwanted traffic and probing.

## Setup Instructions

1. Log in to your Cloudflare dashboard
2. Select your domain (ontap.coffee)
3. Navigate to **Security** → **WAF** → **Custom rules**
4. Click **Create rule**

## Rule Configuration

**Rule name:** `Block Invalid Paths`

### Expression

```
(not http.request.uri.path eq "/" and not http.request.uri.path contains "/images/" and not starts_with(http.request.uri.path, "/css/") and not starts_with(http.request.uri.path, "/js/"))
```

### Simpler Allowlist Approach

Only allow known valid paths:

```
(not http.request.uri.path in {"/" "/index.html" "/images/logo.svg"})
```

### Action

Select: **Block**

## Current Valid Paths

Based on the site structure:

- `/` - Homepage
- `/index.html` - Homepage
- `/images/logo.svg` - Logo

## Notes

- Update this rule when adding new pages or assets
- Monitor **Security** → **Events** for legitimate requests being blocked
- Consider using **Managed Challenge** instead of **Block** for less aggressive filtering
