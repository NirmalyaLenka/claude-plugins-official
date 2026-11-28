# Claude Code Plugins Directory

A curated directory of high-quality plugins for Claude Code.

> **⚠️ Important:** Make sure you trust a plugin before installing, updating, or using it. Anthropic does not control what MCP servers, files, or other software are included in plugins and cannot verify that they will work as intended or that they won't change. See each plugin's homepage for more information.

## Structure

- **`/plugins`** - Internal plugins developed and maintained by Anthropic
- **`/external_plugins`** - Third-party plugins from partners and the community

## Installation

Plugins can be installed directly from this marketplace via Claude Code's plugin system.

To install, run `/plugin install {plugin-name}@claude-plugins-official`

or browse for the plugin in `/plugin > Discover`


## Before installing a plugin

Treat plugins as executable extensions, not just documentation. Depending on the plugin, commands, lifecycle hooks, helper scripts, or configured MCP servers may run processes, read or write files, access the network, or use credentials available to the host session. Marketplace inclusion is not a guarantee that a plugin is safe for every environment.

1. Read the marketplace entry and the plugin's README, license, homepage, and source repository.
2. Inspect `.claude-plugin/plugin.json`, `.mcp.json`, `hooks/hooks.json`, `commands/`, `agents/`, `skills/`, and any referenced scripts that are present. Check shell commands, Python/Node helpers, network destinations, file writes, environment variables, and credential handling.
3. For remote sources, verify the `source.url`, `ref`, and `sha` when provided. Re-review the source before upgrades because a branch or tag can change.
4. Test unfamiliar plugins in a disposable project or isolated environment without production credentials or sensitive files. Grant only the permissions needed for the task.
5. Review hook behavior and tool output after installation; remove the plugin if it performs unexpected actions.

For example, this repository includes hook configurations that launch shell/Python helpers, including `plugins/ralph-loop/hooks/hooks.json` and `plugins/security-guidance/hooks/hooks.json`. Inspect the referenced code before enabling such hooks.

## Contributing

### Internal Plugins

Internal plugins are developed by Anthropic team members. See `/plugins/example-plugin` for a reference implementation.

### External Plugins

Third-party partners can submit plugins for inclusion in the marketplace. External plugins must meet quality and security standards for approval. To submit a new plugin, use the [plugin directory submission form](https://clau.de/plugin-directory-submission).

## Plugin Structure

Manifest-based plugins commonly include `.claude-plugin/plugin.json`. Marketplace skill bundles may instead declare skills directly in the marketplace entry when configured for non-strict loading. Optional components vary by plugin:

```
plugin-name/
├── .claude-plugin/
│   └── plugin.json      # Manifest for manifest-based plugins
├── .mcp.json            # MCP server configuration (optional)
├── hooks/
│   └── hooks.json       # Lifecycle hook declarations (optional)
├── commands/            # Slash commands (optional)
├── agents/              # Agent definitions (optional)
├── skills/              # Skill definitions (optional)
├── scripts/             # Helper code referenced by plugin components
└── README.md            # Documentation
```

## Plugin names are immutable

The `name` field in a marketplace entry is an **immutable slug**. Once a plugin has been published, its `name` must not change — users have it installed under that slug, and renaming it breaks their install with a `plugin-not-found` error.

- To change how a plugin is labeled in the UI, set or update `displayName` instead.
- If a rename is genuinely unavoidable, add an entry to the top-level `renames` map in `.claude-plugin/marketplace.json` so existing installs auto-migrate:

```json
"renames": {
  "old-name": "new-name"
}
```

The Claude Code plugin loader reads this map and transparently rewrites the old slug to the new one on the user's next sync.

## Skill-bundle plugins

When a plugin's source repository ships skills (`SKILL.md` files) without a `.claude-plugin/plugin.json` manifest, the marketplace entry can declare the skills directly using `strict: false` and an explicit `skills` array.

```json
{
  "name": "example-bundle",
  "description": "Brief description of the bundled skills.",
  "author": { "name": "Author Name" },
  "category": "development",
  "source": {
    "source": "git-subdir",
    "url": "https://github.com/example-org/sdk.git",
    "path": "packages/agent-skills",
    "ref": "main",
    "sha": "<commit sha>"
  },
  "strict": false,
  "skills": [
    "./skill-a",
    "./skill-b",
    "./skill-c"
  ],
  "homepage": "https://github.com/example-org/sdk"
}
```

Each path in `skills` is relative to `source.path` and points at a directory containing a `SKILL.md`. Paths can reach deeper than a single level — for example, `["./libA/skill-1", "./libB/skill-2"]` exposes a curated subset across multiple library subdirectories. Each skill is registered as `<plugin-name>:<skill-name>` in Claude Code.

For the underlying schema, see [Strict mode](https://code.claude.com/docs/en/plugin-marketplaces) in the marketplace documentation.

## License

Please see each linked plugin for the relevant LICENSE file.

## Documentation

For more information on developing Claude Code plugins, see the [official documentation](https://code.claude.com/docs/en/plugins).


---

## Maintenance & Audit

- **Audit date:** 11 November 2026
- **Documentation status:** README reviewed and audit record added on this date.
- **Scope and evidence:** This is a documentation-level maintenance pass. It does not establish that source code compiles, dependencies are current, security has been tested, or runtime behavior has been verified.
- **Code and tests:** No new functional code changes or test results are claimed by this README update. Run project-specific build, test, and security checks before relying on the project.
- **Maintenance baseline:** Documentation status recorded through 11 November 2026; revisit setup instructions, dependencies, security, known limitations, and tests when the project changes.

## Contact

Maintained up to 25 November 2026.

If you encounter any problems, please contact [carmodbhai@gmail.com](mailto:carmodbhai@gmail.com).
