# Zero

**A terminal-based coding agent for Anthropic-protocol LLM APIs — built for third-party and self-hosted providers.**

Zero is a command-line coding assistant that speaks the Anthropic Messages API, but is designed from the ground up for the ecosystem of *non-Anthropic* providers that implement the same protocol: Zhipu GLM (z.ai), Alibaba Qwen, DeepSeek, Volcengine Doubao, Paean, and any self-hosted model you expose through an Anthropic-protocol-compatible gateway (Ollama with a proxy, vLLM, TabbyAPI, LiteLLM, etc.).

The harness — context windows, token budgets, prompt caching, permission prompts, MCP servers, skills, slash commands, agent runtime — is tuned for the realities of these providers rather than the defaults of Anthropic's first-party endpoint. Bring your own key, pick a model, start coding.

> **This repository contains public-facing documentation only.** The Zero CLI source lives in a separate repository and is distributed as a prebuilt binary via npm. The `CHANGELOG.md` here is the source of truth that the CLI fetches at runtime to render release notes.

---

## Highlights

- **Works with any Anthropic-protocol-compatible endpoint.** Point Zero at z.ai, Volcengine, DeepSeek, your self-hosted gateway, or Anthropic itself — same CLI, same workflow.
- **BYOK-first.** No account required. No subscription paywalls. You hold the key; Zero just drives the API.
- **Provider-aware defaults.** Context windows, cache TTLs, token accounting, and welcome-banner metadata adapt per provider instead of assuming Anthropic's defaults.
- **No telemetry. No analytics. No remote feature flags.** Every observability / experimentation code path is a stub that silently no-ops — see [Privacy](#privacy) for specifics.
- **Model tier aliases.** Use `pro` / `flash` / `lite` (provider-agnostic) or the original `opus` / `sonnet` / `haiku` names — whichever is more natural for your provider.
- **Full MCP + skills + slash-command surface.** The plugin ecosystem your agent already expects, minus the vendor lock-in.

---

## Installation

Zero ships as a single prebuilt bundle on npm.

```bash
# Recommended — install globally with bun
bun add -g @paean-ai/zero-cli

# Or with npm
npm install -g @paean-ai/zero-cli

# Or run without installing
npx @paean-ai/zero-cli
```

The binary is named `zero`:

```bash
zero --version
zero --help
```

**Requirements:** Node.js 18 or newer. macOS, Linux, and Windows (WSL strongly recommended on Windows) are supported.

---

## Quick start

```bash
# 1. Launch the interactive setup
zero

# 2. Pick a provider on first run, or configure one explicitly:
zero /provider

# 3. Start a session in any project directory
cd ~/code/my-project
zero
```

Zero stores its configuration under `~/.zero/` and honors environment variables with the precedence:

```
ZERO_CLI_*   >   ZERO_*   >   ANTHROPIC_*
```

So `ZERO_CLI_API_KEY`, `ZERO_API_KEY`, and `ANTHROPIC_API_KEY` all work — the more specific one wins. Legacy `ANTHROPIC_*` variables are honored for drop-in compatibility with existing scripts and CI.

### Point Zero at a specific provider

```bash
# z.ai / Zhipu GLM
export ZERO_API_KEY=<your-zai-key>
export ZERO_BASE_URL=https://api.z.ai/api/anthropic
zero

# DeepSeek (Anthropic-protocol endpoint)
export ZERO_API_KEY=<your-deepseek-key>
export ZERO_BASE_URL=https://api.deepseek.com/anthropic
zero

# Self-hosted / local gateway (Ollama + proxy, vLLM, LiteLLM, etc.)
export ZERO_API_KEY=<any-token-your-gateway-accepts>
export ZERO_BASE_URL=http://localhost:11434/anthropic
zero
```

Or use the interactive `/provider` command inside the CLI — Zero ships with presets for the most common third-party providers, so you can skip the environment-variable dance.

---

## Supported providers

Zero is protocol-agnostic by design. These providers have been validated with provider-aware defaults (context window, cache behavior, model catalog):

| Provider              | Notes                                                      |
| --------------------- | ---------------------------------------------------------- |
| Zhipu GLM (z.ai)      | GLM-4.6 and newer; long-context variants                   |
| DeepSeek (Official)   | Anthropic-protocol endpoint                                |
| Volcengine (Doubao)   | Coding Plan presets                                        |
| Alibaba Qwen          | Qwen-Coder series                                          |
| Paean                 | Paean Pro / Flash / Lite tiers                             |
| Anthropic (BYOK)      | Works out of the box; not the focus of this project        |
| Self-hosted / local   | Any Anthropic-protocol gateway (LiteLLM, vLLM + adapter, …) |

If your endpoint speaks the Anthropic Messages API and supports streaming, Zero will drive it. Provider-specific polish (context window hinting, cache strategy, welcome-banner metadata) can be contributed via issues on this repo.

---

## Privacy

Zero is designed to be **locally observable only**. The CLI:

- **Does not send telemetry, analytics, usage metrics, or crash reports anywhere.** The internal analytics and OpenTelemetry code paths (`logEvent`, `logOTelEvent`, Statsig-style gates, GrowthBook feature flags, BigQuery exporter, Perfetto traces, session spans, plugin telemetry) are all compiled-in stubs that return immediately without writing to any network socket.
- **Does not phone home for feature flags or A/B experiments.** Feature-flag calls return the safe default baked into the binary.
- **Does not upload conversation content, tool traces, prompts, or completions** to any third party beyond the LLM provider you have explicitly configured.
- **Does not require a subscription, login, or account.** Authentication against your chosen provider happens directly, via a key you supply.

What *is* sent over the network:

1. Your API requests to the LLM provider you configured (with the key you supplied), and
2. A startup check that fetches this repository's `CHANGELOG.md` to render the "What's new" section of the welcome banner. This is an unauthenticated request to `raw.githubusercontent.com`; it contains no session or user identifiers.

That is the complete list. If you discover any code path that violates the first list, please open an issue — we consider it a privacy bug.

---

## Changelog

See [`CHANGELOG.md`](./CHANGELOG.md) for the full release history.

The Zero CLI fetches this file at startup and shows the most recent entries in the welcome banner's **What's new** section.

---

## Feedback & issues

This repo accepts issues and discussion about:

- Provider compatibility and defaults
- Documentation clarity
- Feature requests and UX friction
- Privacy or telemetry concerns

Please open an issue at [paean-ai/openzero/issues](https://github.com/paean-ai/openzero/issues).

For bugs that require reproducing against specific CLI internals, the maintainers may ask you for a `zero --debug` transcript or an anonymized config dump — both are produced on demand and stay on your machine unless you choose to share them.

---

## License

[MIT](./LICENSE) — the documentation in this repository is published under the MIT license. The Zero CLI binary on npm is also MIT-licensed.
