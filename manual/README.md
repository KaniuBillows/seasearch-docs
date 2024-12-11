# Introduction

**SeaSearch** is a lightweight search engine built on open source search engine ([ZincSearch](https://zincsearch-docs.zinc.dev/)), implemented in Go language. Our goal is to use SeaSearch to replace ElasticSearch as the default search engine for Seafile.

## SeaSearch vs. ElasticSearch:

- **Lightweight**: SeaSearch is implemented in Go language. So it's more lightweight than ElasticSearch, which is implemented in Java.
- **One Index per Library**: SeaSearch can support large number of indexes in the system. With this feature, we can create one index for each library in Seafile. This limits the amount of data that needs to be searched for queries. With ElasticSearch, we have to save data from all libraries into one index, which is not good for performance when you have large amount of data.
- **Good Compatibility**: API compatible with ElasticSearch
- **Support S3 Storage**: SeaSearch can use S3 as storage
- **Shared Storage Architecture for Cluster**: ElasticSearch's cluster architecture is based on replication of data among nodes. It's complex to maintain and not easy to scale. SeaSearch uses a shared-storage architecture. Cluster nodes share the same storage (usually S3 compatible object storage). With this architecture, it's easier to provide HA guarantees and easier to maintain. It's also possible to scale query performance by using more query nodes.

## Project Status and Plan

At the moment (end of 2024) the text indexing and search feature is ready for Beta usage. Upcoming features includes:

- Clustering
- Vector indexing and search
