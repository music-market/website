@def title = "Music market data"
@def hascode = true

~~~
<div class="home-page">
<section class="hero" aria-labelledby="hero-title">
  <div class="hero-copy"><span class="kicker">Ten repositories · Cultural markets research</span>
    <h1 id="hero-title">The music market,<br>as <em>data.</em></h1>
    <p>Code and data pipelines that collect the recorded-music market from ten public sources: charts and awards, catalogues and metadata, streaming platforms, and the communities that review, categorise and sample music. This site describes what each repository collects, how it is collected, and where the data end up.</p>
    <div class="actions"><a class="button button-primary" href="/sources/">Explore the sources <span aria-hidden="true">→</span></a><a class="button button-secondary" href="/methods/">How data are collected</a></div>
  </div>
  <figure class="hero-figure"><div class="figure-header"><span>SAMPLING NETWORK</span><span>01 / TRACKS × SAMPLES</span></div>
<svg class="hero-network" viewBox="0 0 450 370" role="img" aria-labelledby="network-title network-description">
<title id="network-title">Tracks connected by the samples they share</title><desc id="network-description">An illustrative network of tracks. Each line is a sample taken from one track by another. Three tracks are sampled far more often than the rest and sit at the centre of the clusters. This is a schematic, not an empirical dataset.</desc>
<g class="edges" stroke-width="1.4"><line x1="60" y1="132" x2="109" y2="74"/>
<line x1="60" y1="132" x2="150" y2="160"/>
<line x1="60" y1="132" x2="75" y2="278"/>
<line x1="109" y1="74" x2="150" y2="160"/>
<line x1="109" y1="74" x2="205" y2="106"/>
<line x1="150" y1="160" x2="205" y2="106"/>
<line x1="150" y1="160" x2="132" y2="237"/>
<line x1="205" y1="106" x2="259" y2="58"/>
<line x1="205" y1="106" x2="318" y2="129"/>
<line x1="205" y1="106" x2="266" y2="196"/>
<line x1="259" y1="58" x2="318" y2="129"/>
<line x1="259" y1="58" x2="363" y2="91"/>
<line x1="318" y1="129" x2="363" y2="91"/>
<line x1="318" y1="129" x2="266" y2="196"/>
<line x1="318" y1="129" x2="405" y2="184"/>
<line x1="363" y1="91" x2="405" y2="184"/>
<line x1="334" y1="224" x2="266" y2="196"/>
<line x1="334" y1="224" x2="382" y2="287"/>
<line x1="334" y1="224" x2="405" y2="184"/>
<line x1="266" y1="196" x2="208" y2="258"/>
<line x1="266" y1="196" x2="132" y2="237"/>
<line x1="208" y1="258" x2="132" y2="237"/>
<line x1="208" y1="258" x2="230" y2="330"/>
<line x1="132" y1="237" x2="75" y2="278"/>
<line x1="382" y1="287" x2="405" y2="184"/>
<line x1="382" y1="287" x2="230" y2="330"/></g>
<g class="halo"><circle cx="205" cy="106" r="23"/><circle cx="266" cy="196" r="24"/><circle cx="132" cy="237" r="23"/></g>
<g><circle class="node" cx="60" cy="132" r="6"/>
<circle class="node" cx="109" cy="74" r="6"/>
<circle class="node" cx="150" cy="160" r="6"/>
<circle class="hub" cx="205" cy="106" r="10"/>
<circle class="node" cx="259" cy="58" r="6"/>
<circle class="node" cx="318" cy="129" r="6"/>
<circle class="node" cx="363" cy="91" r="6"/>
<circle class="node" cx="334" cy="224" r="6"/>
<circle class="hub" cx="266" cy="196" r="10"/>
<circle class="node" cx="208" cy="258" r="6"/>
<circle class="hub" cx="132" cy="237" r="10"/>
<circle class="node" cx="75" cy="278" r="6"/>
<circle class="node" cx="382" cy="287" r="6"/>
<circle class="node" cx="405" cy="184" r="6"/>
<circle class="node" cx="230" cy="330" r="6"/></g>
<g font-size="9"><text x="22" y="113">TRACKS</text><text x="318" y="333">SAMPLES</text><text x="150" y="40">MOST SAMPLED</text></g></svg>
    <figcaption class="figure-caption"><span>Collect · parse · load · query</span><span>Illustrative network</span></figcaption>
  </figure>
