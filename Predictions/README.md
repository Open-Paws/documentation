# Animal Advocacy Prediction Models

## Overview
A suite of prediction models designed to evaluate animal advocacy content through two complementary approaches:

1. **Performance Prediction** – Evaluates content based on real-world advocacy metrics.
2. **Preference Prediction** – Assesses content using human and synthetic feedback.

## Using the Models 

These models are text regression models that predict various metrics for animal advocacy content. You can easily use them through the Hugging Face `transformers` library pipeline.

### Using HuggingFace's Transformers Library

```python
from transformers import pipeline

# Initialize the pipeline for text regression
predictor = pipeline(
    "text-classification",
    model="open-paws/perceived_trustworthiness_prediction",
    return_all_scores=True
)

# Make predictions
text = "Animals deserve to live free from exploitation and suffering."
result = predictor(text)

# Results will be a score between 0 and 1
print(result[0][0]['score'])  # Example output: 0.856
```

### Batch Processing

For processing multiple texts efficiently:

```python
texts = [
    "Animals deserve to live free from exploitation.",
    "Factory farming causes immense suffering.",
    "We must take action to protect animals."
]

# Process multiple texts at once
results = predictor(texts)

# Access scores
for text, result in zip(texts, results):
    score = result[0]['score']
    print(f"Text: {text}\nScore: {score}\n")
```

## Known Limitations and Best Practices

### Handling Out-of-Range Predictions

These models are regression-based and can occasionally predict scores outside the intended 0-1 range. This occurs when the model encounters content that it predicts should score more extremely than examples from its training data. To handle this gracefully, we recommend clipping the values to the valid range:

```python
from transformers import pipeline
import numpy as np

predictor = pipeline(
    "text-classification",
    model="open-paws/perceived_trustworthiness_prediction",
    return_all_scores=True
)

def clip_score(score):
    """Clips the score to be between 0 and 1."""
    return np.clip(score, 0, 1)

text = "Animals deserve to live free from exploitation and suffering."
result = predictor(text)
score = result[0][0]['score']
clipped_score = clip_score(score)

# For batch processing
texts = [
    "Animals deserve to live free from exploitation.",
    "Factory farming causes immense suffering.",
    "We must take action to protect animals."
]

results = predictor(texts)
for text, result in zip(texts, results):
    score = clip_score(result[0]['score'])
    print(f"Text: {text}\nClipped Score: {score}\n")
```

## [Available Models on Hugging Face](https://huggingface.co/collections/open-paws/ranking-models-67b94c024535b84d0b73648b)
