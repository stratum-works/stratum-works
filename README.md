# stratum-works.com

Static site, served by **Cloudflare Workers static assets** (`wrangler.jsonc`,
`assets.directory: "."`). There is no build step and no worker script — the
repository *is* the site. Deploy with:

```sh
npx wrangler deploy
```

There is no CI, so nothing reaches the domain until somebody runs that.

## Files that are load-bearing

Most of this repo is pages. These three are not — something outside the repo
breaks if they go away, and nothing here will tell you:

| Path | Why it exists |
|---|---|
| `.well-known/apple-app-site-association` | iOS reads this to decide that `stratum-works.com` links may open the Outspell app. Delete it and every match invite silently opens Safari instead. The `appIDs` entry must match Outspell's team and bundle id. |
| `apple-app-site-association` | The same file at the root, which iOS falls back to. Keep the two identical. |
| `_headers` | Serves both of the above as `application/json`. Apple refuses to parse them otherwise and will not follow a redirect to get it. |

`outspell/join/` is the page somebody lands on when they follow an invite
without the app installed. It reads the join code from `?c=` and shows it.

After changing any of them, check the deploy actually worked:

```sh
curl -sI https://stratum-works.com/.well-known/apple-app-site-association
# expect: 200, content-type: application/json
```
