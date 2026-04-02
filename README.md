# claude-router

Pick an [OpenRouter](https://openrouter.ai) model, launch [Claude Code](https://docs.anthropic.com/en/docs/claude-code) with it.

## Install

```bash
# Clone and symlink
git clone https://github.com/your-user/claude-router.git
ln -s "$(pwd)/claude-router/claude-router" ~/.local/bin/claude-router

# Set your API key (in ~/.env or export)
echo "OPENROUTER_API_KEY=sk-or-..." >> ~/.env
```

Requires `curl`, `jq`, and optionally `fzf` for interactive picking.

## Usage

```bash
claude-router                        # Interactive picker (fzf)
claude-router auto                   # Best free model
claude-router --free                 # Pick from free models only
claude-router --list                 # Browse available models
claude-router openai/gpt-4o         # Launch with specific model
claude-router auto -- -p "hello"    # Pass args to claude
claude-router --refresh              # Force-refresh model cache
```

## How it works

1. Fetches models from the OpenRouter API (cached daily in `~/.cache/claude-router/`)
2. Filters for text models with tool support
3. Sets `ANTHROPIC_BASE_URL` and friends, then `exec claude`

## License

[MIT](LICENSE)
