# OpenClaw vs Nanobot — Detailni porovnani

## 1. Zakladni identita

| | **OpenClaw** | **Nanobot** |
|---|---|---|
| **Popis** | Osobni self-hosted AI asistent s maskotem (space lobster Molty) | Lehky osobni AI asistent framework inspirovany OpenClaw |
| **Jazyk** | TypeScript (ESM), Swift (macOS/iOS), Kotlin (Android) | Python 3.11+, TypeScript (WhatsApp bridge) |
| **Licence** | MIT | MIT |
| **Runtime** | Node.js 22+ | Python 3.12+ |
| **Build** | pnpm, tsdown, Vitest | Hatchling, pip/pipx |
| **Verze** | 2026.2.18 (zraly, aktivne vyvijeny) | 0.1.4 (rana faze) |
| **Velikost kodu** | Masivni monorepo (stovky tisic radku) | ~3 761 radku core kodu |

---

## 2. Architektura

### OpenClaw

- **Gateway daemon** — centralni long-lived proces na `127.0.0.1:18789`
- **WebSocket API** s typovanymi JSON framy (TypeBox schemata)
- HTTP server: WebChat, Canvas, A2UI, webhooky, OpenAI-compatible API
- Typovany wire protokol: Request/Response/Event framy, idempotency keys, protocol versioning
- **Plugin system** — runtime loaded TypeScript moduly (in-process)
- Device pairing s keypair challenge-response
- Operator/Node role system se scopes

### Nanobot

- **Message bus pattern** — asyncio queues (inbound/outbound) oddeluji kanaly od agenta
- Jednoduchy gateway = agent loop + channel manager + cron + heartbeat
- Zadny wire protokol, zadne WebSocket API pro klienty
- Zadny plugin system
- Jednodussi, primocara architektura

**Verdikt:** OpenClaw ma enterprise-grade architekturu s formalnim protokolem a plugin systemem. Nanobot je minimalisticky a snadno pochopitelny.

---

## 3. Podporovane chat kanaly

| Kanal | **OpenClaw** | **Nanobot** |
|---|---|---|
| WhatsApp | Baileys (vestaveny) | Baileys (Node.js bridge) |
| Telegram | grammY | python-telegram-bot |
| Slack | Bolt (Socket Mode) | slack-sdk (Socket Mode) |
| Discord | discord.js | Raw WebSocket (bez knihovny) |
| Signal | signal-cli | — |
| BlueBubbles (iMessage) | Ano | — |
| iMessage (legacy macOS) | Ano | — |
| Google Chat | Ano | — |
| WebChat | Vestaveny v Gateway | — |
| MS Teams | Plugin | — |
| Matrix | Plugin | — |
| Zalo / Zalo Personal | Plugin | — |
| Nostr | Plugin | — |
| IRC | Extension | — |
| LINE | Extension | — |
| Mattermost | Extension | — |
| Twitch | Extension | — |
| Tlon (Urbit) | Extension | — |
| Nextcloud Talk | Extension | — |
| Voice Call (Twilio) | Plugin | — |
| Feishu (Lark) | Extension | lark-oapi SDK |
| Email | — | IMAP polling + SMTP |
| MoChat | — | Socket.IO + msgpack |
| DingTalk | — | stream SDK (jen 1:1) |
| QQ | — | qq-botpy (jen C2C) |

**OpenClaw: ~20 kanalu** | **Nanobot: 9 kanalu**

OpenClaw ma masivni prevahu v poctu kanalu. Nanobot ma unikatni Email, MoChat, DingTalk a QQ (cinsky trh).

---

## 4. LLM poskytovatele

