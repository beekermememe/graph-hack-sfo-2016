# Graph Hack 2016

## Overview

### Schedule

|               |                       |
| --------------| ----------------------|
| 4:00pm-4:30pm | Arrive                |
| 4:30pm-5:00pm | Project pitches       |
| 5:00pm-5:30pm | Teams form            |
| 5:30pm-9:00pm | Hacking               |
| 9:00pm-9:45pm | Project presentations |
| 10:00pm       | Prize announcements   |

### Rules

## Datasets

### Panama Papers + (Offshore Leaks + Bahamas Leaks)

![](img/pp_screenshot.png)

* [Download from ICIJ Offshore Leaks]()

### Legis-graph (US Congress)

![](img/lg_datamodel.png)

* [Github repository](https://github.com/legis-graph/legis-graph)
* [Quickstart import from your web browser](http://johnymontana.github.io/LazyWebCypher/?file=https://raw.githubusercontent.com/legis-graph/legis-graph/master/quickstart/114/legis_graph_import_114.cypher)


### Campaign Finance

#### US FEC Filings

#### California Filings

### US Election Data

#### Election Tweets

~[](img/trump_tweets.png)

~~~
wget http://demo.neo4j.com.s3.amazonaws.com/electionTwitter/neo4j-election-twitter-demo.tar.gz
tar -xvzf neo4j-election-twitter-demo.tar.gz
cd neo4j-enterprise-3.0.3
bin/neo4j start

open web browser to http://localhost:7474

~~~

#### Election Forecast

Fivethirtyeight has made the data behind their famous election forecast publicly available:

[http://projects.fivethirtyeight.com/2016-election-forecast/summary.json](http://projects.fivethirtyeight.com/2016-election-forecast/summary.json)

You can easily pull this into Neo4j using [apoc.load.json]():

~~~
CALL apoc.load.json("http://projects.fivethirtyeight.com/2016-election-forecast/summary.json") YIELD value AS data
RETURN data
~~~

### Fivethirtyeight

The Fivethirtyeight teams does an amazing job of providing the data behind many of their stories in their [Github repo](https://github.com/fivethirtyeight/data). There are a lot of possibilities but here are a few ideas we hacked up:

#### `hip-hop-candidate-lyrics`


*Import*

~~~
LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/fivethirtyeight/data/master/hip-hop-candidate-lyrics/genius_hip_hop_lyrics.csv" AS row
MERGE (c:Candidate {name: row.candidate})
MERGE (a:Artist {name: row.artist})
MERGE (s:Sentiment {type: row.sentiment})
MERGE (t:Theme {type: row.theme})
MERGE (song:Song {name: row.song})
MERGE (line:Line {text: row.line})
SET line.url = row.url
MERGE (line)-[:MENTIONS]->(c)
MERGE (line)-[:HAS_THEME]->(t)
MERGE (line)-[:HAS_SENTIMENT]->(s)
MERGE (song)-[:HAS_LINE]->(line)
MERGE (a)-[r:PERFORMS]->(song)
SET r.data = row.album_release_date
~~~

![](img/hiphop_datamodel.png)

![](img/hiphop_example.png)

### Other resources

Beyond the resources listed above.

We don't have Neo4j import scripts or graph exports for these, but we think they might be interesting to explore:

* [Deep and interesting datasets for computational journalism](http://cjlab.stanford.edu/2015/09/30/lab-launch-and-data-sets/)
* [US Government's open data](http://www.data.gov/)
* [NASA's Data Portal](https://data.nasa.gov/)
* [US City Data](http://us-city.census.okfn.org/)
* [US Spending data](https://www.usaspending.gov/Pages/default.aspx)

## Resources

### Neo4j

You'll need to use Neo4j to participate in the hackathon. You can download Neo4j [here](http://neo4j.com/download) or use one of the hosted versions above.

### APOC - Awesome Procedures on Cypher

Graph algorithms, data import, job scheduling, full text search, geospatial, ...

#### Install

#### Examples

### Neo4j Folks

Grab your friendly Neo4j staff and community members if you have *any* questions.
