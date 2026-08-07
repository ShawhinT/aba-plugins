# aba-skills

Three skills that help you build, improve, and cost-optimize your own Claude skills. From [AI Builder Academy](https://aibuilder.academy).

## What's inside

| Skill | What it does | Try saying |
|---|---|---|
| `skill-helper` | Walks you from "I should probably automate something" to a working skill — researches how you work, suggests 3–5 high-leverage candidates, and builds the one you pick | *"Help me figure out what skill to build"* |
| `skill-updater` | Improves an existing skill from real usage — turns your corrections and edits into principles the skill keeps | *"Update the skill based on what we just did"* |
| `cost-optimizer` | Estimates what the current conversation cost in tokens and dollars, then translates it into a break-even hourly wage | *"What did this conversation cost — was it worth it?"* |

The three compose: **helper** finds and builds the skill worth having, **updater** keeps it sharp as you use it, and **optimizer** tells you what your sessions cost so you know where the leverage is.

## Install

### Claude Code — just you

In any Claude Code session (CLI, desktop app, or web):

```
/plugin marketplace add ShawhinT/aba-plugins
/plugin install aba-skills@aba
```

### Claude Desktop app

1. **Settings → Plugins → Add → Add marketplace**
2. Enter `ShawhinT/aba-plugins` (or `https://github.com/ShawhinT/aba-plugins`)
3. **Browse** the `aba` marketplace and install **ABA Skills**

### No plugin support on your surface?

Each skill is also packaged as a standalone `.skill` file in [`dist/`](https://github.com/ShawhinT/aba-plugins/tree/main/dist) — download and upload via **Settings → Skills**:

- [skill-helper.skill](https://github.com/ShawhinT/aba-plugins/raw/main/dist/skill-helper.skill)
- [skill-updater.skill](https://github.com/ShawhinT/aba-plugins/raw/main/dist/skill-updater.skill)
- [cost-optimizer.skill](https://github.com/ShawhinT/aba-plugins/raw/main/dist/cost-optimizer.skill)

This route has no auto-update — re-download and re-upload when a new version ships.

### Claude Code — your whole team

Add this to your project's `.claude/settings.json` and commit it — everyone on the repo gets the plugin automatically:

```json
{
  "extraKnownMarketplaces": {
    "aba": {
      "source": {
        "source": "github",
        "repo": "ShawhinT/aba-plugins"
      },
      "autoUpdate": true
    }
  },
  "enabledPlugins": {
    "aba-skills@aba": true
  }
}
```

### Org-wide

Admins can deploy the plugin to every user from Claude.ai Admin Settings → Plugins, or via managed settings. See the [managed settings docs](https://code.claude.com/docs/en/plugins-marketplaces). For teams living purely in claude.ai chat, admins can instead upload the `.skill` files workspace-wide.

## Updates

Plugin installs (Claude Code and Desktop): with `autoUpdate` on, updates arrive automatically at session start; otherwise run `/plugin marketplace update aba` or refresh from Settings → Plugins. Standalone `.skill` installs: re-download and re-upload.

Current version: **1.0.0** — release history lives in the [commit log](https://github.com/ShawhinT/aba-plugins/commits/main).

## Using the skills

**skill-helper** — say *"help me build my first skill."* It quietly researches how you work (sessions, connectors, calendar if connected), plays back what it sees, interviews you to fill the gaps, then suggests 3–5 candidate skills scored by hours saved vs. build complexity — and builds the one you pick.

**skill-updater** — at the end of a session where a skill misfired or you corrected its output, say *"update the skill with what we learned."* It extracts the general principle from your corrections and makes surgical edits to the skill — no bloat, no one-off rules.

**cost-optimizer** — at the end of a chat, say *"run the cost estimate."* It sizes the conversation's token usage, prices it, and reports a break-even wage — "as long as your time is worth more than $Y/hour, this paid off" — plus one concrete optimization if the setup was overkill.

## Requirements

- Claude Code (CLI or web sessions) or the Claude Desktop app for the plugin install; the standalone `.skill` route works anywhere skills can be uploaded
- `cost-optimizer` needs no setup
- `skill-helper` works best with connectors (Gmail, Calendar, Notion, …) so it can research how you actually work

---

Built by Shaw Talebi · [aibuilder.academy](https://aibuilder.academy) · questions → shaw@aibuilder.academy
