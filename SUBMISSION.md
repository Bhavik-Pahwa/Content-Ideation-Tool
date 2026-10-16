*This is a submission for the [Hacktoberfest Open-Source AI Challenge Week 2: Side Hustle](https://dev.to/challenges/hacktoberfest-week2-2026-10-12)*

## What I Built

I built **Reelflow**, a content ideation tool that helps creators turn fresh stories in their niche into a rough Instagram Reel script.

The problem starts before someone presses record: finding something worth talking about, understanding the story, and turning it into a short video with a clear beginning and ending. For a solo creator or freelance social media manager, repeating that research for every Reel can take time away from actually creating.

Reelflow brings those steps into one focused flow:

1. **Choose a niche:** AI, technology, startups, coding, design, or space.
2. **Find a spark:** explore cards containing fresh stories, source links, publication dates, and available community signals.
3. **Get the context:** review the source highlights and choose a 30-, 45-, or 60-second Reel with a conversational, educational, or bold delivery.
4. **Generate the flow:** get a **hook, plot, twist, and conclusion**, with voiceover text, shot suggestions, and on-screen overlays.
5. **Make it yours:** edit the voiceover, then copy or download the script with its source attribution.

There is no login or signup. The interface moves through one stage at a time, using warm colors, modern typography, and animated transitions to make the process feel like a creative onboarding experience.

The potential paying customer is a creator or freelance social media manager who needs to research and outline niche content regularly. The value I want to sell is less time between finding a story and having a usable recording plan. Willingness to pay is still a hypothesis; I have not validated it with paying customers.

## Demo

**Demo video:** TODO — add a public video walkthrough before publishing this submission.

The current version runs locally. It does not have a public deployment yet. To try it with Node.js 22+ and Ollama installed, run:

```bash
ollama pull qwen3:8b
npm start
```

Then open `http://localhost:3000` with Ollama running.

A walkthrough can follow the complete journey: select Space & Discovery, choose a live article, review its summary, generate a 30-second Reel, edit a line, and download the finished draft.

During development, all six source feeds returned live stories. A real generation using Qwen3 8B produced all four script stages in approximately **96 seconds on the development machine's CPU**. That is one observed run, not a guaranteed generation time. Browser visual verification was not completed in the development session.

## Code

**GitHub repository:** TODO — add the public repository URL before publishing this submission.

The project includes an MIT license, setup instructions, API research notes, and five passing server tests.

The main files are:

- `server.mjs`: source integrations, feed caching, Ollama requests, and generated-output validation.
- `public/app.js`: the niche-to-script journey, editable voiceovers, and exports.
- `public/style.css`: responsive layouts, transitions, and reduced-motion support.
- `server.test.mjs`: server contracts, generation input checks, and malformed-output checks.
- `docs/API-RESEARCH.md`: the niche-to-API mapping and source limitations.

## How I Built It

**Qwen3 8B, an open-weight model running locally through Ollama, is the creative engine.** Every generated script comes from the model; the app does not substitute a canned script when inference fails.

The application uses a Node.js HTTP server and a plain HTML, CSS, and JavaScript frontend. It has no npm runtime dependencies. The backend fetches stories from free public API endpoints and caches each niche's feed for five minutes:

- [Hacker News search through Algolia](https://hn.algolia.com/api) supplies recent AI, technology, and startup stories with points and comment counts.
- [The DEV API](https://developers.forem.com/api/v1) supplies coding and design posts with descriptions and community reactions.
- [Spaceflight News API](https://www.spaceflightnewsapi.net/) supplies recent space reporting with publisher summaries.

These signals need context: community activity is not proof that something is trending on Instagram, and recent space reporting is not a popularity ranking. Reelflow labels the distinction instead of inventing a viral score.

After a user selects a story, the server sends its available source context, desired duration, and delivery style to Ollama's `/api/chat` endpoint. A JSON schema defines the four required scenes and their voiceover, visual, and overlay fields. The server validates the response again before displaying it.

The prompt tells the model to treat source text as data, use only supplied facts, and avoid invented statistics or quotes. When a source supplies only a headline, the interface discloses that limitation and the prompt requests an exploratory angle. These safeguards do not guarantee factual accuracy: the creator still needs to check the original source before recording.

I used Codex to help implement and test the application. The generated Reel content itself is powered by the local Qwen model.

## Why Does Open Innovation Matter?

Open innovation makes the economics and ownership of this tool more flexible.

**Experimenting does not incur a paid inference API charge for each draft.** A creator can change the tone or regenerate an outline using their own machine. Hardware and electricity still cost money, and running a hosted service would add costs, but there is no per-generation model-provider bill in the local setup.

**The writing process stays on the machine running Reelflow.** With the default local configuration, the selected story context and generated script are processed by Ollama on that computer. Discovering fresh stories still requires internet access, so this is not a fully offline research tool.

**The creative engine can change with the product.** The model is configurable through `OLLAMA_MODEL`, and the prompt and output schema are directly editable. I can adapt the workflow to a specific creator's niche or try another compatible local model without rebuilding around a different paid API contract. This version uses the existing Qwen3 model; I have not fine-tuned it.

A closed API could also generate a script. The difference here is that I can run and inspect the application around a model I control locally, choose when to change it, and keep inference independent of a hosted provider's pricing. For a small tool aimed at repeat creative work, those choices matter.

## My Agent Session

I used Codex during development to research source APIs, implement the application, and check the server and local inference flow.

**Agent session:** TODO — optionally add a saved, sanitized session link or DevRelay `agent_session` embed before publishing. No session has been uploaded yet; remove this section if no session will be shared.