| Poskytovatel | **OpenClaw** | **Nanobot** |
|---|---|---|
| Anthropic (Claude) | Ano | Ano (pres LiteLLM) |
| OpenAI | Ano | Ano |
| OpenRouter | Ano | Ano |
| Ollama | Ano | Ano (pres LiteLLM) |
| vLLM | Ano | Ano |
| Together AI | Ano | — |
| Amazon Bedrock | Ano | — |
| Z.AI | Ano | — |
| Moonshot (Kimi) | Ano | Ano |
| GLM (Zhipu) | Ano | Ano |
| MiniMax | Ano | Ano |
| Venice AI | Ano | — |
| Xiaomi | Ano | — |
| Qwen (DashScope) | Ano | Ano |
| Qianfan (Baidu) | Ano | — |
| NVIDIA | Ano | — |
| Hugging Face | Ano | — |
| LiteLLM | Ano | Ano (core abstrakce) |
| Vercel AI Gateway | Ano | — |
| Cloudflare AI Gateway | Ano | — |
| GitHub Copilot | Ano | Ano (OAuth device flow) |
| OpenAI Codex | Ano | Ano (Responses API) |
| Google Gemini | Plugin | Ano |
| Deepgram (transcription) | Ano | — |
| DeepSeek | — | Ano |
| Groq | — | Ano (+ Whisper transcription) |
| SiliconFlow | — | Ano |
| AIHubMix | — | Ano |
| Custom OpenAI-compatible | Ano | Ano |

**OpenClaw: 25+ poskytovatelu** | **Nanobot: 16 poskytovatelu**

### Pokrocile funkce modelu

| Feature | **OpenClaw** | **Nanobot** |
|---|---|---|
| Model failover chain | Ano (primary -> fallbacks -> auth failover) | — |
| Auth profile rotation | Ano (cooldowny, auto-expiry, probing) | — |
| Model aliasy | Ano | — |
| Per-model config | Ano (contextWindow, thinkingDefault, params) | Ano (model_overrides v registry) |
| Image model | Ano (oddeleny od chat modelu) | — |
| Auto-detekce providera | — | Ano (podle klicovych slov v nazvu modelu) |

---

## 5. System nastroju (Tools)

| Nastroj | **OpenClaw** | **Nanobot** |
|---|---|---|
| Shell/exec | Ano (pty, background, timeout, elevated mode) | Ano (background, timeout, deny patterns) |
| Read file | Ano (auto-paging, context budget) | Ano (offset, limit, line numbers) |
| Write file | Ano | Ano |
| Edit file | Ano | Ano (exact string replacement) |
| Apply patch | Ano (multi-hunk) | — |
| List directory | — | Ano |
| Web search | Ano (Brave, cachovano) | Ano (Brave) |
| Web fetch | Ano (HTML->MD, Firecrawl fallback) | Ano (readability-lxml) |
| Browser automation | Ano (CDP, Playwright, multi-profile, snapshots, actions) | — |
| Canvas/A2UI | Ano (agent-driven visual workspace) | — |
| Image analysis | Ano (dedikovany image model) | — |
| Message sending | Ano (send/poll/react/edit/delete/pin/thread/search/sticker/role/moderation) | Ano (send, basic routing) |
| Cron management | Ano (cron expr, interval, one-shot, timezone, deliver) | Ano (cron expr, interval, one-shot, timezone) |
| Gateway management | Ano (restart, config, update) | — |
| Session management | Ano (list/history/send/spawn) | — |
| Memory search | Ano (vector BM25 + semantic, SQLite-vec) | — |
| Memory get | Ano | — |
| Node control | Ano (camera, screen, location, notifications) | — |
| Spawn subagent | Ano (configurable depth, max children) | Ano (max 15 iter, restricted tools) |
| MCP tools | Ano (vestaveny) | Ano (stdio + HTTP transport) |

### Tool bezpecnost

| Feature | **OpenClaw** | **Nanobot** |
|---|---|---|
| Allow/deny lists | Ano (globalni, per-agent, per-provider, wildcard) | Ano (deny regex patterns, optional allow) |
| Tool profiles | Ano (minimal/coding/messaging/full) | — |
| Sandbox (Docker) | Ano (per-session, per-agent, shared, workspace access modes) | — |
| Exec approvals | Ano (ask always/on-miss/off, per-node allowlists) | — |
| Loop detection | Ano (repeat, poll, ping-pong, circuit breaker) | — |
| Workspace restriction | Ano | Ano (+ symlink escape prevention) |
| Elevated mode | Ano (opt-in host execution from sandbox) | — |

---

## 6. Agent system

### OpenClaw

