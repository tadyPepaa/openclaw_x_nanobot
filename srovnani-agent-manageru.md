# Srovnání OpenClaw Agent Managerů

> Datum analýzy: 2026-02-19
> Analyzováno 5 open-source projektů pro správu AI agentů postavených na/pro OpenClaw

---

## 1. Přehledová tabulka

| | Mission Control (crshdn) | LobsterBoard | AI Maestro | MC Convex (manish-raana) | MC FastAPI (abhi1693) |
|---|---|---|---|---|---|
| **GitHub Stars** | 464 | 397 | 331 | 189 | 186 |
| **Licence** | MIT | BSL-1.1 (→ MIT 2030) | MIT | Apache 2.0 | MIT |
| **Stáří projektu** | 20 dní | 11 dní | 4.5 měsíce | 9 dní | aktivní (819 commitů) |
| **Commits** | 87 | 119 | 803 | 22 | 819 |
| **Kontributoři** | 12 | 3 | N/A (shallow clone) | 4 | 21 |
| **Primární jazyk** | TypeScript | JavaScript (vanilla) | TypeScript | TypeScript | Python + TypeScript |
| **Frontend** | Next.js 14 + React 18 | Vanilla JS (zero frameworks) | Next.js 14 + React 18 | React 19 + Vite 7 | Next.js 16 + React 19 |
| **Backend** | Next.js API Routes | Node.js raw HTTP | Custom server (Next.js + WS) | Convex (BaaS) | FastAPI (Python) |
| **Databáze** | SQLite | Flat JSON soubory | CozoDB + PostgreSQL | Convex Cloud | PostgreSQL |
| **Real-time** | SSE + polling fallback | SSE (systém stats) | WebSocket (terminály, AMP) | Convex reactive queries | SSE |
| **LOC (odhad)** | ~15K | ~10K | ~108K | ~5K | ~30K+ |

---

## 2. Tech Stack — detailní srovnání

### Frontend

| | Mission Control | LobsterBoard | AI Maestro | MC Convex | MC FastAPI |
|---|---|---|---|---|---|
| **Framework** | Next.js 14 (App Router) | Žádný (vanilla JS) | Next.js 14 (App Router) | React 19 + Vite 7 | Next.js 16 (App Router) |
| **State management** | Zustand | Žádný (DOM) | Custom hooks (žádný store) | Props drilling (žádný store) | TanStack React Query |
| **Styling** | Tailwind CSS | Vanilla CSS + custom properties | Tailwind CSS + Framer Motion | Tailwind CSS v4 | Tailwind CSS + Radix UI |
| **Drag & Drop** | @hello-pangea/dnd | Custom (editor canvas) | Žádný (kanban) | @dnd-kit | Žádný |
| **Ikony** | Lucide React | Žádná knihovna | Lucide React | Tabler Icons | Lucide React |
| **Font** | JetBrains Mono | Systémový | Systémový | Outfit | IBM Plex Sans + Sora |
| **Téma** | Dark (GitHub-inspired) | Dark (terminal) | Dark (terminal) | Light (orange accent) | Dark (custom) |
| **Mobile** | Ne | Ne | Ano (3-tier responsive) | Ano (off-canvas drawers) | Částečně |

### Backend

| | Mission Control | LobsterBoard | AI Maestro | MC Convex | MC FastAPI |
|---|---|---|---|---|---|
| **Runtime** | Node.js 18+ | Node.js 16+ | Node.js 18+ | Convex Cloud | Python 3.12+ |
| **Framework** | Next.js API Routes | Raw http module | Custom server.mjs | Convex functions | FastAPI |
| **DB** | SQLite (better-sqlite3) | Flat JSON files | CozoDB + PostgreSQL | Convex (cloud DB) | PostgreSQL + Redis |
| **Migrace** | Custom (numbered) | Žádné | Žádné (code-defined) | Automatické (Convex) | Alembic (18 migrací) |
| **ORM** | Raw SQL helpers | N/A | N/A | Convex API | SQLModel + SQLAlchemy |
| **Background jobs** | Ne | Ne | Ne | Ne | RQ (Redis Queue) |
| **Validace** | Zod | Žádná | Žádná formální | Convex schema | Pydantic + Zod |

