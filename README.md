# Urban Service Inequality — Colombo District (Web GIS)

## Folder structure required
```
UrbanServiceWebMap/
├── index.html
├── style.css
├── script.js
└── data/
    ├── Accessibility Index.geojson   (polygons: GN_NAME, POPULATION, accessibility_index, priority_index)
    ├── Hospitals.geojson
    ├── Schools.geojson
    ├── Banks.geojson
    ├── Parks.geojson
    └── Bus Stops.geojson
```

Place your own GeoJSON files in `data/` using exactly these names (or edit the
`DATA_FILES` object at the top of `script.js` to match your own file names).

## Run it
Open the folder in VS Code and click **Go Live** (Live Server extension).
No build step, no npm install — everything loads from CDN.

## Community Feedback (Supabase)
The layout now has three columns: **left sidebar → map → right Community
Feedback panel**. The right panel lets users report a service issue tied to
a GN Division (typed freely, not a dropdown), pin a location on the map, and
see recent reports.

To wire it to your own database:
1. Create a Supabase project → in **Project Settings → API**, copy your
   **Project URL** and **Publishable (anon) key**.
2. In `script.js`, replace `YOUR_PROJECT_URL` and `YOUR_PUBLISHABLE_KEY` near
   the top with those values.
3. In Supabase, create a table called `community_feedback` with columns:
   `id` (uuid, default `gen_random_uuid()`), `created_at` (timestamptz,
   default `now()`), `gn_division` (text), `service` (text), `category`
   (text), `description` (text), `latitude` (float8), `longitude` (float8).
4. Enable Row Level Security and add an insert + select policy (e.g. allow
   anon insert/select) so the public form can write and the map can read.

Until you add real credentials, the panel stays visible but shows a friendly
"not configured yet" message instead of erroring — the rest of the app is
unaffected.

## Notes
- Accessibility Index classes (5) and Priority Index classes (3) are computed
  automatically at load time using quantile breaks over your data — no manual
  thresholds to maintain.
- If a service layer file is missing, the app logs a warning and continues
  loading the rest (it won't crash the whole map).
- Field names (`GN_NAME`, `POPULATION`, `accessibility_index`, `priority_index`)
  are configurable in the `FIELD` object in `script.js`.
