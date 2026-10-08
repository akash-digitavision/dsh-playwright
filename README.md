# dsh-playwright

[![Listed on dsh-plugin.org](https://dsh-plugin.org/badges/listed.svg)](https://dsh-plugin.org/plugins/akash-digitavision/dsh-playwright)

**One-line value:** install this and every DeepSeek Harness agent can drive a real browser — navigation, clicking, typing, snapshots, screenshots — using the official Microsoft Playwright MCP.

DeepSeek Harness profile bundle installing the **official Microsoft Playwright MCP** (`@playwright/mcp`) — browser automation through structured **accessibility snapshots** (no vision model needed), from [github.com/microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp). Same bundle pattern as `dsh-chrome-devtools`.

**Capability category:** 工具与能力 (Tools & Capabilities)
**License:** MIT · **Compatibility:** DSH ≥ 0.2.0-rc.2 (bundles API), Windows/macOS/Linux (Node.js + npx + Chrome/Chromium required)

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

**One command (recommended):**

```bash
dsh plugin --profile web add github:akash-digitavision/dsh-playwright
```

or in **Settings → Plugin Market** (DSH Plugin Hub), search for `dsh-playwright`.

**Manual alternative** — add `dsh-playwright` to the profile `package.json` `dependencies` + `dsh.profile.bundles`, run `pnpm install` in the profile, restart or re-apply.

**Requirements:** Node.js ≥ 18 with npx (the MCP server downloads on first use), and an installed Chrome or Chromium.

> Note: `file:` dependencies are **copied** into the profile by pnpm — after editing this bundle's `cordis.patch.yml`, run `pnpm install` again (or edit the installed copy under `node_modules\dsh-playwright`) before re-applying.

## Permissions & external services

- Spawns a local `npx @playwright/mcp` process that launches a real browser on your machine
- No accounts, no API keys, no telemetry; network access is whatever pages you browse
- Edits/automation run with your user privileges — review prompts before approving destructive actions

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
