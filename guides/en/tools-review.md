# Review of 47 tools from reels and chats

[Українська](../tools-review.md) | **English**

All of this came from reels, videos and chats in a single day. We went through every item and took only a few into our system. The rest isn't necessarily bad: it either duplicates what we already have or fits a task we don't have right now.

**How to read the verdicts:**
- ✅ installed for ourselves
- 📌 useful for a specific task, don't install ahead of time
- 📖 learning material, nothing to install
- ❌ didn't fit us, reason alongside

**What was checked (30.10.2026):** author, license, last activity, README. We read the code of the skill we shortlisted, and checked vulnerability records in [OSV](https://osv.dev) for packages only. For most items this is a review, not an audit. Stars are no proof, so we leave the counts out on purpose.

**Before installing anything:** see what runs at install time, pin the version to a commit, install into the project, not globally. A skill is text plus scripts the agent runs with your permissions.

## Skills and frameworks for Claude Code

| What | Verdict | For whom and when | Caveats |
|---|---|---|---|
| [cloudflare/security-audit-skill](https://github.com/cloudflare/security-audit-skill) | ✅ | auditing your own code, with each finding checked separately | many agents, so expensive. Without a sandbox it won't run target code. We copied it at a pinned commit, no `npx` |
| [obra/superpowers](https://github.com/obra/superpowers) | 📌 | if you want a ready "spec → plan → TDD" process | a hook loads text into every session, and the logo loads from their server by default (stated in the README). If you already have a process, take one skill |
| [msitarzewski/agency-agents](https://github.com/msitarzewski/agency-agents) | 📌 | a library of roles: read 1-2 for your task | hundreds of files, some for Chinese platforms. We took one: Discovery Coach. We did not use the app or the installer |
| [aicodeking/leanrea](https://github.com/aicodeking/leanrea) | 📌 | understanding how a program works. This is reverse engineering, not site cloning | built around macOS, the benchmark is the author's own, one contributor. Only analyze what you have the right to |
| [latent-spaces/brag](https://github.com/latent-spaces/brag) | 📌 | a short video about your project for LinkedIn or a portfolio. `brag-slim` is a single file | needs ffmpeg and render time |
| [Oldcircle/geo-sleuth](https://github.com/Oldcircle/geo-sleuth) | 📌 | finding where a photo was taken | your own photos only: the same technique finds where a person lives |
| [unclebob/swarm-forge](https://github.com/unclebob/swarm-forge) | 📌 idea | worth stealing the thought: QA verifies independently of the code's author | `zsh` + `tmux`, not for Windows, six agents, no license in the repo. The author himself warns not to buy any SWARM token |
| [google-research/harness2](https://github.com/google-research/harness2) | 📌 idea | for researchers: an agent improves its own harness on a task | Linux, Vertex AI, unrestricted network, not an official Google product |
| [ruvnet/ruflo](https://github.com/ruvnet/ruflo) | ❌ | | a huge layer on top, starts via `@latest`, mods run with your account's permissions |
| [affaan-m/ECC](https://github.com/affaan-m/ECC) | ❌ | | 71 agents, 314 skills, hooks. It lands on top of your system, not next to it |
| [morluto/rea](https://github.com/morluto/rea) | ❌ | | 207 MCP tools. If you really need reverse engineering, try `leanrea` first |
| [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) | ❌ | | duplicates [taste-skill and impeccable](../../README.en.md#-what-we-installed-and-what-we-rejected), which we already run |

## Connectors

| What | Verdict | For whom and when | Caveats |
|---|---|---|---|
| [Playwright](https://github.com/microsoft/playwright-mcp) | 📌 | driving a browser and testing a site | its own README recommends CLI plus skills over MCP for coding agents: fewer tokens |
| [Firecrawl](https://docs.firecrawl.dev/mcp-server) | 📌 | turning a page into clean text | [1,000 credits a month](https://www.firecrawl.dev/pricing) for free |
| [PostHog](https://posthog.com/docs/model-context-protocol) | 📌 | once you have a product with users | connecting is free, some tools are billed |
| [Perplexity](https://docs.perplexity.ai/getting-started/pricing) | ❌ | | paid per request, and Claude Code already has search |
| [Composio](https://docs.composio.dev/docs/authentication) | ❌ | | by default they hold the OAuth tokens of your accounts. Don't connect client accounts |

## Scraping and browser agents

| What | Verdict | For whom and when | Caveats |
|---|---|---|---|
| [crawl4ai](https://github.com/unclecode/crawl4ai) | 📌 | local site-to-Markdown | many OSV records, including critical ones for the Docker API. Latest pinned version, no public port |
| [browser-use](https://github.com/browser-use/browser-use) | 📌 | an agent that needs a browser | the model runs on every step, so it costs more than a plain script |
| [Maxun](https://github.com/getmaxun/maxun) | 📌 | no-code scraper on your own server | AGPL |
| [ScrapeGraphAI](https://github.com/ScrapeGraphAI/Scrapegraph-ai) | 📌 | name a site and fields, an LLM extracts them | you pay for the model on every page. Collecting leads means personal data: GDPR |

## Platforms and your own server

| What | Verdict | For whom and when | Caveats |
|---|---|---|---|
| [Supabase](https://github.com/supabase/supabase) | 📌 | when Google Sheets is no longer enough: Postgres, auth, APIs | |
| [Stirling-PDF](https://github.com/Stirling-Tools/Stirling-PDF) | 📌 | local PDF work without online services | part of the code is proprietary |
| [Dify](https://github.com/langgenius/dify) | 📌 | RAG and chatbots on your own server | the license restricts multi-tenant use |
| [Voicebox](https://github.com/jamiepine/voicebox) | 📌 | local voiceover and voice cloning | your own voice only, or with written consent |
| [Coolify](https://github.com/coollabsio/coolify) | ❌ for us | a handy start if you have no server yet | we run compose + Caddy, a panel would add one more entrance with server access |
| [OpenHands](https://github.com/OpenHands/OpenHands) | ❌ for us | always-on agents on a server | no task for it |
| [Open WebUI](https://github.com/open-webui/open-webui) | ❌ for us | your own chat for local models | a great many OSV records, custom license |
| [Langflow](https://github.com/langflow-ai/langflow) | ❌ for us | visual agent builder | many OSV records, including RCE. Don't expose it to the network |

## Launching for free

| What | Verdict | What for |
|---|---|---|
| [free-for-dev](https://github.com/ripienaar/free-for-dev) | 📖 | list of services with free tiers |
| [awesome-selfhosted](https://github.com/awesome-selfhosted/awesome-selfhosted) | 📖 | what you can host yourself |
| [FreeDomain](https://github.com/DigitalPlatDev/FreeDomain) | 📌 | a subdomain for demos. For a business, buy your own domain |

## Learning

| What | Verdict | What for |
|---|---|---|
| [freeCodeCamp](https://github.com/freeCodeCamp/freeCodeCamp) | 📖 | free certifications, including Python and Relational Databases, plus English for developers |
| [system-design-primer](https://github.com/donnemartin/system-design-primer) | 📖 | system design interview prep |
| [project-based-learning](https://github.com/practical-tutorials/project-based-learning) | 📖 | learning through your own project |
| [build-your-own-x](https://github.com/codecrafters-io/build-your-own-x) | 📖 | rebuild a technology from scratch to understand it |
| [developer-roadmap](https://github.com/nilbuild/developer-roadmap) | 📖 | a skills map for a role |
| [free-programming-books](https://github.com/EbookFoundation/free-programming-books) | 📖 | free books in many languages |

## Two ideas without a repo

**Opus plans, Sonnet writes, Haiku searches.** The built-in `opusplan` uses Opus only in plan mode, and the rest runs on Sonnet. Look at `/usage` first: if you don't hit limits, quality matters more. Use a cheaper model selectively: a skill or subagent has a `model` field, and `context: fork` runs a skill in a separate thread.

**A model has to keep its reasoning between steps.** In the Claude API this is a rule, not advice: in a tool loop, the thinking blocks of the last turn must go back unchanged or the request fails. It matters when you write your own agent. Claude Code handles it for you.

## Video: how not to hit limits

[Austin Marchese](https://www.youtube.com/watch?v=SFh6MMe-XcM), 18 min, English, has an ad. Useful habits: `/usage` shows where your limits go, `/context` in an empty chat shows what always loads, `/clear` when you switch tasks, turn off unused MCPs. Skills with scripts instead of repeated model work are also a good recipe. Test the 60-90% promise on your own commands: [RTK](../../README.en.md#-what-we-installed-and-what-we-rejected) gave us 0.25%. We skip swapping text for images: it's a workaround that can cost accuracy.
