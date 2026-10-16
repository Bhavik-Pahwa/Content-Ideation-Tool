# Reelflow

A local, no-login content studio that turns fresh niche stories into editable Instagram reel drafts. Six niches, live source cards, source highlights, and four scripted scenes: **Hook → Plot → Twist → Conclusion**.

## Run

Requirements: Node.js 22+ and [Ollama](https://ollama.com/). No npm dependencies or API keys are required.

```powershell
ollama pull qwen3:8b
npm start
```

Open http://localhost:3000. Ollama must be running. On this machine, Qwen3 8B was already installed. A first request may take longer while the model loads. Scripts have a four-minute timeout, with a visible error and retry path. Run `npm test` for server contract tests.

Optional PowerShell configuration:

```powershell
$env:OLLAMA_MODEL = 'qwen3:8b'
$env:OLLAMA_URL = 'http://127.0.0.1:11434'
$env:PORT = '3000'
npm start
```

The app listens on loopback intentionally. It is a local MVP, not a multi-tenant production service. A public deployment needs an inference host, request quotas, concurrency management and appropriate access controls. A hosted static site cannot reach this machine’s local Ollama server.

## Experience

1. Choose AI, technology, startups, coding, design or space.
2. Pick a live story. Each card shows its source, date and actual source signal.
3. Review the available source highlights. Choose 30/45/60 seconds and a delivery style.
4. Generate a local-model draft, edit voiceover text, then copy or download it with source attribution.

Warm paper colors, coral accents, Manrope/DM Sans with Instrument Serif, animated scene entry, responsive layouts, keyboard focus states and reduced-motion support. Fonts use Google Fonts with system fallbacks.

## Why open innovation is core

Qwen3 through Ollama writes every generated script. There is no canned fallback masquerading as AI. You can change the model, modify the prompt and inspect the JSON schema in `server.mjs`. With the default loopback configuration, prompts and scripts stay on this machine; there is no paid inference API or per-generation vendor fee. Hardware, electricity and any future hosting still have costs. Live discovery requires internet, and the current in-memory feed cache does not survive restarts. This is not a fully offline discovery app.

The target customer is a solo creator or freelance social media manager who repeatedly researches and outlines reels. Potential paid value is time saved in research and scripting; willingness to pay has not yet been validated with users.

## Data and accuracy

See [API research](docs/API-RESEARCH.md) for the researched niche/source map and limitations. HN and DEV are community signals, **not Instagram trend measurements**. Spaceflight News is explicitly marked fresh news, not popularity. The server caches feeds for five minutes. Upstream failures surface as errors, never fabricated stories.

HN often provides only a headline. In that case the app labels its limited coverage and asks the model for a question/exploration rather than inventing article details. DEV descriptions and Spaceflight publisher summaries provide richer source highlights. Generated drafts still need editorial fact checking. Timings are approximate editorial guides, not measured audio durations.

Source text is treated as untrusted model input. Model responses are constrained with a JSON schema and validated again before presentation. UI content is escaped, external links allow only HTTP(S), generation inputs refer to server-held stories, and cross-origin generation requests are blocked. The server uses fixed upstream URLs rather than fetching arbitrary article URLs.

## Project layout

- `server.mjs`: HTTP server, source adapters, cache, local inference and validation.
- `public/`: buildless browser application and responsive styles.
- `server.test.mjs`: server contracts and malformed-output checks.
- `docs/API-RESEARCH.md`: free API research and niche coverage.

## Challenge context

The supplied theme matches [Hacktoberfest Open-Source AI Challenge: Week 2](https://dev.to/events/80), October 12–18, 2026. Its stated deadline is October 18 at 11:59 PM PDT (October 19 at 12:29 PM IST). The entry needs a DEV post with `#hf26challenge`, a demo, a clear intended customer and an explanation of open innovation. No entry has been submitted, no customer validation is claimed, and no public deployment has been made.

## Research note

[Ollama’s official structured-output guidance](https://docs.ollama.com/capabilities/structured-outputs) informed schema-constrained generation. [Jangwook Kim’s community walkthrough](https://dev.to/jangwook_kim_e31e7291ad98/ollama-structured-outputs-in-practice-getting-type-safe-json-from-local-llms-with-pydantic-m38) discusses schema validation and small-model limitations. The comments endpoint was unavailable during research; no consensus or benchmark claim is inferred from that single article.
