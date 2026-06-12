# Race Split Viewer

A single-file static site for visualizing per-100m race splits and stroke rates
(e.g. from an NK SpeedCoach), with explicit uncertainty buffers for the parts of
the piece you can't fully trust — build hand-offs, paddle-down after the line,
GPS dropouts, anything.

Everything lives in `index.html`. No build step, no dependencies to install —
React and Recharts load from a CDN.

## Features

- **Paste-friendly data entry**: one segment per line as `meters split spm`
  (`100 1:32.0 36.7`), or paste raw 4-column SpeedCoach rows
  (`1600M 5:13.7 1:37.8 35.5`) — the cumulative-time column is detected and
  ignored. Spaces, tabs, or commas all work, so spreadsheet pastes are fine.
  You can also upload a `.csv`/`.txt` file.
- **Segment-start or segment-end distance marks**, auto-detected (4-column
  SpeedCoach pastes are treated as end marks).
- **Uncertainty buffers anywhere in the race**: each buffer hides the solid
  line over its range and replaces it with a shaded split range + rate range,
  tapering into the nearest measured points. Make a buffer one-sided (only
  faster / higher-rate than the adjacent data) by setting its slow edge to the
  neighboring measured value.
- **Quarter summaries** along the top of the plot at boundaries you choose
  (e.g. `400, 900, 1400` for a 1900m piece where the first official 500 is
  short), marked `~` when they include buffered segments.
- **Official time reconciliation**: enter the official result and the header
  + average-split line use it instead of the sum of splits.
- **Share links**: the full configuration (data, buffers, everything) is
  encoded into the URL — copy it and send it to the crew. Bookmark it to
  save a race.
- Split axis in `1:XX` format, inverted so up = faster; responsive 100m/200m
  x-axis labeling; hover tooltips with exact per-segment values or buffer
  ranges.

## Deploy to GitHub Pages

1. Create a new repository on GitHub (e.g. `race-splits`), public.
2. Add `index.html` (and this README) to it:
   ```bash
   git init
   git add index.html README.md
   git commit -m "Race split viewer"
   git branch -M main
   git remote add origin git@github.com:YOUR_USERNAME/race-splits.git
   git push -u origin main
   ```
3. In the repo on github.com: **Settings → Pages → Source: Deploy from a
   branch → Branch: `main` / `(root)` → Save**.
4. After a minute, the site is live at
   `https://YOUR_USERNAME.github.io/race-splits/`.

Updates are just commits — push a new `index.html` and Pages redeploys
automatically.

## Data format reference

```
# 3-column (segment starts by default):
0    1:30.7  37.7
100  1:32.0  36.7
...

# 4-column raw SpeedCoach (segment ends, cumulative time ignored):
100M  0:21.9  1:49.2  37.2
200M  0:40.1  1:31.1  37.0
...
```

Splits accept `1:32.0` or plain seconds (`92.0`). Lines that don't parse are
skipped, so headers in pasted tables are harmless.