### Deployment

| | Mission Control | LobsterBoard | AI Maestro | MC Convex | MC FastAPI |
|---|---|---|---|---|---|
| **Docker** | Ano (multi-stage, docker-compose) | Ne | Ano (agent container) | Ne | Ano (docker-compose, 5 služeb) |
| **Cloud** | Docker-agnostic | Ne | AWS Terraform | Convex Cloud + static hosting | Docker-agnostic |
| **PM2** | Ano | Ne | Ano | Ne | Ne |
| **One-command install** | Ne | npx lobsterboard | curl install script | Ne | Ano (install.sh) |
| **Headless mode** | Ne | Ne | Ano (~100MB, API-only) | Ne | Ne |

---

## 3. Funkce — detailní srovnání

### Správa agentů

| Funkce | Mission Control | LobsterBoard | AI Maestro | MC Convex | MC FastAPI |
|---|---|---|---|---|---|
| **CRUD agentů** | Ano | Ne (read-only widgety) | Ano (plný) | Ano | Ano |
| **Agent profily** | Ano (SOUL.md, USER.md, AGENTS.md) | Ne | Ano (rozšířené: docs, links, notes) | Ano (system prompt, character, lore) | Ano (identity profile, soul template) |
| **Stavy agentů** | standby/working/offline | Ne | active/hibernated + session states | idle/active/blocked | provisioning/active/deleted |
| **Import z Gateway** | Ano (discover + batch import) | Ne | Ne (registrace existujících) | Ne | Ano (auto-provisioning) |
| **Export/Import** | Ne | Ne | Ano (ZIP archiv) | Ne | Ne |
| **Wake/Hibernate** | Ne | Ne | Ano | Ne | Ne |
| **Model selection** | Ano (per agent) | Ne | Ano (per agent) | Ne | Ne |
| **Agent role system** | Master orchestrator flag | Ne | Žádný formální | LEAD/INT/SPC badges | Board Lead vs Worker (enforced) |
| **Heartbeat** | Ne | Ne | Ne | Ne | Ano |
| **Agent auth tokens** | Ne | Ne | Ne | Ne | Ano (hashed tokens) |
| **Metriky agentů** | Ne | Ne | Ano (sessions, tokens, cost) | Ne | Ne |
| **Skill system** | Ne | Ne | Ano (marketplace + plugins) | Ne | Ano (marketplace + packs) |

### Task Management

| Funkce | Mission Control | LobsterBoard | AI Maestro | MC Convex | MC FastAPI |
|---|---|---|---|---|---|
| **Kanban board** | Ano (7 sloupců) | Ne | Ano (5 stavů) | Ano (5 sloupců + archived) | Ano (4 sloupce) |
| **Drag & Drop** | Ano | Ne | Ano | Ano | Ne |
| **Task priorita** | low/normal/high/urgent | Ne | Ne | Ne | medium (default) |
| **Due date** | Ano | Ne | Ne | Ne | Ano |
| **Task dependencies** | Ne | Ne | Ano (blocked states) | Ne | Ano (cascading unblock) |
| **Tagy** | Ne | Ne | Ano | Ano (Enter/comma) | Ano (org-scoped catalog) |
| **Custom fields** | Ne | Ne | Ne | Ne | Ano |
| **Task komentáře** | Ne | Ne | Ne | Ano (markdown) | Ano (@mentions) |
| **Auto-dispatch** | Ano (on drag to In Progress) | Ne | Ne | Ano (play button trigger) | Ne |
| **AI planning** | Ano (interactive Q&A) | Ne | Ne | Ne | Ano (onboarding chat) |
| **Task fingerprints** | Ne | Ne | Ne | Ne | Ano (deduplication) |
| **Approval workflow** | Ne | Ne | Ne | Ne | Ano (pending/approved/rejected) |

### Multi-agent komunikace

