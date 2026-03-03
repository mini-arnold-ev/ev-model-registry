# EV Model Registry

AI model registry for Exponential View services. Tracks flagship models, pricing, and capabilities across all major providers.

**Updated:** 4× daily (00:50, 06:50, 12:50, 18:50 UTC)  
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

## Flagship change alerts

When a provider's flagship model changes, a Discord alert fires to `#model-registry` on the EV server.

## Usage in Replit services

```javascript
const REGISTRY_URL = 'https://raw.githubusercontent.com/mini-arnold-ev/ev-model-registry/main/registry.json';

async function getRegistry() {
  const res = await fetch(REGISTRY_URL);
  return res.json();
}

// Get cheapest model with reasoning + web search
async function getBestSearchReasoner() {
  const reg = await getRegistry();
  return reg.capability_index.web_search_and_reasoning[0];
}

// Get current Anthropic flagship
async function getAnthropicFlagship() {
  const reg = await getRegistry();
  return reg.providers.anthropic.flagship;
}
```
