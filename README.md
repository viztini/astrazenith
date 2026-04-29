# AstraZenith [ALPHA]

AstraZenith is a **coding agent** with an interactive REPL, tool use (read/write files, shell, search, notebooks, diagnostics, and more), and support for **many model providers**—Anthropic (e.g. `claude-*` models), OpenAI, Gemini, Kimi, Qwen, Zhipu, DeepSeek, Ollama, LM Studio, and any OpenAI-compatible HTTP API.

## Why AstraZenith?

Claude Code is a powerful, production-grade AI coding assistant — but its source code is a compiled, 12 MB TypeScript/Node.js bundle (~1,300 files, ~283K lines). It is tightly coupled to the Anthropic API, hard to modify, and impossible to run against a local or alternative model.

AstraZenith reimplements the same core loop in ~10K lines of readable Python, allowing access to any model you want, open-source, keeping everything you need and dropping what you don't.

## Quick start

```bash
pip install -r requirements.txt
export ANTHROPIC_API_KEY=...   # or another provider key; see below
python astra_zenith.py
```

Non-interactive one-shot:

```bash
python astra_zenith.py --print "Summarize this repo in five bullets"
```

Common flags: `-m / --model`, `--accept-all`, `--verbose`, `--thinking` (Anthropic only), `--version`, `-h`.

## Where data lives

| Location | Purpose |
|----------|---------|
| `~/.astra_zenith/config.json` | Default model, API keys (optional), permissions |
| `~/.astra_zenith/sessions/` | Saved REPL sessions |
| `~/.astra_zenith/memory/` | User-scoped persistent memories |
| `~/.astra_zenith/skills/` | User markdown skills |
| `~/.astra_zenith/agents/` | Custom sub-agent type definitions |
| `~/.astra_zenith/mcp.json` | User-level MCP server config |
| `~/.astra_zenith/plugins/` | Installed plugins (plugin system) |
| `.astra_zenith/` under the project cwd | Project skills, memories, plugins, tasks (`tasks.json`), etc. |

Project-level `.mcp.json` merges with the user MCP config (project wins on server name).

## API keys

Set environment variables for your provider(s), for example:

- `ANTHROPIC_API_KEY`, `OPENAI_API_KEY`, `GEMINI_API_KEY`, `MOONSHOT_API_KEY`, `DASHSCOPE_API_KEY`, `ZHIPU_API_KEY`, `DEEPSEEK_API_KEY`

You can also persist keys with `/config key=value` inside the REPL.

## Models (examples)

```bash
python astra_zenith.py --model claude-sonnet-4-6
python astra_zenith.py --model gpt-4o
python astra_zenith.py --model gemini/gemini-2.0-flash
python astra_zenith.py --model ollama/qwen2.5-coder
python astra_zenith.py --model lmstudio/<model-name>
```

For a self-hosted OpenAI-compatible server:

```bash
export CUSTOM_BASE_URL=http://localhost:8000/v1
export CUSTOM_API_KEY=none   # if unused
python astra_zenith.py --model custom/Your-Model-Name
```

Use `/help` in the REPL for slash commands (`/model`, `/memory`, `/mcp`, `/tasks`, `/voice`, …).

## Voice input (optional)

Install recording + STT dependencies (for example `sounddevice` and `faster-whisper`), then use `/voice` in the REPL. Override the local Whisper size with `ASTRAZENITH_WHISPER_MODEL` (default `base`).

## Tests

```bash
python -m pytest tests/ -v
```

## Developer docs

See [docs/architecture.md](docs/architecture.md) for module layout and extension points (tool registry, agent loop, memory, skills, MCP).

## License

See [LICENSE](LICENSE).
