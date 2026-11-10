# pi-config

Personal [pi](https://github.com/badlogic/pi) configuration — extensions, skills, and themes.

## Quick start

1. Clone this repo to `~/.pi/agent` (fresh setup) **or** copy individual pieces into your existing config.
2. Install root dependencies:
   ```bash
   cd ~/.pi/agent
   pnpm install
   ```
3. Install deps for directory extensions that have their own `package.json`:
   ```bash
   cd ~/.pi/agent/extensions/bash-guard   && npm install
   cd ~/.pi/agent/extensions/filechanges  && npm install
   cd ~/.pi/agent/extensions/pi-hashline  && npm install
   cd ~/.pi/agent/extensions/todo         && npm install
   cd ~/.pi/agent/extensions/web-fetch    && npm install
   ```
4. (Optional) Copy `.env.example` to `.env` and fill in API keys:
   ```bash
   cp .env.example .env
   ```
5. (Optional) Install system tools used by some extensions:
   ```bash
   brew install yt-dlp ffmpeg
   ```
6. (Optional) Set up the PDF reader skill:
   ```bash
   python3 -m venv ~/.pi/agent/skills/pdf-reader/.venv
   ~/.pi/agent/skills/pdf-reader/.venv/bin/pip install -r ~/.pi/agent/skills/pdf-reader/requirements.txt
   ```
7. Restart pi or run `/reload`.

## Copy individual pieces

Single-file extension:
```bash
cp extensions/copy-all.ts ~/.pi/agent/extensions/
```

Directory extension:
```bash
cp -r extensions/web-fetch ~/.pi/agent/extensions/
cd ~/.pi/agent/extensions/web-fetch && npm install
```

Skill:
```bash
cp -r skills/reddit ~/.pi/agent/skills/
```

## Do not clone over your config

Avoid cloning this repo directly into `~/.pi/agent` unless it is a fresh setup. If you already use pi, copy individual files/folders instead so you do not replace your own config.

## API keys & auth

- **Firecrawl** — `FIRECRAWL_API_KEY` in `.env` (used by `firecrawl-search.ts`)
- **Google Search** — `GOOGLE_SEARCH_API_KEY` and `GOOGLE_CSE_ID` env vars, or a local `auth.json` copied from `auth.example.json` (used by `web-search/` and `google-image-search/`)

## Contents

### Extensions

| Extension | Type | Description |
|-----------|------|-------------|
| `ask-user-question.ts` | Tool | Interactive question UI with single/multi-select and free-text |
| `bash-guard/` | Hook | Safety guardrails for bash commands |
| `budget-guard/` | Hook | Counter that hard-caps subagent tool calls (max 100) to prevent runaway processes |
| `context.ts` | Command | Visualize context window usage as a colored grid (`/context`) |
| `copy-all.ts` | Command | Copy all messages to clipboard (`/copy-all`) |
| `custom-header.ts` | UI | Static ASCII art session header |
| `diff.ts` | Command | Show files changed by last agent run, open in Zed (`/diff`) |
| `filechanges/` | Hook | Track file changes across agent runs |
| `firecrawl-search.ts` | Tool | Web search & scrape via Firecrawl API |
| `flow-title.ts` | UI | Animated blue gradient session header (alternative to `custom-header.ts`) |
| `google-image-search/` | Tool | Google image search |
| `md-link.ts` | Command | Link a markdown file for collaborative editing (`/link-md`) |
| `memory.ts` | Command | Persistent project memory via `MEMORY.md` (`/memory`) |
| `opencode-zen-login.ts` | Provider | OpenCode Zen API key provider registration |
| `pi-hashline/` | Tool/Hook | Collision-free edits/reads verification using xxHash32 anchored lines |
| `rtk-wrap/` | Hook | Command optimizer wrapping terminal calls via `rtk` binary and truncating outputs |
| `safety/` | Hook | Regex pattern blocker to stop dangerous bash execution calls |
| `subagents/` | Tool | Spawn sub-agents (auditor, researcher, scout, worker) |
| `todo/` | Tool/Command | Persistent tasks tracking and subagents dispatch checklist (`/todos` / `todo`) |
| `tps-tracker.ts` | UI | Live tokens-per-second display during generation |
| `usage.ts` | Command | Usage & cost report for last 1/7/30/90 days (`/usage`) |
| `video-extract/` | Tool | Extract content from videos |
| `web-fetch/` | Tool | Fetch web pages as cleaned markdown |
| `web-search/` | Tool | Google Custom Search integration |
| `yeet.ts` | Command | Add, commit, push in one command (`/yeet`) |
| `youtube-search/` | Tool | YouTube video search |
| `zsh-user-bash.ts` | Hook | Run pi bash commands through user's zsh |
| `zz-read-only-mode.ts` | Command | Hard-enforced read-only mode (`/read-only`) |

<br/>

> [!NOTE] 
> `custom-header.ts` and `flow-title.ts` both set the session header. Use one or the other — `custom-header.ts` loads first alphabetically, and `flow-title.ts` replaces it on `session_start`. To use only one, delete or rename the other.

### Agents

Role definitions for specialized subagents stored in the `agents/` folder:

| Agent | Description |
|-------|-------------|
| `architect.md` | Formulates design plans and structural definitions |
| `backend-developer.md` | Handles server, database, API, and backend integrations |
| `code-researcher.md` | Audits, searches, and details files inside the codebase |
| `frontend-developer.md` | Handles UI/UX, markup, stylesheet designs, and client-side logic |
| `infrastructure-developer.md` | Handles Docker, Kubernetes, CI/CD, and cloud infrastructure tasks |
| `internal-documentation.md` | Documents developer instructions, code structure, and schemas |
| `orchestrator.md` | Decomposes complex tasks and delegates them to specialized child agents |
| `planner.md` | Reviews specifications and brainstorms requirements prior to coding |
| `refactor.md` | Handles codebase refactoring, optimization, and code cleanliness |
| `reviewer.md` | Audits pull requests and code changes for style consistency and bugs |
| `security.md` | Analyzes vulnerability issues, secrets exposure, and dependency bugs |
| `tester.md` | Creates unit, integration, and end-to-end automated test suites |
| `user-documentation.md` | Generates user guides, feature documentation, and APIs reference guides |
| `web-researcher.md` | Crawls documentation, searches online resources, and harvests package guides |

### Skills

| Skill | Description |
|-------|-------------|
| `improve/` | Retrospective audit — scans config, presents findings interactively, applies approved changes |
| `orchestrator/` | Multi-step task orchestration patterns |
| `paper-reading/` | Deep, structured analysis and critique of academic papers |
| `pdf-reader/` | PDF text extraction, search, and rendering (requires Python venv) |
| `poke-humanizer/` | Chat/texting voice — mirrors the user's tone, strips assistant-speak |
| `productivity/grilling/` | Stress-tests plans and decisions through relentless, interactive questioning |
| `reddit/` | Reddit content fetching |
| `stop-slop/` | Reduce verbose/filler language in responses |
| `ts-rules/` | TypeScript best practices — 63 rules across 19 categories (any safety, narrowing, type design, generics, async, config, TS 7 migration) |

### Themes

- `github-dark-default` — GitHub's dark theme ported for pi

### Other

- `settings.json` — Default provider/model/theme configuration
- `AGENTS.md` — Coding conventions for the agent
- `.env.example` — Template for API keys

## Dependencies

Root `package.json` provides shared dependencies used by single-file extensions. Directory extensions manage their own deps via local `package.json` files.

Extensions with local npm deps:
- `bash-guard/`
- `filechanges/`
- `pi-hashline/`
- `todo/`
- `web-fetch/`

Optional system tools:
```bash
brew install yt-dlp ffmpeg
```
Used by `youtube-search/` and `video-extract/`.
