# EV Model Registry

AI model registry for Exponential View services. Tracks flagship models, pricing, and capabilities across all major providers.

**Updated:** Regularly  
**Source:** OpenRouter API  
**Schema version:** 2

## Quick fetch

```bash
curl -s https://raw.githubusercontent.com/mini-arnold-ev/ev-model-registry/main/registry.json
```

## Schema

Each model entry:

```json
{
  "id": "perplexity/sonar-reasoning-pro",
  "name": "Sonar Reasoning Pro",
  "context_k": 128,
  "tier": "flagship",
  "input_mtok": 2.0,
  "output_mtok": 8.0,
  "reasoning": true,
  "web_search": true,
  "vision": true
}
```

## Capability index

Top-level `capability_index` lets you query across providers:

- `web_search` — models with native web search (Perplexity sonar, Grok)
- `reasoning` — models with reasoning/thinking modes (o-series, R1, sonar-reasoning, :thinking)
- `vision` — models that accept image inputs
- `web_search_and_reasoning` — models with both

## Providers tracked

Anthropic, OpenAI, Google, xAI, Meta, Mistral, Qwen, DeepSeek, Liquid AI, Perplexity, Cohere, Baidu, ByteDance

## Usage

```javascript
const reg = await fetch(
  'https://raw.githubusercontent.com/mini-arnold-ev/ev-model-registry/main/registry.json'
).then(r => r.json());

// Current Anthropic flagship:
reg.providers.anthropic.flagship

// Models with web search + reasoning:
reg.capability_index.web_search_and_reasoning

// All models under $5/MTok input:
reg.providers.perplexity.models.filter(m => m.input_mtok < 5)
```
