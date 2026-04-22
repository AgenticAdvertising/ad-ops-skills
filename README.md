# Ad Ops Skills

Open-source practical AI‑agent Ad Ops skills for running paid ad accounts with AI — diagnostics, monitoring and optimizations for Meta, Google, LinkedIn, and other channels. PRs are welcome. 🤗

Built for performance marketers/agencies who want to accelerate their Ad Ops loop with AI. Compatible with [Adside.ai](https://adside.ai), ChatGPT, Claude Code, the Claude Agent SDK, and any agent runtime that supports Agent Skills.

## Skills (17)

### Briefings — start here

| Skill                                           | What it does                                                                                                                                       |
| :---------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------- |
| 🗞 [daily-brief](daily-brief.md)                | Morning digest that chains `am-i-on-track`, `whats-running`, `bleeders-winners`, `fatigue-detector`, and `copy-rotation` into a prioritized brief with replacement copy for every paused creative. |
| 📊 [weekly-exec-summary](weekly-exec-summary.md) | Monday client‑meeting roll‑up — narrative synthesis written for a stakeholder, not the operator. Agency‑facing counterpart to `daily-brief`.      |

### Performance & Budget

| Skill                                     | What it does                                                                                                                           |
| :---------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------- |
| ⏱️ [am-i-on-track](am-i-on-track.md)      | Intraday pacing: compares today's spend to expected‑at‑this‑hour, projects EOD, flags over/under‑pacing.                               |
| 📅 [past-7days](past-7days.md)            | Weekly campaign‑level performance with WoW deltas and the top ad (name + copy) driving each campaign.                                  |
| 🏃 [whats-running](whats-running.md)      | Active‑campaign inventory with daily budgets, flagging leaks: expired tests, past‑end‑date launches, zombies.                          |
| 💰 [budget-shifter](budget-shifter.md)    | Reallocates budget across campaigns based on ROAS/CPA — finds donors and receivers, with guardrails against learning‑phase resets.     |
| 🚨 [anomaly-watch](anomaly-watch.md)      | Off‑cadence alerts: CPM spikes, conversion outages, delivery freezes, spend runaway. Fires between scheduled reports.                  |

### Creative Optimization

| Skill                                          | What it does                                                                                                              |
| :--------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------ |
| 🩸 [bleeders-winners](bleeders-winners.md)     | Identifies ads to stop (Bleeders) and ads to scale (Winners) with headline/body/CTA context and an action workflow.       |
| 📉 [fatigue-detector](fatigue-detector.md)     | Early warning for creative decay using frequency creep, CPM inflation, CTR erosion, and sentiment shift.                  |
| 👥 [audience-health](audience-health.md)       | Ad‑set‑level saturation, frequency, and overlap — tells you when to refresh, expand, split, or retire an audience.        |
| 🪝 [hook-architect](hook-architect.md)         | Isolates the first 3 seconds — scores Hook Rate vs. account ±1σ, cross‑checks Hold Rate, tags archetypes, outputs iteration briefs. |
| 🎨 [creative-brief](creative-brief.md)         | Mines winning angles from top performers and produces 3 visual concepts (format, mood, key elements) to brief a designer. |
| ✍️ [copy-rotation](copy-rotation.md)           | Generates image‑matched Meta ad copy using the Four Horsemen psychology framework, matched to the account's signature.    |

### Market Intelligence

| Skill                                                 | What it does                                                                                                                       |
| :---------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------- |
| 🔭 [competitor-ads-watch](competitor-ads-watch.md)    | Weekly scan of Meta Ad Library for tracked competitors — surfaces new angles, formats, long‑running winners, and gaps vs. your mix. |
| 👂 [social-listening](social-listening.md)            | Brand sentiment across Reddit, X, and G2 — clusters themes, extracts verbatim quotes, and translates each into a paid‑ads move (hook, preempt, versus angle, pause). |

### Launch & Audit

| Skill                                          | What it does                                                                                                              |
| :--------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------ |
| 📋 [ad-qa-checklist](ad-qa-checklist.md)       | Pre‑launch preflight: tracking (UTMs, pixel, events), naming, placements, creative specs, landing‑page message match.     |
| 🏷 [asset-rename](asset-rename.md)             | Audits campaign/ad‑set/ad names against the Foundr three‑level convention and proposes standardized renames with confidence scores. |

## Format

Each skill is a single markdown file with frontmatter (`name`, `description`, `metadata.version`) and numbered sections covering detection logic, thresholds, action workflow, and a concrete example output. Skills are self‑contained — no shared context file is required.

## Usage

All these skills are directly available in [Adside.ai](https://adside.ai), within the Skills section under Settings:

<img width="1822" height="1109" alt="Screenshot 2026-04-22 at 09 29 45" src="https://github.com/user-attachments/assets/500c5402-41ff-456c-a0c0-cf8be255cdb6" />

Then you can invoke them with the /command in the Chat:

<img width="1822" height="1109" alt="Screenshot 2026-04-22 at 09 29 58" src="https://github.com/user-attachments/assets/167a7f38-00b3-4161-8208-68ebdbba292c" />

And voilà:

<img width="1822" height="1109" alt="Screenshot 2026-04-22 at 09 31 00" src="https://github.com/user-attachments/assets/8f82df96-8371-4d0c-9172-38c4b9f16655" />

You an also just drop a skill file into your agent's skills directory (e.g., `~/.claude/skills/` for Claude Code) or reference the raw file from this repo. The agent will pick it up on next invocation.

## License

MIT — see [LICENSE](LICENSE). Inspired by [coreyhaines31/marketingskills](https://github.com/coreyhaines31/marketingskills) and [TheMattBerman/meta-ads-kit](https://github.com/TheMattBerman/meta-ads-kit) 
