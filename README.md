# radio-stations

A single deduplicated JSON file of internet radio stations, aggregated from four public directories and matched by stream URL to remove duplicates across sources.

- **98,625 stations**
- Sources: [TuneIn](https://tunein.com) (unofficial API), [Radio-Browser.info](https://www.radio-browser.info/) (open API), [Radio Garden](https://radio.garden/), and [Icecast](https://icecast.org/)'s public YP directory
- 14,015 of the records were confirmed present in 2+ sources

## Schema

Each entry in `radio-stations.json` is an object:

| Field | Type | Description |
|---|---|---|
| `name` | string | Station name |
| `country` | string \| null | Country of origin, where known |
| `language` | string \| null | Broadcast language, where known |
| `tags` | string \| null | Genre/format tags or descriptive text |
| `codec` | string \| null | Stream codec (e.g. `MP3`, `AAC`) |
| `bitrate` | number \| null | Stream bitrate in kbps |
| `image` | string \| null | Logo/favicon URL |
| `homepage` | string \| null | Station's own website |
| `place` | string \| null | City, where known |
| `geo` | `[lon, lat]` \| null | City coordinates, where known |
| `stream_urls` | string[] | One or more playable stream URLs (varying quality/format) |
| `sources` | object | Which source(s) this record was found in, keyed by source name, with that source's own ID(s) |

## How it was built

Each source uses an unrelated ID scheme, so records were deduplicated by normalizing stream URLs (scheme + host + path, dropping query strings and the http/https distinction) and merging any records that resolved to the same stream endpoint. TuneIn and Radio Garden's own directory listings are structurally capped/curated rather than exhaustive, so this file is a large practical aggregate rather than a claim of absolute completeness.

## Known caveats

- A small number of station names/tags contain unrecoverable mojibake (`�`) — the original text was corrupted upstream before this data was collected, and no re-encoding can restore it.
- A handful of streams use `mms://`, `rtmp://`, or `mmsh://` instead of `http(s)://` (older Windows Media/RTMP streaming servers) — these are valid, just not web-playable without an appropriate client.
- Stream availability isn't actively re-checked here; some URLs may go offline over time.

## License / usage

This is an aggregation of data from multiple public sources with differing provenance and terms. Radio-Browser.info's data is explicitly open for reuse; TuneIn, Radio Garden, and Icecast listings were collected from their public-facing directories. If you plan to use this commercially or redistribute it, review each original source's own terms.
