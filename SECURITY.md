# Security policy

## Reporting a vulnerability

Email **security@respira.press** with the issue and a way to reproduce it. You should expect an acknowledgement within 48 hours and a status update within five business days.

Please do not file public GitHub issues for vulnerabilities.

## Scope

This security policy covers:

- The MCP server published to npm as `@respira/wordpress-mcp-server` (the wrapper exposed by this repo). Source: open, MIT-licensed.
- The **Respira for WordPress** plugin's REST API surface (`/wp-json/respira/v1/*`, `/wp-json/respira/v2/*`, `/wp-json/webmcp/v1/*`). Source: closed, commercial license.
- The Respira account / billing surfaces hosted at `https://www.respira.press` (signup, login, dashboard, billing, license issuance).

Out of scope: third-party MCP directories (Glama, Smithery, MCP Registry). Please report issues with those listings to the directory operators directly.

## Architecture & trust model

The MCP server in this repo is a thin wrapper. **License validation, business logic, and per-site permission gating all live in the WordPress plugin.** That separation is intentional:

- The npm package is MIT and contains no license-validation logic. Anyone can read, fork, or vendor it.
- The plugin, which is not open source, enforces the API key check on every write-capable REST route. It also runs the snapshot and duplicate-first safety, the builder intelligence, and governance.
- Without a valid Respira API key bound to an active license, the plugin's write surface is closed. A small set of read-only endpoints are intentionally anonymous (`permission_callback => __return_true`) so an agent can introspect a site before authenticating: `/divi/modules/<slug>/schema`, `/server/compatibility`, and the `/status` liveness probe. None of them return site content, user data, or configuration.

So a way to make the npm package do something it should not on its own is an MCP-server bug. A way to bypass the plugin's API key gate is a plugin bug, and far higher severity.

## Telemetry the package sends

The npm package emits **anonymous crash reports to Sentry** at startup and on unhandled errors. The DSN is embedded in the build. Sentry treats DSNs as public by design, since they are how the SDK reaches the project. No customer data, no API keys and no site URLs are sent. Only stack traces, Node version, OS, and the agent client identifier (Claude Code, Cursor, and so on).

There is currently **no runtime opt-out** for crash reporting. An earlier version of this policy named `--no-telemetry` and `RESPIRA_MCP_DISABLE_UPDATE_CHECK` as ways to disable it. Neither does: the flag does not exist, and `RESPIRA_MCP_DISABLE_UPDATE_CHECK=1` only suppresses the version update check in `version-checker.ts`. An opt-out is on the list. If you need one before it lands, say so at security@respira.press.

Sentry is skipped entirely on Node 25 and above, because the SDK ships a zero-byte prebuilt native binary for that ABI and loading it aborts the process before any error handler can run. On those runtimes the package sends nothing.

The separate update check calls the npm registry to compare your installed version against the latest. Disable it with:

```bash
export RESPIRA_MCP_DISABLE_UPDATE_CHECK=1
```

## What the package contains

Inspect what `npx -y @respira/wordpress-mcp-server` actually downloads:

```bash
npm pack @respira/wordpress-mcp-server
tar -tzf respira-wordpress-mcp-server-*.tgz
```

Should show `package/dist/**` (compiled JS, sourcemaps, type definitions), `package/skills/**` (the bundled Claude Code skills, plain markdown), `package/certs/**`, `package/README.md`, `package/CHANGELOG.md`, `package/TOOL_CATALOG.md`, `package/SOUL.md`, `package/tool-capabilities.json`, `package/icon.png` and `package/package.json`. No build scripts, no postinstall hooks, no native binaries.

## Supported versions

| Version line | Status | Security fixes |
|---|---|---|
| 8.3.x | Current | Active |
| 8.2.x | Older minor | Critical only |
| 8.1.x and below | End of life | Upgrade |

The plugin and the MCP server version independently. The `respira_get_server_compatibility` tool returns the MCP version range a given plugin build supports, and a mismatch surfaces a stderr warning at startup.

## Coordinated disclosure

If your report results in a CVE, you get credited in the release notes unless you ask not to be. There is no bounty programme, but a vulnerability affecting the plugin's auth gate or the npm package's runtime safety earns a thank-you and Respira credit: a free year on the plan of your choice.