- **Multi-agent** — vice izolovanych agentu v jednom Gateway, kazdy s vlastnim workspace
- **Deterministic routing** — peer match -> parentPeer -> guild+roles -> guild/team -> account -> channel -> default
- **Subagenti** — `sessions_spawn` s configurable depth (maxSpawnDepth), max children per agent
- **Agent-to-agent messaging** — `sessions_send` s reply-back
- **Workspace per agent** — AGENTS.md, SOUL.md, USER.md, TOOLS.md, BOOTSTRAP.md, IDENTITY.md, BOOT.md
- **Bootstrap ritual** — one-time setup pri prvnim spusteni

### Nanobot

- **Single agent** — jeden agent loop zpracovava sekvencne
- **Subagent spawning** — izolovane background agenty (max 15 iteraci)
  - Restricted tools: jen file, shell, web (zadne message, zadne spawn -> zadna rekurze)
- **Workspace files** — AGENTS.md, SOUL.md, USER.md, TOOLS.md, IDENTITY.md, HEARTBEAT.md
- Zadne multi-agent routing

---

## 7. Pamet a kontext

### OpenClaw

- **Markdown files** — `memory/YYYY-MM-DD.md` (denni log), `MEMORY.md` (kurovane)
- **Vektorove vyhledavani** — BM25 + semanticka podobnost
  - Embedding providers: OpenAI, Gemini, Voyage, node-llama-cpp (lokalni)
  - SQLite-vec akcelerace
  - MMR re-ranking pro diverzitu
  - Temporal decay pro boost cerstvosti
  - Embedding cache v SQLite
- **QMD backend** (experimentalni) — lokalni BM25 + vectors + reranking
- **Automatic memory flush** — agentic turn pred compaction
- **Session pruning** — trimuje stare tool results z kontextu
- **Compaction** — automaticka sumarizace stareho kontextu
- **Session management** — DM scope modes, identity links, daily reset, per-channel overrides

### Nanobot

- **Dva soubory** — `MEMORY.md` (vzdy v kontextu) + `HISTORY.md` (append-only log)
- **Zadne vektorove vyhledavani** — ciste file-based
- **Memory consolidation** — LLM sumarizuje po 50 zpravach do MEMORY.md/HISTORY.md
- **Append-only sessions** — zpravy se nikdy nemazou (LLM cache efficiency)
- **JSONL sessions** — jednoduche file storage, in-memory cache
- Zadna compaction, zadny pruning

**Verdikt:** OpenClaw ma sofistikovany memory stack s vektorovym vyhledavanim. Nanobot ma elegantni dvouvrstva system — jednoduchy, ale funkcni.

---

## 8. Automatizace

| Feature | **OpenClaw** | **Nanobot** |
|---|---|---|
| Cron jobs | Ano (cron expr, interval, one-shot, timezone, stagger, multi-agent, run history) | Ano (cron expr, interval, one-shot, timezone) |
| Heartbeat | Ano (configurable, HEARTBEAT.md, skip-when-empty) | Ano (30min interval, HEARTBEAT.md) |
| Webhooks (inbound) | Ano (/hooks/wake, /hooks/agent, named hooks, Gmail Pub/Sub) | — |
| Internal hooks | Ano (20+ event types: command, agent:bootstrap, gateway:startup, message:received/sent, tool lifecycle) | — |
| Plugin hooks | Ano (before/after model, prompt, agent, compaction, tool, message, session, gateway, LLM I/O) | — |

---

## 9. Companion aplikace

| App | **OpenClaw** | **Nanobot** |
|---|---|---|
| **macOS** | Menu bar app — Gateway control, Voice Wake, Talk Mode, WebChat, Canvas, Debug, Remote SSH, Skills UI, Node mode | — |
| **iOS** | Canvas, Voice Wake, Talk Mode, Camera, Share extension, Apple Watch companion, QR onboarding, APNs push | — |
| **Android** | Canvas, Talk Mode, Camera, SMS, Bridge pairing | — |
| **Web UI** | Lit-based Control UI, WebChat, Dashboard | — |
| **Chrome Extension** | CDP relay pro existujici Chrome taby | — |
| **CLI** | Rozsahly (50+ prikazu) | Zakladni (onboard, agent, gateway, status, channels, cron, provider) |

---

