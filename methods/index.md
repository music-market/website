@def title = "Methods"
@def hascode = true

# Methods

The repositories reach their sources in three ways: documented APIs, bulk dumps, and crawlers. Whatever the way in, collectors write JSON Lines or MongoDB documents at collection time, checkpoint so that an interrupted run resumes, and record what they collected and when. The largest collections then move to PostgreSQL for analysis.

## Official APIs

The Spotify Web API is crawled with `spotipy`. Artist identifiers come first, from MusicBrainz links, Google searches of MusicBrainz and Grammy artist names, related-artist links on Spotify "About" pages, and Musixmatch track lookups. The crawler then requests artist metadata in batches of fifty, albums per artist, tracks per album, and audio features in batches of a hundred, storing each level in its own collection. The incremental crawl adds a date filter, checkpoints per artist, rotates credentials on rate-limit responses, and disables audio features gracefully where the endpoint is no longer available.

SoundCloud's official API is used only to resolve permalinks, through an OAuth client-credentials flow with a cached token. Its terms of use prohibit scraping, a tension the repository documents rather than hides.

```bash
# Spotify: collect identifiers, then crawl artist → album → track → features
python scraper/collect_artist_ids.py
cd scraper/crawlers/api_crawler && python run.py

# Spotify: only albums released after a cutoff date
python scraper/incremental_crawl.py --since 2025-07-01
```

## Bulk dumps

Discogs publishes a complete dump every month. The four XML files are converted to CSV by a streaming parser, so that an eight-million-artist file is processed without holding it in memory. Dumps are pinned to a date, so that an analysis can name the month it describes.

The Spotify repository also imports external dumps: a Kaggle dataset of 160,000 tracks, Pushshift's Spotify dumps, and a metadata dump of about 148 GB that loads into MongoDB as fourteen typed and indexed collections.

```python
from discogsxml2db.exporter import main

main({"INPUT_DIR": "/path/to/xml/dumps/", "<INPUT_FILE>": None,
      "--output": "/path/to/csv/", "--export": ["artist", "label", "master", "release"],
      "--limit": None, "--bz2": False, "--debug": False,
      "--dry-run": False, "--apicounts": False})
```

## Crawlers

Most community sites have no API, so the repositories crawl them. Three patterns recur.

- **Sitemap first.** WhoSampled, the Grammys and SoundCloud are entered through their sitemaps, which give a complete list of pages before any page is parsed. The list is stored and the crawl works through it, so coverage can be checked against it.
- **Scrapy spiders.** WhoSampled, Genius, Rate Your Music, Resident Advisor, the Grammy nominations and SoundCloud engagement are crawled with [Scrapy](https://scrapy.org/) projects. Pipelines write JSON Lines or upsert into MongoDB; a merge step joins the output of multi-stage spiders.
- **Browser automation.** Where pages are rendered by JavaScript, Selenium drives a headless browser: Grammy artist pages, Spotify "About" pages and Rate Your Music. Older SoundCloud spiders rendered pages with Splash and parsed them with XPath; that path has been retired in favour of the platform's JSON endpoints.

Where a search engine is the only index, as for artists without a MusicBrainz link, a Google Search scraper runs through a Zyte proxy with multiprocessing.

## Fleet-scale collection

The SoundCloud crawl runs at a scale no single machine supports. Seeds are partitioned into shards by a hash of the URL, and each shard runs on its own self-healing group of spot virtual machines on Google Compute Engine, uploading chunks to cloud storage as they complete. Capacity was measured before each campaign: per-IP benchmarks established the request rate an address can sustain, and the per-client-identifier rate limit, not the IP, turned out to be the binding constraint. Engagement waves share four client identifiers, so they run strictly one after another. Fleets publish heartbeats to cloud storage, and read-only laptop-side tools report progress from them.

The Genius crawl was tuned the same way on EC2, from a single spider at sixteen concurrent requests to four parallel spiders, before the production run.

## Storage

MongoDB is the collection-time store across the organisation: one database per source, one collection per entity or crawl stage, with identifiers preserved so that levels join. The analysis destination for the largest collection is PostgreSQL 18. The SoundCloud database keeps two source-isolated schemas for the directory and sitemap crawls, a third schema for the longitudinal panel, hash-partitioned fact tables, and a documented genre taxonomy and sampling frame. Its documentation records both how the data were acquired and how they became queryable. The [Data](/data/) page lists the stores.

## Etiquette

Collectors identify themselves, honour rate limits and back off on errors. The SoundCloud drivers use an adaptive limiter that increases its rate slowly and halves it on a refusal, and they treat an edge block as a signal to pause. Credentials are kept in environment files that are never committed. Collected content stays under the terms of the site it came from; the code is MIT licensed.
