# keep-alive

Keeps free-tier Render services warm by pinging them on a schedule with GitHub Actions, so they do not spin down between visits.

## How it works

- `.github/workflows/keep-alive.yml` runs every 10 minutes (cron) and can also be started manually with an optional `force` input.
- Each run reads `config.json` and pings only inside the configured IST time window, and only for apps switched on there.
- Pings are plain GET requests to public health/home URLs: no sign-in and no data changes.

## Configure

Edit `config.json`:

```json
{
  "enabled": true,
  "start": "08:00",
  "end": "22:00",
  "apps": {
    "rio": true,
    "neotiabot": true
  }
}
```

- `enabled`: master switch - `false` pauses everything.
- `start` / `end`: IST window (24h) in which pings are allowed.
- `apps`: per-app on/off switches; each key must match a `key` in the workflow matrix.

To keep another service awake, add it to the matrix in `.github/workflows/keep-alive.yml` and give it a matching key in `config.json`.