## 10. Voice a speech

| Feature | **OpenClaw** | **Nanobot** |
|---|---|---|
| Voice Wake | Ano (on-device, macOS/iOS/Android) | — |
| Talk Mode | Ano (continuous loop, barge-in, voice directives) | — |
| TTS | Ano (ElevenLabs, OpenAI, Edge TTS) | — |
| Voice transcription | Ano (Deepgram, on-device) | Ano (Groq Whisper, jen Telegram) |
| Voice calls | Ano (Twilio plugin) | — |
| Swabble (wake-word daemon) | Ano (Swift, Speech.framework, zero network) | — |

---

## 11. Browser automation

| Feature | **OpenClaw** | **Nanobot** |
|---|---|---|
| Managed browser | Ano (Chrome/Brave/Edge/Chromium, izolovany profil) | — |
| Multi-profile | Ano | — |
| CDP + Playwright | Ano | — |
| Actions | Ano (click, type, press, hover, drag, select, fill, resize, wait, evaluate) | — |
| Snapshots | Ano (AI snapshot, role snapshot) | — |
| State control | Ano (cookies, storage, offline, headers, credentials, geo, media, timezone, locale, viewport) | — |
| Network inspection | Ano (console, errors, requests, response body) | — |
| Chrome Extension relay | Ano | — |
| Remote browser | Ano (Browserless, node proxy) | — |

---

## 12. Bezpecnost

| Feature | **OpenClaw** | **Nanobot** |
|---|---|---|
| Gateway auth | Ano (token, password, none) | — |
| Device pairing | Ano (keypair challenge-response) | — |
| DM pairing | Ano (pairing codes) | — |
| Docker sandboxing | Ano (per-session/per-agent/shared, workspace access modes) | — |
| Exec approvals | Ano (ask always/on-miss/off) | — |
| Security audit CLI | Ano (`openclaw security audit --fix`) | — |
| Threat model | Ano (formalni THREAT-MODEL-ATLAS.md) | — |
| SSRF protection | Ano (NAT64, 6to4, Teredo blocking) | — |
| Rate limiting | Ano (auth failures) | — |
| Shell deny patterns | Ano | Ano (rm -rf /, fork bomb, mkfs, dd, shutdown) |
| Workspace restriction | Ano | Ano (+ symlink escape) |
| Channel allowlists | Ano (per-channel allowFrom) | Ano (per-channel, pipe-separated matching) |
| Email consent gate | — | Ano |
| Tailscale integrace | Ano (Serve/Funnel, identity headers) | — |

---

## 13. Deployment

| Metoda | **OpenClaw** | **Nanobot** |
|---|---|---|
| npm/pip install | `npm install -g openclaw` | `pip install nanobot-ai` |
| Docker | Multi-variant (sandbox, browser, common) | Multi-stage + docker-compose |
| launchd/systemd | Ano | — |
| Cloud VPS guides | Ano (Hetzner, GCP, DO, Oracle, Fly.io) | — |
| Nix | Ano | — |
| Ansible | Ano | — |
| Raspberry Pi | Ano | — |
| Multiple instances | Ano (OPENCLAW_PROFILE) | — |
| Tailscale remote | Ano | — |
| PyPI | — | Ano |

---

## 14. Konfigurace

### OpenClaw

- `~/.openclaw/openclaw.json` (JSON5)
- Desitky top-level sekci: agent, agents, channels, session, tools, gateway, browser, hooks, cron, plugins, skills, messages, talk, memory, logging, models
- `$include` pro modularni kompozici
- Extensive env var support
- TypeBox schemata s JSON Schema generaci
- Config uiHints pro web formulare

### Nanobot

- `~/.nanobot/config.json`
- Pydantic v2 modely s camelCase aliasy
- Sekce: agents, channels, providers, gateway, tools, workspace
- Auto-migrace starych config formatu
- Jednodussi, plossi struktura

---

## 15. Skills system

