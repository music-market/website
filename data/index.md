@def title = "Data"
@def hascode = true

# Data

Collected data live in two kinds of store. MongoDB holds what the collectors write, one database per source. PostgreSQL holds the SoundCloud analysis database, built from the crawl output and documented table by table. Discogs is kept as CSV tables converted from the monthly dump.

## MongoDB collections

| Database | Collection | Content |
| --- | --- | --- |
| `spotify` | `artist_id` | Accumulated Spotify artist identifiers, deduplicated |
| `spotify` | `artists`, `albums`, `tracks`, `acousticAttributes` | The four levels of the API crawl |
| `spotify` | `*_incremental` | Artists, albums, tracks and audio features from the post-cutoff crawl |
| `spotify` | `lyrics`, `lyrics_queue` | Song lyrics with language and provider, and per-track crawl status |
| `grammyAwards` | `artists` | Nominated and awarded artists with wins, nominations and award names |
| `grammyAwards` | `spotify_about` | Scraped Spotify "About" pages |
| `musicbrainz` | `artist_urls` | MusicBrainz artist links to Spotify |
| `whoSampled` | `whoSampled` | One document per song with its samples and sampled works |
| `soundcloud` | `userURL`, `userURL_directory` | Profile URLs discovered through the sitemap and the directory |
| `soundcloud` | `userURL_10pct`, `userURL_90pct`, `userMeta_90pct` | The sampled profile sets and their metadata |

Identifiers are preserved across levels. A Spotify track carries the identifiers of its artists, and its audio features carry the track identifier, so a query walks from artist to feature without a separate key table.

```python
# Spotify: audio features of one track
db.acousticAttributes.find_one({"id": track_id},
    {"tempo": 1, "energy": 1, "valence": 1, "danceability": 1})

# WhoSampled: the songs that sample a given track
db.whoSampled.find({"samples.track_link": "/Kanye-West/Bound-2/"},
    {"song_name": 1, "artists_raw_text": 1, "release_year": 1})
```

## Spotify fields

| Collection | Fields |
| --- | --- |
| `artists` | `id`, `name`, `genres`, `popularity`, `followers.total`, `external_urls`, `images` |
| `albums` | `id`, `artists[].id`, `album_type`, `release_date`, `total_tracks`, `available_markets` |
| `tracks` | `id`, `artists[].id`, `duration_ms`, `explicit`, `disc_number`, `track_number` |
| `acousticAttributes` | `id`, `danceability`, `energy`, `key`, `loudness`, `mode`, `speechiness`, `acousticness`, `instrumentalness`, `liveness`, `valence`, `tempo`, `time_signature` |

## WhoSampled fields

Each of the 870,602 documents describes one song: `song_name`, `artists` with links, `artists_raw_text` marking main and featured artists, `album_name`, `release_year`, `producers`, `genre`, `tags`, the counts `n_sample` and `n_sampled`, and two lists, `samples` and `sampled`, whose entries give the other track, its main artist, its year, its genre and the sampled element. Crawl date, spider and source URL are stored with every document.

## SoundCloud analysis database

The SoundCloud crawl is loaded into a PostgreSQL 18 database of about 2.9 billion rows and 779 GB, completed and verified on 27 June 2026. Its schemas are kept separate by source and by design.

| Schema | Contents |
| --- | --- |
| `rich_backfill` | 46.4 million directory seeds, nested objects exploded into tables, large tables hash-partitioned |
| `rich_sitemap` | 1.67 million sitemap seeds, thin and flat |
| `panel` | The longitudinal tie-formation data, kept apart from the static snapshot |

Within each snapshot schema, crawled seeds sit in `*_core` tables and the 128.6 million users they point to in `bf_user`, linked into one social and consumption graph. Two analysis layers sit on top: a canonical genre taxonomy with a 3.9-million-row crosswalk, and a sampling frame built on the users who liked a track in 2026, with per-user genre aggregates and an auditable sample ledger.

The `panel` schema holds the longitudinal collection:

| Table | Rows | What a row is |
| --- | ---: | --- |
| `seed_user` | 60,035,334 | A user in the collection frame, with its provenance |
| `like_event` | 11.2 B | One like by one user, with its timestamp, hash-partitioned by user |
| `following_edge` | 3.06 B | One following tie observed in the stock snapshot |
| `comment_event` | partial | One comment, with its body and the track's uploader; the wave is paused |

A schema review notes what the tables do not declare: the sampling frame is ego-centric and single-wave, and long feeds are page-censored. Analyses are expected to read that review first.

## Discogs tables

The monthly dump converts to one CSV table per entity and relation: `artist`, `artist_alias`, `label`, `master`, `master_genre`, `release`, `release_track` and their companions. The January 2023 dump holds about 8.4 million artists. The March 2026 dump is in conversion.

## Key figures

| Source | Figure | As of |
| --- | ---: | --- |
| Spotify | 75,926,513 tracks, 2,139,061 artists | 2022 |
| SoundCloud | 2.9 B rows in the analysis database | June 2026 |
| SoundCloud | 11.2 B like events in the panel | July 2026 |
| WhoSampled | 870,602 songs | September 2022 |
| Discogs | 8.4 M artists | January 2023 dump |
| Grammy Awards | 5,244 artists; nominations 1959 to 2019 | April 2022; September 2019 |
| Billboard | 20 charts, weekly | 1957 to 2022 |

## Licensing

The code in every repository is released under the MIT licence. The collected content remains subject to the terms of the site or service it came from, and the databases are not redistributed. Each repository documents how to rebuild its collection from the source.
