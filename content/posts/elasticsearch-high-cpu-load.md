---
title: "elasticsearch - high CPU and load"
date: 2018-05-16T18:02:55-07:00
draft: true
---
Ran into an issue with out production elasticsearch cluster today. Looking at the metrics for our cluster (via Kopf), CPU and load shot through the roof and was pretty much all maxed out. Taking a look at the application logs within our ELK cluster also did not help. All I saw were a bunch of "slowlog" log entries. Fortunately, with a couple of Google searches, I found that by calling the ["hot threads" endpoint](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-nodes-hot-threads.html), I can examine threads for each node along with their respective CPU usage. This was when I saw the following:

>87.9% (439.5ms out of 500ms) cpu usage by thread 'elasticsearch[node_name][[index_name][3]: Lucene Merge Thread #6778]'

Segment merging was the culprit as all of my data nodes had similar entries such as above. Turns our we were doing a large amount of bulk indexing which I suspect is related to background [segement merging](https://www.elastic.co/guide/en/elasticsearch/guide/current/indexing-performance.html#segments-and-merging) falling behind the ingestion rate. 

### References:

- https://www.ebayinc.com/stories/blogs/tech/elasticsearch-performance-tuning-practice-at-ebay/
- https://thoughts.t37.net/designing-the-perfect-elasticsearch-cluster-the-almost-definitive-guide-e614eabc1a87