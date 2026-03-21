# Ultrapowers

I found [superpowers](https://github.com/obra/superpowers) sometime last year. And honestly? It clicked immediately.

The idea is simple: give Claude a set of structured skills — brainstorm before you code, write a real plan before you touch anything, review your own work like you hate it — and suddenly the whole "AI coding assistant" thing stops feeling like autocomplete on steroids and starts feeling like an actual workflow. I liked that. I still like that.

**But.**

Superpowers is a framework. A general one. And **general things have this annoying habit of not fitting your specific situation.**

The brainstorming skill was close to what I wanted, but it was missing something specific. When you're doing novel work like translating between languages, implementing an unfamiliar algorithm, designing a custom protocol — **there's a class of assumptions that will silently wreck your implementation** if you don't surface them early. Superpowers didn't have a step for that. My version adds a novelty detection phase: if the task is uncertain enough, you generate explicit IF...THEN hypotheses for the riskiest assumptions before writing the design. Small addition, but it catches a different category of mistake than clarifying questions do.

I also cut the visual companion (token-heavy, not worth it) and the automated spec review loop. **Simpler.**

The code review flow existed, but I wanted something more adversarial. Not "here are some suggestions" — I wanted the reviewer to actually try to tear the work apart. Find the thing I missed. **Be uncomfortable about it.**

And quick iteration? That didn't exist at all. Sometimes you just need to fix one thing. You don't want to brainstorm for 20 minutes, write a spec, create a whole plan, and *then* change three lines. **You want to do the thing, have someone briefly sanity-check the plan, and move on.**

So I built Ultrapowers. It's not a replacement — most of **it is still fundamentally superpowers, just tuned**. What I added were the gaps.

Feel free to use it. Adapt it. The whole point is that these things should fit how *you* work.

---

## Skills

| Skill | What it does |
|-------|-------------|
| `brainstorming` | Explore ideas before implementing. For novel/uncertain tasks, generates testable IF...THEN hypotheses for risky assumptions before writing the design. Mandatory before any creative work. |
| `writing-plans` | Convert an approved design into a real implementation plan. |
| `validate-plan` | Validate implementation plans against DRY, YAGNI, TDD principles — catches over-engineering before you start coding. |
| `adversarial-review` | Rigorous self-critique. Not polite feedback — it tries to tear the work apart. |
| `requesting-code-review` | Kicks off review using the `code-reviewer` agent. |
| `quick-iteration` | For small changes that don't need a full ceremony. Still has a plan + subagent review gate. |
| `skillkit` | Tools for creating *great* skills. |
| `subagent-driven-development` | Execute plans by dispatching subagents per task, with two-stage review. |
| `systematic-debugging` | Forces you to find root cause before proposing a fix. Works with the `bug-hunter` agent. |
| `releasing` | Full release workflow: version bump, changelog, git tag, push, GitHub release. |

## Agents

| Agent | What it does |
|-------|-------------|
| `code-reviewer` | Senior reviewer — checks implementation against plan, architecture, and best practices. |
| `bug-hunter` | Debugging specialist. Finds root cause first, fixes second. |

---

## Workflows

### When you need a real plan (4+ tasks, anything non-trivial)

Design first. Context is cheap before you write code. Expensive after.

1. **brainstorming** — Figure out what you're actually building. Run `adversarial-review` on the spec that comes out. Kill bad assumptions now.
2. **writing-plans** — Turn the approved spec into a step-by-step implementation plan.
3. **Exit. Open a fresh chat.** Run `validate-plan` on the plan. Fix or consciously skip every issue it finds. Don't carry stale context into implementation.
4. **Exit again. Fresh chat.** Start implementing with `executing-plans` + `using-git-worktrees` — isolated branch, clean slate.
5. **requesting-code-review** — Review per batch of ~3 tasks, not all at once. Smaller batches, better feedback.
6. **finishing-a-development-branch** — Merge the worktree back into main when review passes.
7. **releasing** — Version bump, changelog, tag, push, GitHub release.

The context resets between steps 2→3 and 3→4 are intentional. **A fresh context catches things a tired one misses.**

---

### When you need a quick fix or fast feature

**Just use `quick-iteration`.**

It's not cowboy coding — there's still a brief plan (3–5 bullets) and a mandatory subagent review before you touch anything. But there's no spec, no ceremony, no worktree. Write the bullets, let the reviewer sanity-check them, implement. That's it.

Rule of thumb: if you can describe the change in 5 bullets and it doesn't touch architecture, `quick-iteration` is the right tool.

---

### When something is broken

Run `systematic-debugging` first. Not after you've already tried three things. First.

Once you have a hypothesis worth testing, ask the main agent to spawn a `bug-hunter` subagent. It specializes in finding root cause without jumping to fixes. When the root cause is confirmed, return to `quick-iteration` or the full workflow depending on how deep the fix goes.

---

## Usage

Skills are invoked via Claude Code's Skill tool, or directly:

```bash
/brainstorming
/writing-plans
/quick-iteration
/etc..
```

Claude will also detect relevant skills automatically based on context — you don't always have to invoke them manually.

---

## What's different from superpowers

| Ultrapowers | Original |
|-------------|----------|
| `brainstorming` | `superpowers:brainstorming` |
| `writing-plans` | `superpowers:writing-plans` |
| `validate-plan` | Custom |
| `adversarial-review` | Custom |
| `requesting-code-review` | `superpowers:requesting-code-review` |
| `quick-iteration` | Custom |
| `skillkit` | Custom |
| `subagent-driven-development` | `superpowers:subagent-driven-development` |
| `systematic-debugging` | `superpowers:systematic-debugging` |
| `releasing` | Custom |
| `code-reviewer` (agent) | `superpowers:code-reviewer` |
| `bug-hunter` (agent) | Custom |

---

## License

WTFPL
