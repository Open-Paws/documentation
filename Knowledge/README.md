# Quick Start Guide: Vector-Graph Database with OpenAI Integration

This guide demonstrates how to use our read-only vector-graph hybrid database powered by [Weaviate](https://weaviate.io/developers/weaviate) with OpenAI integration for AI-enabled knowledge management. You'll need to provide your own OpenAI API key for generating embeddings and using generative AI features.

For complete documentation, visit the [Weaviate Documentation](https://weaviate.io/developers/weaviate).

## Connection Details

```
REST Endpoint: https://larnd9rle1zwapsdcqw.c0.us-east1.gcp.weaviate.cloud
gRPC Endpoint: https://grpc-larnd9rle1zwapsdcqw.c0.us-east1.gcp.weaviate.cloud
ReadOnly API Key: 77dlitQ5eF1NHidCTp2f3kjPQUQiPF8C6aei
```

## 1. Database Connection

See [How to Connect](https://weaviate.io/developers/weaviate/client-libraries/python) for more details.

```python
import weaviate
from weaviate.classes.init import Auth
import os

# Connect with read-only API key
client = weaviate.connect_to_weaviate_cloud(
    cluster_url=os.environ["WEAVIATE_URL"],
    auth_credentials=Auth.api_key(os.environ["WEAVIATE_API_KEY"]),
    headers={
        "X-OpenAI-Api-Key": os.environ["OPENAI_APIKEY"]
    }
)
```

## 2. Search Operations

See [Search Guide](https://weaviate.io/developers/weaviate/search) for complete search documentation.

### Vector Search

```python
content = client.collections.get("Content")
response = content.query.near_text(
    query="factory farming investigation techniques",
    limit=3
)

for result in response.objects:
    print(result.properties)
```

### Hybrid Search (Vector + Keyword)

```python
response = content.query.hybrid(
    query="effective animal advocacy strategies",
    alpha=0.5,  # Balance between vector and keyword search
    limit=3
)
```

### Filtered Search

```python
from weaviate.classes.query import Filter

response = content.query.near_text(
    query="sanctuary best practices",
    filters=Filter.by_property("content_type").equal("report"),
    limit=3
)
```

## 3. Advanced Features

### RAG (Retrieval Augmented Generation)

See [RAG Documentation](https://weaviate.io/developers/weaviate/search/rag) for more details.

#### Single Prompt (Per-Result Generation)

```python
response = content.generate.near_text(
    query="factory farming investigation techniques",
    single_prompt="Summarize this content: {main_text}",
    limit=3
)

print(response.generated)
```

#### Grouped Task (Single Generation for All Results)

```python
response = content.generate.near_text(
    query="successful campaign strategies",
    grouped_task="What patterns emerge from these successful campaigns?",
    limit=5
)

print(response.generated)
```

### Aggregate Data

See [Aggregations Documentation](https://weaviate.io/developers/weaviate/aggregate) for more options.

```python
response = content.aggregate.over_all(
    group_by=weaviate.classes.aggregate.GroupByAggregate(
        prop="content_type"
    )
)

for group in response.groups:
    print(f"Type: {group.grouped_by.value} Count: {group.total_count}")
```

## 4. Best Practices

1. Use hybrid search for optimal retrieval
2. Leverage filters to narrow results
3. Consider RAG for dynamic content generation
4. Use aggregations for movement insights
5. Start with small result limits and increase as needed
6. Test queries with different alpha values in hybrid search to find the optimal balance

Note: Always refer to [OpenAI's documentation](https://platform.openai.com/docs) and [Weaviate's documentation](https://weaviate.io/developers/weaviate) for the most up-to-date information.
