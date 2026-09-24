<p align="center">
  <a href="https://www.respira.press">
    <img src="https://www.respira.press/og/respira-home-og.jpg" alt="Respira for WordPress" width="100%">
  </a>
</p>

<h1 align="center">Respira WordPress MCP Server</h1>

<p align="center">
  <strong>A catalog of 337 MCP tools and 339 WordPress Abilities across 17 page builders. The AI infrastructure layer for WordPress.</strong><br>
  Element-level editing, full page creation, HTML and Figma to native builder conversion, design directions, site memory, accessibility and security scanning, per-tool governance, snapshots and rollback.
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/@respira/wordpress-mcp-server"><img src="https://img.shields.io/npm/v/@respira/wordpress-mcp-server.svg?style=flat-square&color=10b981" alt="npm version"></a>
  <a href="https://www.npmjs.com/package/@respira/wordpress-mcp-server"><img src="https://img.shields.io/npm/dm/@respira/wordpress-mcp-server.svg?style=flat-square" alt="npm downloads"></a>
  <img src="https://img.shields.io/badge/tools-337-10b981?style=flat-square" alt="337 tools">
  <img src="https://img.shields.io/badge/builders-17-10b981?style=flat-square" alt="17 page builders">
  <img src="https://img.shields.io/badge/TypeScript-100%25-blue?style=flat-square&logo=typescript&logoColor=white" alt="TypeScript">
</p>

<p align="center">
  <a href="https://www.respira.press">Website</a> •
  <a href="https://www.respira.press/docs">Docs</a> •
  <a href="https://www.respira.press/plugin">Plugin</a> •
  <a href="https://www.respira.press/mcp">MCP setup</a> •
  <a href="https://www.respira.press/skills">Skills</a> •
  <a href="https://www.respira.press/support">Support</a>
</p>

---

## What this repo is, what it isn't

