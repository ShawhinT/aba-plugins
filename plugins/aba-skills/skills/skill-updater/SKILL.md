---
name: skill-updater
description: This skill owns how existing skills change, not how new ones get authored (that's skill-creator's job) — how a change is framed (root cause over symptom, principle over rule), how it's approved (a write to a saved skill is a commit, so it sits behind a greenlight), and how it's delivered. It applies whenever a saved skill is about to change, whatever phrasing got there — a one-line edit engages the same craft, because a bad skill edit compounds across every future run. Also owns where a learning should live — skill, reference file, external doc, or memory. Use when the user asks to update, improve, or fix a skill, says "the skill should know about this" or "add this to the skill", or casually points an edit at a skill ("let's just do the skill", "add this link to the table"). Also fires for pruning — "audit this skill", "prune the skill", "slim this down" — or sweeping skills for outdated scaffolding after a new model release.
metadata:
  author: Shaw Talebi (aibuilder.academy)
  source: https://github.com/ShawhinT/aba-plugins
---

# Skill Updater

Improve existing skills by extracting principles from real usage — corrections, edits, and patterns that emerged during a conversation.

## Philosophy

Skills should be **minimal and principle-based**. A good skill explains *how* and *why*, not a checklist of dos and don'ts. The model using the skill is smart — give it understanding and it will handle edge cases it's never seen. Give it rigid rules and it will follow them blindly or break in novel situations.

## Two modes

- **Feedback mode** (the default — the workflow below): improve a skill from what a conversation surfaced.
- **Audit mode**: prune a skill of content that has stopped earning its place — scaffolding written for weaker models, stale platform facts, redundancy. Run it when the user asks to audit, prune, or slim down a skill, or after a new model release. The decay taxonomy and procedure live in `references/pruning.md`; audit mode replaces steps 1–3 below and rejoins the workflow at step 3.5.

During a feedback-mode update, scaffolding you notice near the edit can be flagged as optional prune items in the change list — never silently removed as scope creep.

## Workflow

### 1. Reflect on what happened

Read the conversation and identify:
- **User corrections** — where the user said "no, not that" or fixed something
- **User edits** — changes the user made directly to files that differed from your output
- **New patterns** — structural or formatting patterns that the skill doesn't currently cover
- **Wasted effort** — things the skill made you do that weren't useful

### 1.5 Find the root cause before categorizing

Step 1 produces a list; most conversations produce one root cause. Trace the chain — the observations usually connect (a permissive instruction → a bad artifact → a shipped mistake → a user correction) — and propose the fix at the deepest point the evidence supports. A fix for a symptom the root fix would have prevented doesn't belong in the proposal: it guards against nothing once the root is fixed, and it's how skills bloat one defensible item at a time.

Weight the user's actual complaint over self-observed friction. What the user corrected is signal; what merely slowed you down is usually nowhere, unless it's certain to recur and the user's complaint doesn't touch it. Four individually well-framed principles are worse than the one right one — minimalism is a property of the proposal, not just of each item.

### 2. Categorize each learning

Not everything belongs in the skill. Route each insight to the right place:

