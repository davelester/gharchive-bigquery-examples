# Analyzing GitHub Data with BigQuery Using GH Archive

[GH Archive](https://www.gharchive.org) publishes GitHub event data as a [BigQuery public dataset](https://cloud.google.com/bigquery/public-data/) and through downloadable archives.

The project includes [over 15 event types](https://docs.github.com/en/rest/using-the-rest-api/github-event-types?apiVersion=2022-11-28) of GitHub metadata, including Issues, Releases, Stars, Pull Requests, Commits.

You can easily analyze GH Archive data by using the [Google Cloud Console](http://console.cloud.google.com) to query the dataset. This repository shares some examples of how you can use BigQuery and the GH Archive dataset, along with some insight into what's happening within each query.

## Tips before getting started

- At the time of writing, the first 1 TiB per month of queries to BigQuery is free. Up-to-date details on pricing is available on the [BigQuery pricing](https://cloud.google.com/bigquery/pricing) page.
- Within the Google Cloud Console, after you type your query and before it is run you'll be told how much data will be queried.
- When starting with a query, start by querying a smaller amount of data (for example, one day) to reduce costs and imrpove your queries. After confirming that it works as intended, expand your query to more data (for example, a month or year).

## Querying Basic Community Metrics
### Project Contributors on a Single Day

- Metric: unique contributors to a single project on a single day
- Example project: Apache Cassandra on February 1, 2024

```sql
SELECT
  COUNT(DISTINCT events.actor.id)
FROM `githubarchive.day.20240201` AS events
  WHERE
    events.repo.name = 'apache/cassandra'
```

### Project Contributors in a Month

- Metric: unique contributors to a single project for an entire month
- Example project: Apache Cassandra in February, 2024

```sql
SELECT
  COUNT(DISTINCT events.actor.id)
FROM `githubarchive.month.202402` AS events
  WHERE
    events.repo.name = 'apache/cassandra'
```

### Organizational metrics

- Metric: unique monthly contributors last month to all projects in a GitHub org
- Example project: the Apache Software Foundation

```sql
SELECT
  COUNT(DISTINCT events.actor.id)
FROM `githubarchive.month.202402` AS events
  WHERE
    events.org.login = 'apache'
```