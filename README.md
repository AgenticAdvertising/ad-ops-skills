# Ad Ops Skills

Practical AI‑agent skills for running paid ad accounts — diagnostics, monitoring, and creative generation for Meta, Google, LinkedIn, and other channels.

Built for media buyers and agencies who want an AI assistant that can answer *"how are my accounts doing?"* and *"what should I do about it?"* without rebuilding the logic from scratch. Compatible with Claude Code, the Claude Agent SDK, and any agent runtime that supports Agent Skills.

## Skills (6)

### Performance & Budget Monitoring

| Skill                                  | What it does                                                                                                     |
| :------------------------------------- | :--------------------------------------------------------------------------------------------------------------- |
| [am-i-on-track](am-i-on-track.md)      | Intraday pacing: compares today's spend to expected‑at‑this‑hour, projects EOD, flags over/under‑pacing.         |
| [past-7days](past-7days.md)            | Weekly campaign‑level performance with WoW deltas and the top ad (name + copy) driving each campaign.            |
| [whats-running](whats-running.md)      | Active‑campaign inventory with daily budgets, flagging leaks: expired tests, past‑end‑date launches, zombies.    |

### Creative Optimization

| Skill                                       | What it does                                                                                                              |
| :------------------------------------------ | :------------------------------------------------------------------------------------------------------------------------ |
| [bleeders-winners](bleeders-winners.md)     | Identifies ads to stop (Bleeders) and ads to scale (Winners) with headline/body/CTA context and an action workflow.       |
| [fatigue-detector](fatigue-detector.md)     | Early warning for creative decay using frequency creep, CPM inflation, CTR erosion, and sentiment shift.                  |
| [copy-rotation](copy-rotation.md)           | Generates image‑matched Meta ad copy using the Four Horsemen psychology framework, matched to the account's signature.    |

## Format

Each skill is a single markdown file with frontmatter (`name`, `description`, `metadata.version`) and numbered sections covering detection logic, thresholds, action workflow, and a concrete example output. Skills are self‑contained — no shared context file is required.

## Usage

All these skills are directly available in Adside.ai within the Skills section under Settings:

<img width="1822" height="1109" alt="skills-ad-side" src="https://github.com/user-attachments/assets/1c402fe2-3a4d-42b0-831c-4acb181f4855" />

You an also just drop a skill file into your agent's skills directory (e.g., `~/.claude/skills/` for Claude Code) or reference the raw file from this repo. The agent will pick it up on next invocation.

## License

MIT — see [LICENSE](LICENSE). Inspired by [coreyhaines31/marketingskills](https://github.com/coreyhaines31/marketingskills).