This repository is the **public listing** for the Respira WordPress MCP server. The server source ships on npm as [`@respira/wordpress-mcp-server`](https://www.npmjs.com/package/@respira/wordpress-mcp-server). That wrapper code is **MIT-licensed** and you are welcome to read, fork, or vendor it.

The server is a **client for the Respira WordPress plugin**, not a standalone product. To do real work it needs:

- The [Respira for WordPress](https://www.respira.press/plugin) plugin installed on your site
- A valid Respira API key bound to a license

**The plugin is not open source.** It is distributed under a commercial license. Free trial at [respira.press](https://www.respira.press), no card required. Paid plans start at 9 EUR a month.

In short: the wrapper you `npx -y` is open. The product behind it is not, and that is deliberate. If you want a self-contained "AI edits WordPress" stack with no commercial dependency, this is not it. The plugin is built and maintained full time by one person, and the license fees are how that happens.

For security reports see [SECURITY.md](./SECURITY.md).

---

## What makes Respira different

Most WordPress MCP servers wrap the REST API. They can create posts and pages, but they cannot touch page builder content, which is where the actual site lives.

Respira ships a WordPress plugin that gives an AI agent native access to the builder's own data structures, plus the safety rails that make writing to a live site survivable.

| Capability | Respira | REST API wrappers |
|---|---|---|
| Page builder support | **17 builders** | None |
| Element-level find / update / move / remove | **Yes** | No |
| Build full pages from a declarative structure | **Yes** | No |
| Convert HTML or Figma to native builder output | **Yes** | No |
| Design directions (site-wide art direction) | **Yes** | No |
| Site memory that persists across agent sessions | **Yes** | No |
| Accessibility scan and auto-fix | **Yes** | No |
| Security audit and core hardening | **Yes** | No |
| Snapshot before every write, rollback anytime | **Yes** | No |
| Duplicate-before-edit safety | **Yes** | No |
| Per-tool governance from wp-admin | **Yes** | No |
| WordPress Abilities API and WebMCP | **Yes** | Rare |
| WooCommerce (products, orders, inventory, storefront) | **Yes** (add-on) | No |

---

## Quick start

### 1. Install the WordPress plugin

Download from [respira.press/plugin](https://www.respira.press/plugin), upload to WordPress, activate, then go to **Respira > API Keys** and generate a key.

### 2. Connect your AI tool

The fastest path is the generated command at [respira.press/dashboard/mcp](https://www.respira.press/dashboard/mcp), which fills in your site and key for you. Manual configs below.

<details>
<summary><b>Claude Code</b></summary>

```bash
claude mcp add respira-wordpress -- npx -y @respira/wordpress-mcp-server
```

</details>

<details>
<summary><b>Claude Desktop</b></summary>

Add to `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{
  "mcpServers": {
    "respira-wordpress": {
      "command": "npx",
      "args": ["-y", "@respira/wordpress-mcp-server"]
    }
  }
}
```

</details>

<details>
<summary><b>Cursor</b></summary>

Create `.cursor/mcp.json` in your project:

```json
{
  "mcpServers": {
    "respira-wordpress": {
      "command": "npx",
      "args": ["-y", "@respira/wordpress-mcp-server"]
    }
  }
}
```

</details>

<details>
<summary><b>Codex, Windsurf, and other clients</b></summary>

Same shape as above. Codex uses TOML in `~/.codex/config.toml`:

```toml
[mcp_servers.respira-wordpress]
command = "npx"
args = ["-y", "@respira/wordpress-mcp-server"]
```

Or let [add-mcp](https://github.com/neondatabase/add-mcp) detect your tool:

```bash
npx add-mcp "npx -y @respira/wordpress-mcp-server"
```

</details>

<details>
<summary><b>ChatGPT and remote clients</b></summary>

Respira also runs as a hosted remote MCP server with OAuth, so no local Node process is needed. Setup instructions per client are at [respira.press/mcp](https://www.respira.press/mcp).

</details>

### 3. Add your site

Run the wizard:

```bash
npx @respira/wordpress-mcp-server --setup
```

Or write `~/.respira/config.json` yourself:

```json
{
  "sites": [
    {
      "id": "my-site",
      "name": "My WordPress Site",
      "url": "https://yoursite.com",
      "apiKey": "respira_your-api-key",
      "default": true
    }
  ]
}
```

Restart your AI tool and start editing.

### Hitting a client tool limit?

Some MCP clients cap the number of active tools, often at 100. Respira advertises far more than that, so add `enabledTools` to your config and only those appear in the listing:

```json
{
  "sites": [{ "...": "..." }],
  "preferences": {
    "enabledTools": [
      "respira_read_page",
      "respira_update_page",
      "respira_list_pages",
      "respira_find_element",
      "respira_update_element",
      "respira_build_page",
      "respira_get_site_context",
      "respira_get_builder_info"
    ]
  }
}
```

Site management tools (`respira_list_sites`, `respira_switch_site`, `respira_get_active_site`) are always included. Unlisted tools still work if called directly. The filter only controls what is advertised.

On top of that, the server filters the tool list by what your site actually runs. A Divi site with no WooCommerce does not see the commerce tools. Detection fails open: if it cannot tell, you get the full list.

---

## 17 supported page builders

`level` is Respira's write depth, not the builder's quality.

| Builder | Level | Notes |
|---|---|---|
| **Bricks** | Deep | Global classes, theme styles, ACSS, components, query loops, BEM linting |
| **Elementor** | Full | Native API, runtime control registry |
| **Divi** | Full | Divi 4 and Divi 5, 40+ module definitions on Divi 5 |
| **Oxygen** | Full | Classic and Oxygen 6 |
| **Gutenberg** | Full | Block registry, FSE templates, patterns, navigation |
| **Flatsome** | Full | Round-trip shortcode editing, 55-element intelligence |
| **Beaver Builder** | Smart | Tree utility, static schemas |
| **Breakdance** | Smart | Tree utility, static schemas |
| **Brizy** | Smart | Tree utility |
| **WPBakery** | Smart | Tree utility |
| **Spectra** | Smart | Block target |
| **Kadence Blocks** | Smart | Block target |
| **GenerateBlocks** | Smart | Block target |
| **GreenShift** | Smart | Block target |
| **Visual Composer** | Smart | Limited write |
| **Thrive Architect** | Smart | Text edits |
| **SeedProd** | Smart | Audit only |

Mixed-builder sites are handled per page, not per site.

---

## What the tools cover

The full reference lives in the [docs](https://www.respira.press/docs). The families:

| Family | What it does |
|---|---|
| **Pages, posts, custom posts** | Read and write with builder-aware payloads, outlines, duplicates, translations |
| **Element operations** | Find, update, move, duplicate, reorder, remove single elements by ID, type, class, or content text |
| **Page building** | `respira_build_page` from a declarative structure, plus one-line widget shortcuts (heading, button, image, form, slider, pricing table, and more) |
| **Conversion** | HTML to native builder with CSS extraction, responsive mapping, and a fidelity score. Figma to Bricks, Divi, Elementor, or Gutenberg via the bundled skills |
| **Design directions** | Save, apply, activate, export a site-wide art direction. Preview before commit, apply the same direction to another site |
| **Design tokens** | Create, update, import DTCG tokens, sync a design system into the theme |
| **Site memory** | `respira_remember`, `respira_forget`, `respira_list_memory`. Facts about the site that survive between agent sessions |
| **Playbooks** | Named, reusable procedures an agent can create, fetch, and run |
| **Snapshots** | Automatic before every write, diff two snapshots, restore any of them |
| **Analysis** | SEO, AEO, readability, RankMath, structured data, images, Core Web Vitals, PageSpeed |
| **Accessibility** | Scan a page, list scans, apply fixes |
| **Security** | Security audit, validation, core hardening, debug log reading |
| **Theme and templates** | FSE templates, template parts, patterns, navigation, theme builder templates, theme file read and write |
| **Site structure** | Custom post types, taxonomies, ACF field groups, menus, mega menus, terms |
| **Media** | Upload, sideload from URL, batch metadata updates, Openverse stock image search with auto-attribution |
| **Bulk operations** | Up to 100 pages per call, with mandatory snapshots |
| **Multi-site** | List, switch, and act on many sites from one config |
| **WooCommerce** | 104 tools in the paid [add-on](https://www.respira.press/addons/woocommerce): catalog, pricing, inventory, orders, storefront design |

All tools use `respira_*` names. The legacy `wordpress_*` aliases are deprecated and will be removed.

### What the numbers mean

Counts on this page describe the **catalog on a fully enabled site**, not what your client will list.

| Figure | Count |
|---|---|
| MCP tools, whole product | 337 |
| MCP tools included in every plan | 214 |
| WooCommerce tools (paid add-on) | 104 |
| WordPress Abilities | 339 |
| Unique tools and abilities, whole product | 320 |

A live `tools/list` returns fewer than 337, and that is correct behaviour rather than a missing feature. The server context-filters the catalog per site, so Bricks tools never reach an Elementor site and the commerce tools stay hidden without WooCommerce. A block-theme site with no page builder sees roughly 28 fewer core tools.

The Abilities registry is not context-filtered, so on the same site the abilities total legitimately exceeds the tools total. The two are not directly comparable.

### Bundled skills

The package ships Respira's Claude Code skills, which are the recipes that make the tools useful together: page-builder migrations (Divi to Bricks, Elementor to Gutenberg, WPBakery to Bricks, and others), Figma-to-builder conversion, site audits, SEO and AEO passes, and WooCommerce workflows.

```bash
npx @respira/wordpress-mcp-server install-skills
```

Browse the catalog at [respira.press/skills](https://www.respira.press/skills).

---

## Safety

Writing to a live production site is the whole problem. The rails:

1. **Snapshot before every mutation.** Restore with `respira_restore_snapshot`, compare with `respira_diff_snapshots`.
2. **Duplicate before edit.** The original stays untouched while the agent works on a copy.
3. **Per-tool governance.** Admins enable or disable individual tools from wp-admin. Governance applies to the REST path, the Abilities API path, and WebMCP alike.
4. **Read-only mode.** Point an agent at a site and let it look without letting it write.
5. **Activity log.** Every call is recorded and readable from wp-admin and from `respira_list_activity`.

---

## WordPress AI ecosystem

Respira works with the official WordPress AI stack, not around it.

| Path | How it works | Requirements |
|---|---|---|
| **Standalone MCP** (this package) | `npx @respira/wordpress-mcp-server` | Node 18+, Respira plugin |
| **Remote MCP** | Hosted endpoint with OAuth, no local process | Respira plugin, account |
| **WordPress Abilities API** | 339 abilities registered, auto-discovered | WP 6.9+, Respira plugin |
| **MCP Adapter** | Abilities exposed over WP-CLI STDIO | WP 6.9+, MCP Adapter plugin |
| **WebMCP** | Browser-native MCP via the Chrome Abilities API | Chrome 146+, Respira plugin |

[Inhale](https://wordpress.org/plugins/inhale-mcp-abilities/) is the free companion plugin that registers a small set of Abilities with no Respira account at all, if you want to try the Abilities path before anything else.

---

## Install options

### npx

```bash
npx -y @respira/wordpress-mcp-server
```

Zero install, good for trying it. The downside is that the npx cache can corrupt itself after an interrupted install, a disconnected external drive, or antivirus quarantine, and the resulting `ENOENT` errors are confusing. See Troubleshooting.

### Global install (most stable)

```bash
npm install -g @respira/wordpress-mcp-server
respira-wordpress-mcp
```

Avoids the npx cache entirely. The better choice for daily use.

### CLI options

| Flag | Alias | Description |
|---|---|---|
| `--setup` | `-s` | Interactive setup wizard |
| `--list` | `-l` | List configured sites |
| `--test` | `-t` | Test the connection |
| `--doctor` | `-d` | Health diagnostics, add `--json` for machine-readable output |
| `--install-config` | | Write the MCP config for a detected client |
| `install-skills` | | Copy the bundled skills into your local skills directory |
| `--stdio` | | STDIO transport for the MCP Adapter |
| `--version` | `-v` | Print the version |
| `--help` | `-h` | Help |

### Environment variables

```bash
export WORDPRESS_URL="https://yoursite.com"
export WORDPRESS_API_KEY="respira_your_key"
```

`RESPIRA_CONFIG_FILE` and `RESPIRA_CONFIG_B64` are the multi-site alternatives.

---

## Health check

```bash
npx @respira/wordpress-mcp-server --doctor
```

Checks Node version, config file, site connectivity, plugin version, API compatibility, and available updates, and reports pass or fail per check with something you can act on. Add `--json` for CI pipelines.

---

## Troubleshooting

<details>
<summary><b>Windows: 'npx' is not recognized</b></summary>

Use the full path:

```json
{ "command": "C:\\Program Files\\nodejs\\npx.cmd", "args": ["-y", "@respira/wordpress-mcp-server"] }
```

Or install globally with `npm install -g @respira/wordpress-mcp-server` and use `{ "command": "respira-wordpress-mcp" }`.

</details>

<details>
<summary><b>Connection failed</b></summary>

1. Check the API key at WordPress > Respira > API Keys
2. The URL must include `https://`
3. The plugin must be activated
4. Check whether your host blocks the REST API

`respira_diagnose_connection` reports all four in one call.

</details>

<details>
<summary><b>HTML instead of JSON, or a homepage redirect on every call</b></summary>

Some sites have plugin or theme rewrite rules that catch `/wp-json/[anything]` and rewrite the path to `index.php` without the `?rest_route=` query var. WordPress then 301-redirects to the homepage (you will see `x-redirect-by: WordPress` in the chain) and the MCP server gets HTML where it expected JSON.

The server auto-detects this, retries the call as `?rest_route=...` against the site root, and if that returns JSON it routes every later call the same way for the rest of the session, with one stderr warning on first activation.

If you already know the rewrite shadowing is in play, skip the probe with `forceRestRoute`:

```json
{
  "sites": [
    {
      "id": "my-site",
      "url": "https://yoursite.com",
      "apiKey": "respira_your-api-key",
      "default": true,
      "forceRestRoute": true
    }
  ]
}
```

`respira_diagnose_connection` probes both forms and reports `rest_route_fallback_worked`, `rest_route_fallback_active`, and `force_rest_route_configured`.

</details>

<details>
<summary><b>Tools not showing up</b></summary>

1. Restart the AI tool completely, not just the window
2. Validate the JSON syntax in the config file
3. Confirm the config file location for your client
4. Run `npx @respira/wordpress-mcp-server --test`

If the client caps active tools, see `enabledTools` above.

</details>

<details>
<summary><b>ENOENT errors mentioning <code>/_npx/</code> or <code>node_modules</code></b></summary>

The npx cache is corrupted. Common causes: interrupted install, external drive disconnected mid-install, antivirus quarantine, or `npm cache clean` running while npx was active.

```bash
# 1. Switch to a global install, most stable
npm install -g @respira/wordpress-mcp-server
# then use "command": "respira-wordpress-mcp" with no npx wrapper

# 2. Or clear the npx cache and let it rebuild
npx clear-npx-cache
npx -y @respira/wordpress-mcp-server

# 3. Or clear the whole npm cache
npm cache clean --force
```

</details>

---

## Security

API key validation happens server side in the WordPress plugin. The MCP server passes credentials through and does not store or validate them.

Report vulnerabilities to security@respira.press. Full policy in [SECURITY.md](./SECURITY.md).

---

## Links

- [Website](https://www.respira.press)
- [Documentation](https://www.respira.press/docs)
- [Download the plugin](https://www.respira.press/plugin)
- [MCP setup](https://www.respira.press/mcp)
- [Skills catalog](https://www.respira.press/skills)
- [Abilities](https://www.respira.press/abilities)
- [WooCommerce add-on](https://www.respira.press/addons/woocommerce)
- [Accessibility scanner add-on](https://www.respira.press/addons/accessibility-scanner)
- [Support](https://www.respira.press/support)

## Where to find Respira

| Directory | Listing |
|---|---|
| **npm** | [`@respira/wordpress-mcp-server`](https://www.npmjs.com/package/@respira/wordpress-mcp-server) |
| **Official MCP Registry** | `io.github.webmyc/respira-wordpress` |
| **Smithery** | [smithery.ai](https://smithery.ai) |
| **Glama** | [glama.ai/mcp/servers](https://glama.ai/mcp/servers) |
| **mcp.so** | [mcp.so](https://mcp.so) |
| **cursor.directory** | [cursor.directory](https://cursor.directory) |

---

## License

MIT © [Respira](https://www.respira.press)

---

<p align="center">
  <strong>337 tools in the catalog. 17 builders. The AI infrastructure layer for WordPress.</strong><br>
  <a href="https://www.respira.press">respira.press</a>
</p>