| Feature | **OpenClaw** | **Nanobot** |
|---|---|---|
| Format | SKILL.md + YAML frontmatter | SKILL.md + YAML frontmatter |
| Zdroje | Bundled + managed + workspace | Built-in (8) + workspace |
| Gating | bins, env, config, OS, `always` | bins, env, OS, `always` |
| Progressive loading | — | Ano (summary XML -> full on demand) |
| Verejny registr | Ano (ClawHub, clawhub.com) | — |
| Installers | Ano (brew, node, go, uv, download) | — |
| Pocet built-in skills | Desitky | 8 (memory, github, weather, tmux, summarize, cron, skill-creator, clawhub) |

---

## 16. Unikatni features

### Pouze OpenClaw

- **ACP Bridge** — IDE integrace (Zed editor)
- **OpenAI-compatible HTTP API** — `/v1/chat/completions` endpoint
- **Canvas + A2UI** — agent-driven vizualni workspace
- **Browser automation** — plny CDP/Playwright stack
- **Voice ecosystem** — Wake, Talk Mode, TTS, Swabble daemon, Voice Calls
- **Companion apps** — macOS, iOS (+ Apple Watch), Android
- **Plugin system** — runtime loaded TypeScript moduly
- **Multi-agent routing** — deterministic peer/guild/team/channel routing
- **Docker sandboxing** — izolovane tool execution
- **Vector memory** — BM25 + embeddings + SQLite-vec
- **Block streaming** — progressive message delivery
- **Device pairing** — keypair-based trust
- **Lobster plugin** — typed workflow runtime
- **i18n** — cinska a japonska dokumentace
- **Security audit** — automatizovany audit konfigurace
- **20+ messaging channels**

### Pouze Nanobot

- **Email kanal** — IMAP polling + SMTP reply s threading
- **MoChat kanal** — Socket.IO + msgpack
- **DingTalk kanal** — stream SDK
- **QQ kanal** — qq-botpy
- **Progressive skill loading** — XML summaries v kontextu, full load on demand
- **Append-only sessions** — LLM cache efficiency (prefix caching)
- **Dvouvrstva pamet** — MEMORY.md (vzdy v kontextu) + HISTORY.md (searchable log)
- **json-repair** — robustni parsovani malformovaneho JSON z LLM
- **Groq Whisper transcription** — pro Telegram voice zpravy
- **Python ekosystem** — snadna integrace s datascience/ML tooling

---

## 17. Shrnuti

| Dimenze | **OpenClaw** | **Nanobot** |
|---|---|---|
| **Zralost** | Produkcni, aktivni komunita | Rane stadium (0.1.x) |
| **Slozitost** | Vysoka (enterprise-grade) | Nizka (snadno pochopitelny) |
| **Channels** | ~20 | 9 |
| **LLM providers** | 25+ | 16 |
| **Tools** | 20+ vestavenych | 10 vestavenych |
| **Voice** | Kompletni ecosystem | Minimalni (jen Telegram transcription) |
| **Browser** | Plny CDP/Playwright | Zadny |
| **Apps** | macOS + iOS + Android + Watch | Zadne |
| **Security** | Multi-layer (sandbox, pairing, audit) | Zakladni (deny patterns, workspace restrict) |
| **Memory** | Vector search + BM25 + embeddings | File-based dvouvrstvovy |
| **Multi-agent** | Plny routing + orchestrace | Single agent + basic subagents |
| **Plugin system** | Ano (in-process TS moduly) | Ne |
| **Deployment** | 7+ metod | pip + Docker |
| **Cinsky trh** | Castecne (Qwen, GLM, MiniMax) | Silnejsi (DingTalk, QQ, MoChat, Feishu) |
| **Hackability** | Stredni (velky codebase) | Vysoka (maly, cisty Python) |

---

## 18. Zaver

**OpenClaw** je zraly, feature-complete osobni AI asistent s masivnim ekosystemem — apps, voice, browser, 20+ kanalu, vector memory, plugin system, sandboxing. Vhodny pro uzivatele, kteri chteji maximalni funkcionalitu "out of the box" a jsou ochotni investovat do konfigurace.

**Nanobot** je lehky Python framework s cistou architekturou, dobrym pokrytim cinskeho trhu (DingTalk, QQ, MoChat), unikatnim email kanalem a elegantnim dvouvrstvym memory systemem. Idealn pro customizaci, rychle prototypovani a uzivatele preferujici Python ekosystem.
