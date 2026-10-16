# Free topic-source research

Researched October 16, 2026. Free data is not automatically social trend data. The initial six niches deliberately use accessible APIs with no account or key required for the endpoints used. All six feeds returned live results during implementation.

| Niche | Free API | What it supplies | Implementation |
|---|---|---|---|
| AI & the future | [HN Algolia Search](https://hn.algolia.com/api) | Search `AI`, stories from the past 7 days; points/comments | Live |
| Tech & gadgets | [HN Algolia Search](https://hn.algolia.com/api) | Search `technology`, stories from the past 7 days; points/comments | Live |
| Startups & business | [HN Algolia Search](https://hn.algolia.com/api) | Search `startup`, past 7 days; points/comments | Live |
| Code & builders | [Forem/DEV API](https://developers.forem.com/api/v1) | `articles?tag=programming&top=7`; reactions/comments and descriptions | Live |
| Design & creativity | [Forem/DEV API](https://developers.forem.com/api/v1) | `articles?tag=design&top=7`; developer/design community posts | Live |
| Space & discovery | [Spaceflight News API](https://www.spaceflightnewsapi.net/) | Recent articles, publisher summaries, timestamps, source links | Live; freshness only |

HN's official [Firebase API](https://github.com/HackerNews/API) provides top/new story IDs and item metadata, but no niche search. Algolia’s HN index adds search and time filters without requiring a key. Search ranking is Algolia relevance/popularity within the time window, not a custom viral score. Keyword relevance is imperfect, and DEV design coverage is web/developer-oriented.

## Additional niches assessed

| Niche | Candidate API | Decision |
|---|---|---|
| Crypto | [CoinGecko](https://www.coingecko.com/en/api) trending search | A free Demo plan exists; key/account and current quota/usage terms should be checked before integrating. Not enabled in this no-key MVP. |
| Food & cooking | [TheMealDB](https://www.themealdb.com/api.php) | Useful recipe inspiration, but recipe lookup/random recipes do not establish current trends. Not presented as a trend feed. |
| Art & culture | [Art Institute of Chicago](https://api.artic.edu/docs/) | Public collection API; useful reference material, not a current popularity signal. Not enabled as trending. |
| Sports | TheSportsDB candidate | Documentation could not be verified in this research pass; deferred. |

Do not fill unsupported niches with old static sample cards. A broader consumer niche catalog should add a defensible recent-news or popularity source first. These APIs do not measure Instagram hashtags, audio usage, view velocity or Reel performance. The product labels each signal honestly.

## Operational behavior

- Five-minute in-memory cache, at most 12 cards per niche.
- Twenty-second source timeout and visible retry errors.
- All source requests run on the Node server, avoiding browser CORS dependencies.
- Attribution and original links stay visible and are included in exported scripts.
- Full article scraping is intentionally absent; only supplied headlines/descriptions/summaries enter generation.
- Free APIs have fair-use limits and changing terms. Check production permissions before a paid public rollout; no unrestricted commercial reuse of publisher content is implied.
