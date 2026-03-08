# CLAUDE.md

Claude Code plugin. Skills, agents, and commands are pure markdown.

## Structure

```
.claude-plugin/plugin.json        # Plugin manifest at repo root (points to plugin/ paths)
plugin/skills/<name>/SKILL.md     # Skills — auto-invocable by Claude
plugin/commands/<name>.md         # Commands — user-invoked slash commands
plugin/agents/<name>.md           # Agent definitions
```

## Conventions

- **YAML frontmatter** in SKILL.md files has two fields: `name` (matches directory name) and `description`.
- **Command files** use `description` only (no `name` field).
- **Skills vs commands:** Skills are for things Claude needs to invoke automatically. Commands are for user-invoked actions. If only the user types it, make it a command.
- **Changelog** in README.md is append-only per version. Don't edit historical entries.

## Releasing

1. Bump `version` in `.claude-plugin/plugin.json`
2. Add a changelog section in README.md under `## Changelog`
3. Commit, push to `main`
4. Tag with `git tag v<version>` and push the tag
5. Update `version` in the marketplace repo (`justinjdev/claude-plugins` → `.claude-plugin/marketplace.json`)
