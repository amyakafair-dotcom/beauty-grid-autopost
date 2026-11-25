# Beauty-Grid — Instagram + Facebook autopost

Publishes scheduled posts to @beauty.griderp and the Beauty-Grid Facebook Page through the official Meta Graph API. Runs hourly on GitHub Actions; no computer needs to stay on.

Meta app: **Beauty-Grid Autopost**, business portfolio **Beauty Grid**.

## Secrets (environment `autopost`)

Settings → Environments → **autopost** → Add environment secret:

- `IG_TOKEN` — token from the app (Use cases → Instagram API → API setup with Unified login → Generate token). Must include `pages_manage_posts` for Facebook.
- `IG_USER_ID` — Instagram account id of @beauty.griderp (optional; looked up when missing).

The Facebook Page id is set in the workflow (`PAGE_ID`, Beauty-Grid = 1516231928232345).

Test: Actions → *Beauty-Grid autopost* → **Run workflow** (with "Only check the token" ticked).

## Adding posts

Edit `posts.csv`, one row per post:

| column | value |
|---|---|
| `time` | publish time with offset, e.g. `2026-12-01T20:00:00+02:00` (Khartoum) |
| `type` | `IMAGE` or `REEL` |
| `media_url` | public direct link to a JPEG or MP4 (files in `media/` work: this repo is public) |
| `caption` | post text; wrap in quotes if it has commas or line breaks |
| `platforms` | `instagram`, `facebook`, or `instagram,facebook`; empty = both |
| `status` | leave empty — the bot fills in ids or `<platform>:ERROR ...` |

A failed platform is retried on the next run; platforms already posted are skipped.
Posts land up to ~1 hour after `time`.