| Funkce | Mission Control | LobsterBoard | AI Maestro | MC Convex | MC FastAPI |
|---|---|---|---|---|---|
| **Agent-to-agent messaging** | Ne (plánováno, DB ready) | Ne | Ano (AMP protokol) | Ne (task komentáře) | Ne (gateway dispatch) |
| **Cross-host komunikace** | Ne | Ne | Ano (peer mesh) | Ne | Ne |
| **Kryptografické podepisování** | Ne | Ne | Ano (Ed25519) | Ne | Ne |
| **Federation** | Ne | Ne | Ano (external providers) | Ne | Ne |
| **Push notifikace** | Ne | Ne | Ano (tmux-based) | Ne | Ano (gateway messages) |
| **Team meetings** | Ne | Ne | Ano (war room) | Ne | Ne |
| **Sdílená paměť** | Ne | Ne | Ano (CozoDB + semantic search) | Ne | Ano (board/group memory) |

### Multi-host / Distributed

| Funkce | Mission Control | LobsterBoard | AI Maestro | MC Convex | MC FastAPI |
|---|---|---|---|---|---|
| **Multi-host** | Částečně (remote WS) | Ne | Ano (peer mesh) | Ne | Ano (multi-gateway) |
| **Agent transfer** | Ne | Ne | Ano (between hosts) | Ne | Ne |
| **Host sync** | Ne | Ne | Ano (automatic) | Ne | Ne |
| **Gateway management** | Připojení k jednomu | Ne | Ne | Ne | Plný CRUD + template sync |

### Organizace & Multi-tenancy

| Funkce | Mission Control | LobsterBoard | AI Maestro | MC Convex | MC FastAPI |
|---|---|---|---|---|---|
| **Workspaces** | Ano (scoped agents + tasks) | Ne | Ne | Ne | Ano (boards + board groups) |
| **Multi-tenancy** | Ne | Ne | Ne | Schema-only (hardcoded default) | Plný (org-scoped) |
| **Organizace** | Ne | Ne | Ne | Ne | Ano (CRUD + members + roles) |
| **RBAC** | Ne | Ne | Ne | Ne | Ano (owner/admin/member + board access) |
| **Pozvánky** | Ne | Ne | Ne | Ne | Ano (email + token) |

### Terminál & Runtime

| Funkce | Mission Control | LobsterBoard | AI Maestro | MC Convex | MC FastAPI |
|---|---|---|---|---|---|
| **Embedded terminál** | Ne | Ne | Ano (xterm.js + WebGL) | Ne | Ne |
| **PTY management** | Ne | Ne | Ano (node-pty + tmux) | Ne | Ne |
| **Session playback** | Ne | Ne | Ano | Ne | Ne |
| **Session logging** | Ne | Ne | Ano | Ne | Ne |
| **Code graph** | Ne | Ne | Ano (Cytoscape.js) | Ne | Ne |
| **RAG pipeline** | Ne | Ne | Ano (embeddings, indexing) | Ne | Ne |
| **Voice/TTS** | Ne | Ne | Ano (OpenAI + browser) | Ne | Ne |

### Monitoring & Analytics

| Funkce | Mission Control | LobsterBoard | AI Maestro | MC Convex | MC FastAPI |
|---|---|---|---|---|---|
| **Live event feed** | Ano (SSE, 7 event typů) | Ne | Ne | Ano (activity stream) | Ano (activity timeline) |
| **System monitoring** | Ne | Ano (CPU, RAM, disk, network, Docker) | Ne | Ne | Ne |
| **API cost tracking** | Ne | Ano (Anthropic + OpenAI) | Ano (per-agent) | Ne | Ne |
| **Dashboard KPIs** | Agent count + task count | Ne | Ne | Agent count + task count | Ano (WIP series, charts) |
| **Session management** | Ano (per agent) | Ano (read-only count) | Ano (tmux sessions) | Ne | Ano (gateway sessions) |

---

## 4. Autentizace & Bezpečnost