</section>
<div class="fact-strip"><span><strong>10</strong> repositories</span><span><strong>75.9 M</strong> Spotify tracks</span><span><strong>11.2 B</strong> SoundCloud like events</span><span><strong>870,602</strong> songs on WhoSampled</span></div>
<section class="home-section" aria-labelledby="layers-title">
  <div class="section-heading"><div><span class="kicker">From public sources to research databases</span><h2 id="layers-title">Three ways in, one destination</h2></div><a href="/methods/">See the methods →</a></div>
  <div class="workflow-grid">
    <a class="workflow-card" href="/sources/#catalogues_and_platforms"><span class="card-number">01 / APIS AND DUMPS</span><h3>Catalogues at scale</h3><p>The Spotify Web API crawled from artist to album to track to audio features. Discogs monthly XML dumps converted to tables. MusicBrainz links that seed the rest.</p><span class="card-action">spotify · discogs · musicbrainz <span aria-hidden="true">→</span></span></a>
    <a class="workflow-card" href="/sources/#communities"><span class="card-number">02 / CRAWLS</span><h3>Communities, as they talk</h3><p>Scrapy and Selenium crawlers for WhoSampled, Rate Your Music, Genius and Resident Advisor, and for the Grammy and Billboard records of who was nominated, who won and what charted.</p><span class="card-action">whosampled · rateyourmusic · grammys <span aria-hidden="true">→</span></span></a>
    <a class="workflow-card" href="/data/"><span class="card-number">03 / STORES</span><h3>From crawl to query</h3><p>MongoDB at collection time, then documented PostgreSQL databases for analysis. The SoundCloud store alone holds about 2.9 billion rows of profiles plus a panel of 11.2 billion like events.</p><span class="card-action">MongoDB · PostgreSQL 18 <span aria-hidden="true">→</span></span></a>
  </div>
</section>
<section class="home-section analysis-feature" aria-labelledby="query-title">
  <div><span class="kicker">Querying the stores</span><h2 id="query-title">One artist.<br>Every track, every feature.</h2><p>Each level of the Spotify crawl sits in its own collection, linked by identifiers. A track joins its audio features by ID, and an artist joins their tracks through the embedded artist list.</p><a href="/data/#mongodb_collections">See the collections →</a></div><div>
~~~

```python
# Tracks by one artist, joined to their audio features
db.tracks.aggregate([
    {"$match": {"artists.id": "4Z8W4fKeB5YxbusRsdQVPb"}},
    {"$lookup": {"from": "acousticAttributes",
                 "localField": "id", "foreignField": "id",
                 "as": "features"}},
    {"$project": {"name": 1, "duration_ms": 1,
                  "features.tempo": 1, "features.energy": 1}},
])
```

~~~
  </div>
</section>
<section class="home-section project-note" aria-label="About this project">
  <div><h3>Why cultural markets?</h3><p>Music is a market where value is uncertain and made socially. Charts rank, juries award, critics rate, communities vote on what counts as a genre, and listeners follow one another. The repositories gather these traces so that categorisation, evaluation and taste can be studied together.</p><a href="/research/">Read the research themes →</a></div>
  <div><h3>Built to be re-run</h3><p>Collectors checkpoint and resume. Dumps are pinned to a date. Stores are documented down to the field, and each repository records its own coverage and collection date, so a figure on this site can be traced to the run that produced it.</p><a href="/methods/#storage">See how the data are stored →</a></div>
</section>
</div>
~~~
