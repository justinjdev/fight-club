# Fight Club

> The first rule of fight club: your code is not as good as you think it is.

An adversarial code review plugin for Claude Code. Fight Club challenges every design decision, hunts for bugs before production does, and delivers verdicts without sugarcoating.

## Installation

Register the marketplace (once):

```bash
/plugin marketplace add justinjdev/claude-plugins
```

Install the plugin:

```bash
/plugin install fight-club@justinjdev
```

## Components

### `/fight-club:fight <target>`

Run an adversarial review on a file, diff, or PR:

```bash
/fight-club:fight src/auth.ts      # review a file
/fight-club:fight 42               # review PR #42
/fight-club:fight diff             # review current git diff
```

## Adding Skills

Drop skill directories into `skills/` — Claude Code will discover them automatically.

## License

MIT
