# Audit mode — pruning a skill as models and platforms move

Feedback mode asks "what did this conversation teach us to add?" Audit mode asks the symmetric question: "what has the world taught us to remove?" Every model release makes some instructions unnecessary — guidance written to compensate for a weaker model becomes dead weight, or worse, constrains a stronger model into a path it would have improved on its own. Skills also accumulate facts that quietly go stale as platforms change.

Run this mode when the user asks to audit, prune, or slim down a skill, or wants a sweep after a new model release. A whole-library sweep is just this procedure looped per skill, one change list each.

## The decay taxonomy

Classify every piece of skill content into one of five classes. Each class has its own decay clock and its own audit action.

| Class | What it looks like | Decays with | Audit action |
|---|---|---|---|
| **Capability scaffolding** | Process micromanagement ("mark the task complete *and* the next one in-progress in the same update"), tutorials on when to use which tool, hand-holding steps, ALL-CAPS emphasis | Model releases | Delete, or compress to the one-line constraint |
| **Diagnosis baggage** | Paragraphs explaining the failure a rule prevents | Model releases | Keep the rule, cut the essay |
| **Platform facts** | Prices, API mechanics, rate limits, surface behaviors, "as of" dates | Platform changes | Verify still true. Where a live source of truth exists, point to it instead of storing the fact |
| **Redundancy** | The same guidance stated in multiple homes | Nothing — it's just bloat | Pick one home, pointer from the rest |
| **Durable context** | User preferences, business decisions, frameworks, domain facts, earned one-line constraints | Nothing | Keep — this is the skill |

**The classification test: "Would the current model do this correctly given only the goal and the durable facts?"** If yes, it's scaffolding. The test is *derivability*, not obviousness — a capable model finds good methodology "obvious-seeming" too, and pruning methodology is how an audit destroys a skill. Frameworks, taxonomies, and judgment criteria stay underivable no matter how smart the model gets.

**Durable content can still be mislocated.** The taxonomy asks "should this content exist?"; for durable context, also ask "*where* should it live?" The dividing line is the audience: content only Claude needs belongs bundled with the skill; content that humans also read or edit — and potentially other agents consume — belongs in an external record (a Notion page, doc, or live URL) the skill points to and pulls at run time. A negotiation playbook, a brand guide, an SOP: as a record, it evolves on its own editorial cadence with no skill-update ceremony, and every consumer sees the same version. The tradeoff is a fetch per use, so Claude-only content that changes only when the skill changes stays a bundled reference. The audit action is **relocate**: move the content out, leave the pointer and the "when to pull it" instruction.

## Procedure

1. **Read the entire skill** — SKILL.md plus every reference file and script. Never prune a pointer whose target you haven't read, and never audit from SKILL.md alone.
2. **Classify section by section** against the taxonomy. Look especially for: instructions teaching the model how to use its own tools, multi-sentence justifications trailing a rule, facts carrying an "as of" date, and content that appears in both SKILL.md and a reference file.
3. **Produce the prune list** — one row per finding: location, class, proposed action (delete / compress / relocate / externalize / verify), and a risk note.
4. **Rejoin the main workflow at step 3.5** — the prune list *is* the change list: present it, wait for the greenlight, then make targeted edits, verify, and deliver exactly as in feedback mode.

## Safety rails

- **Earned guardrails get demoted, not deleted.** A rule specific enough to smell like a past incident compresses to its one-line form; outright deletion needs the user's explicit confirmation, because the incident history behind it isn't visible to you.
- **Frontmatter descriptions are off-limits.** They're routing, not instructions — trigger matching doesn't improve on the same curve as instruction following.
- **Length is not the target.** The goal is signal density. A long skill that's all durable context passes the audit untouched; don't let audit mode become "make it shorter."
- **No style rewrites while pruning.** Same rule as feedback mode: leave working sections alone.

## Worked examples

- **Scaffolding — delete:** "At each transition, flip the previous task to `completed` *and* the next one to `in_progress` in the same update." A current model keeps a task list synced without instruction. → "Keep the list current."
- **Diagnosis baggage — compress:** "Don't suggest candidates during the interview. Listing candidates at this stage biases the conversation: the user starts reacting to your suggestions instead of filling the gaps you need filled..." → keep the rule plus one clause of why; cut the rest.
- **Platform fact — externalize:** a hardcoded price table with an "as of" date. → Delete the table; fetch the provider's live pricing page at run time and pass the values in. The fact now has one source of truth and nothing to keep in sync.
- **Durable context — keep:** "When a discount applies, list sessions at full price and add a discount row in green." No model release makes this derivable — it's a preference, not a capability gap.
