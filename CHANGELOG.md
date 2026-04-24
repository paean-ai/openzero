# Changelog

## 0.9.1

- Fixed long-session reasoning loss: thinking content from earlier turns is now preserved across cache-miss boundaries (previously an internal latch discarded all but the most recent turn after ~1 hour idle)
- Reasoning effort now defaults to `high` across all providers and subscription tiers (removed a stale branch that downgraded the default to `medium`)
- Removed an experimental length-anchor system prompt section (≤25 words between tool calls / ≤100 words per reply) after ablation confirmed a negative impact on coding quality

## 0.9.0

- Added `pro` / `flash` / `lite` model-tier aliases alongside `opus` / `sonnet` / `haiku`; provider-agnostic tier names are preferred in displays

## 0.7.1

- Added DeepSeek (Official) provider preset
- Welcome banner now shows the active model's context window size

## 0.7.0

- Added Volcengine coding-plan provider preset
- Added in-memory provider configuration cache
- Fixed third-party (BYOK) auth edge cases that mis-identified the active provider on cold start

## 0.5.5

- Per-provider context-window disclosure in `/provider`
- Standalone updater path for users not installing via npm

## 0.5.0

- Added `/provider` command for switching between Anthropic-protocol providers
- Provider-scoped model list in the `/model` picker
