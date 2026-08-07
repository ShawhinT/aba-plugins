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

### Claude Desktop app (just you)

1. **Settings → Plugins → Add → Add marketplace**
2. Enter `ShawhinT/aba-plugins` and hit "Sync" — toggle on **auto-update** here if you want new versions automatically
3. In Plugin Directory click "+" button for "ABA Skills" plugin

### Claude Code (just you)

In any Claude Code session (CLI, desktop app, or web):

```
/plugin marketplace add ShawhinT/aba-plugins
/plugin install aba-skills@aba
```

### Org-wide (Team / Enterprise)

Owners can deploy the plugin to the whole organization from **Organization settings → Plugins**:

1. **Add plugins → GitHub** and enter `ShawhinT/aba-plugins` (the Claude GitHub App needs access to the repo; alternatively **Upload a file** with the plugin ZIP)
2. Set ABA Skills' installation preference: **Installed by default**, **Available for install**, or **Required**
3. Optionally open the marketplace menu (upper right) and toggle **Sync automatically** to pick up new versions

Changes reach members on their next session or plugin refresh. Full details in the [Help Center article](https://support.claude.com/en/articles/13837433-manage-plugins-for-your-organization).

## Updates

- **Claude Code:** with `autoUpdate` on, updates arrive automatically at session start; otherwise run `/plugin marketplace update aba`.
- **Claude Desktop app:** if you toggled auto-update on when adding the marketplace, updates arrive automatically; otherwise click **Update** on the plugin in the Plugin Directory.

## Using the skills

**skill-helper** — say *"help me build my first skill."* It quietly researches how you work (sessions, connectors, calendar if connected), plays back what it sees, interviews you to fill the gaps, then suggests 3–5 candidate skills scored by hours saved vs. build complexity — and builds the one you pick.

**skill-updater** — at the end of a session where a skill misfired or you corrected its output, say *"update the skill with what we learned."* It extracts the general principle from your corrections and makes surgical edits to the skill — no bloat, no one-off rules.

**cost-optimizer** — at the end of a chat, say *"run the cost estimate."* It sizes the conversation's token usage, prices it, and reports a break-even wage — "as long as your time is worth more than $Y/hour, this paid off" — plus one concrete optimization if the setup was overkill.

## Requirements

- `skill-helper` works best with connectors (Gmail, Calendar, Notion, …) so it can research how you actually work

---

Built by Shaw Talebi · [aibuilder.academy](https://aibuilder.academy) · questions → shaw@aibuilder.academy
