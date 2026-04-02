# claude-router

Pick an [OpenRouter](https://openrouter.ai) model, launch [Claude Code](https://docs.anthropic.com/en/docs/claude-code) with it. Also includes provider scripts for MiniMax and Z.ai.

## Install

```bash
# Clone and symlink all scripts
git clone https://github.com/burka/claude-router.git
ln -s "$(pwd)/claude-router/claude-router"  ~/.local/bin/claude-router
ln -s "$(pwd)/claude-router/claude-minimax" ~/.local/bin/claude-minimax
ln -s "$(pwd)/claude-router/claude-zai"    ~/.local/bin/claude-zai

# Set API keys in ~/.env
echo "OPENROUTER_API_KEY=sk-or-..."  >> ~/.env
echo "MINIMAX_API_KEY=..."           >> ~/.env
echo "ZAI_API_KEY=..."               >> ~/.env
```

Requires `curl`, `jq`, and optionally `fzf` for interactive picking.

## Usage

```bash
claude-router                        # Interactive picker (fzf)
claude-router auto                   # Best free model
claude-router --free                 # Pick from free models only
claude-router --list                 # Browse available models
claude-router openai/gpt-4o          # Launch with specific model
claude-router auto -- -p "hello"     # Pass args to claude

claude-minimax                       # Launch with MiniMax MoE
claude-zai                           # Launch with Zhipu GLM (Z.ai token plan)
```

## Default Parameters

Set default `claude` CLI args via environment variable or `~/.env`:

```bash
# ~/.env
CLAUDE_DEFAULT_PARAMS="--permission-mode auto"
CLAUDE_DEFAULT_PARAMS="--team"
CLAUDE_DEFAULT_PARAMS="-t"
```

Or via environment:
```bash
CLAUDE_DEFAULT_PARAMS="--permission-mode auto" claude-router openai/gpt-4o
```

User-specified args always override `CLAUDE_DEFAULT_PARAMS`.

## Default Wrapper

Wrap the `claude` launch command with `nice`, `taskset`, `prlimit`, etc.
Set via `~/.env` or environment:

```bash
# ~/.env — pin to cores 2-7, set memory limits
CLAUDE_DEFAULT_WRAPPER="nice -n 20 taskset -c 2-7 prlimit --as=128000000000 --rss=64000000000"
```

```bash
# Via environment
CLAUDE_DEFAULT_WRAPPER="echo running:" claude-router -p "hello"
```

## How it works

1. `claude-router` fetches models from the OpenRouter API (cached daily in `~/.cache/claude-router/`)
2. Filters for text models with tool support
3. Sets `ANTHROPIC_BASE_URL`, `ANTHROPIC_AUTH_TOKEN`, and model env vars, then launches `claude`
4. `claude-minimax` and `claude-zai` set their provider's endpoint and auth directly

## License

[MIT](LICENSE)