| | Mission Control | LobsterBoard | AI Maestro | MC Convex | MC FastAPI |
|---|---|---|---|---|---|
| **Auth mechanismus** | Bearer token (optional) | PIN (4-6 digit) | Žádný (localhost only) | Convex Auth (email/password) | Clerk JWT nebo local token |
| **Multi-user** | Ne (single token) | Ne (single admin) | Ne (single user) | Ano (ale hardcoded "Manish") | Ano (Clerk mode) |
| **RBAC** | Ne | Ne | Ne | Ne | Ano (owner/admin/member) |
| **Agent auth** | Ne | Ne | API key (AMP) | Ne | Ano (X-Agent-Token, hashed) |
| **Webhook auth** | HMAC-SHA256 | Ne | HMAC | Ne (žádná) | Ne (plánovano) |
| **Device identity** | Ed25519 keypair | Ne | Ed25519 (AMP) | Ne | Ne |
| **Secrets management** | Ne | Ano (server-side, masked) | Ne | Ne | Ne |
| **SSRF protection** | Ne | Ano | Ne | Ne | Ano (skill packs) |
| **Content security** | Ne | XSS escaping | 34+ injection patterns | Ne | Ne |
| **Security headers** | Ano (X-Frame, X-XSS, etc.) | Ne | Ne | Ne | CORS middleware |
| **Demo mode** | Ano (read-only) | Ano (public mode) | Ne | Ne | Ne |

---

## 5. Kvalita kódu

| | Mission Control | LobsterBoard | AI Maestro | MC Convex | MC FastAPI |
|---|---|---|---|---|---|
| **Testy** | 0 | 0 | ~486 (Vitest) | 0 | 56 backend + 21 frontend + 4 E2E |
| **CI/CD** | Ne | Ne | Ano (GitHub Actions) | Ne | Ano (GitHub Actions, 3 joby) |
| **Linting** | ESLint (relaxed) | Ne | ESLint | ESLint + Biome | ESLint + Black + isort + flake8 + mypy |
| **Type safety** | TypeScript strict | Ne (vanilla JS) | TypeScript strict | TypeScript | Python mypy strict + TypeScript |
| **Validace vstupu** | Zod schemas | Request body limits | Žádná formální | Convex schema | Pydantic + Zod |
| **Dokumentace** | Extensive (README, ORCHESTRATION, HEARTBEAT, PRODUCTION_SETUP) | Dobrá (README, CONTRIBUTING, widget guides) | Rozsáhlá (CLAUDE.md 975 lines, 10+ docs) | Základní (README + 2 docs) | Dobrá (6+ doc files, inline docstrings) |
| **DB migrace** | Custom numbered | N/A | Žádné | Automatické | Alembic (18 migrací) |
| **Pre-commit hooks** | Ne | Ano (private data check) | Ne | Ano (bun build) | Ano (full pre-commit config) |

---

## 6. Architektura — vizuální porovnání

### Mission Control (crshdn)
```
Browser ←→ Next.js (SSR + API) ←→ SQLite
                ↕ (WebSocket)
         OpenClaw Gateway
```

### LobsterBoard
```
Browser ←→ Node.js HTTP server ←→ JSON files on disk
                ↕ (SSE)
         System stats (systeminformation)
```

### AI Maestro
```
Browser ←→ Next.js + Custom WS Server ←→ CozoDB + PostgreSQL
                ↕ (WebSocket + PTY)              ↕
         tmux sessions ← AI Agents    Peer Mesh (other hosts)
                ↕ (AMP Protocol)
         Other agents / External providers
```

### MC Convex (manish-raana)
```
Browser ←→ Vite SPA ←→ Convex Cloud (DB + functions + auth)
                              ↕ (HTTP webhook)
                       OpenClaw Gateway ← Hook handler
```

### MC FastAPI (abhi1693)
```
Browser ←→ Next.js 16 ←→ FastAPI ←→ PostgreSQL + Redis
                              ↕ (WebSocket RPC)      ↕ (RQ)
                       OpenClaw Gateways      Webhook Worker
```

---

## 7. Silné a slabé stránky

### Mission Control (crshdn) — ⭐ 464
**Silné stránky:**
- Nejlepší kanban board (7 sloupců, D&D, AI planning)
- Dobrá integrace s OpenClaw Gateway (WebSocket, agent discover, import)
- Docker-ready (multi-stage build, docker-compose)
- Sub-agent tracking a orchestration
- File upload/download pro agenty
- Dobrá dokumentace

