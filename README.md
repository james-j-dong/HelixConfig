# Helix Editor Configuration

My personal Helix editor configuration.

## Installation

```bash
# Clone this repo to the correct location
git clone git@github.com:james-j-dong/HelixConfig.git ~/.config/helix
```

The config references language servers and formatters by name. Helix does **not**
warn loudly when they are missing — it just silently drops the feature — so
install the dependencies below too.

### Language servers & formatters

```bash
# Python — type checker, linter/LSP, formatter
uv tool install basedpyright
uv tool install ruff

# Rust
rustup component add rust-analyzer

# TypeScript / JavaScript / CSS / HTML / JSON
npm install -g typescript typescript-language-server \
               @tailwindcss/language-server \
               vscode-langservers-extracted \
               prettier
```

`vscode-langservers-extracted` provides the `eslint`, `css`, `html`, and `json`
servers.

### Verify

```bash
hx --health python    # and: rust typescript tsx javascript jsx css html json
```

Every configured server should show a `✓`. `Configured formatter` should name a
binary for Python, TS/TSX, JS/JSX, CSS, HTML, and JSON.

## Notes

- **Python** uses `basedpyright` rather than stock `pyright`. Stock pyright does
  not advertise `inlayHintProvider`, so Python gets no inlay hints despite
  `editor.lsp.display-inlay-hints` being enabled. Formatting is handled by
  `ruff format`, since neither pyright variant provides a formatter.
- `typeCheckingMode` in `languages.toml` is a **global default**. A project's
  `pyproject.toml` overrides it. Turn it down to `"basic"` if diagnostics are
  noisy, or up to `"strict"` / `"recommended"`.
- Formatters use `--stdin-filepath %{buffer_name}` so prettier and ruff resolve
  per-directory config (`.prettierrc`, `pyproject.toml`) relative to the file
  being formatted rather than Helix's working directory.
- `ty` (Astral's type checker) is a faster alternative to basedpyright but is
  still pre-1.0 and serves no inlay hints. Worth revisiting at 1.0.

## Updating

```bash
cd ~/.config/helix
git pull
```
