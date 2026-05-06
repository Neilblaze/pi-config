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
| `subagents/` | Tool | Spawn sub-agents (auditor, researcher, scout, worker) |
| `tps-tracker.ts` | UI | Live tokens-per-second display during generation |
| `usage.ts` | Command | Usage & cost report for last 1/7/30/90 days (`/usage`) |
| `video-extract/` | Tool | Extract content from videos |
| `web-fetch/` | Tool | Fetch web pages as cleaned markdown |
| `web-search/` | Tool | Google Custom Search integration |
| `yeet.ts` | Command | Add, commit, push in one command (`/yeet`) |
| `youtube-search/` | Tool | YouTube video search |
| `zsh-user-bash.ts` | Hook | Run pi bash commands through user's zsh |
| `zz-read-only-mode.ts` | Command | Hard-enforced read-only mode (`/read-only`) |

> **Note:** `custom-header.ts` and `flow-title.ts` both set the session header. Use one or the other — `custom-header.ts` loads first alphabetically, and `flow-title.ts` replaces it on `session_start`. To use only one, delete or rename the other.

### Skills

| Skill | Description |
|-------|-------------|
| `improve/` | Retrospective audit — scans config, presents findings interactively, applies approved changes |
| `orchestrator/` | Multi-step task orchestration patterns |
| `pdf-reader/` | PDF text extraction, search, and rendering (requires Python venv) |
| `reddit/` | Reddit content fetching |
| `stop-slop/` | Reduce verbose/filler language in responses |

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
- `web-fetch/`

Optional system tools:
```bash
brew install yt-dlp ffmpeg
```
Used by `youtube-search/` and `video-extract/`.