**Slabé stránky:**
- Žádné testy, žádné CI/CD
- SQLite = single-writer, no scaling
- Žádný multi-user, žádný RBAC
- Conversations feature rozpracovaný ale opuštěný
- Žádná paginace

---

### LobsterBoard — ⭐ 397
**Silné stránky:**
- Extrémně lehký (1 runtime dependency)
- Vizuální dashboard builder s 50+ widgety
- Drag & drop editor s template systémem
- Dobrý secrets management
- Custom pages systém (auto-discovery)
- System monitoring (CPU, RAM, disk, network, Docker)
- Exportovatelný standalone HTML

**Slabé stránky:**
- **Není agent manager** — je to dashboard builder, agenty nespravuje
- Žádné CRUD pro agenty, žádné task management
- JSON file persistence = no scaling
- PIN auth je client-side only
- Žádné testy, žádné CI/CD
- Widget JS běží přes `new Function()` (bezpečnostní riziko)
- BSL-1.1 licence (ne MIT do 2030)

---

### AI Maestro — ⭐ 331
**Silné stránky:**
- **Nejkompletnější řešení** — pokrývá téměř vše
- Peer mesh network (decentralizovaný, multi-host)
- AMP protokol (agent-to-agent messaging s Ed25519)
- Embedded terminál (xterm.js + node-pty + tmux)
- Skills marketplace + plugin systém
- RAG pipeline + semantic search + memory consolidation
- Code graph visualization
- Voice/TTS companion
- Team meetings (war room)
- Agent export/import (ZIP)
- ~486 testů, GitHub Actions CI
- Headless mode pro worker nodes
- AWS Terraform deployment
- 4.5 měsíce aktivního vývoje, 803 commitů

**Slabé stránky:**
- Žádná autentizace (design decision, Phase 2+)
- Komplexní — 108K LOC, mnoho nativních závislostí (node-pty, cozo-node, onnxruntime)
- Instalace může být fragile (C++ kompilace)
- Tvrdá závislost na tmux
- Některé komponenty jsou obrovské (AgentList 2452 lines)
- ~300MB RAM (full mode)
- Žádný RBAC, žádný multi-user

---

### MC Convex (manish-raana) — ⭐ 189
**Silné stránky:**
- Nejmodernější stack (React 19, Vite 7, Tailwind v4)
- Convex = real-time "for free" (reaktivní queries, no polling)
- OpenClaw hook systém (lifecycle events, document capture)
- Agent trigger (play button) + resume z Review
- Coding task detection (auto → Review)
- Needs-input detection (otázky → Review)
- Mobile responsive design
- Skeleton loading states

**Slabé stránky:**
- **Hardcoded current user** (`agents?.find(a => a.name === "Manish")`) — kritický bug
- Žádné testy, žádné CI/CD
- Žádný routing (single view, no deep linking)
- Žádný RBAC
- Webhook endpoint bez autentizace
- Multi-tenancy jen schema (hardcoded "default")
- Závislost na Convex Cloud (vendor lock-in)
- Nejmladší projekt (9 dní, 22 commitů)
- Timestamps hardcoded na "just now"

---

### MC FastAPI (abhi1693) — ⭐ 186
**Silné stránky:**
- **Nejrobustnější architektura** — proper backend (FastAPI + PostgreSQL + Redis + RQ)
- **Jediný s plným RBAC** (owner/admin/member + per-board access)
- **Jediný s plnou multi-tenancy** (organizace, členové, pozvánky)
- Approval workflow (governance)
- Task dependencies s cascading unblock
- Custom fields per organizaci
- Board groups (cross-board coordination)
- Board + group memory (sdílená paměť)
- Agent Lead/Worker role enforcement (ne jen vizuální)
- Dual auth (Clerk JWT + local token)
- Agent auth (X-Agent-Token, hashed)
- Alembic migrace (18 migrací)
- 56 backend + 21 frontend + 4 E2E testy
- GitHub Actions CI (3 joby: check, installer, e2e)
- Webhook systém s background worker (RQ + retry + backoff)
- One-command installer (install.sh)
- API-first (OpenAPI spec → auto-generated frontend client)
- Skills marketplace + packs
- Gateway management (CRUD + template sync + version enforcement)
- Conventional commits