| Belongs in... | Examples |
|---|---|
| **SKILL.md** | Formatting conventions, structural patterns, workflow steps, new capabilities |
| **Supplemental file** | Any file shipped inside the skill alongside SKILL.md — a reference example (`references/`), a script, an asset, a template. Use one when the learning is better carried by a file than described in prose, and only Claude needs it |
| **External record** | A source of truth outside the skill — a Notion page, doc, or live URL — that the skill points to and pulls at run time. Use when the content is needed by more than just Claude (humans read or edit it, other agents consume it), when it evolves on its own editorial cadence, or when an external authority already exists (e.g., a provider's pricing page). The dividing line from a supplemental file is the audience: Claude-only stays bundled; shared moves out |
| **Memory** | User preferences about Claude's behavior, project context, who/what/when |
| **Nowhere** | One-off decisions, things already derivable from code/examples |

### 3. Frame updates as principles

Transform specific corrections into general principles. The test: would this help with a *different* client/project, or does it only make sense for this one case?

**Bad (rule):** "Don't ask the user about discounts"
**Good (principle):** "When a discount applies, list sessions at full price and add a discount row in green"

**Bad (rule):** "Always check the investment table after editing session titles"
**Good (principle):** "Content that appears in multiple sections (titles, durations, structural terms) must stay consistent — when one instance changes, check all others including payment terms"

Each principle should convey the *why* so the model can reason about analogous situations. If you find yourself writing ALWAYS or NEVER in caps, reframe as an explanation instead.

Write for someone who wasn't in the conversation: they need the rule and why it holds, not the failure that prompted it. A draft that runs long is usually carrying that diagnosis, and cutting it makes the text shorter and more general at once.

### 3.5 Get the greenlight

Before touching any file, present the proposed updates as a short change list — each item: what changes, where, and the conversation evidence behind it. Wait for explicit approval; the user may veto items, reframe them, or add their own. Only then edit and package. Never deliver a repackaged `.skill` containing changes the user hasn't seen — an installable file is a commit, not a draft. A `save_skill` call is equally a commit — it writes to the user's saved skill immediately — so it sits behind this greenlight too.

The change list and the skill text are different artifacts. Evidence — what broke, which file, which phase — belongs in the change list so the user can judge the change, not in the skill text. Keeping them separate is what stops proposals from arriving pre-bloated.

### 4. Make targeted edits

- Read the current SKILL.md before changing anything
- Make surgical additions — don't rewrite sections that are working fine
- When a new artifact shows a different structural pattern, copy it to `references/` as an additional example rather than trying to describe every variation in prose
- Remove instructions that caused unproductive work (check run transcripts or conversation history for evidence)
- **The frontmatter `description` is capped at 1024 characters.** It's a routing trigger, not documentation — when a capability grows, the instinct is to bloat the description with more example phrases, but that's the wrong home. Add at most a phrase or two of new trigger language; put the actual *how* in the body or a `references/` file and point to it from the routing table. A trigger list that already covers the territory rarely needs another near-synonym.

### 5. Verify

- Re-read the updated SKILL.md to confirm coherence
- Check that new reference examples match the final version of the artifact
- Ensure no duplication with existing instructions
- If you touched the frontmatter `description`, confirm it's still ≤1024 characters — exceeding the cap makes the skill fail to load, so catch it here rather than at load time

### 6. Deliver

Only reached after the greenlight in 3.5. The route depends on what the session can actually write — which differs by surface and changes over time. Read it off the tools you have, not off what this skill documented last.

- **Writable in place (e.g., Claude Code)** — edit the files directly and you're done; the user's saved skill updates live.
- **A skill that ships inside a plugin** — none of the routes below. A save writes a *personal* skill that shadows the plugin's copy instead of updating it, so the change silently fails to reach anyone who installed the plugin. Route to the plugin's own update path.
- **`propose_skills` available** — the current Cowork default. Installed skill files are a read-only cache and there is no write tool, so this call renders a review card and writes nothing; the update isn't landed until the user saves from it. Pass the complete SKILL.md, frontmatter included — the card replaces the whole file.
- **`save_skill` available** — call it with `overwrite: true`: it replaces SKILL.md, keeps every other file in the skill, and persists across sessions. It cannot write sidecar files, so a change touching `references/` or assets takes the packaged route instead: copy the *entire* skill directory to outputs, apply the edits there, zip it as `<name>.skill`, and present that file.

**The card's `description` becomes the skill's routing description.** `propose_skills` takes a `description` parameter that reads like a display field — a one-liner for the card — but it lands on the saved skill and overwrites the frontmatter description. Summarize the diff there and the skill stops triggering, because nothing in "adds X and Y" matches how anyone asks for it. Pass the routing description verbatim in *both* the parameter and the frontmatter; the change summary belongs in the chat message.

On the package route, a skill is its whole directory (SKILL.md *plus* every `references/` file and asset), so package all of it — an installed `.skill` missing its references is broken. Never substitute chat-pasted text, a "paste this into Settings" instruction, or SKILL.md on its own for the packaged skill; those force the user to do the assembly you were supposed to do.

**Scale the delivery route to the file, not the change.** Every route here retransmits the whole SKILL.md, so risk tracks the file's length, not the edit's size — and nothing echoes back what landed. On a long SKILL.md, compose the new file as an anchored substitution against a copy on disk and diff it before sending, so the bytes you verified are the bytes you transmit. Where the `.skill` package route is available, prefer it on long files for the same reason.

**Cache staleness (all routes):** the installed skill files you can read in-session are a read-only cache, and it does not refresh after a write. After any same-session update, the source of truth is the content you last sent, not a re-read of the cache. This bites hardest on the package route: zipping the cached SKILL.md silently drops earlier same-session fixes, and installing that bundle rolls them back — so re-apply any updates the cache is missing before zipping.

## Anti-patterns

- **Encoding one-off decisions as permanent rules** — if it only applied to this specific client or situation, it probably doesn't belong in the skill
- **Bloating with edge cases** — if you need more than a sentence to describe a variation, add a reference example instead
- **Adding rules the user didn't ask for** — only update based on actual feedback or observed problems, not hypothetical improvements
- **Rewriting working sections** — if the user didn't have issues with a section, leave it alone
