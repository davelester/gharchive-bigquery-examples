# Analyzing GitHub Data with BigQuery Using GH Archive

[GH Archive](https://www.gharchive.org) is a record of the public GitHub timeline, made available as a [BigQuery public dataset](https://cloud.google.com/bigquery/public-data/) and through downloadable archives.

All public GitHub Issues, Releases, Stars, Pull Requests, Commits, and more included on the public GitHub timeline. GH Archive makes this metadata available for analysis, including [over 15 event types](https://docs.github.com/en/rest/using-the-rest-api/github-event-types?apiVersion=2022-11-28). You can easily analyze GH Archive data by using the [Google Cloud Console](http://console.cloud.google.com) to query the dataset. 

This repository shares examples for how you can use BigQuery and the GH Archive dataset to analyze public GitHub activity for your next project.

## Outline

- [Getting Started](#getting-started)
- [Querying Basic Community Metrics](#querying-basic-community-metrics)
- [Additional Resources](#additional-resources)

## Getting Started

### What can GH Archive data be used for?

GH Archive has a [list of research, visualizations, and talks based on their dataset](http://www.gharchive.org/#resources) which may spur some ideas! A sampling of some of those projects include:

- [GitLive](https://www.gitlive.net/): a visualization of what's happening on GitHub in real-time
- [Changelog Nightly](https://changelog.com/nightly/): an email newsletter featuring the top new and top starred projects on GitHub
- [GitHut](https://githut.info): a visualization of programming languages used on GitHub

Whether you're an individual developer, a community, or an [Open Source Program Office (OSPO)](https://todogroup.org) managing multiple projects, the GH Archive dataset may be useful for you. Once your queries are written, you can apply them to new repositories and GitHub organizations, as well as adjust the time periods you're analyzing.

### Tips for using BigQuery as you begin

- If you're new to BigQuery, check out [A Beginner's Guide to BigQuery](https://www.datacamp.com/tutorial/beginners-guide-to-bigquery) as well as Google Cloud's guide: "[Explore the Google Cloud Console](https://cloud.google.com/bigquery/docs/bigquery-web-ui)" to become familiar with the console which we'll use in these examples.
- At the time of writing, the first 1 TiB per month of queries to BigQuery is free. Up-to-date details on pricing is available on the [BigQuery pricing](https://cloud.google.com/bigquery/pricing) page.
- Within the Google Cloud Console, after you type your query and before it is run you'll be told how much data will be queried.
- When starting with a query, start by querying a smaller amount of data (for example, one day) to reduce costs and imrpove your queries. After confirming that it works as intended, expand your query to more data (for example, a month or year).

## Querying Basic Community Metrics

Each of the following example queries build upon one another, looking at GitHub activity for projects released by the [Apache Software Foundation](https://www.apache.org). We'll run these queries by entering them into the [Google Cloud Web Console](http://console.cloud.google.com).

Note that while these queries look similar, there are noticable differences that impact what data is being queried as well as the results. Do you notice the differences?

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

### Comparing these examples

Each examples retrieves a single value as their output using `COUNT()` in the query. The examples are also all counting unique contributors (note the use of `DISTINCT` in the query to ensure that contributors are not double-counted).

What's different? A few things!

1. The data being queried. Notice the difference between these examples:

- Day: `githubarchive.day.20240201`
- Month: `githubarchive.month.202402`
- Year: `githubarchive.year.2023`

2. The scope of the query's `WHERE` statements:

- Specific Repository: `events.repo.name = 'apache/cassandra'`
- GitHub Organization: `events.org.login = 'apache'`

## Additional Resources

"[Exploring GitHub with BigQuery at GitHub](https://www.youtube.com/watch?v=Ast3-RFVHkM)" (video)(2017) introduces you to the BigQuery UI, writing queries to access GH Archive, and visualizing data with tools like Tableau and Looker.

GH Archive data is also used by multiple services which analyze GitHub activity and provide higher-level interfaces, APIs, and open source tools:

- [Ecosyste.ms Timeline](https://timeline.ecosyste.ms): WebUI and open API service of over 8 billion events for every public repo on GitHub dating back to 2015
- [DevStats](https://github.com/cncf/devstats): CNCF-created tool for that visualizes GH Archive data using Grafana dashboards