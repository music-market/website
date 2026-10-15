@def title = "Sources"
@def hascode = true

# Sources

The [music-market](https://github.com/music-market) organisation holds one repository per source. Each fetches, cleans and stores data from a single website, catalogue or platform, and documents its own coverage. The sources fall into three groups: the records of success kept by charts and award juries, the catalogues and platforms that describe and distribute recorded music, and the communities in which listeners rate, categorise, annotate and trace music.

~~~<div class="notice">~~~
**Figures are dated.** Each count below is reported as of the date its repository records it. The collections were run at different times, between 2018 and 2026, and some are still in progress.
~~~</div>~~~

## Charts and awards

### Billboard

[billboard](https://github.com/music-market/billboard) fetches weekly Billboard charts through the `billboard` Python library, chart by chart, from the first *Hot 100* of 29 December 1957 to the end of 2022. The run covers twenty charts, among them the Hot 100, Bubbling Under, Radio Songs, Digital Song Sales, Streaming Songs and the adult, Christian and gospel formats.

| Charts | Coverage | Status |
| --- | --- | --- |
| hot-100, bubbling-under-hot-100-singles, radio-songs, digital-song-sales, streaming-songs, top-40-recurrents | 1957-12-29 to 2022-12-31 | Collected |
| adult-pop-songs, adult-contemporary, christian-songs, christian-digital-song-sales | 1957-12-29 to 2022-12-31, in two runs split at 1987-10-17 | Collected |
| christian-airplay, hot-christian-adult-contemporary, gospel-airplay, gospel-streaming-songs | 1957-12-29 to 2022-12-31 | Collected |
| gospel-digital-song-sales | 1957-12-29 to 2022-12-31 | Flagged for checking |

The code is complete; deployment of the full chart list is still open.

### Grammy Awards

[grammys](https://github.com/music-market/grammys) scrapes the Grammy website from its sitemap. One spider collects nominations, ceremony by ceremony, from 1959 to 2019, in runs made in February 2018, April 2019 and September 2019. A second collects artist names. A cleaning workflow turns the raw JSON into ceremonies, awards, nominations and wins, with a hand-curated deduplication of award labels and a clerical check of the categories kept for analysis.

A Selenium crawler then visits the page of every nominated or awarded artist and records their name, number of wins, number of nominations, and the awards and nominations themselves. As of April 2022 it had collected 5,244 artists. A companion script searches each name on Google and parses the knowledge panel and the Wikipedia page it links to.

## Catalogues and platforms

### Discogs

[discogs](https://github.com/music-market/discogs) works from the monthly XML dumps that [Discogs](https://www.discogs.com/) has published since March 2008. Four files per month, for artists, labels, masters and releases, are converted to CSV tables with a bundled copy of `discogs-xml2db`, which streams through the XML and writes one table per entity and relation: genres, tracks, aliases and so on. The January 2023 dump, with about 8.4 million artists, is loaded in MongoDB; the March 2026 dump is being processed.

### MusicBrainz

[musicbrainz](https://github.com/music-market/musicbrainz) uses the MusicBrainz database as a bridge to Spotify. It retrieves artist-level Spotify links where MusicBrainz records them, and collects the names of the artists it does not link, so that the Spotify pipeline can search for them by name.

### Spotify

[spotify](https://github.com/music-market/spotify) is the largest collection. Artist identifiers are gathered from five sources, deduplicated, and crawled through the Spotify Web API from artist to album to track to audio features, each level in its own MongoDB collection. As of 2022 the pipeline had collected:

| Collection | Records |
| --- | ---: |
| Artists | 2,139,061 |
| Albums | 11,876,566 |
| Tracks | 75,926,513 |
| Audio features | 66,243,012 |

Later additions extend the crawl: an incremental crawl of albums released after a cutoff date, which checkpoints per artist and rotates credentials; a lyrics crawler that stores synced and unsynced lyrics with language and provider; a Selenium scraper of artist "About" pages, with followers, monthly listeners by country and related artists; and importers for external dumps, including a Spotify metadata dump of about 3 billion rows in fourteen collections.

### SoundCloud

[soundcloud](https://github.com/music-market/soundcloud) collects and cleans SoundCloud user data for sociological research on cultural markets. It reaches the platform three ways: the official OAuth API, used to resolve permalinks; the unofficial `api-v2` endpoints, which carry the production crawl of profiles and engagement feeds; and a legacy Splash-rendered path kept for one recurring spider.

Discovery found 1.67 million users through the sitemap and 46.4 million through the directory. A profile backfill in June 2026 fed a PostgreSQL 18 analysis database of about 2.9 billion rows. On top of it sits a panel for a tie-formation study: a seed population of 60,035,334 users with at least ten likes, a LIKES wave of 11.2 billion like events, a FOLLOWINGS wave of 3.06 billion following edges, and a COMMENTS wave paused at about ten percent. Reposts and recurrent following snapshots are planned.

## Communities

### WhoSampled

[whosampled](https://github.com/music-market/whosampled) traces musical reuse. A first pass reads WhoSampled's sitemap and loads every track URL into MongoDB. Two Scrapy spiders then crawl each track page and its full list of samples and sampled works, since a track page shows at most three of each. A merge script joins the two and loads the result. As of September 2022 the collection held 870,602 songs, each with its artists, producers, album, release year, genre, tags, and the tracks it samples and is sampled by, down to the sampled element.

### Rate Your Music

[rateyourmusic](https://github.com/music-market/rateyourmusic) covers four domains of the site: product-level metadata such as credits, genres and descriptors; reviews and ratings; the categorisation of products, where members vote for or against applying a genre to an album; and the creation and maintenance of the genre system itself, where members vote on proposals for new genres. The parser for genre proposals records the proposal, who submitted it and when, the yes, hold and no votes, the approval decision, the old and new genre profile, and every comment with its vote. Collection runs through a Selenium workflow behind a Tor proxy.

### Genius

[genius](https://github.com/music-market/genius) crawls lyrics and annotations with a Scrapy spider that runs with or without page rendering. Deployment benchmarks on EC2 settled on four parallel scrapers at about 109,000 pages an hour, against an estimated 6.8 million pages. Documentation of the collected data is still to be written.

### Resident Advisor

[residentadvisor](https://github.com/music-market/residentadvisor) holds a Scrapy crawler for Resident Advisor, the electronic-music listings and reviews site. The repository is at an early stage and its data are not yet documented.
