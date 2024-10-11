# Introduction

**SeaSearch** is a lightweight search engine will replace ElasticSearch as the default search engine, built on open source search engine ([ZincSearch](https://zincsearch-docs.zinc.dev/)) implemented in Go language. 

## SeaSearch vs. ElasticSearch:

- **Lightweight**: SeaSearch implemented in Go language, which is more lightweight than ElasticSearch the heavyweight Java program
- **Single Index per Library**: SeaSearch can search entire storage inside a libary and filter out the results with access permissions of the user, as ElasticSearch does not support
- **Good Compatibility**: API compatible with ElasticSearch
- **Support S3 Storage**: SeaSearch can use S3 as storage
- **Easier Shared Storage in a cluster**: ElasticSearch replicates data between the nodes so consistency is more complex
