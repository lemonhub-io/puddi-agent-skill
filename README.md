# Puddi Agent Skill

An [Agent Skill](https://agentskills.io/) for writing, reviewing, and debugging Puddi code. It covers real syntax, target-specific APIs, common traps, and ways to avoid unnecessary allocation. The installer is a dependency-free Puddi package.

## Install

Install [Puddi 0.1.2 or later](https://github.com/puddi-lang/puddi/releases/tag/v0.1.2). In a Puddi package, add the installer:

```sh
pud add github.com/lemonhub-io/puddi-agent-skill@0.1.0
```

For a dedicated installer, first run `pud new puddi-skill-installer` and enter the new directory. After adding the dependency, replace its `src/main.pud` with:

```puddi
import puddi_agent_skill

fn main() {
    puddi_agent_skill.run()
}
```

Run `pud run` to install the skill for detected agents. The package includes the skill text, so the caller only needs the resolved Puddi dependency.

Use `pud run -- --dry-run` to preview destinations. Use `pud run -- --agent codex` to select one agent explicitly, including an installation the detector misses. The two flags can be combined.

On Windows, use the Node host with Node.js 22 or newer so the installer can reach drive-letter paths:

```powershell
$env:PUD_HOST = "node"
pud run
```

You can also run `pud run` directly from a checkout of this repository.

## Supported agents

The installer looks for agent configuration directories and CLI files on `PATH`. It also recognizes the Cursor macOS app and the GitHub Copilot extension in standard VS Code extension directories.

| Agent | ID | Personal skills directory |
| --- | --- | --- |
| Codex | `codex` | `~/.agents/skills` |
| Claude Code | `claude` | `~/.claude/skills` or `$CLAUDE_CONFIG_DIR/skills` |
| Cursor | `cursor` | `~/.cursor/skills` |
| Gemini CLI | `gemini` | `~/.gemini/skills` |
| OpenCode | `opencode` | `~/.config/opencode/skills` or `$XDG_CONFIG_HOME/opencode/skills` |
| GitHub Copilot CLI and VS Code Copilot | `copilot` | `~/.copilot/skills` |

A matching installation is left in place. A different existing `puddi` directory is preserved and reported, and installation continues for the other agents. Each agent receives the same [`puddi/SKILL.md`](skills/puddi/SKILL.md) with no additional runtime dependencies. You can also copy the `puddi` directory into a skills directory yourself. Start a new agent session if the skill does not appear.

Installation paths are documented by [Codex](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills), [Claude Code](https://code.claude.com/docs/en/skills#choose-where-skills-load), [Cursor](https://cursor.com/docs/skills#skill-directories), [Gemini CLI](https://geminicli.com/docs/cli/skills/#skill-discovery-tiers), [OpenCode](https://opencode.ai/docs/skills/#discovery), [GitHub Copilot](https://docs.github.com/en/copilot/concepts/customizations/agent-skills), and [VS Code](https://code.visualstudio.com/docs/agent-customization/agent-skills#_skill-locations).

## Updating the skill

Edit `skills/puddi/SKILL.md` and run `python3 scripts/embed-skill.py` to regenerate the bundled Puddi module. Commit both files together.
