---
name: skill-updater
description: Update and improve existing skills based on conversation feedback. Use when the user asks to update a skill, improve a skill, or reflect on what was learned in a conversation to improve a skill. Also use when the user says things like "update the skill", "the skill should know about this", or "add this to the skill."
metadata:
  author: Shaw Talebi (aibuilder.academy)
  source: https://github.com/ShawhinT/aba-plugins
---

# Skill Updater

Improve existing skills by extracting principles from real usage — corrections, edits, and patterns that emerged during a conversation.

## Philosophy

Skills should be **minimal and principle-based**. A good skill explains *how* and *why*, not a checklist of dos and don'ts. The model using the skill is smart — give it understanding and it will handle edge cases it's never seen. Give it rigid rules and it will follow them blindly or break in novel situations.

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
| **Skill** | Formatting conventions, structural patterns, workflow steps, new capabilities |
| **Reference example** | A new completed artifact showing a pattern the skill doesn't have an example of |
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

Only reached after the greenlight in 3.5. How you hand the update back depends on where the skill lives and what the change touches.

- **Writable in place (e.g., Claude Code)** — edit the files directly and you're done; the user's saved skill updates live.
- **Cowork/Chat, change touches only SKILL.md** — call `save_skill` with `overwrite: true`. This is the default path: it replaces SKILL.md, keeps every other file in the skill (references stay intact), and persists across sessions — no packaging, no install click. The call requires the full new SKILL.md body plus the description, so the Step 5 description-cap check happens naturally at call time. For a long SKILL.md this default flips — see below.
- **Cowork/Chat, change adds or edits `references/` files or assets** — `save_skill` can only write SKILL.md, not sidecar files, so use the repackaged `.skill` route: copy the *entire* skill directory to outputs, apply the edits there, zip it as `<name>.skill`, and present that file so the user can install it with one click.

On the package route, a skill is its whole directory (SKILL.md *plus* every `references/` file and asset), so package all of it — an installed `.skill` missing its references is broken. Never substitute chat-pasted text, a "paste this into Settings" instruction, or SKILL.md on its own for the packaged skill; those force the user to do the assembly you were supposed to do.

**Scale the delivery route to the file, not the change.** `save_skill` retransmits the whole SKILL.md, so its risk tracks the file's length, not the edit's size — and nothing can confirm what landed, since the response doesn't echo content and the cache doesn't refresh. On a long SKILL.md, take the `.skill` package route even for a SKILL.md-only change: apply the edit as an anchored substitution against a copy, diff it, then zip and deliver *that* file. The bytes you verified are then the bytes installed — a diff on a sandbox copy proves nothing about content you retype into a tool call by hand.

**Cache staleness (both routes):** the installed skill files you can read in-session are a read-only cache, and it does not refresh after a `save_skill` call. After any same-session update, the source of truth is the content you last sent, not a re-read of the cache. This bites hardest on the package route: zipping the cached SKILL.md silently drops earlier same-session fixes, and installing that bundle rolls them back — so re-apply any updates the cache is missing before zipping.

## Anti-patterns

- **Encoding one-off decisions as permanent rules** — if it only applied to this specific client or situation, it probably doesn't belong in the skill
- **Bloating with edge cases** — if you need more than a sentence to describe a variation, add a reference example instead
- **Adding rules the user didn't ask for** — only update based on actual feedback or observed problems, not hypothetical improvements
- **Rewriting working sections** — if the user didn't have issues with a section, leave it alone
