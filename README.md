# linked-culture-galaxy-data

Generated **deploy data** for the LinkedCulture visualizations — the star-map
galaxy (https://galaxy.lemondeweb.com) and the 3D Pangaea terrain
(https://galaxy.lemondeweb.com/terrain). This repo is a backup / system-of-record
for the artifacts; the **source that produces them** lives in
[`linked-culture-topics-pipeline`](https://github.com/edwardmonk/linked-culture-topics-pipeline)
(`rebuild_world.sh`, `export_galaxy*.py`, `terrain/export_terrain.py`).

Current snapshot: the **307,519-object / 1,653-cluster** rebuild (2026-06-20),
sources ARTIC · Rijksmuseum · Cleveland · Getty · Harvard · Met · Paris Musées · MIA
(SI / Smithsonian-hold and image-less Joconde sources filtered out).

## Contents (the repo root)

| File | What |
|---|---|
| `positions.f32.bin` | Float32 `[x,y]` per object, two-level galaxy layout |
| `cluster_ids.i16.bin` | Int16 cluster id per object (`-1` = noise) |
| `source_ids.u8.bin` / `sources.json` | museum index per object + name table |
| `record_keys.json` | stable `record_key` per object (search-hit → cluster) |
| `clusters.json` | per-cluster: label, description, count, centroid, radius, region, superId, slug |
| `supers.json` | ~16 thematic super-regions (galaxy nebulae / terrain continents) |
| `clusters_obj/{id}.json` | per-cluster object list (title, source, imageURL, objectURL) for carousels |
| `manifest.json` | build metadata |
| `heightmap.bin` | Float32 terrain heightfield (`meshW×meshH`, normalised) |
| `albedo.png` | hypsometric terrain surface texture (no hillshade) |
| `terrain_meta.json` | mesh dims, sea level, per-cluster peak positions + labels |

These are **regenerable** from the topics-pipeline cache via `rebuild_world.sh`
(galaxy) + `terrain/export_terrain.py` (terrain). Deployed to
`/srv/topic-galaxy/data/` on the VPS, fronted by Cloudflare (bump `DATA_VER` in
the front-ends on each rebuild so the CDN serves the new binaries).
