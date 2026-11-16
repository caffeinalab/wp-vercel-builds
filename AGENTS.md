# Vercel Builds

WordPress plugin (`caffeinalab/wp-vercel-builds`) that stores Vercel deployment webhooks as private `vercel_builds` posts and shows status, duration, and error logs in wp-admin.

## Decisions

- No framework dependencies. `illuminate/support` pinned `illuminate/*` to 9.x and blocked host apps on newer Laravel. String, array, and list helpers are plain PHP.
- PHP 8.0+ (constructor promotion, union types, named arguments, `str_starts_with`).
- `extra.installer-name` keeps the install folder `vercel-builds`: the package was renamed from `timezhero/vercel-builds`, and a new folder would change the plugin basename and deactivate it on every site.
- Installed with Composer into a WordPress project. Autoload is PSR-4 `Builds\` → `src/`. The plugin file does not load `vendor/autoload.php`; the host project does.
- Webhook auth is HMAC-SHA1 of the raw body with `VERCEL_SIGNATURE_KEY`. Deployment API calls use `VERCEL_API_BEARER_TOKEN`.

## Surfaces

- `POST /wp-json/builds/update` — Vercel webhook
- `GET /wp-json/builds/status` — admin badge poll
- Dashboard submenu `index.php?page=build_status`
- Logs modal `admin.php?page=build_logs`
