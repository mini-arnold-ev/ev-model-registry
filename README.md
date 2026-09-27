# EV Model Registry

Model catalogue for Exponential View services, fetched from the [OpenRouter models API](https://openrouter.ai/docs/api/api-reference/models/list-all-models-and-their-properties).

## Freshness and scope

Read `registry.json.updated_at` to establish freshness. The intended schedule is 00:50, 06:50, 12:50 and 18:50 UTC. A schedule alone does not prove a successful update.

The catalogue lists text-input/text-output models and their published metadata. It does not set OpenClaw routing or prove that a particular account can use a model. Runtime defaults and session selections live in the local generated `clawd-v2/MODELS.md` reference.

## Schema version 2

Existing `providers`, `flagships`, pricing and capability-index fields are retained. Provider model lists now contain every eligible returned model instead of only six candidates.

- `id`, `name`, `context_length`, `context_k`, `created`: API identifiers and metadata.
- `input_mtok`, `output_mtok`: USD per million tokens. Zero is preserved; missing or invalid prices are null.
- `vision`: established from image input modality.
- `reasoning`: reasoning controls listed in `supported_parameters`; null if that metadata is missing.
- `web_search`: true when the API declares `web_search_options`; otherwise null. A provider name alone is not evidence of search support.
- `input_modalities`, `output_modalities`, `supported_parameters`: source metadata for checking these fields.
- `tier` and `flagships`: legacy heuristic display candidates. They are not authoritative rankings, latest-version guarantees or model recommendations. Verify current provider information before choosing a model.

`capability_index` includes only entries whose corresponding capability field is true.

## Update guarantees

The updater uses a fixed script, without a language-model turn. It rejects empty, malformed or unexpectedly reduced catalogues, checks HTTP failures, and prevents overlapping runs. Failed fetches preserve the previous catalogue and timestamp.

GitHub writes use the existing file SHA to prevent overwriting concurrent edits. The updater reads each published commit back and compares the bytes before installing the local catalogue. Local files are replaced atomically. `update-status.json` on the host records each stage and its outcome; credentials are never included in published files or status output.

Before publication, the updater checks the GitHub account and repository write access. It can use an existing named GitHub CLI login, then configured tokens if needed. An expired token cannot hide a valid CLI login. The Desktop repair launcher checks authentication before changing the job and offers GitHub browser sign-in when renewal is required. The scheduled updater never starts an interactive login.
