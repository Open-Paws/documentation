# Animal Advocacy Language Models

## Overview

A suite of language models specialized for animal advocacy, available in different formats and optimizations. These are small models optimized for local and low-memory usage, so performance expectations should be realistic for their size.

## Available Models

### Core Chat Model (8B Parameters)

- [8B-base-model](https://huggingface.co/open-paws/8B-base-model) - Continually pre-trained model without instruction tuning
- [8B-instruct-chat](https://huggingface.co/open-paws/8B-instruct-chat) - Fine-tuned chat model with instruction tuning

## Usage Guide

### Basic Generation through HuggingFace's Transformers Library

The simplest way to use these models is through the HuggingFace `transformers` pipeline:

```python
from transformers import pipeline

# Initialize generation pipeline
generator = pipeline(
    "text-generation",
    model="open-paws/8B-instruct-chat",
    torch_dtype="auto",
    device_map="auto"
)

# Generate text
response = generator(
    "How can we effectively advocate for animals?",
    max_new_tokens=128,
    do_sample=True,
    temperature=0.7
)
print(response[0]['generated_text'])
```

### Batch Processing

For multiple inputs:

```python
texts = [
    "How can we help farm animals?",
    "What are effective ways to reduce animal suffering?",
    "How can we create systemic change for animals?"
]

results = generator(texts, max_new_tokens=128, batch_size=3)
```

### Recommended Hardware

- GPU: Any CUDA-compatible GPU with appropriate VRAM
- CPU: Possible, but significantly slower

### Generation Parameters

```python
# Recommended settings for different use cases
# Creative exploration
creative_settings = {
    "temperature": 0.7,
    "top_p": 0.9,
    "top_k": 50,
    "repetition_penalty": 1.2,
    "max_new_tokens": 256
}

# Focused responses
focused_settings = {
    "temperature": 0.2,
    "top_p": 0.9,
    "top_k": 50,
    "repetition_penalty": 1.1,
    "max_new_tokens": 128
}
```

## Known Limitations

1. **Model Size Trade-offs**: Being smaller models, they may not match the capabilities of larger language models for complex tasks.
2. **Base Model Influence**: While largely aligned for animal advocacy, there are edge cases where the models will act in unaligned ways, such as recommending nonvegan products. To address this, we recommend explicitly addressing animal alignment in the system message.
3. **User Context Awareness**: These models perform best when given clear contextual information about the user and their goals. For example, if engaging with a vegetarian considering veganism, the system prompt should explain that the model should provide supportive and practical guidance on eliminating dairy and eggs, suggesting suitable replacements, and addressing common concerns such as nutrition and meal planning. Ensuring prompts contain user context and developer intentions will maximize alignment with advocacy goals.

## Additional Resources

- [LLaMA Documentation](https://www.llama.com/docs/)
- [HuggingFace Transformers Documentation](https://huggingface.co/docs/transformers/)
- [Open Paws HuggingFace Models](https://huggingface.co/open-paws)
