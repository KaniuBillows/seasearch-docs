## Search Documents
### Query DSL
To perform full-text search, use the DSL. For usage, refer to:

[Query DSL Documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html)

We do not support all query parameter options provided by ES. Unsupported parameters include: indices_boost, knn, min_score, retriever, pit, runtime_mappings, seq_no_primary_term, stats, terminate_after, version.

Search API: [Search API](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-search.html)

### Delete by Query
To delete documents based on a query, use the delete-by-query operation. Like search, we do not support some ES parameters.

ElasticSearch API: [Delete by Query](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-delete-by-query.html)

### Multi-Search
Multi-search supports searching multiple indexes and running different queries on each index.

ElasticSearch API: [Multi-Search API Documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-multi-search.html)

```
[POST] /es/_msearch
{"index": "t1"}
{"query": {"bool": {"should": [{"match": {"filename": {"query": "数据库", "minimum_should_match": "-25%"}}}, {"match": {"filename.ngram": {"query": "数据库", "minimum_should_match": "80
```

### Unified-Search
Unified-Search supports searching multiple indexes and running different queries on each index.
We use the same scoring information across different indexes for more accurate score calculation.

This API is meaningful only in this scenario: when searching the same query across multiple indexes. For example, in Seafile, we create an index for each library. When globally searching across all accessible libraries, using this API ensures consistent scoring across different repositories, providing more accurate search results.

```
[POST] /api/unified_search
{
    "index_queries": [
        {
            "index": "index1",
            "query": {}
        },
        {
            "index": "index2",
            "query": {}
        }
      ]
}
```
