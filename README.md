# dsh-playwright

DeepSeek Harness profile bundle installing the **official Microsoft Playwright MCP** (`@playwright/mcp`) — browser automation through structured **accessibility snapshots** (no vision model needed), from [github.com/microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp). Same bundle pattern as `dsh-chrome-devtools` / `dsh-serena` / `dsh-context7`.

Configuration source: verified via Context7 against the project's own README — the standard client config is exactly:

```json
{ "mcpServers": { "playwright": { "command": "npx", "args": ["@playwright/mcp@latest"] } } }
```

## What it adds to every agent on the profile

| Row | Module | Purpose |
|---|---|---|
| `mcp-playwright` | `@deepseek-ai/dsh-mcp-client` | Spawns `npx @playwright/mcp@latest` (stdio) → tools namespaced `mcp__playwright__*`: `browser_navigate`, `browser_snapshot`, `browser_click`, `browser_type`, `browser_take_screenshot`, tabs, requests, and more |

- Uses your **installed Google Chrome** by default (headed, persistent profile)
- First start downloads the package via npx (30–60s); afterwards it starts in ~1s
- Coexists with `dsh-chrome-devtools` — separate tool namespace and browser instance

## Install

Add to the profile `package.json` `dependencies` + `dsh.profile.bundles` (or `dsh plugin --profile desktop add file:C:/Users/IT LAB/Documents/deepseek-harness/dsh-playwright`), `pnpm install`, restart or re-apply.

> Note: `file:` dependencies are **copied** into the profile by pnpm — after editing this bundle's `cordis.patch.yml`, run `pnpm install` again (or edit the installed copy under `node_modules\dsh-playwright`) before re-applying.

## Optional launch flags

Append to `args` in `cordis.patch.yml`:

| Flag | Effect |
|---|---|
| `--isolated` | In-memory profile; no persisted cookies/storage |
| `--headless` | No visible browser window |
| `--browser chromium` | Playwright's bundled Chromium instead of system Chrome |
| `--caps=vision,pdf` | Extra capability tools |
| `--config <file.json>` | Full JSON configuration (browser, timeouts, network rules, secrets — see project docs) |

## Verify

Ask the agent to `browser_navigate` to any page and `browser_snapshot` it — or check the plugin list for the live `include:mcp-playwright` entry.

## Publish / uninstall

Push this folder to GitHub and install `"dsh-playwright": "github:<you>/dsh-playwright"`. Remove from `bundles` + `dependencies` to uninstall; the npx cache can be kept or cleared separately.
