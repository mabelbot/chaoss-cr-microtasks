# Microtask 4:
Anything you want to show us. Even if you find bugs in our documentation and want to issue a PR for those!


**Issues Opened**:
- https://github.com/chaoss/grimoirelab-elk/issues/1045
- https://github.com/chaoss/augur-community-reports/issues/42
- https://github.com/chaoss/community-handbook/issues/30
- https://github.com/chaoss/wg-evolution/issues/440/#issuecomment-1084141143

**PRs**:
- https://github.com/chaoss/wg-evolution/pull/443
- https://github.com/chaoss/grimoirelab-elk/pull/1046


# Definition of Conversion Rate Metric Model
https://docs.google.com/document/d/1kNM5ch9uP7cVBa_60CHdWGKcFNRAf5nH3500YIEafxE/edit?usp=sharing


# Results of Investigating GrimoireLab Elasticsearch Data (Grimoire ELK)
1. Check cluster health
```
$ curl "http://localhost:9200/_cluster/health?pretty"
{
  "cluster_name" : "docker-cluster",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 21,
  "active_shards" : 21,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 20,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 51.21951219512195
}
```
2. Looking at indices that we have in Elasticsearch (https://www.techtarget.com/searchitoperations/tutorial/Manage-Elasticsearch-documents-with-indices-and-shards)
```
mabel@Mabels-MacBook-Pro CHAOSS % curl -XGET http://localhost:9200/_cat/indices
yellow open git_chaoss_enriched     sOFbXzPBQgCM9UWKSBUvRQ 5 1 1738   48   4.2mb   4.2mb
yellow open git-aoc_chaoss_enriched WqV1_bmbSca-FhCLcmAZzg 5 1 6448 2534   8.1mb   8.1mb
yellow open git-onion_enriched      SCnCNxEAT86-I2UfDgaw1Q 5 1  544    0 473.4kb 473.4kb
yellow open git_chaoss              I9xIeFNeRym5LfJWobRVBw 5 1 1738    0   2.9mb   2.9mb
green  open .kibana_1               AMDD9J3hRw6A-EjUtp_WvA 1 0    1    0   3.6kb   3.6kb
```

To view the headers
```
mabel@Mabels-MacBook-Pro CHAOSS % curl -s 'http://localhost:9200/_cat/indices?v'
health status index                   uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   git_chaoss_enriched     sOFbXzPBQgCM9UWKSBUvRQ   5   1       1738           48      4.2mb          4.2mb
yellow open   git-aoc_chaoss_enriched WqV1_bmbSca-FhCLcmAZzg   5   1       6448         2534      8.1mb          8.1mb
yellow open   git-onion_enriched      SCnCNxEAT86-I2UfDgaw1Q   5   1        544            0    473.4kb        473.4kb
yellow open   git_chaoss              I9xIeFNeRym5LfJWobRVBw   5   1       1738            0      2.9mb          2.9mb
green  open   .kibana_1               AMDD9J3hRw6A-EjUtp_WvA   1   0          1            0      3.6kb          3.6kb
mabel@Mabels-MacBook
```
3. View documents inside `git_chaoss_enriched`. Note, the default is 10 hits as the result. 
```
curl "localhost:9200/git_chaoss_enriched/_search?pretty"
curl "localhost:9200/git-onion_enriched/_search?pretty"
```
4. Here's an example of a "hit" inside git-onion-enriched.
```
      {
        "_index" : "git-onion_enriched",
        "_type" : "item",
        "_id" : "_global___global__2016-07-01t00:00:00.000z_9aed245d9df109f8d00ca0e656121c3bdde46a2a",
        "_score" : 1.0,
        "_source" : {
          "timeframe" : "2016-07-01T00:00:00.000Z",
          "author_uuid" : "9aed245d9df109f8d00ca0e656121c3bdde46a2a",
          "author_name" : "Alvaro del Castillo",
          "contributions" : 4,
          "metadata__timestamp" : "2022-04-05T19:49:28.730Z",
          "project" : "_Global_",
          "author_org_name" : "_Global_",
          "cum_net_sum" : 71,
          "percent_cum_net_sum" : 91.02564102564102,
          "onion_role" : "regular",
          "quarter" : "2016Q3",
          "metadata__enriched_on" : "2022-04-05T19:50:34.277683",
          "data_source" : "git",
          "grimoire_creation_date" : "2016-07-01T00:00:00.000Z"
        }
```


# Testing GrimoireLab - Data Collection from Git backend (Commits)
1. I picked up off from the end of the Getting Started, https://github.com/chaoss/grimoirelab-sirmordred/blob/master/Getting-Started.md#getting-started-. 
2. If containers are not running, `docker-compose up -d`
This should result in
```
Starting chaoss_elasticsearch_1 ... done
Starting chaoss_mariadb_1       ... done
Starting chaoss_kibiter_1       ... done
```
3. Make sure the requirement is installed: `pip install 'elasticsearch<7.14.0'` (UnsupportedProductError). From Jupyter Notebook make surenv is `source $HOME/.virtualenvs/comm_reports_env/bin/activate`
4. Collect via micro-mordred
    1. Default micro-mordred already contains data collected from Perceval's git commits. I have added additional Augur data using projects.json (with tutorial here: https://github.com/chaoss/grimoirelab-sirmordred)  
    2. This extra collection output
    ```
    Collection for git: starting...
    2022-04-17 20:48:05,326 Reading projects data from  ./projects.json 
    2022-04-17 20:48:05,327 [git] collection phase starts
    2022-04-17 20:48:05,327 [git] collection starts for https://github.com/chaoss/augur
    2022-04-17 20:48:05,415 [git] Incremental from: None for https://github.com/chaoss/augur
    2022-04-17 20:48:14,391 Fetching commits: 'https://github.com/chaoss/augur' git repository from 1970-01-01 00:00:00+00:00 to 2100-01-01 00:00:00+00:00; all branches
    2022-04-17 20:49:16,041 Fetch process completed: 7774 commits fetched
    2022-04-17 20:49:16,096 [git] Done collection for https://github.com/chaoss/augur
    2022-04-17 20:49:16,096 [git] collection finished for https://github.com/chaoss/augur
    ```
5. Collect without micro-mordred - will output data on the Terminal buffer: follow dev installation https://github.com/chaoss/grimoirelab-perceval (use Git install to easily install requirements)
    1. I did this with my own `python3 -m venv perceval` trying collecting from the Git backend from Augur (`$ perceval git 'https://github.com/chaoss/augur.git' --from-date '2016-01-01'`)
    2. To get an idea of how much data this returns for a single repository of Augur's size, this results in 7774 items (from 7774 commits) produced.

# Python Connectors
I tested out 2 connectors
1. Elasticsearch -> Python Dataframe https://github.com/mabelbot/chaoss-cr-microtasks/blob/main/ElasticSearch_Python_Client.ipynb
2. Python Dataframe -> Opensearch https://github.com/mabelbot/chaoss-cr-microtasks/blob/main/viz_example.ipynb


# Testing OpenSearch & OpenSearch Dashboards
Goal: Investigate OpenSearch Dashboards usage for Conversion Rate final visualization. Data will be retrieved from database with index patterns, similar to how Kibana & ElasticSearch interact.

Installation using the following tutorials:
- https://opensearch.org/docs/latest/dashboards/install/docker/
- https://opensearch.org/docs/latest/opensearch/install/docker#sample-docker-compose-file

Install
```
docker pull opensearchproject/opensearch:latest
docker pull opensearchproject/opensearch-dashboards:latest
```


Test setup:
- To start Docker cluster: `docker-compose up` [this](https://github.com/mabelbot/chaoss-conversion-rate/blob/main/docker-compose.yml) is the Docker compose file I used, after some debugging (not the same as the tutorial)
- To shut down Docker cluster: `docker-compose down [-v]`
- To access with `curl`
```
curl https://localhost:9200 -u admin:admin --insecure
curl -POST https://localhost:9200/documents/document/1 -u admin:admin -H 'Content-Type: application/json' -d '{"title":"TITLE HERE", "text": "An example test field"}' --insecure
```
<img width="1102" alt="Screen Shot 2022-04-16 at 9 16 25 PM" src="https://user-images.githubusercontent.com/70232089/163734686-77216fae-8e25-4c2c-b68b-e6c980335d71.png">