**Slabé stránky:**
- Žádný embedded terminál
- Žádné agent-to-agent messaging
- Žádný multi-host/mesh
- SSE polling (2s) místo WebSocket pro frontend
- Nízká test coverage (acknowledged)
- GitHub-only skill packs
- Žádné rate limiting (documented v OpenAPI ale neimplementováno)
- Pre-release stability warning

---

## 8. Výběrový průvodce

### Podle use case

| Potřebuješ... | Doporučený projekt |
|---|---|
| **Plný agent manager s governance** | MC FastAPI (abhi1693) |
| **Nejkompletnější all-in-one řešení** | AI Maestro |
| **Rychlý start, jednoduchý kanban** | Mission Control (crshdn) |
| **Dashboard pro monitoring (ne správu)** | LobsterBoard |
| **Real-time Convex stack** | MC Convex (manish-raana) — ale pozor na bugy |
| **Multi-host mesh síť** | AI Maestro |
| **Enterprise (RBAC, orgs, approvals)** | MC FastAPI (abhi1693) |
| **Embedded terminály pro agenty** | AI Maestro |
| **Minimální závislosti** | LobsterBoard (1 dep) nebo Mission Control |

### Podle zralosti

| Úroveň | Projekt |
|---|---|
| **Production-ready** | MC FastAPI (abhi1693) — nejblíže, má testy, CI, migrace, auth |
| **Beta** | AI Maestro — feature-rich ale bez auth |
| **Alpha** | Mission Control (crshdn) — funguje ale žádné testy |
| **Early Alpha** | MC Convex (manish-raana) — kritické bugy |
| **Jiná kategorie** | LobsterBoard — není agent manager |

### Podle technické náročnosti nasazení

| Složitost | Projekt | Požadavky |
|---|---|---|
| **Nejjednodušší** | LobsterBoard | Node.js, `npx lobsterboard` |
| **Jednoduché** | Mission Control | Node.js, SQLite (embedded) |
| **Střední** | MC Convex | Convex account + Node.js |
| **Střední** | MC FastAPI | Docker Compose (5 služeb) |
| **Nejvyšší** | AI Maestro | Node.js + tmux + native deps (C++) |

---

## 9. Celkové hodnocení

| Projekt | Funkce | Architektura | Kvalita kódu | Zralost | Celkem |
|---|---|---|---|---|---|
| **MC FastAPI (abhi1693)** | ★★★★☆ | ★★★★★ | ★★★★☆ | ★★★★☆ | **17/20** |
| **AI Maestro** | ★★★★★ | ★★★★☆ | ★★★☆☆ | ★★★☆☆ | **15/20** |
| **Mission Control (crshdn)** | ★★★☆☆ | ★★★☆☆ | ★★☆☆☆ | ★★☆☆☆ | **10/20** |
| **MC Convex (manish-raana)** | ★★★☆☆ | ★★★☆☆ | ★☆☆☆☆ | ★☆☆☆☆ | **8/20** |
| **LobsterBoard** | ★★☆☆☆ | ★★☆☆☆ | ★★☆☆☆ | ★★☆☆☆ | **8/20** |

### Závěr

**MC FastAPI (abhi1693)** je nejlépe architektonicky navržený projekt — je jediný s plným RBAC, multi-tenancy, approval workflow, DB migracemi, testy a CI/CD. Je to nejbližší "production-ready" řešení.

**AI Maestro** je feature-wise nejkompletnější — embedded terminály, peer mesh, AMP messaging, RAG, skills marketplace, voice. Je to ale komplexnější na provoz a nemá zatím autentizaci.

**Mission Control (crshdn)** je dobrý střední kompromis — jednoduchý, funkční kanban s OpenClaw integrací, ale chybí mu testy a robustnost.

**MC Convex** a **LobsterBoard** jsou v rané fázi — MC Convex má kritické bugy a LobsterBoard vlastně není agent manager, ale dashboard builder.
