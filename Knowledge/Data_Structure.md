# Quick Start Guide: Weaviate Vector Database with OpenAI Integration

This guide demonstrates how to use our read-only vector database powered by [Weaviate](https://weaviate.io/developers/weaviate) with OpenAI integration for AI-enabled knowledge management. You'll need to provide your own OpenAI API key for generating embeddings and using generative AI features.

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

## 4. Database Schema

This schema defines the structure of our unified, open-access database designed to support animal advocacy. By integrating data from diverse sources, it supports AI applications such as retrieval-augmented generation (RAG) and model training, as well as general strategic planning and referencing. 

The Content class functions as the primary centralized repository for materials related to veganism and animal advocacy, such as articles, reports, blogs, and transcripts. It supports AI applications like semantic search, knowledge graphs, and retrieval-augmented generation (RAG).

**Structure**:

- **summary:** text
- **main_text:** text
- **content_type:** text
- **source_url:** array of texts
- **date**: date
- **scores**: object
    - **good_for_animals:** number
    - **cultural_sensitivity**: number
    - **relevance**: number
    - **insight**: number
    - **trustworthiness**: number
    - **emotional_impact**: number
    - **rationality**: number
    - **influence**: number
    - **alignment**: number
    - **predicted_performance**: number
    - **average_score**: number
- **tags**: array of text

## 5. Best Practices

### General Recommendations

1. Use hybrid search for optimal retrieval
2. Leverage filters to narrow results
3. Consider RAG for dynamic content generation
4. Use aggregations for movement insights
5. Start with small result limits and increase as needed
6. Test queries with different alpha values in hybrid search to find the optimal balance

### Optimized Retrieval Strategy

7. **Retrieve and Re-rank Pattern**: When quality matters more than speed, retrieve a larger set of results (e.g., 20-30 items) and then apply post-processing to select the best ones:

```python
# Retrieve more results initially
response = content.query.near_text(
    query="effective outreach strategies",
    limit=20
)

# Sort by relevant scores and select top performers
sorted_results = sorted(
    response.objects,
    key=lambda x: x.properties['scores']['predicted_performance'],
    reverse=True
)

# Pass only top 5 to your application
top_results = sorted_results[:5]
```

8. **Multi-Score Ranking**: Combine multiple scores for more nuanced ranking:

```python
def calculate_composite_score(item):
    scores = item.properties['scores']
    # Weight different scores based on your use case
    return (
        scores['relevance'] * 0.3 +
        scores['trustworthiness'] * 0.3 +
        scores['insight'] * 0.2 +
        scores['predicted_performance'] * 0.2
    )

sorted_results = sorted(
    response.objects,
    key=calculate_composite_score,
    reverse=True
)
```

### Advanced Filtering for Better RAG Results

9. **Score-Based Filtering**: Use score thresholds to ensure quality before RAG generation:

```python
from weaviate.classes.query import Filter

# Only retrieve highly trustworthy and relevant content
response = content.generate.near_text(
    query="campaign evaluation methods",
    filters=(
        Filter.by_property("scores.trustworthiness").greater_than(0.7) &
        Filter.by_property("scores.relevance").greater_than(0.6)
    ),
    grouped_task="Synthesize the most reliable evaluation methods",
    limit=10
)
```

10. **Time-Aware Retrieval**: Combine date filters with scores for recency-weighted results:

```python
from datetime import datetime, timedelta

# Get high-quality content from the last 6 months
recent_date = datetime.now() - timedelta(days=180)

response = content.generate.near_text(
    query="emerging advocacy trends",
    filters=(
        Filter.by_property("date").greater_than(recent_date) &
        Filter.by_property("scores.insight").greater_than(0.5)
    ),
    grouped_task="What are the most promising recent trends?",
    limit=15
)
```

11. **Progressive Filtering**: Start broad, then narrow based on initial results:

```python
# First pass: broad retrieval
initial_response = content.query.near_text(
    query="corporate campaigns",
    limit=30
)

# Analyze score distribution
avg_trustworthiness = sum(
    obj.properties['scores']['trustworthiness'] 
    for obj in initial_response.objects
) / len(initial_response.objects)

# Second pass: refined search with dynamic threshold
refined_response = content.generate.near_text(
    query="corporate campaigns",
    filters=Filter.by_property("scores.trustworthiness").greater_than(avg_trustworthiness),
    grouped_task="What makes these campaigns particularly effective?",
    limit=10
)
```

Note: Always refer to [OpenAI's documentation](https://platform.openai.com/docs) and [Weaviate's documentation](https://weaviate.io/developers/weaviate) for the most up-to-date information.
